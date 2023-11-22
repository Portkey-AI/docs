# 🧠 Simple & Semantic Cache

Respond to previously served customers queries from Portkey cache instead of sending them again to your AI provider. **Match exact strings** OR **Semantically similar strings** and serve requests upto **20x times faster** and **cheaper**.

## **🔑 Add to Request Header**

To enable Portkey cache, just add the below params to your API request header, and don't forget to change base URL or set API path to Portkey proxy.

If you are deploying your prompts through Portkey directly (which we recommend), you can add cache headers to those API requests as well.

### **Semantic Cache**

```sh
"x-portkey-cache": "semantic"
```

This advanced caching mechanism considers the contextual similarity between input prompts. It uses cosine similarity to ascertain if the similarity between the input and a cached request exceeds a specific threshold. If the similarity threshold is met, Portkey retrieves the response from the cache, saving model execution time. Check out this [blog](https://portkey.ai/blog/reducing-llm-costs-and-latency-semantic-cache/) for more details.

### **Simple Cache**

```sh
"x-portkey-cache": "simple"
```

Simple cache performs an exact match on the input prompts. If the exact same request is received again, Portkey retrieves the response directly from the cache, bypassing the model execution. This approach is straightforward and effective for repeated identical requests.

## **⚙️ Sub Features**

### **Force Refresh**

```sh
"x-portkey-cache-force-refresh": "True"
```

If you need to force a cache refresh, use the `x-portkey-cache-force-refresh` header. Setting this to `true` ensures the cache is invalidated, and a new value is stored in the cache. If you do not include this flag, the default value is always false.

### **Cache Age**

```sh
"Cache-Control": "max-age:1000"
```

In this flag you can specify the maximmum age of storing the particular cache response, **in seconds**.

If the `Cache-Control` header is not provided, Portkey will automatically cache requests for 72460\*60 seconds, i.e. 7 days. Minimum `max-age` allowed is 60 seconds.

## **💡 Examples**

### **Implementing Semantic Cache in Python**

#### **Portkey Header: Remains same across all providers**

```py
openai.api_base = "https://api.portkey.ai/v1/proxy"

portkey_header = {
    'x-portkey-api-key' :  'PORTKEY_API_KEY',
    'x-portkey-mode' : 'proxy openai',
    'x-portkey-cache' : 'semantic',
    'x-portkey-cache-force-refresh': 'True', #Refresh previously stored value
    'Cache-Control': 'max-age:2592000'
}
```

#### **Provider: OpenAI, Model: GPT3**

```py
response = openai.Completion.create( 
    model="text-davinci-003", 
    prompt="What colour do we get if we mix yellow & blue?",
    headers = portkey_headers, #This is where we pass our Portkey headers
    max_tokens=1024
    )

print (response['choices'][0]['text'])
```

## **🖥️ Portkey Dashboard Guide**

Simple cache hits will show up as **"HIT"** & semantic cache hits will show up as **"SEMANTIC HIT"** on your Portkey logs. We also calculate and show the response time and how much money you saved with each hit.

<figure><img src="../../.gitbook/assets/Cache 1 (3).png" alt=""><figcaption><p>See cache status directly on your Portkey logs</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/Cache 2.png" alt=""><figcaption><p>Each log also shows the response time for cache hit</p></figcaption></figure>

