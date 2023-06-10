# Automatic Retries with Portkey

Portkey has an in-built retry mechanism that makes sure your requests get fulfilled. We implement a retry mechanism with exponential backoff. This can be managed by the `x-portkey-retry-count` header.

```sh
# HEADER:
x-portkey-retry-count: "number of times you want the API to be retried (max 5)"
```

### Adding Retries to any proxy request
Pass `x-portkey-retry-count` in the request header for Portkey to start retrying requests.

For Python
```python
response = openai.ChatCompletion.create(
  model="text-davinci-003",
  prompt="Create a 5 day trip plan for Laos."
  temperature=0.2,
  headers={
    "x-portkey-api-key": "<YOUR PORTKEY API KEY>",
    "x-portkey-mode": "proxy openai",
    "x-portkey-retry-count": "3"
  }
)
```

For Javascript
```js
const configuration = new Configuration({
    organization: "YOUR_ORG_ID",
    apiKey: process.env.OPENAI_API_KEY,
    basePath: "https://api.portkey.ai/v1/proxy",
    baseOptions: {
      headers: {
        "x-portkey-api-key": "<YOUR PORTKEY API KEY>",
        "x-portkey-mode": "proxy openai"
        "x-portkey-retry-count": "3" // Add the number of retries you want Portkey to make
      }
    }
});
```

### Using Tracing with Langchain Agents
A common use case of tracing is to capture the flow of a langchain agent request. When using Portkey with langchain, you can pass your Trace ID in the headers and all the embeddings & completion requests will get tagged with the same Trace ID.

Sample Langchain Agent Code with Tracing Enabled
```python
rom langchain.agents import load_tools
from langchain.agents import initialize_agent
from langchain.agents import AgentType
from langchain.llms import OpenAI
import openai

openai.api_base = "https://api.portkey.ai/v1/proxy"

llm = OpenAI(temperature=0, headers={
    "x-portkey-api-key": "<PORTKEY API KEY>",
    "x-portkey-mode": "proxy openai",
    "x-portkey-retry-count": "3"
}, user="testing_traces")

tools = load_tools(["llm-math"], llm=llm)

agent = initialize_agent(tools, llm, agent=AgentType.ZERO_SHOT_REACT_DESCRIPTION, verbose=True)

agent.run("What is 56 multiplied by 17654 to the 0.43 power?")
```

> NOTE: The max value of retries available is 5

