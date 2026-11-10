# Databricks

Run the DataBuck SDK in a Databricks notebook: install it, generate data quality rules for a table, and enforce them in a Lakeflow Spark Declarative Pipeline. The PyPI package is `databuck-sdk`; the Python import is `databuck`.

## Prerequisites

* A Databricks workspace with compute that can install libraries
* A Unity Catalog Volume with write access, for the license file, JAR and output rules
* Your signed `databucksdk.properties` license file
* Internet access on first import, to download the JAR
* Optional: a Gemini API key in a Databricks secret scope, for AI-generated rules
* Optional: the [DataBuck Databricks notebook](https://datauck-sdk.s3.us-east-1.amazonaws.com/DataBuck_SDK.ipynb), which runs every step below

## Start from the notebook (optional)

The [DataBuck Databricks notebook](https://datauck-sdk.s3.us-east-1.amazonaws.com/DataBuck_SDK.ipynb) contains every step on this page, ready to run. To import it:

1. In the Databricks sidebar, select **Workspace** and open the folder you want to use.
2. Select the **⋮** menu > **Import**.
3. Choose **URL**, paste the notebook link, then select **Import**.

Update the paths and table names in the notebook to match your workspace, then follow the steps below.

## 1. Install the SDK

Pin a version so results are reproducible, then restart Python. Restarting clears all variables, so run the next steps after it.

```python
%pip install --upgrade --no-cache-dir databuck-sdk==0.5.56
```

```python
dbutils.library.restartPython()
```

{% hint style="info" %}
If the older `databuck-spark-sdk` package is installed, uninstall it first. Both provide the same `databuck` import.
{% endhint %}

## 2. Set the license file

The license file holds `customer_id`, `issued_on`, `expires_on` (valid through that date, inclusive) and `license_key`. Store it in a Volume and set its path **before** importing `databuck`:

```python
import os
os.environ["DATABUCK_SDK_CONFIG_FILE"] = "/Volumes/<catalog>/<schema>/<volume>/databucksdk.properties"
```

{% hint style="warning" %}
Do not edit the file; changing any value invalidates the signature. When it expires, replace it with a newly issued file.
{% endhint %}

## 3. Set the JAR path

The SDK runs profiling on Spark through `databuck-sdk.jar`. Set the full file path, ending in `.jar`, **before** importing `databuck`:

```python
os.environ["DATABUCK_SDK_JAR"] = "/Volumes/<catalog>/<schema>/<volume>/databuck-sdk.jar"
```

On first import the SDK downloads the JAR to this path and checks that it is a valid JAR archive. Later imports reuse it.

| Optional variable | Purpose |
| --- | --- |
| `DATABUCK_SDK_AUTO_DOWNLOAD=0` | Skip the automatic download, for example when offline |
| `DATABUCK_SDK_JAR_URL` | Override the download URL |

## 4. Import and verify

```python
from importlib.metadata import version
from databuck import DataBuck

print(version("databuck-sdk"))
print(DataBuck.jar_path())
```

## 5. Attach the JAR to your compute

The import only downloads the JAR; Spark must load it as a compute library.

1. Open **Compute**, select the compute running the notebook, then **Libraries** > **Install New**.
2. Choose **Volumes** as the source, select the JAR path from step 3, and install it.
3. If the compute is running, restart it or reattach the notebook.
4. If an old `databuck-spark-sdk.jar` is attached, remove it first.

See [Databricks compute-scoped libraries](https://docs.databricks.com/aws/en/libraries/cluster-libraries).

## 6. Generate rules

Pass a Unity Catalog table name (`catalog.schema.table`) or a Spark DataFrame, plus an output path:

```python
output_path = DataBuck.discover(
    "<catalog>.<schema>.<table>",
    "/Volumes/<catalog>/<schema>/<volume>/my_table_rules_v1.json",
    context={
        "gemini_api_key": dbutils.secrets.get(scope="<scope>", key="<key>"),
        "domain_name": "banking",
        "pdf_paths": ["/Volumes/<catalog>/<schema>/<volume>/policy.pdf"],
    },
)
print(f"Expectations saved to: {output_path}")
```

DataBuck profiles the table, then BuckGPT generates and audits business rules from the domain and policy documents.

{% hint style="danger" %}
Never hardcode the API key. Read it from a secret scope.
{% endhint %}

| `context` key | Effect |
| --- | --- |
| `gemini_api_key` | Key for the LLM calls |
| `domain_name` | Business domain; output keeps only HIGH and MEDIUM priority rules |
| `pdf_paths` | PDF or DOCX policy and regulation documents that ground the rules |

Omit `context` to export only automatic profiling rules, all in the `warn` group. Use a new version suffix for each run.

## 7. Choose the output format

| Extension | Output |
| --- | --- |
| `.json` | Rules grouped as `warn`, `drop` and `fail` |
| `.yaml` / `.yml` | A flat `expectations` list |
| `.gx.json` | A Great Expectations 1.x suite |

If a dataset-level uniqueness rule is found, the JSON includes a `dataset_checks` section. Run its SQL separately against the full dataset, replacing `{{DATAFRAME}}` with your table or view.

For Great Expectations output, install `great-expectations>=1.0,<2`, restart Python, repeat steps 2 to 4, then call `discover` with a `.gx.json` path. GX reports failures but does not drop rows or stop a pipeline.

## 8. Enforce rules in a Lakeflow pipeline

Load the JSON and pass each group to its expectation decorator:

| Group | Decorator | Effect on bad rows |
| --- | --- | --- |
| `warn` | `expect_all` | Recorded; rows kept |
| `drop` | `expect_all_or_drop` | Removed from the output |
| `fail` | `expect_all_or_fail` | The pipeline update stops |

Results appear in the validated table's **Expectations** view after the pipeline runs. Regenerate the rules when the source schema or business policy changes.
