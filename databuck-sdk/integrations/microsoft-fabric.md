---
description: "Run DataBuck SDK in a Microsoft Fabric notebook on Lakehouse tables."
icon: microsoft
---

# Microsoft Fabric

DataBuck SDK runs inside a Fabric notebook and works with Lakehouse tables.

{% columns %}
{% column %}
### What you need

* A Fabric workspace
* A Lakehouse with at least one table
* A notebook attached to the Lakehouse
{% endcolumn %}

{% column %}
### What you get

* Rules discovered from your table
* Validation results as a DataFrame
* Rules exported to your Lakehouse files
{% endcolumn %}
{% endcolumns %}

## Example

```python
df = spark.read.table("<LAKEHOUSE>.<TABLE>")

from databuck import DataBuck  # placeholder
rules = DataBuck().discover_rules(df)
rules.export("Files/dq/rules.yaml", format="yaml")
```

## Microsoft docs

{% embed url="https://learn.microsoft.com/en-us/fabric/data-engineering/how-to-use-notebook" %}

{% embed url="https://learn.microsoft.com/en-us/fabric/data-engineering/lakehouse-overview" %}
