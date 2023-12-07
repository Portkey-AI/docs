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
```javascript
import Portkey from 'portkey-ai'

const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY",
})

// Make the prompt creation call with the variables
const promptCompletion = portkey.prompts.completions.create({
    promptID: "Your Prompt ID",
    variables: {
       // The variables specified in the prompt
    }
})

// We can also override the hyperparameters
const promptCompletion = portkey.prompts.completions.create({
    promptID: "Your Prompt ID",
    variables: {
       // The variables specified in the prompt
    },
    max_tokens: 250,
    presence_penalty: 0.2
})
```
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

```python
# We can also override the hyperparameters
prompt_completion = client.prompts.completions.create(
    prompt_id="Your Prompt ID",
    variables={
       # The variables specified in the prompt
    },
    max_tokens=250,
    presence_penalty=0.2
)
print(prompt_completion)
```
{% endtab %}

{% tab title="REST" %}
{% code overflow="wrap" %}
```bash
curl -X POST "https://api.portkey.ai/v1/prompts/PROMPT_ID/create" \
-H "Content-Type: application/json" \
-H "x-portkey-api-key: $PORTKEY_API_KEY" \
-d '{
    "variables": {
        # variables to pass
    },
    "max_tokens": 250, # Optional
    "presence_penalty": 0.2 # Optional
}'
```
{% endcode %}
{% endtab %}
{% endtabs %}
