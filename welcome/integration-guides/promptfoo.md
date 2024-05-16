# promptfoo

{% hint style="info" %}
This integration is in **beta** right now, please give us feedback and suggestions on how we could improve this.
{% endhint %}

[`promptfoo`](https://promptfoo.dev/docs/intro) is a CLI and library for evaluating LLM output quality.

Portkey AI and promptfoo are two powerful tools that, when used together, provide a comprehensive solution for managing, versioning, and evaluating AI prompts. This document provides an overview of how to leverage the strengths of both platforms to streamline your AI development workflow.

The integration between Portkey and promptfoo solves the following use cases

1. Reference prompts from Portkey inside promptfoo.
2. Use the Portkey AI gateway when promptfoo makes requests
3. View cost, performance metrics from evaluation runs in Portkey
4. Avoid promptfoo rate limits & leverage cache

Let’s see how these work

## Reference Prompts from Portkey in promptfoo

1. ​​Set the PORTKEY\_API\_KEY environment variable which is accessed by PromptFoo.
2. Use the `portkey://` prefix for your prompts, followed by the Portkey prompt ID.&#x20;

For example:

```yaml
prompts:
  - portkey://pp-test-promp-669f48

providers:
  - openai:gpt-3.5-turbo-0613

tests:
  - vars:
      topic: ...
```

Variables from your Promptfoo test cases will be automatically plugged into the Portkey prompt as variables. The resulting prompt will be rendered and returned to promptfoo, and used as the prompt for the test case.

{% hint style="warning" %}
Note that promptfoo does not follow the temperature, model, and other parameters set in Portkey. You must set them in the providers configuration yourself.
{% endhint %}

## Use the AI gateway for promptfoo requests

Portkey supports the OpenAI signature and we can leverage that to add Portkey parameters to the OpenAI provider in promptfoo. Then, we can make calls to any provider and model using Portkey configs and virtual keys.

For example, we can create a file called `portkey.yaml` which sets up the `gpt-3.5-turbo-0613` model to be used through Portkey’s virtual key.

```yaml
id: openai:gpt-3.5-turbo-0613
config:
  apiBaseUrl: https://api.portkey.ai/v1
  headers:
    {"x-portkey-api-key": "...", "x-portkey-virtual-key": "..."}
```

We could also use any other provider and model combination in the same way. Let’s create the config for Mistral hosted on Bedrock

```yaml
id: mistral-7b-v0.1
config:
  apiBaseUrl: https://api.portkey.ai/v1
  headers:
    {"x-portkey-api-key": "...", "x-portkey-virtual-key": "bedrock_virtual_key"}
```

You can then include this in your evaluation file like this

```yaml
providers:
  - file://./portkey.yaml

```

This lets you manage access to API keys a lot better, through the use of budget controlled virtual keys.

### View cost, performance metrics from evaluation runs in Portkey

We can view the logs and analytics for all these requests within Portkey.&#x20;

Metadata keys can be very helpful in maintaining segmentation in our data. This is how we can attach metadata to our promptfoo runs

```yaml
id: openai:gpt-3.5-turbo-0613
config:
  apiBaseUrl: https://api.portkey.ai/v1
  headers:
    { "x-portkey-api-key": "...", 
    "x-portkey-virtual-key": "...", 
    "x-portkey-metadata": "{\"team\": \"alpha9\"}" }
```

You can filter or group data by these metadata keys on Portkey dashboards.

<figure><img src="../../.gitbook/assets/promptfoo.gif" alt=""><figcaption></figcaption></figure>

### Avoid promptfoo rate limits & leverage cache

Since promptfoo can make a lot of calls very quickly, you can use a loadbalanced config in Portkey with cache enabled. You can pass the config header similar to virtual keys in promptfoo.

```yaml
id: openai:gpt-3.5-turbo-0613
config:
  apiBaseUrl: https://api.portkey.ai/v1
  headers:
    {"x-portkey-api-key": "...", "x-portkey-config": "..."}

```

### \[Roadmap] View the results of promptfoo evals in Portkey

We’re building support to view the eval results of promptfoo in Portkey that will let you view the results of promptfoo evals within the feedback section of Portkey.\
