---
description: "Run DataBuck SDK in a Databricks notebook and on Unity Catalog tables."
icon: database
---

# Databricks

DataBuck SDK runs inside your Databricks notebook. Your data never leaves your workspace.

## Read a Unity Catalog table

```python
df = spark.read.table("main.lending.loans")
rules = DataBuck().discover_rules(df)  # placeholder
```

## Use the rules with DQX

Export the rules to DQX format and apply them with the DQX engine.

```python
rules.export("/Volumes/main/dq/checks/loans.yaml", format="dqx")  # placeholder
```

{% hint style="info" %}
New to DQX? Watch the intro below or read the [DQX guide](https://databrickslabs.github.io/dqx/docs/guide/).
{% endhint %}

{% embed url="https://www.youtube.com/watch?v=e5Qvx_gnxTE" %}
Elevating data quality standards with Databricks DQX
{% endembed %}

## Schedule it as a job

{% stepper %}
{% step %}
## Save the notebook

Put the discovery and validation code in one notebook.
{% endstep %}

{% step %}
## Create a job

In Databricks, go to **Workflows** and create a job that runs the notebook. See [Databricks Jobs](https://docs.databricks.com/en/jobs/index.html).
{% endstep %}

{% step %}
## Pick a schedule

Run it after each data load, for example daily.
{% endstep %}
{% endstepper %}

## Related

* [Databricks documentation](https://docs.databricks.com/)
* [Unity Catalog](https://docs.databricks.com/en/data-governance/unity-catalog/index.html)
* [Microsoft Fabric integration](microsoft-fabric.md)
