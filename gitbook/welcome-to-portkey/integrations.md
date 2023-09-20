# 🌈 Integrations

### Supported LLM Providers

| Provider                   |
| -------------------------- |
| OpenAI                     |
| Azure OpenAI               |
| Anthropic                  |
| Cohere                     |
| Hugging Face (invite-only) |

### Detailed Overview of LLM Functions by Provider

The table below provides a more detailed look at which functions (generate, embed, classify, etc.) are supported by each LLM provider against each Portkey mode (middleware or managed models).

<table><thead><tr><th width="193">Provider</th><th>Middleware Mode</th><th>Managed Models Mode</th></tr></thead><tbody><tr><td>OpenAI</td><td>Chat, Completions, Embed, Images, Embeddings, Audio, Files, Fine-tunes, Moderations, Edits</td><td>Chat, Completions</td></tr><tr><td>Azure OpenAI</td><td>Chat, Completions, Embed, Images, Embeddings, Audio, Files, Fine-tunes, Moderations, Edits</td><td>(coming soon)</td></tr><tr><td>Anthropic</td><td>Completions</td><td>Completions</td></tr><tr><td>Cohere</td><td>Generate, Embed, Classify, Tokenize, Detokenize, Detect-language, Summarise, Rerank</td><td>(coming soon)</td></tr><tr><td>Hugging Face</td><td>Inference</td><td>(coming soon)</td></tr></tbody></table>



{% hint style="info" %}
**Please Note**: This table will be regularly updated as we continue to expand our support for various LLM providers and their functions. Some of the unavailable functions might already be in beta. Do reach out our support if you need something which is missing here.
{% endhint %}

If you have any questions about LLM support or if you're interested in a provider or function that's not currently listed, please reach out to our support team. We're always looking to enhance our offerings based on your needs.
