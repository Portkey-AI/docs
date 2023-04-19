# Quickstart

The best part about Portkey is that you can get started with minimal effort. Just replace your OpenAI (or any other provider’s) endpoint to the portkey proxy and we’ll get you setup within minutes!

This guide allows you to integrate with Portkey in a minute.

## 1. Create an account
You can create an account on Portkey with your work email address and get the authentication key needed later in the integration.

You can get your API key from the profile dropdown
![Portkey copy API key](Portkey-AI/quick-start/images/copy-api-key.png)

## 2. Insert the Portkey endpoint
Replace the OpenAI (or any other provider’s) endpoint to the portkey proxy endpoint and add the portkey authentication headers.

### 2.1 If you’re using the API directly
Change the API endpoint to the portkey API endpoint and add the authentication headers. You can keep everything else the same

### 2.2 Using the OpenAI SDKs for Node.js or Python
You can change the basepath of the SDK to integrate portkey.

For Javascript
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
      }
    }
});
const openai = new OpenAIApi(configuration);
```

For Python
```python
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
    "x-portkey-mode": "proxy openai"
  }
)
```

### 2.3 Using Langchain
Add the following code in your langchain initialisation to enable portkey.

```python
from langchain.llms import OpenAI
import openai
openai.api_base = "https://api.portkey.ai/v1/proxy"

llm = OpenAI(temperature=0.2, headers={
    "x-portkey-api-key": "<YOUR PORTKEY API KEY>",
    "x-portkey-mode": "proxy openai"
})
text = "Create a 5 day trip plan for Laos."
print(llm(text))
```

## 3. View real-time activity in your account
As the API requests start flowing in, you’ll see the activity in your account instantly.

## Metadata in Requests
If you'd like to send additional metadata with your requests, you can add it as part of the headers. [Read more here](https://github.com/Portkey-AI/quick-start/blob/main/metadata.md).
