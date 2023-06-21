# Feedback APIs

When evaluating an LLM system, an important metric to record is real human feedback coming in. In your application you can collect feedback with a simple thumbs up / thumbs down or go deeper with weighted feedback on actions that the user takes.

Portkey allows you to capture this feedback on a generation or conversation level. The feedback can then be analysed on a prompt, user or environment level.

Feedbacks in Portkey are collected on a `trace-id` which can be configured by you.

You can collect feedback using the `/feedback` endpoint. Here's a cURL request to add feedback.

```
curl --location 'https://api.portkey.ai/v1/feedback/' \
--header 'x-portkey-api-key: <YOUR PORTKEY API KEY>' \
--header 'Content-Type: application/json' \
--data '{
    "trace_id": "insert_trace_id_here",
    "value": 1,
    "weight": 1
}'
```
**URL**: `https://api.portkey.ai/v1/feedback/`
**Method**: `POST`
**Request Body**<br>
The request body should contain a `Feedback object` or an array of `Feedback objects`


| Key | Description | Options |
|---|---|---|
| trace_id | The trace_id on which the feedback will be logged. This is usually sent as a header along with requests to Portkey. | string, required |
| value | A numeric value for the feedback. Can be any integer value between -10 and 10 | integer, required |
| weight | Optional parameter to determine the weight of this feedback's value. Especially helpful if you're collecting multiple feedback for a single trace_id. This is a floating point number between 0 and 1 | float, optional, defaults to 1.0 |
| text | Free form text that can be attached to a feedback object. Helpful when users leave comments. | string, optional |

<br>

### Examples

#### Implementing a 👍🏻 / 👎🏻 feedback

In many UI implementations, you would request a user to rate a generation with a simple thumbs up or thumbs down. You can use the `/feedback` endpoint to capture this user intent along with the generation's trace-id so you can aggregate the data at your end.

You can make an API call as the user clicks a thumbs up button like this

```sh
curl --location 'https://api.portkey.ai/v1/feedback/' \
--header 'x-portkey-api-key: <YOUR PORTKEY API KEY>' \
--header 'Content-Type: application/json' \
--data '{
    "trace_id": "trace_id_for_the_generation",
    "value": 1
}'
```
- Thumbs up can have a `value` of 1 and thumbs down can have a value of of -1. We're not adding any weights since both the values should be weighted equally.

- If the user also decides to leave a comment with their selection, you can add the `text` parameter to capture the user's comment.
