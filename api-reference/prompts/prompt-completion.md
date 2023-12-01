# Prompt Completion

## Create Prompt Completion

`POST /prompts/:id/completion`

Creation a completion for the specific prompt ID. The prompt ID can be found in the Portkey UI in the Prompts tab.

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
