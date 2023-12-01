# Embeddings

## Create Embeddings

`POST /embeddings`

Generate embeddings using the selected Large Language Model (LLM).

{% swagger src="../.gitbook/assets/openapi.yaml" path="/embeddings" method="post" %}
[openapi.yaml](../.gitbook/assets/openapi.yaml)
{% endswagger %}

This endpoint allows you to generate embeddings for text inputs using a specific model. The response will be an Embedding Object consistent with [OpenAI's Embedding Object](https://platform.openai.com/docs/api-reference/embeddings/object) format.

## SDK Usage

The `embeddings.create` method in the Portkey SDK facilitates the generation of embeddings using various LLMs. This method provides a straightforward interface similar to the OpenAI API for generating embeddings.

### Method Signature

{% tabs %}
{% tab title="NodeJS" %}
```js
portkey.embeddings.create(requestParams[, configParams]);
```
{% endtab %}

{% tab title="Python" %}
```py
# with only request params
portkey.embeddings.create(requestParams);
# with request and config params
portkey.with_options(configParams).embeddings.create(requestParams);
```
{% endtab %}
{% endtabs %}

### Parameters

1. **requestParams (Object)**: Parameters for the embedding request. All [OpenAI params](https://platform.openai.com/docs/api-reference/embeddings/create) are supported. These parameters include the input text and model, and are automatically transformed by Portkey for LLMs other than OpenAI. Parameters not supported by other LLMs will be omitted.
2. **configParams (Object)**: Additional configuration options for the request. This is an optional parameter that can include custom config options for this specific request. These will override the configs set in the Portkey Client.

### Example Usage

{% tabs %}
{% tab title="NodeJS" %}
```js
import Portkey from 'portkey-ai';

// Initialize the Portkey client
const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY",  // Replace with your Portkey API key
    virtualKey: "VIRTUAL_KEY"   // Optional: For virtual key management
});

// Generate embeddings
async function getEmbeddings() {
    const embeddings = await portkey.embeddings.create({
        input: "embed this",
        model: "text-embedding-ada-002",
    });

    console.log(embeddings);
}
getEmbeddings();

// Generate embeddings with config params
async function getEmbeddingsWithConfig() {
    const embeddings = await portkey.embeddings.create({
        input: "embed this",
        model: "text-embedding-ada-002",
    }, {config: "custom-config-123"});

    console.log(embeddings);
}
getEmbeddingsWithConfig();
```
{% endtab %}

{% tab title="Python" %}
```py
from portkey import Portkey

# Initialize the Portkey client
portkey = Portkey(
    api_key="PORTKEY_API_KEY",  # Replace with your Portkey API key
    virtual_key="VIRTUAL_KEY"   # Optional: For virtual key management
)

# Generate embeddings
def get_embeddings():
    embeddings = portkey.embeddings.create(
        input='embed this',
        model='text-embedding-ada-002'
    )
    print(embeddings)

get_embeddings()

# Generate embeddings with config parameters
def get_embeddings_with_config():
    embeddings = portkey.with_options(config='custom-config-123').embeddings.create(
        input='embed this',
        model='text-embedding-ada-002'
    })
    print(embeddings)

get_embeddings_with_config()
```
{% endtab %}
{% endtabs %}

#### Response Format

The response will conform to the Embedding Object schema from the Portkey API, typically including a list of embedding vectors consistent with the format provided by OpenAI for embedding objects.

