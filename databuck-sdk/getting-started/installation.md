# Installation

Install the DataBuck SDK, add your license file, and let the SDK download its Spark JAR. The PyPI package is `databuck-sdk`; the Python import is `databuck`.

## Requirements

* Python with a Spark environment (PySpark)
* Your signed `databucksdk.properties` license file
* Internet access and write access to the JAR location, for the first download

## 1. Install the package

```bash
python -m pip install databuck-sdk
```

{% hint style="info" %}
If the older `databuck-spark-sdk` package is installed, remove it first. Both packages provide the same `databuck` import.

```bash
python -m pip uninstall databuck-spark-sdk
python -m pip install --upgrade databuck-sdk
```
{% endhint %}

In a notebook, install with `%pip install databuck-sdk`, then restart the Python kernel so the new version loads.

## 2. Add your license file

Each customer receives one signed `databucksdk.properties` file:

```properties
customer_id=your-customer-id
issued_on=2026-11-02
expires_on=2027-12-31
license_key=signed-key-created-by-the-issuer
```

The license is valid through its `expires_on` date, inclusive. When it expires, replace it with a newly issued file.

{% hint style="warning" %}
Do not edit the file. Changing the dates or customer ID invalidates the signature.
{% endhint %}

Set `DATABUCK_SDK_CONFIG_FILE` to the file's path **before** importing the SDK:

```python
import os
os.environ["DATABUCK_SDK_CONFIG_FILE"] = "/path/to/databucksdk.properties"

from databuck import DataBuck
```

## 3. Download the JAR

The SDK runs on Spark through `databuck-sdk.jar`. The JAR is too large for PyPI, so it is not in the package. Instead, the first `import databuck` downloads it, with progress output. Later imports reuse it.

By default the JAR goes to `databuck/jars/` inside the installed package. To choose another location, or to use a JAR you already have, set `DATABUCK_SDK_JAR` to the full file path, ending in `.jar`, before importing.

| Environment variable | Purpose |
| --- | --- |
| `DATABUCK_SDK_CONFIG_FILE` | Path to your license file (required) |
| `DATABUCK_SDK_JAR` | Path to download the JAR to, or an existing JAR to use |
| `DATABUCK_SDK_AUTO_DOWNLOAD=0` | Skip the download on import, for example when offline |
| `DATABUCK_SDK_JAR_URL` | A new HTTPS download URL, if it changes |

To download the JAR explicitly and print its path, run:

```bash
python -m databuck
```

{% hint style="warning" %}
The download checks that the file is a valid JAR archive but does not verify a checksum. Only set `DATABUCK_SDK_JAR_URL` to a URL you trust.
{% endhint %}

## 4. Verify the install

```python
from importlib.metadata import version
from databuck import DataBuck

print(version("databuck-sdk"))
print(DataBuck.jar_path())
```

This prints the installed version and the JAR's local path.

## 5. Load the JAR into Spark

Downloading the JAR does not add it to Spark. Load it before calling `DataBuck.discover(...)` by passing its path when you create the Spark session:

```python
from pyspark.sql import SparkSession
from databuck import DataBuck

spark = (
    SparkSession.builder
    .config("spark.jars", DataBuck.jar_path())
    .getOrCreate()
)
```

On a managed Spark platform, install the JAR as a library on the compute that runs discovery instead. Platform steps are under **Integrations**.

Next: [Quickstart](quickstart.md).
