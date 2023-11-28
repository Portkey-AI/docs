---
description: >-
  Portkey SDK is the best way to interact with Portkey and bring your LLMs to
  production.
---

# Node

```bash
npm install portkey-ai
```

[**Check out the Github repo for more details.**](https://github.com/Portkey-AI/portkey-node-sdk)

***

## **4-Step Guide**

### **Step 1️ : Get your Portkey API Key and your Virtual Keys for AI providers** <a href="#user-content-step-1-get-your-portkey-api-key-and-your-virtual-keys-for-ai-providers" id="user-content-step-1-get-your-portkey-api-key-and-your-virtual-keys-for-ai-providers"></a>

**Portkey API Key:** Log into [Portkey here](https://app.portkey.ai/), then click on the profile icon on top left and “Copy API Key”.

```bash
export PORTKEY_API_KEY="PORTKEY_API_KEY"
```

**Virtual Keys:** Navigate to the "Virtual Keys" page on [Portkey](https://app.portkey.ai/) and hit the "Add Key" button. Choose your AI provider and assign a unique name to your key. Your virtual key is ready!

### **Step 2️ : Construct your LLM, add Portkey features, provider features, and prompt** <a href="#user-content-step-2-construct-your-llm-add-portkey-features-provider-features-and-prompt" id="user-content-step-2-construct-your-llm-add-portkey-features-provider-features-and-prompt"></a>

**Portkey Features**: You can find a comprehensive list of Portkey features [here](node.md#user-content--full-list-of-portkey-config). This includes settings for caching, retries, metadata, and more.

**Provider Features**: Portkey is designed to be flexible. All the features you're familiar with from your LLM provider, like `top_p`, `top_k`, and `temperature`, can be used seamlessly. Check out the complete list of provider features [here](https://github.com/Portkey-AI/portkey-node-sdk/blob/7338a31668048a504a4af7889a4be8646c48c2f7/src/\_types/portkeyConstructs.ts#L34).

**Setting the Prompt Input**: This param lets you override any prompt that is passed during the completion call - set a model-specific prompt here to optimise the model performance. You can set the input in two ways. For models like Claude and GPT3, use `prompt` = `(str)`, and for models like GPT3.5 & GPT4, use `messages` = `[array]`.

### **Step 3 : Construct the Portkey Client** <a href="#user-content-steo-3-construct-the-portkey-client" id="user-content-steo-3-construct-the-portkey-client"></a>

Portkey client's config takes 3 params: `apiKey`, `mode`, `llms`.

* `apiKey`: You can set your Portkey API key here or with `bash script` as done above.
* `mode`: There are **3** modes - Single, Fallback, Loadbalance.
  * **Single** - This is the standard mode. Use it if you do not want Fallback OR Loadbalance features.
  * **Fallback** - Set this mode if you want to enable the Fallback feature.
  * **Loadbalance** - Set this mode if you want to enable the Loadbalance feature.
* `llms`: This is an array where we pass our LLMs constructed using the LLMOptions interface.

```typescript
import { Portkey } from "portkey-ai";

// Portkey Config
const portkey = new Portkey({
    mode: "single",
    llms: [{
        provider: "openai",
        virtual_key: "<>",
        model: "gpt-3.5-turbo",
        max_tokens: 2000,
        temperature: 0,
        // ** more params can be added here.
    }]
})
```

### **Step 4️ : Let's Call the Portkey Client!** <a href="#user-content-step-4-lets-call-the-portkey-client" id="user-content-step-4-lets-call-the-portkey-client"></a>

The Portkey client can do `ChatCompletions` and `Completions`.

Since our LLM is GPT4, we will use ChatCompletions:

```typescript
async function main() {
    const response = await portkey.chatCompletions.create({
        messages: [{
            "role": "user",
            "content": "Who are you ?"
        }]
    })
    console.log(response.choices[0].message)
}

main().catch((err) => {
    console.error(err);
    process.exit(1);
});
```

### You have integrated Portkey's Node SDK in just 4 steps!

***

### [**📔 Full List of Portkey Config**](https://github.com/Portkey-AI/portkey-node-sdk#-full-list-of-portkey-config) <a href="#user-content--full-list-of-portkey-config" id="user-content--full-list-of-portkey-config"></a>

| Feature                | Config Key                | Value(Type)                                                                     | Required                           |
| ---------------------- | ------------------------- | ------------------------------------------------------------------------------- | ---------------------------------- |
| Provider Name          | `provider`                | `string`                                                                        | ✅ Required                         |
| Model Name             | `model`                   | `string`                                                                        | ✅ Required                         |
| Virtual Key OR API Key | `virtual_key` or `apiKey` | `string`                                                                        | ✅ Required (can be set externally) |
| Cache Type             | `cache_status`            | `simple`, `semantic`                                                            | ❔ Optional                         |
| Force Cache Refresh    | `cache_force_refresh`     | `True`, `False` (Boolean)                                                       | ❔ Optional                         |
| Cache Age              | `cache_age`               | `integer` (in seconds)                                                          | ❔ Optional                         |
| Trace ID               | `trace_id`                | `string`                                                                        | ❔ Optional                         |
| Retries                | `retry`                   | `integer` \[0,5]                                                                | ❔ Optional                         |
| Metadata               | `metadata`                | `json object` [More info](https://docs.portkey.ai/key-features/custom-metadata) | ❔ Optional                         |
