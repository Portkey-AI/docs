# Prompt Templates

{% hint style="success" %}
This feature is available for all plans:

[**Developer**](https://portkey.ai/pricing): 3 Prompt Templates

[**Production**](https://portkey.ai/pricing) & [**Enterprise**](https://portkey.ai/docs/product/enterprise-offering): Unlimited Prompt Templates
{% endhint %}

With Prompt Templates, you can seamlessly create and manage your LLM prompts in one place, and deploy them with just an API call. Prompt templates on Portkey are built to be production-ready - Portkey automatically tracks changes, maintains versions, and gives both the developer and the prompt engineer immense flexibility to do fast experimentation without breaking prod.

## How to use Prompt Templates

* On the Portkey app, just click on the "Prompts" button on the left, click on "Create" and a new, blank playground opens up.
* Here you can pick your provider & model of choice - Portkey supports `vision`, `chat`, and `completions` models from 20+ providers. Provider choice here is tied up to [Virtual keys](../ai-gateway-streamline-llm-integrations/virtual-keys/) so you may see multiple options for the same provider, based on the number of virtual keys you have.
* You can write the user/assistant messages as well as configure all the model parameters like `top_p`, `max_tokens`, `logit_bias` etc - right from UI.&#x20;
* Portkey prompts also has support for enabling `JSON Mode`, and writing `Tools/Functions` call chains.

<div align="left">

<figure><img src="../../.gitbook/assets/image (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

</div>

***

## Templating Engine

**Portkey uses** [**Mustache**](https://mustache.github.io/mustache.5.html) **under the hood to power the prompt templates.**

{% hint style="info" %}
Mustache is a commonly used logic-less templating engine that follows a simple schema for defining variables and more.
{% endhint %}

With Mustache, prompt templates become even more extensible by letting you incorporate various **\{{tags\}}** in your prompt template and easily pass your data.

The most common usage of mustache templates is for **\{{variables\}}**, used to pass a value at runtime.

### Using Variables

Let's look at the following template:

<figure><img src="../../.gitbook/assets/image (2) (1) (1).png" alt=""><figcaption></figcaption></figure>

As you can see, `{{customer_data}}` and `{{chat_query}}` are defined as variables in the template and you can pass their value at the runtime:

<pre class="language-javascript"><code class="lang-javascript">import Portkey from 'portkey-ai'

const portkey = new Portkey()

const response = portkey.prompts.completions.create({
    promptID: "pp-hr-bot-5c8c6e",
<strong>    variables: {
</strong><strong>        "customer_data":"",
</strong><strong>        "chat_query":""
</strong><strong>    }
</strong>})
</code></pre>

#### Using variables is just the start! Portkey supports multiple Mustache tags that let you extend the template functionality:

## Supported Tags

<table><thead><tr><th width="151">Tag</th><th width="167">Functionality</th><th width="479">Example</th></tr></thead><tbody><tr><td>{{variable}}</td><td>Variable</td><td><p><strong>Template:</strong></p><p>Hi! My name is {{name}}. I work at {{company}}.</p><p></p><p><strong>Data:</strong></p><pre class="language-json"><code class="lang-json">{
  "name": "Chris",
  "company": "GitHub"
}
</code></pre><p></p><p><strong>Output:</strong><br>Hi! My name is Chris. I work at Github.</p></td></tr><tr><td>{{#variable}} &#x3C;string> {{/variable}}</td><td>Render &#x3C;string> only if variable is <code>true</code> or <code>non Empty</code></td><td><p><strong>Template:</strong><br>Hello I am Tesla bot.<br>{{#chat_mode_pleasant}} Excited to chat with you! {{/chat_mode_pleasant}}</p><p>What can I help you with?<br><br><strong>Data:</strong></p><pre class="language-json"><code class="lang-json">{
  "chat_mode_pleasant": False
}
</code></pre><p></p><p><strong>Output:</strong><br>Hello I am Tesla bot. What can I help you with?</p></td></tr><tr><td><p>{{^variable}} <br>&#x3C;string></p><p>{{/variable}}</p></td><td>Render &#x3C;string> only if variable is <code>false</code> or <code>empty</code></td><td><p><strong>Template:</strong><br>Hello I am Tesla bot.<br>{{^chat_mode_pleasant}} Excited to chat with you! {{/chat_mode_pleasant}}</p><p>What can I help you with?<br><br><strong>Data:</strong></p><pre class="language-json"><code class="lang-json">{
  "chat_mode_pleasant": False
}
</code></pre><p></p><p><strong>Output:</strong><br>Hello I am Tesla bot. Excited to chat with you! What can I help you with?</p></td></tr><tr><td>{{#variable}}<br>{{sub_variable}}<br>{{/variable}}</td><td>Iteratively render all the values of sub_variable if variable is <code>true</code> or <code>non Empty</code></td><td><p><strong>Template:</strong><br>Give atomic symbols for the following:<br>{{#variable}}<br>- {{sub_variable}}<br>{{/variable}}</p><p><br><strong>Data:</strong></p><pre class="language-json"><code class="lang-json">{
  "variable": [
    { "sub_variable": "Gold" },
    { "sub_variable": "Carbon" },
    { "sub_variable": "Zinc" }
  ]
}
</code></pre><p><br><strong>Output:</strong><br>Give atomic symbols for the following:<br>- Gold<br>- Carbon<br>- Zinc</p></td></tr><tr><td>{{! Comment}}</td><td>Comments that are ignored</td><td><p><strong>Template:</strong><br>Hello I am Tesla bot.<br>{{! How do tags work?}} </p><p>What can I help you with?<br><br><strong>Data:</strong></p><pre class="language-json"><code class="lang-json">{}
</code></pre><p></p><p><strong>Output:</strong><br>Hello I am Tesla bot. What can I help you with?</p></td></tr><tr><td>{{>Partials}}</td><td><p>"Mini-templates" that can be called at runtime. </p><p></p><p>On Portkey, you can save partials separately and call them in your prompt templates by typing <code>{{></code></p></td><td><p><strong>Template:</strong><br>Hello I am Tesla bot.<br>{{>pp-tesla-template}} </p><p>What can I help you with?<br><br><strong>Data in<code>pp-tesla-template</code>:</strong></p><pre class="language-json" data-overflow="wrap"><code class="lang-json">Take the context from {{context}}. And answer user questions.
</code></pre><p></p><p><strong>Output:</strong><br>Hello I am Tesla bot. Take the context from {{context}}. And answer user questions. What can I help you with?</p></td></tr><tr><td>{{>>Partial Variables}}</td><td>Pass your privately saved partials to Portkey by creating tags with  double <code>>></code> <br><br>Like: <code>{{>> }}</code> <br><br>This is helpful if you do not want to save your partials with Portkey but are maintaining them elsewhere</td><td><p><strong>Template:</strong><br>Hello I am Tesla bot.<br>{{>>My Private Partial}} </p><p>What can I help you with?</p></td></tr></tbody></table>

## Using Tags

You can directly pass your data object containing all the variable/tags info (in JSON) to Portkey's `prompts.completions` method with the `variables` property.

**For example, here's a prompt partial containing the key instructions for an AI support bot:**

<figure><img src="../../.gitbook/assets/image (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

**And the prompt template uses the partial like this:**

<figure><img src="../../.gitbook/assets/image (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

**We can pass the data object inside the variables:**

<pre class="language-typescript"><code class="lang-typescript">import Portkey from 'portkey-ai'

const portkey = new Portkey({})

const data = {
    "company": "NESTLE",
    "product": "MAGGI",
    "benefits": "HEALTH",
    "phone number": "123456",
    "name": "Sheila",
    "device": "iOS",
    "query": "Product related",
    "test_variable":"Something unrelated" // Your data object can also contain unrelated variables
}

// Make the prompt creation call with the variables
const response = portkey.prompts.completions.create({
    promptID: "pp-system-pro-34a60b",
<strong>    variables: {
</strong><strong>        ...data,
</strong><strong>        "user_query": "I ate Maggi and I think it was stale."
</strong><strong>    }
</strong>})

console.log(response)
</code></pre>

***

## Versioning Prompts

Whenever any changes are made to your prompt template, Portkey saves your changes in the browser **but** they are **not pushed** to Portkey. You can click on the **`Update`** button on the top right to save the latest version of the prompt on Portkey.

<div align="left">

<figure><img src="../../.gitbook/assets/image (3).png" alt="" width="563"><figcaption></figcaption></figure>

</div>

**All** of your prompt versions can be seen on the right column of the playground:

<div align="left">

<figure><img src="../../.gitbook/assets/image (4).png" alt="" width="356"><figcaption></figcaption></figure>

</div>

You can **`Restore`** or **`Publish`** any of the previous versions by clicking on the elipsis.

### Publishing Prompts

Updating the Prompt does not automatically update your prompt in production. While updating, you can tick **`Publish prompt changes`** which will also update your prompt deployment to the latest version.

<div align="left">

<figure><img src="../../.gitbook/assets/image (5).png" alt="" width="563"><figcaption></figcaption></figure>

</div>
