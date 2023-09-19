# 📎 Portkey Headers

Portkey uses request headers to enable, disable, or configure various features at the request level. These headers give you granular control over your interaction with Portkey's API, allowing you to tailor the behavior of specific requests to fit your needs.

All Portkey-specific headers begin with the prefix `x-portkey-`.

### Headers Overview

Portkey headers are part of the HTTP request sent to Portkey's API. They are used to pass additional information with the HTTP request or response. Portkey-specific headers provide directives to control various Portkey features and are prefixed with `x-portkey-`.

Here's an example of how headers can be used to configure a feature - enabling automatic retries:

```python
codeheaders = {
    "x-portkey-api-key": "<YOUR_PORTKEY_API_KEY>",
    "x-portkey-retry-count" : "4"
}

response = requests.post('https://api.portkey.ai/v1/models', headers=headers, data=payload)
```

In this example, if the request fails due to an LLM error, Portkey will automatically retry the request up to four times before returning an error.

### Mandatory Headers

The only mandatory header is `x-portkey-api-key`. Without this header all requests would return with 401 error.

If you're using Middleware mode, `x-portkey-mode` header with the appropriate value is required. To understand the value of mode, please refer to the integration guides.

### Further Reading

Each feature documentation provides specific details on the headers associated with the feature and how to use them to configure the behavior of the feature.

Remember, incorrect usage of headers could lead to unexpected behavior, so it's crucial to understand their purpose and usage before implementation. Feel free to consult the documentation or contact support if you have any questions.
