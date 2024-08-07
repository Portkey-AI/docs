# Feedback

{% hint style="success" %}
This feature is available on all Portkey plans.
{% endhint %}

Portkey's Feedback APIs provide a simple way to get weighted feedback from customers on any request you served, at any stage in your app.&#x20;

You can capture this feedback on a request or conversation level and analyze it by adding meta data to the relevant request.

## **Adding Feedback to Requests**

### **1. Find the \`trace-id\`**

Portkey adds trace ids to all incoming requests. You can find this in the `x-portkey-trace-id` response header.&#x20;

To use your own trace IDs, send them as part of the request headers - [Adding a trace ID to your requests](traces.md#how-to-enable-request-tracing)

### **2. Add feedback**

You can append feedback to a request through the SDKs or the REST API.

{% tabs %}
{% tab title="NodeJS" %}
```javascript
const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY",
    ...
});

// Add the feedback
portkey.feedback.create({
    traceID: "your trace id",
    value: 5, // Integer between -10 and 10
    weight: 1, // Optional
    metadata: {
        ... // Pass any additional context here like comments, _user and more
    }
})
```
{% endtab %}

{% tab title="Python" %}
```python
portkey = Portkey(
    api_key="PORTKEY_API_KEY",
    virtual_key="VIRTUAL_KEY"
)

feedback = portkey.feedback.create(
    trace_id="TRACE_ID",
    value=5,  # Integer between -10 and 10
    weight=1,  # Optional
    metadata={
        # Pass any additional context here like comments, _user and more
    }
)
print(feedback)
```
{% endtab %}

{% tab title="REST" %}
```sh
curl --location 'https://api.portkey.ai/v1/feedback' \
    --header 'x-portkey-api-key: YOUR_API_KEY' \
    --header 'Content-Type: application/json' \
    --data '{
        "trace_id": "YOUR_TRACE_ID",
        "value": -10,
        "weight": 0.5,
        "metadata": {
            "text": "title was irrelevant",
            "_user": "fef653",
            "_organisation": "o9876",
            "_prompt": "test_prompt",
            "_environment": "production"
        }
    }'
```
{% endtab %}
{% endtabs %}

The **Payload** takes the following keys: `traceID/trace_id, value, weight, metadata`

| Key                    | Required?  | Description                                                                                                                                                                                                                  | Type                                     |
| ---------------------- | ---------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------- |
| `trace_id` / `traceID` | ✅ Required | The trace id on which the feedback will be logged                                                                                                                                                                            | `string`                                 |
| `value`                | ✅ Required | Feedback value                                                                                                                                                                                                               | `integer` between `[-10,10]`             |
| `weight`               | ❔ Optional | Add weight value to feedback value. Helpful if you're collecting multiple feedback for a single trace                                                                                                                        | `float` between `[0,1]`, Default = `1.0` |
| `metadata`             | ❔ Optional | <p>JSON string of any metadata you want to send along with the feedback.<br><br><code>_user</code>, <code>_organisation</code>, <code>_prompt</code> and <code>_environment</code> are special fields indexed by default</p> | `string`                                 |

### **Examples**

A simple & effective feedback from the user is a thumbs up or thumbs down. Just set `value` to `1` for 👍 and -1 for 👎. `Weight` would be default `1.0`.

{% tabs %}
{% tab title="NodeJS" %}
```javascript
portkey.feedback.create({
    traceID: "your trace id",
    value: 1
})
```
{% endtab %}

{% tab title="Python" %}
```python
portkey.feedback.create(
    trace_id = "your trace id",
    value = 1
)
```
{% endtab %}

{% tab title="REST" %}
```bash
curl --location 'https://api.portkey.ai/v1/feedback' \
--header 'x-portkey-api-key: <YOUR PORTKEY API KEY>' \
--header 'Content-Type: application/json' \
--data '{
    'trace_id': 'REQUEST_TRACE_ID',
    'value': 1
}'
```
{% endtab %}
{% endtabs %}

#### **Other Ideas for collecting feedback**

* Business metrics make for great feedback. If you're generating an email, the email being sent out could be a positive feedback metric. The level of editing could indicate the value.
* When a user retries a generation, store the negative feedback since something probably went wrong. Use a lower weight for this feedback since it could be circumstantial.

### **Feedback Analytics**

You can see the `Feedback Count` and `Value: Weight` pairs for each `trace-id` on the logs page. You can also view the feedback details on [Analytics](analytics.md#feedback) and on the Prompt Eval tabs.
