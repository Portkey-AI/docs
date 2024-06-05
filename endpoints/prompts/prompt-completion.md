# Prompt Completion

## Create Prompt Completion

`POST /prompts/:id/completions`

Creates a completion for the given prompt ID created in Portkey through the Prompts tab.

## Creates a completion for the selected prompt ID

<mark style="color:green;">`POST`</mark> `https://api.portkey.ai/v1/prompts/:id/completions`

#### Path Parameters

| Name                                 | Type   | Description                                      |
| ------------------------------------ | ------ | ------------------------------------------------ |
| id<mark style="color:red;">\*</mark> | String | The prompt ID to use for the completion request. |

#### Request Body

| Name                                        | Type     | Description                                                                              |
| ------------------------------------------- | -------- | ---------------------------------------------------------------------------------------- |
| variables<mark style="color:red;">\*</mark> | Object   | The variables mentioned in the prompt template being used                                |
| hyperparameters                             | Multiple | Add any hyperparameters to the request to override the ones set in the prompt definition |
| stream                                      | boolean  | Incrementally stream the response using server-sent events. Defaults to false            |

{% tabs %}
{% tab title="200: OK " %}

{% endtab %}
{% endtabs %}

The API expects the prompt ID, prompt variables and optionally any hyperparameters to override for this request. It returns the [chat completion](../chat-completions.md) or [completion](../completions.md) object in the unified format which is OpenAI signature compliant.

## SDK Usage

The `prompts.completions.create` method in the [Portkey SDK](../../api-reference/portkey-sdk-client.md) provides a way to generate a prompt completion.

### Method Signature

{% tabs %}
{% tab title="NodeJS" %}
```javascript
prompt.completions.create(promptParams)
```
{% endtab %}

{% tab title="Python" %}
```python
prompts.completions.create(promptParams)
```
{% endtab %}
{% endtabs %}

#### Parameters

* _promptParams (Object):_ Parameters for the prompt completion request including promptID, variables and optionally any hyperparameters for the completion.

### Example Usage

{% tabs %}
{% tab title="NodeJS" %}
```javascript
import Portkey from 'portkey-ai'

const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY",
})

// Make the prompt creation call with the variables
const promptCompletion = await portkey.prompts.completions.create({
    promptID: "Your Prompt ID",
    variables: {
       // The variables specified in the prompt
    }
})
```

<pre class="language-javascript"><code class="lang-javascript">// We can also override the hyperparameters
const promptCompletion = await portkey.prompts.completions.create({
    promptID: "Your Prompt ID",
    variables: {
       // The variables specified in the prompt
    },
<strong>    max_tokens: 250,
</strong><strong>    presence_penalty: 0.2
</strong>})
</code></pre>
{% endtab %}

{% tab title="Python" %}
```python
from portkey_ai import Portkey

client = Portkey(
    api_key="PORTKEY_API_KEY",  # defaults to os.environ.get("PORTKEY_API_KEY")
)

prompt_completion = client.prompts.completions.create(
    prompt_id="Your Prompt ID",
    variables={
       # The variables specified in the prompt
    }
)

print(prompt_completion)
```

<pre class="language-python"><code class="lang-python"># We can also override the hyperparameters
prompt_completion = client.prompts.completions.create(
    prompt_id="Your Prompt ID",
    variables={
       # The variables specified in the prompt
    },
<strong>    max_tokens=250,
</strong><strong>    presence_penalty=0.2
</strong>)
print(prompt_completion)
</code></pre>
{% endtab %}

{% tab title="REST" %}
<pre class="language-bash" data-overflow="wrap"><code class="lang-bash">curl -X POST "https://api.portkey.ai/v1/prompts/PROMPT_ID/completions" \
-H "Content-Type: application/json" \
-H "x-portkey-api-key: $PORTKEY_API_KEY" \
-d '{
    "variables": {
        # variables to pass
    },
<strong>    "max_tokens": 250, # Optional
</strong><strong>    "presence_penalty": 0.2 # Optional
</strong>}'
</code></pre>
{% endtab %}
{% endtabs %}

### Streaming Mode

{% hint style="info" %}
With the new **`/prompts/:id/completions`** route, it is mandatory to set **`stream:True`** while making the call—if you want to stream the responses.\
\
**This is a departure** from when you could toggle the stream mode in Portkey prompt playground UI, and have the request automatically return streamed response.\
\
Now, **regardless of the stream toggle** state in the prompt playground, if you want streaming mode, you must set it to true at the time of making the call.
{% endhint %}

{% tabs %}
{% tab title="NodeJS" %}
<pre class="language-javascript"><code class="lang-javascript">const streamPromptCompletion = await portkey.prompts.completions.create({
    promptID: "Your Prompt ID",
    variables: {
       // The variables specified in the prompt
    },
<strong>    stream: true // Defaults to false
</strong>})
</code></pre>
{% endtab %}

{% tab title="Python" %}
<pre class="language-python"><code class="lang-python">stream_prompt_completion = client.prompts.completions.create(
    prompt_id="Your Prompt ID",
    variables={
       # The variables specified in the prompt
    },
<strong>    stream=True # Defaults to false
</strong>)

for chunk in stream_prompt_completion:
        print(chunk.choices[0].delta)
</code></pre>
{% endtab %}

{% tab title="REST API" %}
<pre class="language-bash"><code class="lang-bash">curl -X POST "https://api.portkey.ai/v1/prompts/PROMPT_ID/completions" \
-H "Content-Type: application/json" \
-H "x-portkey-api-key: $PORTKEY_API_KEY" \
-d '{
    "variables": {
        # variables to pass
    },
<strong>    "stream": true, # Defaults to false
</strong>    "max_tokens": 250, # Optional
    "presence_penalty": 0.2 # Optional
}'
</code></pre>
{% endtab %}
{% endtabs %}
