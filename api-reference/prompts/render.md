# Render

## Retrieve Prompt Template

Retrieves given prompt ID containing the saved model params (temperature, max\_tokens, etc) along with the prompt/messages body.

<mark style="color:green;">`POST`</mark> `https://api.portkey.ai/v1/prompts/:id/render`

#### Path Parameters

| Name                                 | Type   | Description               |
| ------------------------------------ | ------ | ------------------------- |
| id<mark style="color:red;">\*</mark> | String | The prompt ID to retrieve |

#### Request Body

| Name            | Type     | Description                                                                              |
| --------------- | -------- | ---------------------------------------------------------------------------------------- |
| variables       | Object   | The variables mentioned in the prompt template being used                                |
| hyperparameters | Multiple | Add any hyperparameters to the request to override the ones set in the prompt definition |

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

### For detailed examples:

{% content-ref url="../../product/prompt-library/retrieve-prompt-templates.md" %}
[retrieve-prompt-templates.md](../../product/prompt-library/retrieve-prompt-templates.md)
{% endcontent-ref %}
