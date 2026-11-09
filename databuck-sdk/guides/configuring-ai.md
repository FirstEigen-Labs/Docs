# Configuring AI

DataBuck SDK always profiles your data with statistics. Add AI, and BuckGPT also generates business rules from your domain and policy documents, then audits them so only the useful ones are kept.

## Supported models

| Provider | Status |
| --- | --- |
| Google Gemini | Supported |
| More providers | Coming soon |

## Turn on AI rules

Pass a `context` dictionary to `DataBuck.discover(...)`:

```python
import os
from databuck import DataBuck

context = {
    "gemini_api_key": os.environ["GEMINI_API_KEY"],
    "domain_name": "banking",
    "pdf_paths": [
        "/path/to/customer_policy.pdf",
        "/path/to/data_contract.docx",
    ],
}

output_path = DataBuck.discover(df, "rules.json", context=context)
```

{% hint style="danger" %}
Never hardcode the API key. Read it from an environment variable or your platform's secret store.
{% endhint %}

## Context options

| Key | What it does |
| --- | --- |
| `gemini_api_key` | Your Gemini API key, used for the AI calls |
| `domain_name` | Your business domain, such as `banking`. Output keeps only HIGH and MEDIUM priority rules |
| `pdf_paths` | PDF and DOCX policy, regulation or data contract files that ground the rules |

## What you get

| Setup | Rules in the output |
| --- | --- |
| No `context` | Automatic profiling rules only, all in the `warn` group |
| With `context` | Profiling rules plus AI-generated business rules. JSON output groups them as `warn`, `drop` and `fail` |

## Tips

* Give the documents your data must follow: policies, regulations and data contracts.
* Set `domain_name` to cut noise, so only HIGH and MEDIUM priority rules are kept.
* Review the generated rules against your business policy before using them in production.

Next: [Exporting rules](exporting-rules.md).
