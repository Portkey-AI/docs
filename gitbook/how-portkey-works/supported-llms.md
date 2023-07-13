# 🌈 Supported LLMs

### Supported LLM Providers

| Provider     |
| ------------ |
| OpenAI       |
| Azure OpenAI |
| Anthropic    |
| Cohere       |
| Hugging Face |

{% hint style="info" %}
**Please note:** As of now, Hugging Face is in beta and available by invite only. If you wish to have it enabled for your organization, please reach out to Portkey support.
{% endhint %}

### Detailed Overview of LLM Functions by Provider

The table below provides a more detailed look at which functions (generate, embed, classify, etc.) are supported by each LLM provider against each Portkey mode (middleware or managed models).

| Provider     | Middleware Mode Support                                                                    | Managed Models Mode Support |
| ------------ | ------------------------------------------------------------------------------------------ | --------------------------- |
| OpenAI       | Chat, Completions, Embed, Images, Embeddings, Audio, Files, Fine-tunes, Moderations, Edits | Chat, Completions           |
| Azure OpenAI | Chat, Completions, Embed, Images, Embeddings, Audio, Files, Fine-tunes, Moderations, Edits | -                           |
| Anthropic    | Completions                                                                                | Completions                 |
| Cohere       | Generate, Embed, Classify, Tokenize, Detokenize, Detect-language, Summarise, Rerank        | -                           |
| Hugging Face | Inference                                                                                  | -                           |



{% hint style="info" %}
**Please Note**: This table will be regularly updated as we continue to expand our support for various LLM providers and their functions. Some of the unavailable functions might already be in beta. Do reach out our support if you need something which is missing here.
{% endhint %}

If you have any questions about LLM support or if you're interested in a provider or function that's not currently listed, please reach out to our support team. We're always looking to enhance our offerings based on user needs.
