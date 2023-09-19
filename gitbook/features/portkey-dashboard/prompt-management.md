# ⚡ Prompt Management

Portkey's Prompt Management is the user interface arm of the Managed Model feature, which allows you to define, save, and manage model prompts along with all of its parameters. It acts as a playground for you to try different combinations of parameters, see the output, and iterate on your prompts.

### Setting Up AI Providers

Before you can create and manage prompts, you'll need to set up your AI providers. Under "Settings", in the "Provider API Keys" section, you need to save the respective provider's API Key. This key will enable the AI provider to be used in your prompt management. After saving the key, the respective AI provider can be used to run and manage prompts.

### Defining and Saving Prompts

The UI allows you to input your model prompt, set various parameters like model type, temperature, max tokens, and so on. Once you're happy with the prompt and its parameters, you can save it. Portkey's UI makes it easy for you to test and modify your prompts before saving.

### Versioning of Prompts

Portkey provides versioning of prompts, so any update on the saved prompt will create a new version. You can switch back to an older version anytime. This feature allows you to experiment with changes to your prompts, while always having the option to revert to a previous version if needed.

### Prompt Templating

Portkey supports variables inside prompts to allow for prompt templating. This enables you to create dynamic prompts that can change based on the variables passed in. You can add as many variables as you need, but note that all variables should be strictly string. This powerful feature allows you to reuse and personalize your prompts for different use cases or users.

### API Endpoint for Saved Models

For each saved prompt, there is a tab called "API" which shows the API endpoint for the saved model. Users can directly call this API and pass the variables required in their prompt template. This makes it easy to integrate your saved prompts into your application or service.
