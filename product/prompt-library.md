# Prompt Library

With Portkey's Prompt Library, you can seamlessly create and manage prompts along with all associated model parameters.&#x20;

This enables you to experiment with various combinations of parameters and prompts effortlessly, view the outputs, iterate, and when satisfied, deploy with a single click.

#### **Setting Up AI Providers**

Before you can create and manage prompts, you'll need to set up your [Virtual Keys](ai-gateway-streamline-llm-integrations/virtual-keys/). After saving the key, the respective AI provider can be used to run and manage prompts.

#### **Defining and Saving Prompts**

The UI allows you to input your model prompt, set various parameters like model type, temperature, max tokens, and so on. Once you're happy with the prompt and its parameters, you can save it. Portkey's UI makes it easy for you to test and modify your prompts before saving.

#### **Versioning of Prompts**

Portkey provides versioning of prompts, so any update on the saved prompt will create a new version. You can switch back to an older version anytime. This feature allows you to experiment with changes to your prompts, while always having the option to revert to a previous version if needed.

#### **Prompt Templating**

Portkey supports variables inside prompts to allow for prompt templating. This enables you to create dynamic prompts that can change based on the variables passed in. You can add as many variables as you need, but note that all variables should be strictly string. This powerful feature allows you to reuse and personalize your prompts for different use cases or users.

#### **API Endpoint for Saved Prompts**

For each saved prompt, there is a tab called "API" which shows the API endpoint for the saved model. Users can directly call this API and pass the variables required in their prompt template. This makes it easy to integrate your saved prompts into your application or service.

#### Retrieving Saved Prompt Templates

You can use the `render` endpoint to retrieve your saved prompt template on Portkey. Read more below:

{% content-ref url="prompt-library/retrieve-prompt-templates.md" %}
[retrieve-prompt-templates.md](prompt-library/retrieve-prompt-templates.md)
{% endcontent-ref %}

#### Global Prompt Partials

Docs coming soon!

#### Advanded Prompting with JSON Mode

Docs coming soon!

***

## Multimodal Prompts

Portkey's prompt playground also supports vision models. You can pass image URL/data along with the rest of your messages body and deploy multimodal prompt templates to production easily.

***

## Prompts in Logs

You can easily view only the logs for a specific prompt template by filtering the logs on that relevant prompt template.&#x20;

Portkey also shows you the Prompt template's name separately in the log's details - you can easily jump into viewing/editing the prompt template directly from the log details page.
