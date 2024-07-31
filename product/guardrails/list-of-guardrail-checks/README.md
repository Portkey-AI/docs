# List of Guardrail Checks

Each Guardrail Check has a specific purpose, it's own parameters, supported hooks, and sources.&#x20;

## Partner Guardrails

Portkey has partnered with leading AI Guardrails companies [Aporia](https://www.aporia.com/), [Pillar](https://www.pillar.security/), and [SydeLabs](https://sydelabs.ai/) to bring their Guardrails frameworks on to the Portkey Gateway and make them available for Portkey's worldwide users.

<table data-view="cards"><thead><tr><th></th><th></th><th></th><th data-hidden data-card-cover data-type="files"></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><p><strong>Supported Guardrails</strong></p><ul><li>Validate Aporia policies</li></ul><p>Define your Aporia policies on your Aporia dashboard and just pass the project ID in Portkey Guardrail check.</p></td><td></td><td></td><td><a href="../../../.gitbook/assets/Frame 1000006268.png">Frame 1000006268.png</a></td><td><a href="aporia.md">aporia.md</a></td></tr><tr><td><p><strong>Supported Guardrails</strong></p><ul><li>Scan Prompt</li><li>Scan Response</li></ul><p>For PII, toxicity, prompt injection, detection, and more.</p></td><td></td><td></td><td><a href="../../../.gitbook/assets/Frame 1000006270.png">Frame 1000006270.png</a></td><td></td></tr><tr><td><p><strong>Supported Guardrails</strong></p><ul><li>SydeGuard</li></ul><p>Set threshold levels for prompt injection, toxicity, and evasion.</p></td><td></td><td></td><td><a href="../../../.gitbook/assets/Frame 1000006269.png">Frame 1000006269.png</a></td><td></td></tr></tbody></table>

{% hint style="info" %}
The logic for all of the Guardrail Checks (including Partner Guardrails) is open source.

View it [here](https://github.com/Portkey-AI/gateway/tree/feat/plugins/plugins/default) and [here](https://github.com/Portkey-AI/gateway/tree/feat/plugins/plugins/portkey) on the Portkey Gateway repo.
{% endhint %}

## Portkey's Default Guardrail Checks

Along with the partner Guardrails, there are also deterministic as well as LLM-based Guardrails supported natively by Portkey.&#x20;

Check out their details below:

<table><thead><tr><th width="162">Check Name</th><th width="209">Description</th><th width="179">Parameters</th><th width="212">Supported Hooks</th></tr></thead><tbody><tr><td>Regex Match</td><td>Check if the request or response text matches a regex pattern.</td><td>rule: <code>string</code></td><td><code>beforeRequestHook</code><br> <code>afterRequestHook</code></td></tr><tr><td>Sentence Count</td><td>Checks if the content contains a certain number of sentences. Ranges allowed.</td><td>minSentences: <code>number</code><br> maxSentences: <code>number</code></td><td><code>beforeRequestHook</code><br> <code>afterRequestHook</code></td></tr><tr><td>Word Count</td><td>Checks if the content contains a certain number of words. Ranges allowed.</td><td>minWords: <code>number</code><br><br>maxWords: <code>number</code></td><td><code>beforeRequestHook</code><br> <code>afterRequestHook</code></td></tr><tr><td>Character Count</td><td>Checks if the content contains a certain number of characters. Ranges allowed.</td><td>minCharacters: <code>number</code><br><br>maxCharacters: <code>number</code></td><td><code>beforeRequestHook</code><br> <code>afterRequestHook</code></td></tr><tr><td>JSON Schema</td><td>Check if the response JSON matches a JSON schema.</td><td>schema: <code>json</code></td><td><code>afterRequestHook</code></td></tr><tr><td>JSON Keys</td><td>Check if the response JSON contains any, all or none of the mentioned keys.</td><td>keys: <code>array</code><br><br>operator: <code>string</code></td><td><code>afterRequestHook</code></td></tr><tr><td>Contains</td><td>Checks if the content contains any, all or none of the words or phrases.</td><td>words: <code>array</code><br><br>operator: <code>string</code></td><td><code>afterRequestHook</code></td></tr><tr><td>Valid URLs</td><td>Checks if all the URLs mentioned in the content are valid</td><td>onlyDNS: <code>boolean</code></td><td><code>afterRequestHook</code></td></tr><tr><td>Contains Code</td><td>Checks if the content contains code of format SQL, Python, TypeScript, etc.</td><td>format: <code>string</code></td><td><code>afterRequestHook</code></td></tr><tr><td>Webhook</td><td>Makes a webhook request for custom guardrails</td><td>webhookURL:<code>string</code><br><br>headers: <code>json</code></td><td><code>beforeRequestHook</code><br> <code>afterRequestHook</code></td></tr><tr><td>Moderate Content (<code>LLM-based</code>)</td><td>Checks if the content passes the mentioned content moderation checks.</td><td>categories: <code>array</code></td><td><code>beforeRequestHook</code></td></tr><tr><td>Check Language<br>(<code>LLM-based</code>)</td><td>Checks if the response content is in the mentioned language.</td><td>language: <code>string</code></td><td><code>beforeRequestHook</code></td></tr><tr><td>Detect PII<br>(<code>LLM-based</code>)</td><td>Detects Personally Identifiable Information (PII) in the content.</td><td>categories: <code>array</code></td><td><code>beforeRequestHook</code><br> <code>afterRequestHook</code></td></tr><tr><td>Detect Gibberish<br>(<code>LLM-based</code>)</td><td>Detects if the content is gibberish.</td><td><code>boolean</code></td><td><code>beforeRequestHook</code><br> <code>afterRequestHook</code></td></tr></tbody></table>

***

## Contribute Your Guardrail

We actively welcome Guardrail platforms to integrate their APIs with Portkey Gateway and let Portkey Gateway users use your Guardrail with Portkey's fast & reliable AI Gateway.

[Check out some existing examples here to create your own](https://github.com/portkey-ai/gateway)!
