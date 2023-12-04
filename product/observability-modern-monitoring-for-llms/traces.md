# Traces

Having end-to-end visibility of your requests is crucial. Portkey supports request tracing to help you monitor your applications throughout the lifecycle of a request.

To enable tracing, you can pass a `trace-id` in the header of any request made via Portkey. This trace ID will be associated with the journey of the request, from initiation to completion, providing insights into the entire process.

### How to Enable Request Tracing

To enable tracing, include the `x-portkey-trace-id` in your request header.

```json
{
    "x-portkey-trace-id": "<YOUR TRACE ID>"
}
```

or pass it as request config parameter when using the Portkey or OpenAI SDKs.

{% tabs %}
{% tab title="NodeJS" %}
```javascript
const requestOptions = {traceID: "YOUR TRACE ID"}
const chatCompletion = await portkey.chat.completions.create({
    messages: [{ role: 'user', content: 'Say this is a test' }],
    model: 'gpt-3.5-turbo',
}, requestOptions);

console.log(chatCompletion.choices);
```
{% endtab %}

{% tab title="Python" %}
```python
completion = portkey.with_options(
    trace_id = "39e2a60c-b47c-45d8"
).chat.completions.create(
    messages = [{ "role": 'user', "content": 'Say this is a test' }],
    model = 'gpt-3.5-turbo'
})
```
{% endtab %}

{% tab title="OpenAI NodeJS" %}
<pre class="language-javascript"><code class="lang-javascript">const reqHeaders = {headers: <a data-footnote-ref href="#user-content-fn-1">createHeaders</a>({"traceID": "TRACE ID"})}
const chatCompletion = await openai.chat.completions.create({
    messages: [{ role: 'user', content: 'Say this is a test' }],
    model: 'gpt-3.5-turbo',
}, reqHeaders);
</code></pre>
{% endtab %}

{% tab title="OpenAI Python" %}
```python
req_headers = createHeaders(trace_id="TRACE ID")
chat_complete = client.with_options(headers=req_headers).chat.completions.create(
    model="gpt-4",
    messages=[{"role": "user", "content": "Say this is a test"}],
)
```
{% endtab %}
{% endtabs %}

### Tracing and User Feedback

Trace IDs can also be used to link user feedback to specific generations. This can be used in a system where users provide feedback, like a thumbs up or thumbs down, or something more complex via our feedback APIs. This feedback can be linked to traces which can span over a single generation or multiple ones.

[Learn more about sending feedback](feedback.md)

You can view all the requests with a common `trace-id` easily on the logs page.

<figure><img src="../../.gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>

[^1]: Imported from the Portkey SDK
