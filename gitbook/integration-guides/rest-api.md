# ➡ Rest API

**Open AI**

For OpenAI, change the API basepath and add the `x-portkey-api-key` & `x-portkey-mode` headers. Mode value for OpenAI should **`proxy openai`**

Replace `<PORTKEY API KEY>` & `<OPENAI API KEY>` with actual values from the following snippet.

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location 'http://api.portkey.ai/v1/proxy/completions' \
--header 'x-portkey-api-key: <PORTKEY API KEY>' \
--header 'x-portkey-mode: proxy openai' \
--header 'Authorization: Bearer <OPENAI API KEY>' \
--header 'Content-Type: application/json' \
--data '{
    "n": 1,
    "model": "text-davinci-003",
    "prompt": "Top 20 tallest buildings in the world"
}'
```
{% endtab %}
{% endtabs %}

**Azure Open AI**

For Azure OpenAI, change the basepath by prefixing the Portkey's base URL followed by the Azure deployment endpoint.&#x20;

For example, a typical azure endpoint looks like this:\
`https://YOUR_RESOURCE_NAME.openai.azure.com/openai/deployments/YOUR_DEPLOYMENT_NAME/completions?api-version=202305-15`\
\
After adding the prefix it should change to:\
`https://portkey.ai/v1/proxy/YOUR_RESOURCE_NAME.openai.azure.com/openai/deployments/YOUR_DEPLOYMENT_NAME/completions?api-version=2023-05-15`\


Add the  `x-portkey-api-key` & `x-portkey-mode` headers. Mode value for Azure OpenAI should **`proxy azure-openai`**

Replace `<PORTKEY API KEY>`,`<AZURE OPEN AI KEY>`, `<YOUR_RESOURCE_NAME>`,  `<YOUR_DEPLOYMENT_NAME>`  with actual values from the following snippet.

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location 'https://api.portkey.ai/v1/proxy/<YOUR_RESOURCE_NAME>.openai.azure.com/openai/deployments/<YOUR_DEPLOYMENT_NAME>/completions?api-version=2023-05-15' \
--header 'api-key: <AZURE OPEN AI KEY>' \
--header 'x-portkey-api-key: <PORTKEY API KEY>' \
--header 'x-portkey-mode: proxy azure-openai' \
--header 'Content-Type: application/json' \
--data '{        
    "prompt":"Once upon a time"
}'
```
{% endtab %}
{% endtabs %}

**Cohere**

For Cohere, change the API basepath and add the `x-portkey-api-key` & `x-portkey-mode` headers. Mode value for OpenAI should **`proxy cohere`**

Replace `<PORTKEY API KEY>` & `<COHERE API KEY>` with actual values from the following snippet.

{% tabs %}
{% tab title="cURL" %}
````bash
```powershell
curl --location 'http://portkey.ai/v1/proxy/generate' \
--header 'x-portkey-api-key: <PORTKEY API KEY>' \
--header 'Authorization: Bearer <COHERE API KEY>' \
--header 'x-portkey-mode: proxy cohere' \
--header 'Content-Type: application/json' \
--header 'x-portkey-cache: true' \
--data '{
  "max_tokens": 10,
  "return_likelihoods": "NONE",
  "truncate": "END",
  "prompt": "Once upon a time in a magical land called"
}'
```
````
{% endtab %}
{% endtabs %}

**Anthropic**

For Anthropic, change the API basepath and add the `x-portkey-api-key` & `x-portkey-mode` headers. Mode value for OpenAI should **`proxy anthropic`**

Replace `<PORTKEY API KEY>` & `<ANTHROPIC API KEY>` with actual values from the following snippet.

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location 'https://api.portkey.ai/v1/proxy/complete' \
--header 'x-portkey-api-key: <POTKEY API KEY>' \
--header 'x-api-key: <COHERE API KEY>' \
--header 'x-portkey-mode: proxy anthropic' \
--header 'Content-Type: application/json' \
--header 'x-portkey-cache: true' \
--data '{
    "prompt": "\n\nHuman: Tell me a haiku about trees\n\nAssistant: ",
    "model": "claude-v1",
    "max_tokens_to_sample": 300,
    "stop_sequences": [
        "\n\nHuman:"
    ]
}'
```
{% endtab %}
{% endtabs %}
