---
description: "Install DataBuck SDK locally, in Databricks or in Microsoft Fabric."
icon: download
---

# Installation

## Requirements

* Python `<VERSION>` or later
* PySpark (for Spark DataFrames) or pandas
* A Databricks or Microsoft Fabric notebook, or a local Python environment

## Install

{% tabs %}
{% tab title="Local (pip)" %}
```bash
pip install <PACKAGE_NAME>
```
{% endtab %}

{% tab title="Databricks" %}
Run this in the first cell of your notebook:

```python
%pip install <PACKAGE_NAME>
dbutils.library.restartPython()
```

See [Databricks notebook-scoped libraries](https://docs.databricks.com/en/libraries/notebooks-python-libraries.html) for details.
{% endtab %}

{% tab title="Microsoft Fabric" %}
Run this in the first cell of your notebook:

```python
%pip install <PACKAGE_NAME>
```

See [Fabric inline installation](https://learn.microsoft.com/en-us/fabric/data-engineering/library-management) for details.
{% endtab %}
{% endtabs %}

## Check the install

```python
import databuck
print(databuck.__version__)
```

{% hint style="success" %}
The Developer SDK is free to use. See [Editions](../reference/editions.md) for what the Enterprise SDK adds.
{% endhint %}

<details>
<summary>Troubleshooting: <code>ModuleNotFoundError</code></summary>

Make sure you installed the package in the same environment as your notebook kernel. In Databricks, restart Python after installing.
</details>
