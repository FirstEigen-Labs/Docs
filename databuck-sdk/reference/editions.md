---
description: "Common questions about DataBuck SDK."
icon: circle-question
---

# FAQ

## General

<details>
<summary>What is DataBuck SDK?</summary>

A free tool from FirstEigen that discovers data quality rules automatically. It profiles your data with statistics, then uses AI and your policy documents to add business rules. See [Welcome to DataBuck SDK](../README.md).
</details>

<details>
<summary>Is it free?</summary>

Yes. The Developer SDK is free to use on your own terms. An Enterprise SDK with more features, such as trend monitoring over time, domain-specific models and RAG over your documents, is also available. [Contact us](https://firsteigen.com/contact-us/) for details.
</details>

<details>
<summary>Where can I run it?</summary>

We currently support [Databricks](../integrations/databricks.md) and [Microsoft Fabric](../integrations/microsoft-fabric.md) notebooks, with more platforms on the way.
</details>

<details>
<summary>Does DataBuck store my data?</summary>

No. The SDK runs in your own environment and keeps no statistics from your data.
</details>

<details>
<summary>Does it track data quality over time?</summary>

No. Because the SDK keeps no statistics, it does not monitor trends. Trend monitoring is available in the Enterprise SDK.
</details>

## License

<details>
<summary>Do I need a license?</summary>

No action needed to start. A license valid for one year comes with the installation.
</details>

<details>
<summary>What happens when my license expires?</summary>

The license is valid through its `expires_on` date, inclusive. After that, [contact us](https://firsteigen.com/contact-us/) for a new signed `databucksdk.properties` file, and set `DATABUCK_SDK_CONFIG_FILE` to its path before importing `databuck`.
</details>

<details>
<summary>Can I edit the license file?</summary>

No. Changing the dates or customer ID invalidates the signature.
</details>

## AI

<details>
<summary>Which AI models are supported?</summary>

Google Gemini today, with more providers coming soon. See [Configuring AI](../guides/configuring-ai.md).
</details>

<details>
<summary>Do I need AI to use DataBuck?</summary>

No. Without AI, DataBuck still generates automatic profiling rules. Add AI to get business rules from your domain and policy documents.
</details>

<details>
<summary>What documents can I give it?</summary>

PDF, DOCX and Excel files, such as policies, regulations and data contracts.
</details>

<details>
<summary>How do I get fewer, more relevant rules?</summary>

Set `domain_name` in the AI context. The output then keeps only HIGH and MEDIUM priority rules.
</details>

## Rules and export

<details>
<summary>Which formats can I export to?</summary>

JSON, YAML, Great Expectations and DQX. The output file suffix sets the format. See [Exporting rules](../guides/exporting-rules.md).
</details>

<details>
<summary>What do warn, drop and fail mean?</summary>

They are the actions for rows that fail a rule. `warn` records the failure and keeps the row, `drop` removes the row, and `fail` stops the pipeline run.
</details>

<details>
<summary>Can I pass a table name instead of a DataFrame?</summary>

Yes. Pass a registered table name, and the SDK reads it with the current Spark session.
</details>

<details>
<summary>When should I regenerate rules?</summary>

Whenever the source schema or your business policy changes.
</details>

<details>
<summary>Can I use the rules in production straight away?</summary>

Review them against your business policy first.
</details>

## Troubleshooting

<details>
<summary>The JAR downloaded, but discovery fails</summary>

Downloading the JAR does not load it into Spark. Attach it to your compute or Spark session as shown in your platform guide: [Databricks](../integrations/databricks.md) or [Microsoft Fabric](../integrations/microsoft-fabric.md).
</details>

<details>
<summary>My license or JAR path is ignored</summary>

If you set `DATABUCK_SDK_CONFIG_FILE` or `DATABUCK_SDK_JAR`, set them before importing `databuck`. Setting them after the import is too late.
</details>

<details>
<summary>Everything stopped working after a restart</summary>

Restarting Python or the session clears environment variables and imports. Run the setup steps again: install, set the paths, then import `databuck`.
</details>

<details>
<summary>I get import conflicts</summary>

Uninstall the older `databuck-spark-sdk` package. It provides the same `databuck` import as `databuck-sdk`.
</details>

<details>
<summary>I'm working offline</summary>

Set `DATABUCK_SDK_AUTO_DOWNLOAD=0` to skip the download, and point `DATABUCK_SDK_JAR` to a JAR you already have.
</details>

<details>
<summary>Microsoft Fabric says "path not found"</summary>

Attach your Lakehouse to the notebook. Paths under `/lakehouse/default/Files/` only work when a Lakehouse is attached.
</details>
