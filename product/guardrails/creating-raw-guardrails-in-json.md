# Creating Raw Guardrails (in JSON)

At Portkey, we believe in helping you make your workflows [as modular as possible](https://portkey.ai/blog/what-it-means-to-go-to-prod/). With the raw Guardrails mode, we let you define your Guardrail checks & actions however you want, directly in code.

This is useful when

* You want the same Guardrail checks but want to take different basic actions on them
* Your Guardrail checks definitions are dependent on an upstream task and are updated in code
* You want greater control over how you want to handle Guardrails

With the Raw Guardrails mode, you can achieve all this.&#x20;

### Example of a Raw Guardrail

```json
"beforeRequestHooks": [{
    "type": "guardrail",
    "name": "my_solid_guardrail",
    "checks": [{
      "id": "default.regexMatch",
      "parameters": {
        "regex": ["test"]
      }
    }]
}]
```

In this example:

* **`type`**: Specifies the type of hook, which is `guardrail`.
* **`name`**: Gives a name to the guardrail for identification.
* **`checks`**: Lists the checks that make up the guardrail. Each check includes an `id` and `parameters` for the specific conditions to validate.

### Configuring Guardrail Actions

<pre class="language-json"><code class="lang-json">"beforeRequestHooks": [{
    "type": "guardrail",
    "name": "my_solid_guardrail",
    "checks": [{
      "id": "default.regexMatch",
      "parameters": {
        "regex": ["test"]
      }
    }],
<strong>    "deny": false,
</strong><strong>    "async": false,
</strong><strong>    "on_success": {
</strong><strong>        "feedback": {"value": 1,"weight": 1}
</strong><strong>    },
</strong><strong>    "on_fail": {
</strong><strong>        "feedback": {"value": -1,"weight": 1}
</strong><strong>     }
</strong>}]
</code></pre>

In this example,

* **`deny`**: Is set to <mark style="color:green;">`TRUE`</mark> or <mark style="color:red;">`FALSE`</mark>
* **`async`**: Is set to <mark style="color:green;">`TRUE`</mark> or <mark style="color:red;">`FALSE`</mark>
* `on_success`:  Used to pass custom `feedback`
* `on_failure`: Used to pass custom `feedback`\
