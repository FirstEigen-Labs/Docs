# Quickstart

Generate data quality rules for a Spark DataFrame or table with one call. Complete [Installation](installation.md) first.

{% hint style="info" %}
DataBuck runs on Spark and needs its JAR loaded into the Spark session. We currently support Databricks and Microsoft Fabric notebooks. Follow your platform's guide to set up the license and JAR, then use the steps below.
{% endhint %}

{% content-ref url="../integrations/databricks.md" %}
[databricks.md](../integrations/databricks.md)
{% endcontent-ref %}

{% content-ref url="../integrations/microsoft-fabric.md" %}
[microsoft-fabric.md](../integrations/microsoft-fabric.md)
{% endcontent-ref %}

## 1. Load your data

Use any Spark DataFrame:

```python
import os
os.environ["DATABUCK_SDK_CONFIG_FILE"] = "/path/to/databucksdk.properties"

from pyspark.sql import SparkSession
from databuck import DataBuck

spark = (
    SparkSession.builder
    .config("spark.jars", DataBuck.jar_path())
    .getOrCreate()
)

df = spark.read.csv("customers.csv", header=True)
print(DataBuck.count(df))
```

`DataBuck.count(df)` is a quick check that Spark has loaded the JAR.

## 2. Discover rules

Pass the DataFrame and an output file path:

```python
json_path = DataBuck.discover(df, "expectations.json")
print(json_path)
```

DataBuck profiles the data and writes automatic rules, such as not-null, length and pattern checks. Without AI context, all rules go into the `warn` group.

You can also pass a registered table name instead of a DataFrame. The SDK reads it with the current Spark session:

```python
json_path = DataBuck.discover("my_database.customers", "expectations.json")
```

## 3. Add policy-aware rules

Add a `context` to have BuckGPT generate rules from your business domain and policy documents:

```python
context = {
    "gemini_api_key": os.environ["GEMINI_API_KEY"],
    "domain_name": "banking",
    "pdf_paths": ["customer_policy.pdf", "data_contract.docx"],
}

json_path = DataBuck.discover(df, "expectations.json", context=context)
```

| `context` key | Effect |
| --- | --- |
| `gemini_api_key` | Key for the LLM calls |
| `domain_name` | Business domain; output keeps only HIGH and MEDIUM priority rules |
| `pdf_paths` | PDF and DOCX policy files that ground the rules |

{% hint style="danger" %}
Never hardcode the API key. Read it from an environment variable or a secret store.
{% endhint %}

## 4. Read the output

The JSON groups each rule by the action to take on rows that fail it. Each rule maps a name to a Spark SQL pass condition:

```json
{
  "warn": {"not_null_customer_email": "`customer_email` IS NOT NULL"},
  "drop": {"not_null_order_id": "`order_id` IS NOT NULL"},
  "fail": {"valid_pattern_currency_code": "`currency_code` IS NULL OR CAST(`currency_code` AS STRING) RLIKE '(?:^[A-Z][A-Z][A-Z]$)'"}
}
```

| Group | Meaning |
| --- | --- |
| `warn` | Record the violation and keep the row |
| `drop` | Remove the row from the validated output |
| `fail` | Stop the pipeline update |

The file extension sets the format: `.json` for grouped rules, `.yaml` or `.yml` for a flat list, and `.gx.json` for a Great Expectations suite.

## Next steps

* Output formats in detail: [Exporting rules](../guides/exporting-rules.md)
* Platform setup: [Databricks](../integrations/databricks.md) or [Microsoft Fabric](../integrations/microsoft-fabric.md)
