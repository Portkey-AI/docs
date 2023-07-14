# ➡ Cohere

For Cohere, Portkey provides complete integration with all of Cohere's endpoints using HTTP-based requests. For Python, you can use the popular Requests library, and in Node.js, Axios is a common choice.

{% tabs %}
{% tab title="Python" %}
```python
import requests

url = "https://api.portkey.ai/v1/proxy/generate"

payload = { "prompt": "Tell me a joke." }
headers = {
    "accept": "application/json",
    "content-type": "application/json",
    "authorization": "Bearer g6nKXKAh07io48RPqFZNicCj6oQQ7kl9KN8PqdcA",
    "x-portkey-api-key" : "<PORTKEY_API_KEY>",
    "x-portkey-cache" : "semantic",
    "x-portkey-mode" : "proxy cohere",
}

response = requests.post(url, json=payload, headers=headers)

print(response.text)
```
{% endtab %}

{% tab title="NodeJS" %}
```typescript
import axios, { AxiosRequestConfig, AxiosResponse, AxiosError } from 'axios';

const options: AxiosRequestConfig = {
  method: 'POST',
  url: 'https://api.portkey.ai/v1/proxy/generate',
  headers: {
    'accept': 'application/json',
    'content-type': 'application/json',
    'authorization': 'Bearer <COHERE_API_KEY>',
    "x-portkey-api-key": "<PORTKEY_API_KEY>",
    "x-portkey-mode": "proxy cohere",
    "x-portkey-trace-id": "demo_cohere"
  },
  data: {prompt: 'O Captain! my Captain! our fearful trip is done'}
};

axios
  .request(options)
  .then((response: AxiosResponse) => {
    console.log(response.data);
  })
  .catch((error: AxiosError) => {
    console.error(error);
  });

```
{% endtab %}
{% endtabs %}

{% hint style="danger" %}
**Note - Cohere SDK Integration**

At present, Portkey's integration is not directly compatible with Cohere's official SDKs due to their current lack of support for custom headers. However, we're actively working on this and will provide SDK compatibility as soon as possible.
{% endhint %}

