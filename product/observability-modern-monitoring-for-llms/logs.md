# Logs

The Logs section presents a chronological list of all the requests processed through Portkey. Each log entry provides useful data such as the timestamp, request type, LLM used, tokens generated, and cost. For [multimodal models](../ai-gateway-streamline-llm-integrations/multimodal-capabilities/), Logs will also show the image sent with vision/image models, as well as the image generated.

By clicking on an entry, a side panel opens up, revealing the entire raw data with the request and response objects.

This detailed log can be invaluable when troubleshooting issues or understanding specific interactions. It provides full transparency into each request and response, enabling you to see exactly what data was sent and received.

<figure><img src="../../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

## Share Logs with Teammates

Each log on Portkey has a unique URL. You can copy the link from the address bar and directly share it with anyone in your org.

## Request Status Guide

The Status column on the Logs page gives you a snapshot of the gateway activity for every request.

Portkey’s gateway features—[Cache](broken-reference), [Retries](broken-reference), [Fallback](broken-reference), [Loadbalance](broken-reference)—are all tracked here with their exact states (`disabled`, `triggered`, etc.), making it a breeze to monitor and optimize your usage.

**Common Queries Answered:**

* **Is the cache working?**: Enabled caching but unsure if it's active? The Status column will confirm it for you.
* **How many retries happened?**: Curious about the retry count for a successful request? See it in a glance.
* **Fallback and Loadbalance**: Want to know if load balance is active or which fallback option was triggered? See it in a glance.

<figure><img src="../../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

<table><thead><tr><th width="195">Option</th><th width="238">🔴 Inactive State</th><th>🟢 Possible Active States</th></tr></thead><tbody><tr><td><strong>Cache</strong></td><td>Cache Disabled</td><td><p>Cache Miss,</p><p>Cache Refreshed,</p><p>Cache Hit,</p><p>Cache Semantic Hit</p></td></tr><tr><td><strong>Retry</strong></td><td>Retry Not Triggered</td><td><p>Retry Success on {x} Tries,</p><p>Retry Failed</p></td></tr><tr><td><strong>Fallback</strong></td><td>Fallback Disabled</td><td><p>Fallback Active,</p><p>Fallback Triggered on Option {x}</p></td></tr><tr><td><strong>Loadbalance</strong></td><td>Loadbalancer Disabled</td><td>Loadbalancer Active</td></tr></tbody></table>

## Manual Feedback

As you're viewing logs, you can also add manual feedback on the logs to be analysed and filtered later. This data can be viewed on the [feedback analytics dashboards](analytics.md#feedback).

<figure><img src="../../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

## Configs & Propmt IDs in Logs

If your request has an attached [Config](../ai-gateway-streamline-llm-integrations/configs.md) or if it's originating from a [prompt template](../prompt-library.md), you can see the relevant Config or Prompt IDs separately in the log's details on Portkey. And to dig deeper, you can just click on the IDs and Portkey will take you to the respective Config or Prompt playground where you can view the full details.

<div align="left">

<figure><img src="../../.gitbook/assets/config-prompt-in-logs.png" alt=""><figcaption></figcaption></figure>

</div>

## Debug Requests with Log Replay

You can rerun any buggy request with just one click, straight from the log details page. The `Replay` button opens your reuqest in a fresh prompt playground where you can rerun the request and edit it right there until it works.

<div align="left">

<figure><img src="../../.gitbook/assets/log-replay.png" alt=""><figcaption></figcaption></figure>

</div>

{% hint style="info" %}
**`Replay` button will be inactive for a log in the following cases:**

1. If the reuqest is sent to any endpoint other than `/chat/completions,` `/completions`, `/embeddings`
2. If the virtual key used in the log is archived on Portkey
3. If the request originates from a prompt template which is called from inside a Config target
{% endhint %}
