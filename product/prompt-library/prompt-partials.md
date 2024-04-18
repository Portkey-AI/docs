# Prompt Partials

With Prompt Partials, you can save your commonly used templates (which could be your instruction set, data structure explanation, examples etc.) separately from your prompts and flexibly incorporate them wherever required.

Partials can also serve as a global variable store. You can define common variables that are used across multiple of your prompt template and can reference or update them easily.

## Creating Partials

Partials are directly accessible from the Promps Page:

<figure><img src="../../.gitbook/assets/image (32).png" alt=""><figcaption></figcaption></figure>

You can create a new Partial and use it for any purpose in any of your prompt templates. For example, here's a prompt partial where we are separately storing the instructions:

<figure><img src="../../.gitbook/assets/image (33).png" alt=""><figcaption></figcaption></figure>

Upon saving, each Partial generates a unique ID that you can use inside prompt templates.

### Template Engine

Partials also follow the [Mustache template engine](https://mustache.github.io/) and let you easily handle data input at runtime by using tags.

Portkey supports `{{variable}}`,  `{{#block}} <string> {{/block}}`, `{{^block}}` and other tags.

[**Check out this comprehensive guide on how to use tags**](global-prompt-partials.md#templating-engine).

### Versioning

Portkey follow the same **`Update` & `Publish`** flow as prompt templates. You can keep updating the partial and save new versions, and choose to send any version to prod using the **`Publish`** feature.

All the version history for any partial is avaiable on the right column and any previous version can be restored to be `latest` or `published` to prod easily.

***

## Using Partials

You can call Partials by their ID inside any prompt template by just starting to type **`{{>`**&#x20;

Portkey lists all of the available prompt partials with their names to help you easily pick.

<figure><img src="../../.gitbook/assets/image (34).png" alt=""><figcaption></figcaption></figure>

When a partial is incorporated in a template, all the variables/blocks defined are also rendered on the Prompt variables section:

<figure><img src="../../.gitbook/assets/image (35).png" alt=""><figcaption></figcaption></figure>

When a new Partial version is **Published**, your partial that is in use in any of the prompt templates also gets automatically updated.

### Making a Prompt Completion Request

All the variables/tags defined inside the partial can now be directly called at the time of making a `prompts.completion` request:

<pre class="language-javascript"><code class="lang-javascript">const response = portkey.prompts.completions.create({
    promptID: "pp-system-pro-34a60b",
<strong>    variables: {
</strong><strong>        "user_query":"",
</strong><strong>        "company":"",
</strong><strong>        "product":"",
</strong><strong>        "benefits":"",
</strong><strong>        "phone number":"",
</strong><strong>        "name":"",
</strong><strong>        "device":"",
</strong><strong>        "query":""
</strong><strong>    }
</strong>})
</code></pre>

