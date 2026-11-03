---
description: "Developer SDK vs Enterprise SDK, plus common questions."
icon: layer-group
---

# Editions and FAQ

{% columns %}
{% column %}
### Developer SDK

**Free.** Use it on your own terms.

* Statistical profiling
* AI rule discovery
* Bring your own model
* Export to JSON, YAML, GX, DQX
{% endcolumn %}

{% column %}
### Enterprise SDK

**Contact sales.** Everything in Developer, plus:

* Trend monitoring over time
* Domain-specific SLMs
* RAG over your documents
* Many more enterprise features
{% endcolumn %}
{% endcolumns %}

<a href="https://firsteigen.com/contact-us/" class="button primary" data-icon="envelope">Contact sales</a>

## More from FirstEigen

{% embed url="https://www.youtube.com/watch?v=IolP3ZzjCD0" %}
Customer story: automated data validation with DataBuck
{% endembed %}

## FAQ

<details>
<summary>Does DataBuck store my data?</summary>

No. The SDK runs in your environment and doesn't keep any statistics from your data.
</details>

<details>
<summary>Does the SDK track trends over time?</summary>

No, because it keeps no statistics. Trend monitoring is in the Enterprise SDK.
</details>

<details>
<summary>Where can I run it?</summary>

In [Databricks](../integrations/databricks.md), in [Microsoft Fabric](../integrations/microsoft-fabric.md), or in any Python environment.
</details>

<details>
<summary>Can I use my own LLM?</summary>

Yes. See [Configuring AI](../guides/configuring-ai.md).
</details>
