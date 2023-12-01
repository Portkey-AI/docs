# Feedback

Feedbacks in Portkey provide a simple way to get weighted feedback from customers on any request you served, at any stage in your app.&#x20;

You can capture this feedback on a generation or conversation level and analyze it based on custom tags by adding meta data to the relevant request.

`POST /feedback`

{% swagger src="../.gitbook/assets/openapi (5).yaml" path="/feedback" method="post" %}
[openapi (5).yaml](<../.gitbook/assets/openapi (5).yaml>)
{% endswagger %}

The Feedback API allows you to gather weighted feedback from users on any generation or conversation at any stage within your app. By incorporating custom metadata, you can tag and analyze feedback more effectively.

## SDK Usage

The `feedback.create` method in the Portkey SDK provides a way to capture user feedback programmatically.

### Method Signature

{% tabs %}
{% tab title="NodeJS" %}
```js
portkey.feedback.create(feedbackParams);
```
{% endtab %}

{% tab title="Python" %}
```py
portkey.feedback.create(feedbackParams);
```
{% endtab %}
{% endtabs %}

#### Parameters

* _feedbackParams (Object)_: Parameters for the feedback request, including `trace_id`, `value`, `weight`, and `metadata`.

### Example Usage

{% tabs %}
{% tab title="NodeJS" %}
```javascript
import Portkey from 'portkey-ai';

// Initialize the Portkey client
const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY"  // Replace with your Portkey API key
});

// Send feedback
const sendFeedback = async () => {
    await portkey.feedback.create({
        trace_id: "REQUEST_TRACE_ID",
        value: 1  // For thumbs up
    });
}
sendFeedback();
```
{% endtab %}

{% tab title="Python" %}
```python
from portkey import Portkey

# Initialize the Portkey client
portkey = Portkey(
    api_key="PORTKEY_API_KEY"  # Replace with your Portkey API key
)

# Send feedback
def send_feedback():
    portkey.feedback.create(
        'trace_id': 'REQUEST_TRACE_ID',
        'value': 0  # For thumbs down
    )

send_feedback()
```
{% endtab %}
{% endtabs %}
