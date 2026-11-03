---
description: "Use your own LLM and add context documents for better rules."
icon: robot
---

# Configuring AI

DataBuck SDK combines statistics and AI to discover rules and to filter out noisy ones. The AI is configurable, so you can run it with your own model.

## Use your own model

{% tabs %}
{% tab title="OpenAI" %}
```python
db = DataBuck(
    llm_provider="openai",
    llm_model="<MODEL_NAME>",
    api_key="<YOUR_API_KEY>",
)
```

[OpenAI API docs](https://platform.openai.com/docs)
{% endtab %}

{% tab title="Azure OpenAI" %}
```python
db = DataBuck(
    llm_provider="azure_openai",
    endpoint="https://<RESOURCE>.openai.azure.com/",
    deployment="<DEPLOYMENT_NAME>",
    api_key="<YOUR_API_KEY>",
)
```

[Azure OpenAI docs](https://learn.microsoft.com/en-us/azure/ai-services/openai/)
{% endtab %}

{% tab title="Anthropic" %}
```python
db = DataBuck(
    llm_provider="anthropic",
    llm_model="<MODEL_NAME>",
    api_key="<YOUR_API_KEY>",
)
```

[Claude API docs](https://docs.claude.com)
{% endtab %}

{% tab title="Databricks model serving" %}
```python
db = DataBuck(
    llm_provider="databricks",
    endpoint_name="<SERVING_ENDPOINT>",
)
```

[Databricks model serving docs](https://docs.databricks.com/en/machine-learning/model-serving/index.html)
{% endtab %}
{% endtabs %}

{% hint style="danger" %}
Never hard-code API keys in notebooks. Use a secret scope or environment variables.
{% endhint %}

## Add context documents

You can give the SDK business documents, such as PDFs or data dictionaries. It uses them to create rules that fit your business context, including cross-column business logic.

```python
rules = db.discover_rules(
    df,
    context_files=["docs/data_dictionary.pdf", "docs/loan_policy.pdf"],  # placeholder
)
```

<details>
<summary>What makes a good context document?</summary>

* Column definitions and allowed values
* Business rules, such as "funded amount can't exceed loan amount"
* Regulatory or policy limits
</details>

## Noise filtering

Not every pattern in the data is a useful rule. DataBuck uses statistics and AI together to drop rules that would create noise.

```python
rules = db.discover_rules(df, noise_filter="strict")  # placeholder: "off" | "balanced" | "strict"
```
