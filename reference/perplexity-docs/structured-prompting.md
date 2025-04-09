[Perplexity home page![light logo](https://mintlify.s3.us-west-1.amazonaws.com/perplexity/logo/SonarByPerplexity.svg)![dark logo](https://mintlify.s3.us-west-1.amazonaws.com/perplexity/logo/Sonar_Wordmark_Light.svg)](https://docs.perplexity.ai/home.mdx)

Search docs

Ctrl K

Search...

Navigation

Guides

Structured Outputs Guide

[Home](https://docs.perplexity.ai/home) [Guides](https://docs.perplexity.ai/guides/getting-started) [API Reference](https://docs.perplexity.ai/api-reference/chat-completions) [Changelog](https://docs.perplexity.ai/changelog/changelog) [System Status](https://docs.perplexity.ai/system-status/system-status) [FAQ](https://docs.perplexity.ai/faq/faq) [Roadmap](https://docs.perplexity.ai/feature-roadmap) [Discussions](https://docs.perplexity.ai/discussions/discussions)

The first request with a new JSON Schema or Regex expects to incur delay on the first token. Typically, it takes 10 to 30 seconds to prepare the new schema, and may result in timeout errors. Once the schema has been prepared, the subsequent requests will not see such delay.

## [​](https://docs.perplexity.ai/guides/structured-outputs\#overview)  Overview

We currently support two types of structured outputs: **JSON Schema** and **Regex**. LLM responses will work to match the specified format, except for the following cases:

- The output exceeds `max_tokens`

Enabling the structured outputs can be done by adding a `response_format` field in the request:

**JSON Schema**

- `response_format: { type: "json_schema", json_schema: {"schema": object} }` .

- The schema should be a valid JSON schema object.


**Regex** (only available for `sonar` right now)

- `response_format: { type: "regex", regex: {"regex": str} }` .

- The regex is a regular expression string.


We recommend to give the LLM some hints about the output format in the prompts.

## [​](https://docs.perplexity.ai/guides/structured-outputs\#examples)  Examples

### [​](https://docs.perplexity.ai/guides/structured-outputs\#1-get-a-response-in-json-format)  1\. Get a response in JSON format

**Request**

Copy

```python
import requests
from pydantic import BaseModel

class AnswerFormat(BaseModel):
    first_name: str
    last_name: str
    year_of_birth: int
    num_seasons_in_nba: int

url = "https://api.perplexity.ai/chat/completions"
headers = {"Authorization": "Bearer YOUR_API_KEY"}
payload = {
    "model": "sonar",
    "messages": [\
        {"role": "system", "content": "Be precise and concise."},\
        {"role": "user", "content": (\
            "Tell me about Michael Jordan. "\
            "Please output a JSON object containing the following fields: "\
            "first_name, last_name, year_of_birth, num_seasons_in_nba. "\
        )},\
    ],
    "response_format": {
		    "type": "json_schema",
        "json_schema": {"schema": AnswerFormat.model_json_schema()},
    },
}
response = requests.post(url, headers=headers, json=payload).json()
print(response["choices"][0]["message"]["content"])

```

**Response**

Copy

```
{"first_name":"Michael","last_name":"Jordan","year_of_birth":1963,"num_seasons_in_nba":15}

```

### [​](https://docs.perplexity.ai/guides/structured-outputs\#2-use-a-regex-to-output-the-format)  2\. Use a regex to output the format

**Request**

python

Copy

```python
import requests

url = "https://api.perplexity.ai/chat/completions"
headers = {"Authorization": "Bearer YOUR_API_KEY"}
payload = {
    "model": "sonar",
    "messages": [\
        {"role": "system", "content": "Be precise and concise."},\
        {"role": "user", "content": "What is the IPv4 address of OpenDNS DNS server?"},\
    ],
    "response_format": {
		    "type": "regex",
        "regex": {"regex": r"(?:(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.){3}(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)"},
    },
}
response = requests.post(url, headers=headers, json=payload).json()
print(response["choices"][0]["message"]["content"])

```

**Response**

Copy

```
208.67.222.222

```

## [​](https://docs.perplexity.ai/guides/structured-outputs\#best-practices)  Best Practices

### [​](https://docs.perplexity.ai/guides/structured-outputs\#generating-responses-in-a-json-format)  Generating responses in a JSON Format

For Python users, we recommend using the Pydantic library to [generate JSON schema](https://docs.pydantic.dev/latest/api/base_model/#pydantic.BaseModel.model_json_schema).

**Unsupported JSON Schemas**

Recursive JSON schema is not supported. As a result of that, unconstrained objects are not supported either. Here’s a few example of unsupported schemas:

Copy

```
# UNSUPPORTED!

from typing import Any

class UnconstrainedDict(BaseModel):
   unconstrained: dict[str, Any]

class RecursiveJson(BaseModel):
   value: str
   child: list["RecursiveJson"]

```

### [​](https://docs.perplexity.ai/guides/structured-outputs\#generating-responses-using-a-regex)  Generating responses using a regex

**Supported Regex**

- Characters: `\d`, `\w`, `\s` , `.`
- Character classes: `[0-9A-Fa-f]` , `[^x]`
- Quantifiers: `*`, `?` , `+`, `{3}`, `{2,4}` , `{3,}`
- Alternation: `|`
- Group: `( ... )`
- Non-capturing group: `(?: ... )`
- Positive lookahead: `(?= ... )`
- Negative lookahead: `(?! ... )`

**Unsupported Regex**

- Contents of group: `\1`
- Anchors: `^`, `$`, `\b`
- Positive look-behind: `(?<= ... )`
- Negative look-behind: `(?<! ... )`
- Recursion: `(?R)`

## [​](https://docs.perplexity.ai/guides/structured-outputs\#structured-outputs-for-reasoning-models)  Structured Outputs for Reasoning Models

When using structured outputs with reasoning models like `sonar-reasoning-pro`, the response will include a `<think>` section containing reasoning tokens, immediately followed by the structured output. The `response_format` parameter does not remove these reasoning tokens from the output, so the final response will need to be parsed manually.

**Sample Response:**

Copy

```
<think>
I need to provide information about France in a structured JSON format with specific fields: country, capital, population, official_language.

For France:
- Country: France
- Capital: Paris
- Population: About 67 million (as of 2023)
- Official Language: French

Let me format this information as required.
</think>
{"country":"France","capital":"Paris","population":67750000,"official_language":"French"}

```

For a reusable implementation to extract JSON from reasoning model outputs, see our [example utility on GitHub](https://github.com/ppl-ai/api-discussion/blob/main/utils/extract_json_reasoning_models.py).

[Rate Limits and Usage Tiers](https://docs.perplexity.ai/guides/usage-tiers) [Search Domain Filter Guide](https://docs.perplexity.ai/guides/search-domain-filters)

On this page

- [Overview](https://docs.perplexity.ai/guides/structured-outputs#overview)
- [Examples](https://docs.perplexity.ai/guides/structured-outputs#examples)
- [1\. Get a response in JSON format](https://docs.perplexity.ai/guides/structured-outputs#1-get-a-response-in-json-format)
- [2\. Use a regex to output the format](https://docs.perplexity.ai/guides/structured-outputs#2-use-a-regex-to-output-the-format)
- [Best Practices](https://docs.perplexity.ai/guides/structured-outputs#best-practices)
- [Generating responses in a JSON Format](https://docs.perplexity.ai/guides/structured-outputs#generating-responses-in-a-json-format)
- [Generating responses using a regex](https://docs.perplexity.ai/guides/structured-outputs#generating-responses-using-a-regex)
- [Structured Outputs for Reasoning Models](https://docs.perplexity.ai/guides/structured-outputs#structured-outputs-for-reasoning-models)