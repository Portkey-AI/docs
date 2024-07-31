---
description: >-
  Ship to production more confidently with Portkey Guardrails on your requests &
  responses
---

# Guardrails

{% hint style="info" %}
This feature is in beta and available to select users. To enable it for your org, ping us on the [Portkey Discord](https://portkey.ai/community).
{% endhint %}

LLMs are brittle - not just in API uptimes or their inexplicable 400/500 errors, but also in their core behavior. You can get a response with a <mark style="color:green;">`200`</mark> status code that completely errors out for your app's pipeline due to mismatched output. With Portkey's Guardrails, we now complete the loop on building robust & reliable AI apps that behave **EXACTLY** as you want, every time.

Using Portkey's Guardrail platform, you can now verify your LLM inputs AND outputs to be adhering to your specifed checks; and since Guardrails are built on top of our [Gateway](https://github.com/portkey-ai/gateway), you can orchestrate your request exactly the way you want - with actions ranging from `denying the request`, `logging the guardrail result`, `creating an evals dataset`, `falling back to another LLM or prompt`, `retrying the request`, and more.

#### Examples of Guardrails Portkey offers:

* **Regex match** - Check if the request or response text matches a regex pattern
* **JSON Schema** - Check if the response JSON matches a JSON schema
* **Contains Code** - Checks if the content contains code of format SQL, Python, TypeScript, etc.
* ...and many more.

Portkey currently offers 20+ deterministic guardrails like the ones described above as well as LLM-based guardrails like `Detect Gibberish`, `Scan for prompt injection`, and more.&#x20;

#### Browse the full list of supported Guardrail checks here.

{% hint style="success" %}
Portkey also integrates with your favourite Guardrail platforms like [Aporia](https://www.aporia.com/), [SydeLabs](https://sydelabs.ai/), [Pillar Security](https://www.pillar.security/) and more. Just add their API keys to Portkey and you can enable their guardrails policies on your Portkey calls! \
\
More details on Guardrail Partners here.
{% endhint %}

***

## Using Guardrails

Putting Portkey Guardrails in production is just a 4-step process:

1. Create Guardrail Checks
2. Create Guardrail Actions
3. Enable Guardrail through Configs
4. Attach the Config to a Request

Let's see in detail below:

***

## 1. Create a New Guardrail & Add Checks

On the "Guardrails" page, click on **`Create`** and add your preferred Guardrail checks from the right sidebar.

{% hint style="info" %}
On Portkey, you can configure Guardrails to be run on either the **`INPUT`** (i.e. **`PROMPT`**) or the **`OUTPUT`**.\
\
Hence, for the Guardrail you create, make sure your Guardrail is only validating **ONLY ONE OF** the **Input** or the **Output**.
{% endhint %}

<figure><img src="../../.gitbook/assets/Guardrail Creation.gif" alt=""><figcaption></figcaption></figure>

Each Guardrail Check has a custom input field based on its usecase — just add the relevant details to the form and save your check.

{% hint style="info" %}
* You can add as many checks as you want to a single Guardrail.
* A check ONLY returns a boolean (**`Yes`**/**`No`**) verdict.
{% endhint %}

Here is a list of all the Guardrail checks available on Portkey and what they do.

***

## 2. Add Guardrail Actions

This is where you will define a basic orchestration logic for your Guardrail.

{% hint style="info" %}
Guardrail is created to validate **ONLY ONE OF** the **`Input`** or the **`Output`**. The Actions set here will also apply only to either the **`request`** or the **`response`**.
{% endhint %}

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

### There are 4 Types of Guardrail Actions

<table><thead><tr><th width="135">Action</th><th width="157">State</th><th>Description</th><th>Impact</th></tr></thead><tbody><tr><td><strong>Async</strong></td><td><mark style="color:green;"><strong><code>TRUE</code></strong></mark><br><br>This is the <strong><code>default</code></strong> state</td><td>Run the Guardrail checks <strong>asynchronously</strong> along with the LLM request. </td><td><ul><li>Will add no latency to your request</li><li>Useful when you only want to log guardrail checks without affecting the request</li></ul></td></tr><tr><td><strong>Async</strong></td><td><mark style="color:red;"><strong><code>FALSE</code></strong></mark></td><td><strong><code>On Request</code></strong><br>Run the Guardrail check <strong>BEFORE</strong> sending the request to the <strong>LLM</strong><br><br><strong><code>On Response</code></strong><br>Run the Guardrail check <strong>BEFORE</strong> sending the response to the <strong>user</strong></td><td><ul><li>Will add latency to the request</li><li>Useful when your Guardrail critical and you want more orchestration over your request based on the Guardrail result</li></ul></td></tr><tr><td><strong>Deny</strong></td><td><mark style="color:green;"><strong><code>TRUE</code></strong></mark></td><td><strong><code>On Request &#x26; Response</code></strong><br>If any of the Guardrail checks <strong><code>FAIL</code></strong>, the request will be killed with a <strong><code>446</code></strong> status code.<br><br>If all of the Guardrail checks <strong><code>SUCCEED</code></strong>, the request/response will be sent further with a <strong><code>200</code></strong> status code.</td><td><ul><li>This is useful when your Guardrails are critical and upon them failing, you can not run the request</li><li>We would advice running this action on a subset of your requests to first see the impact</li></ul></td></tr><tr><td><strong>Deny</strong></td><td><mark style="color:red;"><strong><code>FALSE</code></strong></mark><br><br>This is the <strong><code>default</code></strong> state</td><td><strong><code>On Request &#x26; Response</code></strong><br>If any of the Guardrail checks <strong><code>FAIL</code></strong>, the request will STILL sent, but with a <strong><code>246</code></strong> status code.<br><br>If all of the Guardrail checks <strong><code>SUCCEED</code></strong>, the request/response will be sent further with a <strong><code>200</code></strong> status code.</td><td><ul><li>This is useful when you want to log the Guardrail result but do not want it to affect your result</li></ul></td></tr><tr><td><strong>On Success</strong></td><td><strong><code>Send Feedback</code></strong></td><td>If <strong>all of the</strong> Guardrail checks <strong><code>PASS</code></strong>, append your custom defined feedback to the request</td><td><ul><li>We recommend setting up this action</li><li>This will help you build an "Evals dataset" of Guardrail results on your requests over time</li></ul></td></tr><tr><td><strong>On Failure</strong></td><td><strong><code>Send Feedback</code></strong></td><td>If <strong>any of the</strong> Guardrail checks <strong><code>FAIL</code></strong>, append your custom feedback to the request</td><td><p></p><ul><li>We recommend setting up this action</li><li>This will help you build an "Evals dataset" of Guardrail results on your requests over time</li></ul></td></tr></tbody></table>

Set the relevant actions you want with your checks, name your Guardrail and save it! When you save the Guardrail, you will get an associated **`$Guardrail_ID`** that you can then add to your request.

***

## 3. "Enable" the Guardrails through Configs

This is where Portkey's magic comes into play. The Guardrail you created above is yet not an <mark style="color:green;">`Active`</mark> guardrail because it is not attached to any request.

Configs is one of Portkey's most powerful features and is used to define all kinds of request orchestration - everything from caching, retries, fallbacks, timeouts, to load balancing.&#x20;

{% hint style="info" %}
Now, you can use Configs to add **Guardrail checks** & **actions** to your request.
{% endhint %}

### Add Guardrail ID <mark style="color:blue;">**`before the request`**</mark> OR <mark style="color:orange;">**`after the request`**</mark>

<table><thead><tr><th width="115">Type</th><th width="234">Config Key</th><th width="254">Value</th><th>Description</th></tr></thead><tbody><tr><td>Before Request Hook</td><td><mark style="color:blue;"><strong><code>before_request_hooks</code></strong></mark></td><td><code>{"id":"$guardrail_id"}</code></td><td>This key is used to run Guardrail <code>checks</code> &#x26; <code>actions</code> on the <strong><code>INPUT</code></strong>. </td></tr><tr><td>After Request Hook</td><td><mark style="color:orange;"><strong><code>after_request_hooks</code></strong></mark></td><td><code>{"id":"$guardrail_id"}</code></td><td>This key is used to run Guardrail <code>checks</code> &#x26; <code>actions</code> on the <strong><code>OUTPUT</code></strong>. </td></tr></tbody></table>

### Example Config with Guardrails

{% tabs %}
{% tab title="Simple Config" %}
<pre class="language-json"><code class="lang-json">{
	"retry": {
		"attempts": 3
	},
	"cache": {
		"mode": "simple"
	},
	"virtual_key":"openai-xxx",
<strong>	"before_request_hooks": {
</strong><strong>		"id": "input-guardrail-id-xx"
</strong><strong>	},
</strong><strong>	"after_request_hooks": {
</strong><strong>		"id": "output-guardrail-id-xx"
</strong><strong>	}
</strong>}
</code></pre>
{% endtab %}
{% endtabs %}

### Guardrail Behaviour on the Gateway

Based on,

* Guardrail Check Verdict (<mark style="color:green;">**`PASS`**</mark> or <mark style="color:red;">**`FAIL`**</mark>) AND
* Guardrail Action: DENY Setting (<mark style="color:green;">**`TRUE`**</mark> or <mark style="color:red;">**`FALSE`**</mark>)

Portkey sends different **`request status codes`** corresponding to your set Guardrail behaviour.

| Guardrail Verdict                            | DENY Setting                                 | Returned Status Code                         | Description                                                                                                                                                                                                                      |
| -------------------------------------------- | -------------------------------------------- | -------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <mark style="color:green;">**`PASS`**</mark> | <mark style="color:red;">**`FALSE`**</mark>  | <mark style="color:green;">**`200`**</mark>  | Guardrails have **passed**, request will be processed regardless                                                                                                                                                                 |
| <mark style="color:green;">**`PASS`**</mark> | <mark style="color:green;">**`TRUE`**</mark> | <mark style="color:green;">**`200`**</mark>  | Guardrails have **passed**, request will be processed regardless                                                                                                                                                                 |
| <mark style="color:red;">**`FAIL`**</mark>   | <mark style="color:red;">**`FALSE`**</mark>  | <mark style="color:orange;">**`246`**</mark> | <p>Guardrails have <strong>failed</strong>, but the request should still <strong>be processed.</strong><br><br>Portkey introduces a <mark style="color:green;">new Status code</mark> to indicate this state.</p>                |
| <mark style="color:red;">**`FAIL`**</mark>   | <mark style="color:green;">**`TRUE`**</mark> | <mark style="color:red;">**`446`**</mark>    | <p>Guardrails have <strong>failed</strong>, and the request should <strong>not</strong> <strong>be processed.</strong><br><br>Portkey introduces a <mark style="color:green;">new Status code</mark> to indicate this state.</p> |

### Example Config Using the New `246` & `446` Status Codes

{% tabs %}
{% tab title="Fallback to Another Model on Guardrail Fail" %}
<pre class="language-json"><code class="lang-json">{
	"strategy": {
		"mode": "fallback",
<strong>		"on_status_codes": [246,446]
</strong>	},
	"targets": [
		{"virtual_key": "openai-key-xxx"},
		{"virtual_key": "anthropic-key-xxx"}
	],
<strong>	"before_request_hooks": [
</strong><strong>		{"id": "guardrails-id-xxx"}
</strong><strong>	]
</strong>}
</code></pre>
{% endtab %}

{% tab title="Retry on Guardrail Fail" %}
```json
{
	"retry": {
		"on_status_codes": [246],
		"attempts": 5
	},
	"after_request_hooks": [
		{"id": "guardrails-id"}
	]
}
```
{% endtab %}
{% endtabs %}

You can create these Configs in Portkey UI, save them, and get an associated Config ID you can attach to your requests. [More here](../ai-gateway-streamline-llm-integrations/configs.md).

## 4. Final Step - Attach Config to Request

Now, while instantiating your Portkey client or while sending headers, just pass the Config ID.&#x20;

{% tabs %}
{% tab title="NodsJS" %}
```javascript
const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY",
    config: "pc-***" // Supports a string config id or a config object
});
```
{% endtab %}

{% tab title="Python" %}
```python
const portkey = Portkey(
    api_key="PORTKEY_API_KEY",
    config="pc-***" # Supports a string config id or a config object
)
```
{% endtab %}

{% tab title="OpenAI NodeJS" %}
```java
const openai = new OpenAI({
  apiKey: 'OPENAI_API_KEY',
  baseURL: PORTKEY_GATEWAY_URL,
  defaultHeaders: createHeaders({
    apiKey: "PORTKEY_API_KEY",
    config: "CONFIG_ID"
  })
});
```
{% endtab %}

{% tab title="OpenAI Python" %}
```python
client = OpenAI(
    api_key="OPENAI_API_KEY", # defaults to os.environ.get("OPENAI_API_KEY")
    base_url=PORTKEY_GATEWAY_URL,
    default_headers=createHeaders(
        provider="openai",
        api_key="PORTKEY_API_KEY", # defaults to os.environ.get("PORTKEY_API_KEY")
        config="CONFIG_ID"
    )
)
```
{% endtab %}

{% tab title="REST API" %}
```bash
curl https://api.portkey.ai/v1/chat/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -H "x-portkey-api-key: $PORTKEY_API_KEY" \
  -H "x-portkey-config: $CONFIG_ID" \ 
  -d '{
    "model": "gpt-3.5-turbo",
    "messages": [{
        "role": "user",
        "content": "Hello!"
      }]
  }'
```
{% endtab %}
{% endtabs %}

For more, refer to the [Config documentation here](../ai-gateway-streamline-llm-integrations/configs.md).

***

## &#x20;Viewing Guardrail Results in Portkey Logs

Portkey Logs will show you detailed information about Guardrail results for each request.

### On the **`Feedback & Guardrails`** tab on the log drawer, you can see

#### Guardrail Details

* **Overview**: How many checks <mark style="color:green;">`passed`</mark> and how many <mark style="color:red;">`failed`</mark>
* **Verdict**: Guardrail verdict for each of the checks in your Guardrail
* **Latency**: Round trip time for each check in your Guardrail

#### Feedback Details

Portkey will also show the feedback object logged for each request

* **`Value`:** The numerical feedback value you passed
* **`Weight`**: The numerical feedback weight
* **`Metadata Key & Value`:** Any custom metadata sent with the feedback
* **`successfulChecks`**: Which checks associated with this request <mark style="color:green;">`passed`</mark>
* **`failedChecks`**: Which checks associated with this request <mark style="color:red;">`failed`</mark>
* **`erroredChecks`**: If there were any checks that errored out along the way

<figure><img src="../../.gitbook/assets/guardrail feedback.gif" alt=""><figcaption></figcaption></figure>



***

## Defining Guardrails Directly in JSON <a href="#defining-guardrails-directly-in-json" id="defining-guardrails-directly-in-json"></a>

On Portkey, you can also create the Guardrails in code and add them to your Configs. Read more about this here:

{% content-ref url="creating-raw-guardrails-in-json.md" %}
[creating-raw-guardrails-in-json.md](creating-raw-guardrails-in-json.md)
{% endcontent-ref %}

***

## Examples of When to Deny Requests with Guardrails <a href="#examples-of-when-to-deny-requests" id="examples-of-when-to-deny-requests"></a>

1. **Prompt Injection Checks**: Preventing inputs that could alter the behavior of the AI model or manipulate its responses.
2. **Moderation Checks**: Ensuring responses do not contain offensive, harmful, or inappropriate content.
3. **Compliance Checks**: Verifying that inputs and outputs comply with regulatory requirements or organizational policies.
4. **Security Checks**: Blocking requests that contain potentially harmful content, such as SQL injection attempts or cross-site scripting (XSS) payloads.

By appropriately configuring Guardrail Actions, you can maintain the integrity and reliability of your AI app, ensuring that only safe and compliant requests are processed.

***
