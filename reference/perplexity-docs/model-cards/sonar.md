[Perplexity home page![light logo](https://mintlify.s3.us-west-1.amazonaws.com/perplexity/logo/SonarByPerplexity.svg)![dark logo](https://mintlify.s3.us-west-1.amazonaws.com/perplexity/logo/Sonar_Wordmark_Light.svg)](https://docs.perplexity.ai/home.mdx)

Search docs

Ctrl K

Search...

Navigation

Sonar

[Home](https://docs.perplexity.ai/home) [Guides](https://docs.perplexity.ai/guides/getting-started) [API Reference](https://docs.perplexity.ai/api-reference/chat-completions) [Changelog](https://docs.perplexity.ai/changelog/changelog) [System Status](https://docs.perplexity.ai/system-status/system-status) [FAQ](https://docs.perplexity.ai/faq/faq) [Roadmap](https://docs.perplexity.ai/feature-roadmap) [Discussions](https://docs.perplexity.ai/discussions/discussions)

_A lightweight, cost-effective search model optimized for quick, grounded answers with real-time web search._

- **Model Type:** Non-reasoning
- **Use Case:** Ideal for quick searches and straightforward Q&A tasks.
- **Context Length**: 128k

**Key Features:**

- Real-time web search-based answers with citations
- Optimized for speed and cost

**Real-World Examples:**

- Summarizing books, TV shows, and movies
- Looking up definitions or quick facts
- Browsing news, sports, health, and finance content

* * *

## [​](https://docs.perplexity.ai/guides/models/sonar\#pricing)  **Pricing**

## Legacy Pricing (Active Until 04/18/2025)

**Legacy Pricing (Default if no search mode is specified)**

**Pricing:**

| Pricing Component | Cost |
| --- | --- |
| **Input Tokens (Per Million)** | **$1** |
| **Output Tokens (Per Million)** | **$1** |
| **Price per 1,000 Requests** | **$5** |

## New Pricing (Available Now)

**New Pricing with Search Modes**

_Explicit API call with `search_context_size` required to use new pricing._

| Pricing Component | High | Medium | Low |
| --- | --- | --- | --- |
| **Input Tokens (Per Million)** | **$1** | **$1** | **$1** |
| **Output Tokens (Per Million)** | **$1** | **$1** | **$1** |
| **Price per 1,000 Requests** | **$12** | **$8** | **$5** |

Detailed Pricing Breakdown for Sonar

**Input Tokens**

1. Input tokens are priced at $1/1M tokens

2. Input tokens comprise of Prompt tokens (user prompt)


**Search Queries**

1. Each request does 1 search priced at $5/1000 searches

**Output Tokens**

1. Output tokens (Completion tokens) are priced at $1/1M tokens

**Total Price**

Your total price per request finally is a sum of the above 2 components

* * *

## [​](https://docs.perplexity.ai/guides/models/sonar\#test-the-model)  **Test the Model**

cURL

python

Copy

```bash
curl --request POST \
  --url https://api.perplexity.ai/chat/completions \
  --header 'Authorization: Bearer <token>' \
  --header 'Content-Type: application/json' \
  --data '{
  "model": "sonar",
  "messages": [\
    {"role": "user", "content": "What is the latest news in AI research?"}\
  ],
  "max_tokens": 200
}'

```

**Sample Response Metadata**

Copy

```
{
  'id': 'ad748e84-a4bb-4665-afc9-40aac3b39e3c',
  'model': 'sonar',
  'created': 1743529296,
  'usage': {'prompt_tokens': 9, 'completion_tokens': 384, 'total_tokens': 393}, 'citations': ['https://news.mit.edu/topic/artificial-intelligence2', 'https://www.sciencedaily.com/news/computers_math/artificial_intelligence/', 'https://aiweekly.co', 'https://techcrunch.com/category/artificial-intelligence/', 'https://www.artificialintelligence-news.com'], 'object': 'chat.completion', 'choices': [{'index': 0, 'finish_reason': 'stop', 'message': {'role': 'assistant',\
  'content': "Here are some of the latest developments in AI research:\n\n## Recent AI Research Developments\n\n### 1. **Improved AI Image Generation**\n- **MIT Research**: An AI tool has been developed that combines two popular methods to generate high-quality images faster and more efficiently than current state-of-the-art approaches. This tool can run locally on devices like laptops and smartphones, using less energy[1].\n\n### 2. **Enhanced Autonomous Learning**\n- **Torque Clustering Algorithm**: Researchers have introduced a new AI algorithm called Torque Clustering, which significantly enhances autonomous learning and pattern discovery in AI systems, requiring minimal human intervention[2].\n\n### 3. **Cyborg Insects Navigation**\n- **Autonomous Navigation**: A research team has developed two new autonomous navigation systems for cyborg insects, enabling them to better navigate through unknown environments using simple circuits[2].\n\n### 4. **AI in Smart Home Security**\n- **AIoT Framework**: Scientists have developed a new AIoT framework, MSF-Net, to enhance smart home security by integrating AI with IoT technology and WiFi, providing robust security solutions[2].\n\n### 5. **Advancements in Large Language Models**\n- **Reasoning Capabilities**: Studies have shown that large language models reason about diverse data types in a general way, similar to human brains, indicating advanced capabilities in handling complex information[2].\n\n### 6. **Regulatory Frameworks for AI**\n- **European AI Act**: The European Union is working on regulatory models like the AI Act to ensure ethical and safe AI use, particularly in data centers, addressing environmental impacts[3].\n\n### 7. **OpenAI Leadership Changes**\n- **Executive Shifts**: OpenAI has reshuffled its leadership, with CEO Sam Altman focusing more on technical aspects while expanding COO Brad Lightcap's responsibilities, signaling strategic adjustments in the company's direction[3]."},\
  'delta': {'role': 'assistant', 'content': ''}}]
}

```