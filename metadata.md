# Sending metadata along with your requests

You can send custom metadata along with your API requests which can be later be used for filtering and aggregations.

## Proxy Metadata

To include metadata in the proxy requests, you can add a `x-portkey-meta` header with a JSON string in it. Portkey will parse the JSON object and make it available for filtering.

For the Javascript library, here's the sample code

```javascript
import { Configuration, OpenAIApi } from "openai";
const configuration = new Configuration({
    organization: "YOUR_ORG_ID",
    apiKey: process.env.OPENAI_API_KEY,
    basePath: "https://api.portkey.ai/v1/proxy", // Replace openai with portkey's endpoint
    baseOptions: {
      headers: {
        "x-portkey-api-key": "<YOUR PORTKEY API KEY>", // Can be got from your account
        "x-portkey-mode": "proxy openai" // Tells portkey to proxy your request to openai
        "x-portkey-meta": JSON.stringify({"foo": "abc", "bar": "def"}) // Enables filtering on `foo` and `bar`
      }
    }
});
const openai = new OpenAIApi(configuration);
```

For Python
```python
import json

openai.api_base = "https://api.portkey.ai/v1/proxy" # Replace openai with portkey's endpoint

response = openai.ChatCompletion.create(
  model="gpt-3.5-turbo",
  messages=[
        {"role": "system", "content": "You are a helpful assistant"},
        {"role": "user", "content": "Create a 5 day trip plan for Laos."},
    ],
  temperature=0.2,
  headers={ # Add portkey headers for auth and proxy mode
    "x-portkey-api-key": "<YOUR PORTKEY API KEY>",
    "x-portkey-mode": "proxy openai",
    "x-portkey-meta": json.dumps({"foo": "abc", "bar": "def"}) # Enables filtering on `foo` and `bar`
  }
)
```

### 2.3 Using Langchain
Add the following code in your langchain initialisation to enable portkey.

```python
from langchain.llms import OpenAI
import openai
import json
openai.api_base = "https://api.portkey.ai/v1/proxy"

llm = OpenAI(temperature=0.2, headers={
    "x-portkey-api-key": "<YOUR PORTKEY API KEY>",
    "x-portkey-mode": "proxy openai",
    "x-portkey-meta": json.dumps({"foo": "abc", "bar": "def"}) # Enables filtering on `foo` and `bar`
})
text = "Create a 5 day trip plan for Laos."
print(llm(text))
```

## Model completion metadata

Coming soon