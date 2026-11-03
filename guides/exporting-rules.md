---
description: "Export discovered rules as JSON, YAML, Great Expectations or DQX."
icon: file-export
---

# Exporting rules

You can export discovered rules to use them in other tools.

| Format | Use it for | Learn more |
| --- | --- | --- |
| JSON | General use and APIs | [json.org](https://www.json.org/) |
| YAML | Config files and version control | [yaml.org](https://yaml.org/) |
| Great Expectations | Existing GX pipelines | [GX docs](https://docs.greatexpectations.io/) |
| DQX | Databricks DQX checks | [DQX docs](https://databrickslabs.github.io/dqx/) |

## Examples

{% tabs %}
{% tab title="JSON" %}
```python
rules.export("rules.json", format="json")  # placeholder
```

```json
{
  "rule": "loan_amnt_range",
  "column": "loan_amnt",
  "check": "between",
  "min": 500,
  "max": 40000
}
```
{% endtab %}

{% tab title="YAML" %}
```python
rules.export("rules.yaml", format="yaml")  # placeholder
```

```yaml
- rule: loan_amnt_range
  column: loan_amnt
  check: between
  min: 500
  max: 40000
```
{% endtab %}

{% tab title="Great Expectations" %}
```python
rules.export("gx_suite.json", format="great_expectations")  # placeholder
```
{% endtab %}

{% tab title="DQX" %}
```python
rules.export("dqx_checks.yaml", format="dqx")  # placeholder
```
{% endtab %}
{% endtabs %}

## Learn about the target tools

{% embed url="https://www.youtube.com/watch?v=AgSEc90KpeI" %}
Building a data quality framework in Databricks with Great Expectations
{% endembed %}

{% embed url="https://github.com/databrickslabs/dqx" %}
