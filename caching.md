# 🧠 Simple & Semantic Cache

Respond to previously served customers queries from Portkey cache instead of sending them again to your AI provider. Match exact strings OR semantically similar strings and serve requests upto 20x times faster and cheaper.


## **🔑 Add to Request Header**
To enable Portkey cache, just add the below params to your API request header, and don't forget to change base URL or set API path to Portkey proxy. (Check here for details). 

If you are deploying your prompts through Portkey directly (which we recommend), you can add cache headers to those API requests as well.

### **Semantic Cache**

```sh
"x-portkey-cache": "semantic"
```

Semantic cache tries to semantically match an incoming new request to all the requests that are previously cached. If you want to see how it works, check out this blog.

### **Simple Cache**
```sh
"x-portkey-cache": "simple"
```
Simple cache matches queries string to string and returns a previously cached answer. It carries no risks compared to semantic where there is a miniscule chance that the cached response might be ireelevant to the user query.

## **⚙️ Sub Features**

### **Force Refresh**
```sh
"x-portkey-cache-force-refresh": "True"
```
When this flag is passed, the cache value is force refreshed to a new one, even if an old one exists. If you do not include this flag, the default value is always false.

### **Cache Age**
```sh
"Cache-Control": "max-age:1000"
```
In this flag you can specify the age of storing the particular cache response. It takes values in seconds. If the Cache-Control header is not provided, Portkey will automatically cache requests for 72460*60 seconds, i.e. 7 days.

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

![Cache 1](./images/Cache%201.png)
![Cache 2](./images/Cache%202.png)