# Prompt Completion

## Create Prompt Completion

`POST /prompts/:id/completions`

Creates a completion for the given prompt ID created in Portkey through the Prompts tab.

{% swagger method="post" path="/prompts/:id/completions" baseUrl="https://api.portkey.ai/v1" summary="Creates a completion for the selected prompt ID" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="path" name="id" type="String" required="true" %}
The prompt ID to use for the completion request.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="variables" type="Object" required="true" %}
The variables mentioned in the prompt template being used
{% endswagger-parameter %}

{% swagger-parameter in="body" name="hyperparameters" type="Multiple" %}
Add any hyperparameters to the request to override the ones set in the prompt definition
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}

{% endswagger-response %}
{% endswagger %}

The API expects the prompt ID, prompt variables and optionally any hyperparameters to override for this request. It returns the [chat completion](../chat-completions.md) or [completion](../completions.md) object in the unified format which is OpenAI signature compliant.

## SDK Usage

The `prompts.completions.create` method in the [Portkey SDK](../portkey-sdk-client.md) provides a way to generate a prompt completion.

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
<pre class="language-javascript"><code class="lang-javascript">import Portkey from 'portkey-ai'

const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY",
})

// Make the prompt creation call with the variables
<strong>const promptCompletion = await portkey.prompts.completions.create({
</strong><strong>    promptID: "Your Prompt ID",
</strong><strong>    variables: {
</strong><strong>       // The variables specified in the prompt
</strong><strong>    }
</strong>})

// We can also override the hyperparameters
<strong>const promptCompletion = await portkey.prompts.completions.create({
</strong><strong>    promptID: "Your Prompt ID",
</strong><strong>    variables: {
</strong><strong>       // The variables specified in the prompt
</strong><strong>    },
</strong><strong>    max_tokens: 250,
</strong><strong>    presence_penalty: 0.2
</strong>})
</code></pre>
{% endtab %}

{% tab title="Python" %}
<pre class="language-python"><code class="lang-python">from portkey_ai import Portkey

client = Portkey(
    api_key="PORTKEY_API_KEY",  # defaults to os.environ.get("PORTKEY_API_KEY")
)

<strong>prompt_completion = client.prompts.completions.create(
</strong><strong>    prompt_id="Your Prompt ID",
</strong><strong>    variables={
</strong><strong>       # The variables specified in the prompt
</strong><strong>    }
</strong>)

print(prompt_completion)
</code></pre>

<pre class="language-python"><code class="lang-python"># We can also override the hyperparameters
<strong>prompt_completion = client.prompts.completions.create(
</strong><strong>    prompt_id="Your Prompt ID",
</strong><strong>    variables={
</strong><strong>       # The variables specified in the prompt
</strong><strong>    },
</strong><strong>    max_tokens=250,
</strong><strong>    presence_penalty=0.2
</strong>)
print(prompt_completion)
</code></pre>
{% endtab %}

{% tab title="REST" %}
<pre class="language-bash" data-overflow="wrap"><code class="lang-bash"><strong>curl -X POST "https://api.portkey.ai/v1/prompts/PROMPT_ID/completions" \
</strong>-H "Content-Type: application/json" \
-H "x-portkey-api-key: $PORTKEY_API_KEY" \
<strong>-d '{
</strong><strong>    "variables": {
</strong><strong>        # variables to pass
</strong><strong>    },
</strong><strong>    "max_tokens": 250, # Optional
</strong><strong>    "presence_penalty": 0.2 # Optional
</strong>}'
</code></pre>
{% endtab %}
{% endtabs %}
