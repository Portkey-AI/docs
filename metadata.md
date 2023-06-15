# Sending Metadata Along with Your Requests

You can send custom metadata along with your API requests in Portkey, which can be later used for auditing or filtering logs. Portkey provides four predefined keys: `_environment`, `_user`, `_organisation`, and `_prompt`. These predefined keys are indexed and allow for filtering data in Portkey analytics and logs sections. You can still pass any other metadata key you desire, but these four predefined keys will be indexed and will be available for filtering data in Portkey.

## Proxy Metadata

To include metadata in the proxy requests, you can add an `x-portkey-meta` header with a JSON string containing your metadata. Portkey will parse the JSON object and make it available for filtering.

For the JavaScript library, use the following sample code:

```javascript
import { Configuration, OpenAIApi } from "openai";
const configuration = new Configuration({
    organization: "YOUR_ORG_ID",
    apiKey: process.env.OPENAI_API_KEY,
    basePath: "https://api.portkey.ai/v1/proxy", // Replace openai with portkey's endpoint
    baseOptions: {
      headers: {
        "x-portkey-api-key": "<YOUR PORTKEY API KEY>", // Can be obtained from your account
        "x-portkey-mode": "proxy openai", // Instructs Portkey to proxy your request to OpenAI
        "x-portkey-meta": JSON.stringify({"_environment": "production", "foo": "abc", "bar": "def"}) //Enables filtering on _environment
      }
    }
});
const openai = new OpenAIApi(configuration);
```

For Python:

```python
import json

openai.api_base = "https://api.portkey.ai/v1/proxy"  # Replace openai with Portkey's endpoint

response = openai.ChatCompletion.create(
    model="gpt-3.5-turbo",
    messages=[
        {"role": "system", "content": "You are a helpful assistant"},
        {"role": "user", "content": "Create a 5-day trip plan for Laos."},
    ],
    temperature=0.2,
    headers={  # Add Portkey headers for authentication and proxy mode
        "x-portkey-api-key": "<YOUR PORTKEY API KEY>",
        "x-portkey-mode": "proxy openai",
        "x-portkey-meta": json.dumps({"_environment": "production", "foo": "abc", "bar": "def"})  # Enables filtering on _environment
    }
)
```

### Using Langchain

To enable Portkey in your Langchain initialization, add the following code:

```python
from langchain.llms import OpenAI
import openai
import json

openai.api_base = "https://api.portkey.ai/v1/proxy"

llm = OpenAI(temperature=0.2, headers={
    "x-portkey-api-key": "<YOUR PORTKEY API KEY>",
    "x-portkey-mode": "proxy openai",
    "x-portkey-meta": json.dumps({"_environment": "production", "foo": "abc", "bar": "def"})  # Enables filtering on _environment
})
text = "Create a 5-day trip plan for Laos."
print(llm(text))
```

#### NOTE : When using the **_user** predefined key, the following behavior applies:

If you pass the `user` key in the OpenAI request body, it will be automatically stored in `_user`. If both the OpenAI request body `user` key and the metadata `_user` key are passed, the metadata `_user` key will be stored.
