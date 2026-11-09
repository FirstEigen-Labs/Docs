---
description: "Free auto rule discovery for your data: statistics plus AI, exported in the format you already use."
icon: hand-wave
---

# Welcome to DataBuck SDK

**Stop writing data quality rules by hand.** DataBuck SDK is a free tool from FirstEigen that discovers them for you. It profiles your data with statistics, then uses AI and your own policy documents to add the business rules that profiling alone misses.

```python
DataBuck.discover(df, "rules.json")
```

One call, and you get rules ready to run in your pipeline.

## Why DataBuck SDK

{% columns %}
{% column %}
### Statistics + AI

Profiling finds the patterns in your data. AI turns your policies and regulations into business rules.
{% endcolumn %}

{% column %}
### Export anywhere

Rules come out as Great Expectations, DQX, JSON, YAML and more, ready for the tools you already use.
{% endcolumn %}

{% column %}
### Runs where your data lives

Everything runs in your own Spark environment. Your data never leaves it, and the SDK keeps no statistics from it.
{% endcolumn %}
{% endcolumns %}

## Built to catch what others miss

In internal testing against 79 expert-written rules, DataBuck SDK covered **86.1%**, compared with 67.1% for PyDeequ and 62.0% for Databricks DQX. It caught **87% of critical rules** and **14 of 22 cross-column business rules**, which neither alternative caught.

[See the full benchmark](reference/benchmark.md)

## Get started

{% content-ref url="getting-started/installation.md" %}
[installation.md](getting-started/installation.md)
{% endcontent-ref %}

{% content-ref url="getting-started/quickstart.md" %}
[quickstart.md](getting-started/quickstart.md)
{% endcontent-ref %}
