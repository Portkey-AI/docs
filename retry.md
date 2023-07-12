# 🔁 Automatic Retries

Automatically reprocess any unsuccessful API requests **`upto 5`** times. We apply an **`exponential backoff`** strategy, which spaces out retry attempts to prevent network overload.

## **🔑 Add to Request Header**
To enable automatic retries, just add the below params to your API request header. Don't forget to change `base URL` or set `API path` to `Portkey proxy`. (Check here for details).

If you are deploying your prompts through Portkey directly (which we recommend), you can add `metadata headers` to those API requests as well.

```sh
'x-portkey-retry-count': '5',
```

## **💡 Examples**

### **Implementing Automatic Retries with `Python`**

#### Portkey Header: Remains same across all providers

```py
import json

openai.api_base = "https://api.portkey.ai/v1/proxy"

portkey_header = {
    'x-portkey-api-key' :  'PORTKEY_API_KEY',
    'x-portkey-mode' : 'proxy openai',
    'x-portkey-retry-count': '3',
}
```
#### Provider: OpenAI, Model: GPT3
```py
response = openai.Completion.create( 
    model="text-davinci-003", 
    prompt="What colour do we get if we mix yellow & blue?",
    headers = portkey_headers, #This is where we pass our Portkey headers
    max_tokens=1024
    )

print (response['choices'][0]['text'])
```

### **Implementing Automatic Retries with `Node.js`**

#### Portkey Header: Remains same across all providers

```js
const portkey_headers = {
    'x-portkey-api-key' :  'PORTKEY_API_KEY',
    'x-portkey-mode' : 'proxy openai',
    'x-portkey-retry-count': '1',
}
```

#### Provider: OpenAI, Model: GPT3

```js
const configuration = new Configuration({
    organization: "YOUR_ORG_ID",
    apiKey: process.env.OPENAI_API_KEY,
    basePath: "https://api.portkey.ai/v1/proxy",
    baseOptions: {
      headers: portkey_headers
    }
});
```



