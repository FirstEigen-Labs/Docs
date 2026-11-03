---
description: "Discover your first data quality rules in under five minutes."
icon: bolt
---

# Quickstart

This example profiles a table, discovers rules and validates the data.

{% stepper %}
{% step %}
## Load your data

```python
df = spark.read.table("<CATALOG>.<SCHEMA>.<TABLE>")
```
{% endstep %}

{% step %}
## Discover rules

```python
from databuck import DataBuck  # placeholder import

db = DataBuck()
rules = db.discover_rules(df)  # placeholder method
print(f"Found {len(rules)} rules")
```
{% endstep %}

{% step %}
## Review the rules

```python
for rule in rules:
    print(rule.name, "-", rule.description)
```
{% endstep %}

{% step %}
## Validate the data

```python
results = db.validate(df, rules)  # placeholder method
results.show()
```
{% endstep %}
{% endstepper %}

## See it in action

{% embed url="https://www.youtube.com/watch?v=aV7hyF0keWI" %}
DataBuck automated data quality demo on BigQuery
{% endembed %}

## Next steps

{% content-ref url="../guides/configuring-ai.md" %}
[configuring-ai.md](../guides/configuring-ai.md)
{% endcontent-ref %}

{% content-ref url="../guides/exporting-rules.md" %}
[exporting-rules.md](../guides/exporting-rules.md)
{% endcontent-ref %}
