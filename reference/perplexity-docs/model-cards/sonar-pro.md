[Perplexity home page![light logo](https://mintlify.s3.us-west-1.amazonaws.com/perplexity/logo/SonarByPerplexity.svg)![dark logo](https://mintlify.s3.us-west-1.amazonaws.com/perplexity/logo/Sonar_Wordmark_Light.svg)](https://docs.perplexity.ai/home.mdx)

Search docs

Ctrl K

Search...

Navigation

Sonar Pro

[Home](https://docs.perplexity.ai/home) [Guides](https://docs.perplexity.ai/guides/getting-started) [API Reference](https://docs.perplexity.ai/api-reference/chat-completions) [Changelog](https://docs.perplexity.ai/changelog/changelog) [System Status](https://docs.perplexity.ai/system-status/system-status) [FAQ](https://docs.perplexity.ai/faq/faq) [Roadmap](https://docs.perplexity.ai/feature-roadmap) [Discussions](https://docs.perplexity.ai/discussions/discussions)

_An advanced search model designed for complex queries, delivering deeper content understanding with enhanced citation accuracy._

- **Model Type:** Non-reasoning
- **Use Case:** Suitable for multi-step Q&A tasks requiring deeper content understanding.
- **Context Length:** 200k

**Key Features:**

- In-depth answers with 2x more citations than Sonar
- Uses advanced information retrieval architecture
- Optimized for multi-step tasks

**Real-World Examples:**

- Conducting academic literature reviews
- Researching competitors and industry trends
- Generating restaurant catalogs with reviews

- `sonar-pro` has a max output token limit of 8k.

* * *

## [​](https://docs.perplexity.ai/guides/models/sonar-pro\#pricing)  **Pricing**

## Legacy Pricing (Active Until 04/18/2025)

**Legacy Pricing (Default if no search mode is specified)**

**Pricing:**

| Pricing Component | Cost |
| --- | --- |
| **Input Tokens (Per Million)** | **$3** |
| **Output Tokens (Per Million)** | **$15** |
| **Price per 1,000 Requests** | **$5** |

## New Pricing (Available Now)

**New Pricing with Search Modes**

_Explicit API call with `search_context_size` required to use new pricing._

| Metric | High | Medium | Low |
| --- | --- | --- | --- |
| **Input Tokens (Per Million)** | **$3** | **$3** | **$3** |
| **Output Tokens (Per Million)** | **$15** | **$15** | **$15** |
| **Price per 1,000 Requests** | **$14** | **$10** | **$6** |

Detailed Pricing Breakdown for Sonar Pro

**Input Tokens**

1. Input tokens are priced $3/1M tokens

2. Input tokens comprise of Prompt tokens (user prompt) + Citation tokens (these are processed tokens from running searches)


**Search Queries**

1. To give detailed answers, the API runs multiple searches on top of the user prompt where necessary for more exhaustive information retrieval

2. Searches are priced at $5/1000 searches

3. A request that does 3 searches will cost $0.015 in this step


**Output Tokens**

1. Output tokens (Completion tokens) are priced at $8/1M tokens

**Total Price**

Your total price per request finally is a sum of the above 3 components

* * *

## [​](https://docs.perplexity.ai/guides/models/sonar-pro\#test-the-model)  **Test the Model**

cURL

python

Copy

```bash
curl --request POST \
  --url https://api.perplexity.ai/chat/completions \
  --header 'Authorization: Bearer <token>' \
  --header 'Content-Type: application/json' \
  --data '{
  "model": "sonar-pro",
  "messages": [\
    {"role": "user", "content": "Summarize the latest research papers on climate change."}\
  ],
  "max_tokens": 300
}'

```

**Sample Response**

Copy

```json
{'
  id': 'f9f96801-2e7f-4400-b0e9-f2cf8cf9ac4d',
  'model': 'sonar-pro',
  'created': 1743529048,
  'usage': {'prompt_tokens': 11, 'completion_tokens': 401, 'total_tokens': 412, 'citation_tokens': 5149, 'num_search_queries': 1},
  'citations': ['https://www.wri.org/insights/2023-ipcc-ar6-synthesis-report-climate-change-findings', 'https://www.ipcc.ch/report/ar6/wg1/', 'https://www.ipcc.ch/report/ar6/syr/downloads/report/IPCC_AR6_SYR_SPM.pdf', 'https://science.nasa.gov/climate-change/scientific-consensus/', 'https://science.nasa.gov/climate-change/effects/', 'https://10insightsclimate.science', 'https://www.noaa.gov/education/resource-collections/climate/climate-change-impacts', 'https://academic.oup.com/bioscience/article/74/12/812/7808595', 'https://www.un.org/en/climatechange/reports'],
  'object': 'chat.completion',
  'choices': [{'index': 0, 'finish_reason': 'stop', 'message': {'role': 'assistant', 'content': "Recent research on climate change has yielded several important findings:\n\n## Key Findings\n\n1. Global warming continues to accelerate. The Earth's surface temperature was 1.09°C higher in 2011-2020 compared to 1850-1900, with larger increases over land than oceans[1][3].\n\n2. Human activities are the dominant cause of observed warming since the mid-20th century. Greenhouse gas emissions from fossil fuels, land use changes, and unsustainable consumption patterns are primary drivers[1][3].\n\n3. Climate impacts are intensifying. Effects like sea ice loss, glacial melting, sea level rise, and more frequent extreme weather events are occurring faster than previously projected[5].\n\n4. Urgent action is needed. To limit warming to 1.5°C, global emissions must peak before 2025 and decline 43% by 2030 relative to 2019 levels[1].\n\n5. Systemic transformations are required across sectors including energy, buildings, industry, transport, and land use to achieve net-zero emissions[1].\n\n## Recent Developments\n\n- Summer 2023 was Earth's hottest on record, 1.2°C warmer than the 1951-1980 average[5].\n\n- The Arctic is likely to become ice-free in late summer before mid-century if current trends continue[5].\n\n- Wildfire seasons are lengthening and intensifying, especially in the Western United States[5].\n\n- Precipitation patterns are changing unevenly, with some regions experiencing increased flooding while others face more severe droughts[5].\n\n## Scientific Consensus\n\nMultiple studies show that 97% of climate scientists agree that recent climate change is primarily caused by human activities[4]. Leading scientific organizations worldwide have issued statements endorsing this position[4].\n\nThe evidence is clear that climate change poses significant risks to human wellbeing and planetary health. Rapid, concerted global action is necessary to secure a livable future and avoid the worst projected impacts[5]."}, 'delta': {'role': 'assistant', 'content': ''}}]
}

```