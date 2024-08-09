# Vercel AI

Portkey is a control panel for your Vercel AI app. It makes your LLM integrations prod-ready, reliable, fast, and cost-efficient.

Use Portkey with your Vercel app for:

1. Calling 100+ LLMs (open & closed)
2. Logging & analysing LLM usage
3. Caching responses
4. Automating fallbacks, retries, timeouts, and load balancing
5. Managing, versioning, and deploying prompts
6. Continuously improving app with user feedback

### Guide: Create a Portkey + OpenAI Chatbot

#### 1. Create a NextJS app

Go ahead and create a Next.js application, and install `ai` and `portkey-ai` as dependencies.

```sh
pnpm dlx create-next-app my-ai-app
cd my-ai-app
pnpm install ai @ai-sdk/openai portkey-ai
```

#### 2. Add Authentication keys to `.env`

1. Login to Portkey [here](https://app.portkey.ai/)
2. To integrate OpenAI with Portkey, add your OpenAI API key to Portkey’s Virtual Keys
3. This will give you a disposable key that you can use and rotate instead of directly using the OpenAI API key
4. Grab the Virtual key & your Portkey API key and add them to `.env` file:

```sh
# ".env"
PORTKEY_API_KEY="xxxxxxxxxx"
OPENAI_VIRTUAL_KEY="xxxxxxxxxx"
```

#### 3. Create Route Handler

Create a Next.js Route Handler that utilizes the Edge Runtime to generate a chat completion. Stream back to Next.js.

For this example, create a route handler at `app/api/chat/route.ts` that calls GPT-4 and accepts a `POST` request with a messages array of strings:

```ts
// filename="app/api/chat/route.ts"
import { streamText } from 'ai';
import { createOpenAI } from '@ai-sdk/openai';
import { createHeaders, PORTKEY_GATEWAY_URL } from 'portkey-ai';

// Create a OpenAI client
const client = createOpenAI({
    baseURL: PORTKEY_GATEWAY_URL,
    apiKey: "xx",
    headers: createHeaders({
        apiKey: "PORTKEY_API_KEY",
        virtualKey: "OPENAI_VIRTUAL_KEY"
    }),
})

// Set the runtime to edge for best performance
export const runtime = 'edge';

export async function POST(req: Request) {
  const { messages } = await req.json();

  // Invoke Chat Completion
  const response = await streamText({
    model: client('gpt-3.5-turbo'),
    messages
  })

  // Respond with the stream
  return response.toTextStreamResponse();
}
```

Portkey follows the same signature as OpenAI SDK but extends it to work with **100+ LLMs**. Here, the chat completion call will be sent to the `gpt-3.5-turbo` model, and the response will be streamed to your Next.js app.

#### 4. Switch from OpenAI to Anthropic

Portkey is powered by an [open-source, universal AI Gateway](https://github.com/portkey-ai/gateway) with which you can route to 100+ LLMs using the same, known OpenAI spec.

Let’s see how you can switch from GPT-4 to Claude-3-Opus by updating 2 lines of code (without breaking anything else).

1. Add your Anthropic API key or AWS Bedrock secrets to Portkey’s Virtual Keys
2. Update the virtual key while instantiating your Portkey client
3. Update the model name while making your `/chat/completions` call
4. Add maxTokens field inside streamText invocation (Anthropic requires this field)

Let’s see it in action:

```tsx
const client = createOpenAI({
    baseURL: PORTKEY_GATEWAY_URL,
    apiKey: "xx",
    headers: createHeaders({
        apiKey: "PORTKEY_API_KEY",
        virtualKey: "ANTHROPIC_VIRTUAL_KEY"
    }),
})

// Set the runtime to edge for best performance
export const runtime = 'edge';

export async function POST(req: Request) {
  const { messages } = await req.json();

  // Invoke Chat Completion
  const response = await streamText({
    model: client('claude-3-opus-20240229'),
    messages,
    maxTokens: 200
  })

  // Respond with the stream
  return response.toTextStreamResponse();
}
```

#### 5. Switch to Gemini 1.5

Similarly, you can just add your [Google AI Studio API key](https://aistudio.google.com/app/) to Portkey and call Gemini 1.5:

```tsx
const client = createOpenAI({
    baseURL: PORTKEY_GATEWAY_URL,
    apiKey: "xx",
    headers: createHeaders({
        apiKey: "PORTKEY_API_KEY",
        virtualKey: "GEMINI_VIRTUAL_KEY"
    }),
})

// Set the runtime to edge for best performance
export const runtime = 'edge';

export async function POST(req: Request) {
  const { messages } = await req.json();

  // Invoke Chat Completion
  const response = await streamText({
    model: client('gemini-1.5-flash'),
    messages
  })

  // Respond with the stream
  return response.toTextStreamResponse();
}
```

The same will follow for all the other providers like **Azure**, **Mistral**, **Anyscale**, **Together**, and [more](https://docs.portkey.ai/docs/provider-endpoints/supported-providers).

#### 6. Wire up the UI

Let's create a Client component that will have a form to collect the prompt from the user and stream back the completion. The `useChat` hook will default use the `POST` Route Handler we created earlier (`/api/chat`). However, you can override this default value by passing an `api` prop to useChat(`{ api: '...'}`).

```tsx
//"app/page.tsx"
'use client';

import { useChat } from 'ai/react';

export default function Chat() {
  const { messages, input, handleInputChange, handleSubmit } = useChat();
  return (
    <div className="flex flex-col w-full max-w-md py-24 mx-auto stretch">
      {messages.map((m) => (
        <div key={m.id} className="whitespace-pre-wrap">
          {m.role === 'user' ? 'User: ' : 'AI: '}
          {m.content}
        </div>
      ))}

      <form onSubmit={handleSubmit}>
        <input
          className="fixed bottom-0 w-full max-w-md p-2 mb-8 border border-gray-300 rounded shadow-xl"
          value={input}
          placeholder="Say something..."
          onChange={handleInputChange}
        />
      </form>
    </div>
  );
}
```

#### 7. Log the Requests

Portkey logs all the requests you’re sending to help you debug errors, and get request-level + aggregate insights on costs, latency, errors, and more.

You can enhance the logging by tracing certain requests, passing custom metadata or user feedback.

![rolling logs and cachescreens](https://portkey.ai/blog/content/images/2024/04/logs.gif)

**Segmenting Requests with Metadata**

While Creating the Client, you can pass any `{"key":"value"}` pairs inside the metadata header. Portkey segments the requests based on the metadata to give you granular insights.

```tsx
const client = createOpenAI({
    baseURL: PORTKEY_GATEWAY_URL,
    apiKey: "xx",
    headers: createHeaders({
        apiKey: {PORTKEY_API_KEY},
        virtualKey: {GEMINI_VIRTUAL_KEY},
        metadata: {
          _user: 'john doe',
          organization_name: 'acme',
          custom_key: 'custom_value'
        }
    }),
})
```

Learn more about [tracing](https://portkey.ai/docs/product/observability-modern-monitoring-for-llms/traces) and [feedback](https://portkey.ai/docs/product/observability-modern-monitoring-for-llms/feedback).

### Guide: Handle OpenAI Failures

#### 1. Solve 5xx, 4xx Errors

Portkey helps you automatically trigger a call to any other LLM/provider in case of primary failures.\
[Create](https://portkey.ai/docs/product/ai-gateway-streamline-llm-integrations/configs) a fallback logic with Portkey’s Gateway Config.

For example, for setting up a fallback from OpenAI to Anthropic, the Gateway Config would be:

```json
{
  "strategy": { "mode": "fallback" },
  "targets": [{ "virtual_key": "openai-virtual-key" }, { "virtual_key": "anthropic-virtual-key" }]
}
```

You can save this Config in Portkey app and get an associated Config ID that you can pass while instantiating your LLM client:

#### 2. Apply Config to the Route Handler

```tsx
const client = createOpenAI({
    baseURL: PORTKEY_GATEWAY_URL,
    apiKey: "xx",
    headers: createHeaders({
        apiKey: {PORTKEY_API_KEY},
        config: {CONFIG_ID}
    }),
})
```

#### 3. Handle Rate Limit Errors

You can loadbalance your requests against multiple LLMs or accounts and prevent any one account from hitting rate limit thresholds.

For example, to route your requests between 1 OpenAI and 2 Azure OpenAI accounts:

```json
{
  "strategy": { "mode": "loadbalance" },
  "targets": [
    { "virtual_key": "openai-virtual-key", "weight": 1 },
    { "virtual_key": "azure-virtual-key-1", "weight": 1 },
    { "virtual_key": "azure-virtual-key-2", "weight": 1 }
  ]
}
```

Save this Config in the Portkey app and pass it while instantiating the LLM Client, just like we did above.

Portkey can also trigger [automatic retries](https://portkey.ai/docs/product/ai-gateway-streamline-llm-integrations/automatic-retries), set [request timeouts](https://portkey.ai/docs/product/ai-gateway-streamline-llm-integrations/request-timeouts), and more.

### Guide: Cache Semantically Similar Requests

Portkey can save LLM costs & reduce latencies 20x by storing responses for semantically similar queries and serving them from cache.

For Q\&A use cases, cache hit rates go as high as 50%. To enable semantic caching, just set the `cache` `mode` to `semantic` in your Gateway Config:

```json
{
  "cache": { "mode": "semantic" }
}
```

Same as above, you can save your cache Config in the Portkey app, and reference the Config ID while instantiating the LLM Client.

Moreover, you can set the `max-age` of the cache and force refresh a cache. See the [docs](https://portkey.ai/docs/product/ai-gateway-streamline-llm-integrations/cache-simple-and-semantic) for more information.

### Guide: Manage Prompts Separately

Storing prompt templates and instructions in code is messy. Using Portkey, you can create and manage all of your app’s prompts in a single place and directly hit our prompts API to get responses. Here’s more on [what Prompts on Portkey can do](https://portkey.ai/docs/product/prompt-library).

To create a Prompt Template,

1. From the Dashboard, Open **Prompts**
2. In the **Prompts** page, Click **Create**
3. Add your instructions, variables, and You can modify model parameters and click **Save**

![verel app prompt](https://raw.githubusercontent.com/Portkey-AI/portkey-cookbook/a358363e2102df4ff7657e56cc592f4e861f9d6c/integrations/images/vercel-portkey-guide/2-vercel-portkey-guide.png)

#### Trigger the Prompt in the Route Handler

```js
import Portkey from 'portkey-ai'

const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY"
})


export async function POST(req: Request) {
  const { movie } = await req.json();

  const moviePromptRender = await portkey.prompts.render({
        promptID: "PROMPT_ID",
        variables: { "movie": movie }
    })
  const messages = moviePromptRender.data.messages

  const response = await streamText({
    model: client('gemini-1.5-flash'),
    messages
  })

  return response.toTextStreamResponse();
}
```

See [docs](https://portkey.ai/docs/api-reference/prompts/prompt-completion) for more information.

### Talk to the Developers

If you have any questions or issues, reach out to us on [Discord here](https://portkey.ai/community). On Discord, you will also meet many other practitioners who are putting their Vercel AI + Portkey app to production.
