# Tracing Requests through Portkey

Portkey supports tracing that allows you to monitor your generative AI apps end-to-end. To enable tracing, you can pass a `trace-id` in the header of any Portkey request (proxy or otherwise) and it will be tracked through the journey of the request.

```sh
# HEADER:
x-portkey-trace-id: "enter_your_trace_id_here"
```

### Adding a Trace ID to any proxy request
Pass `x-portkey-trace-id` in the request header for Portkey to link requests.

For Python
```python
response = openai.ChatCompletion.create(
  model="text-davinci-003",
  prompt="Create a 5 day trip plan for Laos."
  temperature=0.2,
  headers={
    "x-portkey-api-key": "<YOUR PORTKEY API KEY>",
    "x-portkey-mode": "proxy openai",
    "x-portkey-trace-id": "<YOUR TRACE ID>"
  }
)
```

For Javascript
```javascript
const configuration = new Configuration({
    organization: "YOUR_ORG_ID",
    apiKey: process.env.OPENAI_API_KEY,
    basePath: "https://api.portkey.ai/v1/proxy",
    baseOptions: {
      headers: {
        "x-portkey-api-key": "<YOUR PORTKEY API KEY>",
        "x-portkey-mode": "proxy openai"
        "x-portkey-trace-id": "<YOUR TRACE ID>" // Add your trace-id here for Portkey to track it
      }
    }
});
```

### Using Tracing with Langchain Agents
A common use case of tracing is to capture the flow of a langchain agent request. When using Portkey with langchain, you can pass your Trace ID in the headers and all the embeddings & completion requests will get tagged with the same Trace ID.

##### Sample Langchain Agent Code with Tracing Enabled
```python
from langchain.agents import load_tools
from langchain.agents import initialize_agent
from langchain.agents import AgentType
from langchain.llms import OpenAI
import openai

openai.api_base = "https://api.portkey.ai/v1/proxy"

llm = OpenAI(temperature=0, headers={
    "x-portkey-api-key": "<PORTKEY API KEY>",
    "x-portkey-mode": "proxy openai",
    "x-portkey-trace-id": "e4bbb7c0f6a2ff07"
}, user="testing_traces")

tools = load_tools(["llm-math"], llm=llm)

agent = initialize_agent(tools, llm, agent=AgentType.ZERO_SHOT_REACT_DESCRIPTION, verbose=True)

agent.run("What is 56 multiplied by 17654 to the 0.43 power?")
```

When we run this, we get view following logs on Portkey

![Trace Logs in Portkey](/images/trace-logs.gif)

### Managing user feedback via Trace IDs
Trace IDs can also be used to link user feedback to generations. You can implement a thumbs up / thumbs down user feedback system or something more complex with our feedback APIs. This feedback can be linked to traces which can span over 1 generation or multiple ones.

You can read about implementing user feedback in Portkey [here](https://github.com/Portkey-AI/quick-start/blob/main/feedback.md).
