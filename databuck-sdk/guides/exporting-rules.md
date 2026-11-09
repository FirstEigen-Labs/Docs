# Exporting rules

`DataBuck.discover(...)` writes the rules it finds to the output path you give it. The file suffix sets the format.

```python
output_path = DataBuck.discover(df, "rules.json", context=context)
print(output_path)
```

## Formats

| Suffix | Format | Best for |
| --- | --- | --- |
| `.json` | Rules grouped by action: `warn`, `drop`, `fail` | Pipelines that should drop bad rows or stop on failures |
| `.yaml` / `.yml` | A flat list of expectations | Simple monitoring, or tools that read YAML |
| `.gx.json` | A Great Expectations 1.x suite | Teams already using Great Expectations |

## JSON

Each group maps a rule name to a Spark SQL condition that valid rows pass:

```json
{
  "warn": {"not_null_customer_email": "`customer_email` IS NOT NULL"},
  "drop": {"not_null_order_id": "`order_id` IS NOT NULL"},
  "fail": {"valid_pattern_currency_code": "`currency_code` IS NULL OR CAST(`currency_code` AS STRING) RLIKE '(?:^[A-Z][A-Z][A-Z]$)'"}
}
```

| Group | What happens to rows that fail |
| --- | --- |
| `warn` | The failure is recorded; the row is kept |
| `drop` | The row is removed from the validated output |
| `fail` | The pipeline run stops |

Without AI context, all rules go into `warn`.

### Dataset-level checks

If DataBuck finds a rule that applies to the whole dataset, such as uniqueness, the JSON includes a `dataset_checks` section. These are not row-level rules: run their SQL separately against the full dataset, replacing `{{DATAFRAME}}` with your table or view.

## YAML

YAML lists all rules under `expectations`, without action groups:

```yaml
table: my_database.customers

expectations:
  not_null_subscriber_id: "`subscriber_id` IS NOT NULL"
  BuckGPT_Rule_001: "NOT COALESCE((subscription_status_cd = 'INVALID'), FALSE)"
```

The `table` field appears only when you pass a table name to `discover`, not a DataFrame. Use JSON if your rules need to drop rows or stop a pipeline.

## Great Expectations

Install Great Expectations separately, then restart Python:

```bash
pip install "great-expectations>=1.0,<2"
```

After the restart, set the license and JAR paths again, import `databuck`, and use a `.gx.json` path:

```python
gx_path = DataBuck.discover(df, "rules.gx.json", context=context)
```

The suite includes dataset-level checks, and each rule keeps its DataBuck action (`warn`, `drop` or `fail`) in its metadata. With `domain_name` set, it contains only HIGH and MEDIUM priority rules.

{% hint style="info" %}
Great Expectations reports failures but does not drop rows or stop a pipeline.
{% endhint %}

## Check the output

1. Confirm the file exists at the returned `output_path`.
2. Review the generated rules against your business policy before using them in production.

Platform guides: [Databricks](../integrations/databricks.md) (including running the rules in a Lakeflow pipeline) and [Microsoft Fabric](../integrations/microsoft-fabric.md).
