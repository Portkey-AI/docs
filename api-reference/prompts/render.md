# Render

## Retrieve Prompt Template

`POST /prompts/:id/render`

Retrieves given prompt ID containing the saved model params (temperature, max\_tokens, etc) along with the prompt/messages body.

{% swagger method="post" path="/prompts/:id/render" baseUrl="https://api.portkey.ai/v1" summary="Retrieves the details of the selected prompt ID" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="path" name="id" type="String" required="true" %}
The prompt ID to retrieve
{% endswagger-parameter %}

{% swagger-parameter in="body" name="variables" type="Object" required="false" %}
The variables mentioned in the prompt template being used
{% endswagger-parameter %}

{% swagger-parameter in="body" name="hyperparameters" type="Multiple" %}
Add any hyperparameters to the request to override the ones set in the prompt definition
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}

{% endswagger-response %}
{% endswagger %}

The API expects the prompt ID, prompt variables and optionally any hyperparameters to override for this request. It returns a JSON object containing the prompt template details.

## Example Usage

{% tabs %}
{% tab title="REST" %}
```bash
curl -X POST "https://api.portkey.ai/v1/prompts/$PROMPT_ID/render" \
-H "Content-Type: application/json" \
-H "x-portkey-api-key: $PORTKEY_API_KEY" \
-d '{
  "variables": {"movie":"Dune 2"}
}'
```

The Output:

```json
{
    "success": true,
    "data": {
        "model": "gpt-4",
        "n": 1,
        "top_p": 1,
        "max_tokens": 256,
        "temperature": 0,
        "presence_penalty": 0,
        "frequency_penalty": 0,
        "messages": [
            {
                "role": "system",
                "content": "You're a helpful assistant."
            },
            {
                "role": "user",
                "content": "Who directed Dune 2?"
            }
        ]
    }
}
```
{% endtab %}
{% endtabs %}

For detailed examples, refer to:

{% content-ref url="../../product/prompt-library/retrieve-prompt-templates.md" %}
[retrieve-prompt-templates.md](../../product/prompt-library/retrieve-prompt-templates.md)
{% endcontent-ref %}
