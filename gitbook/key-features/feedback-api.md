# 📝 Feedback API

Feedback API provides a simple way to get weighted feedback from customers on any request you served, at any stage in your app. You can capture this feedback on a generation or conversation level and analyze it based on custom tags by adding meta data to the relevant request.

### **🔑 Two Steps to Add Feedback**

#### **1️⃣ Adding trace-id**

Pass the following param in your request header. Add any string for the `trace_id` you would like. We will append feedback to all requests with the same trace-id.

```sh
"x-portkey-trace-id": "<YOUR TRACE ID>"
```

#### **2️⃣ Passing feedback**

You can append feedback to a request with the `/feedback` endpoint like this:

```sh
curl --location 'https://api.portkey.ai/v1/feedback' \
--header 'x-portkey-api-key: <YOUR PORTKEY API KEY>' \
--header 'Content-Type: application/json' \
--data '{
    "trace_id": "insert_trace_id_here",
    "value": -10,
    "weight": 0.5,
    "metadata": '{"text" : "title was irrelevant", "_user": "fef653", "_organisation": "o9876", "_prompt": "test_prompt", "_environment": "production"}'
}'
```

The **Payload** takes the following keys: `trace_id, value, weight, metadata`

| Key        | Required?  | Description                                                                                                                                                                                                                  | Type                                     |
| ---------- | ---------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------- |
| `trace_id` | ✅ Required | The trace\_id on which the feedback will be logged                                                                                                                                                                           | `string`                                 |
| `value`    | ✅ Required | Feedback value                                                                                                                                                                                                               | `integer` between `[-10,10]`             |
| `weight`   | ❔ Optional | Add weight value to feedback value. Helpful if you're collecting multiple feedback for a single `trace_id`                                                                                                                   | `float` between `[0,1]`, Default = `1.0` |
| `metadata` | ❔ Optional | <p>JSON string of any metadata you want to send along with the feedback.<br><br><code>_user</code>, <code>_organisation</code>, <code>_prompt</code> and <code>_environment</code> are special fields indexed by default</p> | `string`                                 |

### **💡 Examples**

One simple & effective feedback you can get from the user is a simple thumbs up or thumbs down. Just set `value` to `1` for 👍 and `0` for 👎. `Weight` would be default `1.0`.

#### **Implementing 👍🏻 / 👎🏻 Feedback with `cURL`**

```sh
curl --location 'https://api.portkey.ai/v1/feedback' \
--header 'x-portkey-api-key: <YOUR PORTKEY API KEY>' \
--header 'Content-Type: application/json' \
--data '{
    'trace_id': 'REQUEST_TRACE_ID',
    'value': 1
}'
```

#### **Implementing 👍🏻 / 👎🏻 Feedback with `Python`**

```py
import requests

portkey_feedback_header = {
    'x-portkey-api-key' : 'PORTKEY_API_KEY',
    'Content-Type': 'application/json',
}

feedback_data = {
    'trace_id': 'REQUEST_TRACE_ID',
    'value': 0, #For thumbs down
}

response = requests.post('https://api.portkey.ai/v1/feedback/', headers=portkey_feedback_header, json=feedback_data)
```

### **🖥️ Portkey Dashboard Guide**

You can see the `Feedback Count` and `Value: Weight` pairs for each `trace-id` on the logs page.

<figure><img src="../.gitbook/assets/Feedback 1.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/Feedback 2.png" alt=""><figcaption></figcaption></figure>
