# Aporia

[Aporia](https://www.aporia.com/) provides state-of-the-art Guardrails for any AI workload. With Aporia, you can setup powerful, multimodal AI Guardrails and just add your Project ID to Portkey to enable them for your Portkey requests.

Aporia supports Guardrails for `Prompt Injections`, `Prompt Leakage`, `SQL Enforcement`, `Data Leakage`, `PII Leakage`, `Profanity Detection`, and many more!

Browse Aporia's docs for more info on each of the Guardrail:

{% embed url="https://gr-docs.aporia.com/get-started/quickstart" %}

## Using Aporia with Portkey

### 1. Add Aporia API Key to Portkey

* Navigate to the "Integrations" page under "Settings"
* Click on the edit button for the Aporia integration and add your API key

<figure><img src="../../../.gitbook/assets/CleanShot 2024-07-31 at 18.27.20@2x.png" alt=""><figcaption></figcaption></figure>

### 2. Add Aporia's Guardrail Check

* Now, navigate to the "Guardrails" page
* Search for "Validate - Project" Guardrail Check and click on `Add`&#x20;
* Input your corresponding Aporia Project ID where you are defining the policies
* Save the check, set any actions you want on the check, and create the Guardrail!

<table><thead><tr><th width="167">Check Name</th><th>Description</th><th width="146">Parameters</th><th>Supported Hooks	</th></tr></thead><tbody><tr><td>Validate - Projects</td><td>Runs a project containing policies set in Aporia and returns a <mark style="color:green;"><code>PASS</code></mark> or <mark style="color:red;"><code>FAIL</code></mark>  verdict</td><td>Project ID: <code>string</code></td><td><code>beforeRequestHooks</code><br><br><code>afterRequestHooks</code></td></tr></tbody></table>

<figure><img src="../../../.gitbook/assets/CleanShot 2024-07-31 at 18.22.49@2x.png" alt=""><figcaption></figcaption></figure>

Your Aporia Guardrail is now ready to be added to any Portkey request you'd like!

### 3. Add Guardrail ID to a Config and Make Your Request

* When you save a Guardrail, you'll get an associated Guardrail ID - add this ID to the `before_request_hooks` or `after_request_hooks` methods in your Portkey Config
* Save this Config and pass it along with any Portkey request you're making!&#x20;

Your requests are now guarded by your Aporia policies and you can see the Verdict and any action you take directly on Portkey logs! More detailed logs for your requests will also be available on your Aporia dashboard.

***

## Get Support

If you face any issues with the Aporia integration, just ping the @Aporia team on the [community forum](https://discord.gg/portkey-llms-in-prod-1143393887742861333).
