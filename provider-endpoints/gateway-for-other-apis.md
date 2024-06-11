# Gateway for Other APIs

The LLM providers you're using might support other endpoints than the one's supported by Portkey. The gateway supports directly proxying the requests for them.

#### You can call the endpoint by adding it after `https://api.portkey.ai/v1`

We identify the provider and parameters needed based on the `provider` or the `virtual_key` passed in the header or in the config object.

### Example

Let's take an example of using the Cohere Rerank API (which is not directly documented here)

The cohere endpoint for this is `/rerank`

We'll create a virtual key for cohere in the UI and use that to make the call through Portkey or just use the mode as `cohere`.&#x20;

**Sample cURL request using the virtual key (recommended)**

```bash
curl --request POST \
     --url https://api.portkey.ai/v1/rerank \
     --header 'accept: application/json' \
     --header 'content-type: application/json' \
     --header 'x-portkey-api-key: $PORTKEY_API_KEY' \
     --header 'x-portkey-virtual-key: $COHERE_VIRTUAL_KEY' \
     --data '
{
  "return_documents": false,
  "max_chunks_per_doc": 10,
  "model": "rerank-english-v2.0",
  "query": "What is the capital of the United States?",
  "documents": [
    "Carson City is the capital city of the American state of Nevada.",
    "The Commonwealth of the Northern Mariana Islands is a group of islands in the Pacific Ocean. Its capital is Saipan.",
    "Washington, D.C. (also known as simply Washington or D.C., and officially as the District of Columbia) is the capital of the United States. It is a federal district.",
    "Capital punishment (the death penalty) has existed in the United States since beforethe United States was a country. As of 2017, capital punishment is legal in 30 of the 50 states."
  ]
}
'
```

**Sample cURL request using provider**

```bash
curl --request POST \
     --url https://api.portkey.ai/v1/rerank \
     --header 'accept: application/json' \
     --header 'content-type: application/json' \
     --header 'authorization: Bearer $COHERE_API_KEY' \
     --header 'x-portkey-api-key: $PORTKEY_API_KEY' \
     --header 'x-portkey-provider: cohere' \
     --data '
{
  "return_documents": false,
  "max_chunks_per_doc": 10,
  "model": "rerank-english-v2.0",
  "query": "What is the capital of the United States?",
  "documents": [
    "Carson City is the capital city of the American state of Nevada.",
    "The Commonwealth of the Northern Mariana Islands is a group of islands in the Pacific Ocean. Its capital is Saipan.",
    "Washington, D.C. (also known as simply Washington or D.C., and officially as the District of Columbia) is the capital of the United States. It is a federal district.",
    "Capital punishment (the death penalty) has existed in the United States since beforethe United States was a country. As of 2017, capital punishment is legal in 30 of the 50 states."
  ]
}
'
```

### SDK Usage

Portkey SDKs expose 2 special functions to enable method calls like this:

**`portkey.post(url, requestParams)`** and **`portkey.get(url[, requestParams])`**

Make these calls with the relevant headers and configParams as all other requests.

**Sample request for cohere rerank**

{% tabs %}
{% tab title="NodeJS" %}
<pre class="language-javascript"><code class="lang-javascript">import Portkey from 'portkey-ai';

// Initialize the Portkey client
const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY",  // Replace with your Portkey API key
    virtualKey: "VIRTUAL_KEY"   // Pass cohere's virtual key
})
// Generate a text completion
async function getRerank() {
<strong>    const rerank = await portkey.post('/rerank', {
</strong>        return_documents: false,
        max_chunks_per_doc: 10,
        model: "rerank-english-v2.0",
        query: "What is the capital of the United States?",
        documents: [
          "Carson City is the capital city of the American state of Nevada.",
          "The Commonwealth of the Northern Mariana Islands is a group of islands in the Pacific Ocean. Its capital is Saipan.",
          "Washington, D.C. (also known as simply Washington or D.C., and officially as the District of Columbia) is the capital of the United States. It is a federal district.",
          "Capital punishment (the death penalty) has existed in the United States since beforethe United States was a country. As of 2017, capital punishment is legal in 30 of the 50 states."
        ]
    });

    console.log(rerank);
}
getRerank();
</code></pre>
{% endtab %}

{% tab title="Python" %}
<pre class="language-python"><code class="lang-python">from portkey_ai import Portkey

# Initialize the Portkey client
portkey = Portkey(
    api_key="PORTKEY_API_KEY",  # Replace with your Portkey API key
    virtual_key="VIRTUAL_KEY"   # Pass cohere's virtual key
)

# Generate a text completion
def get_rerank():
<strong>    completion = portkey.post(
</strong>        '/rerank',
        return_documents=False,
        max_chunks_per_doc=10,
        model="rerank-english-v2.0",
        query="What is the capital of the United States?",
        documents=[
            "Carson City is the capital city of the American state of Nevada.",
            "The Commonwealth of the Northern Mariana Islands is a group of islands in the Pacific Ocean. Its capital is Saipan.",
            "Washington, D.C. (also known as simply Washington or D.C., and officially as the District of Columbia) is the capital of the United States. It is a federal district.",
            "Capital punishment (the death penalty) has existed in the United States since beforethe United States was a country. As of 2017, capital punishment is legal in 30 of the 50 states."
        ]
    )
    print(completion)

get_rerank()

</code></pre>
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
These response objects are **not** transformed by Portkey and are returned exactly as received from the LLM provider.
{% endhint %}
