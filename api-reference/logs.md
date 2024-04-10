---
layout:
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Logs

{% hint style="success" %}
This API is an Enterprise only API. Please reach out to us at hello@portkey.ai to enable it for your use case
{% endhint %}

## Create a Log

`POST /log`

Stores a log object into the Portkey log store

The log object being stored in Portkey follows a specific format comprising of 3 parts:

1. **`request`** containing the `url`, `headers`, and `body`
2. **`response`** containing the `status`, `headers`, `body` and `time` (response latency)
3. **`metadata`** containing organization, user and request specific information including the `traceId` and `spanId` optionally.

```javascript
// Sample Log Object
{
 *   "request": {
 *     "url": "https://api.someprovider.com/model/generate",
 *     "method": "POST",
 *     "headers": {
 *       "Content-Type": "application/json"
 *     },
 *     "body": {
 *       "prompt": "What is AI?"
 *     }
 *   },
 *   "response": {
 *     "status": 200,
 *     "headers": {
 *       "Content-Type": "application/json"
 *     },
 *     "body": {
 *       "response": "AI stands for Artificial Intelligence..."
 *     },
 *     "time": 123
 *   },
 *   "metadata": {
 *     "organisationId": "org123",
 *     "userId": "usr456",
 *     "model": "text-embedding-ada-002",
 *     "provider": "azure-openai",
 *     "isSuccess": true,
 *     "retryCount": 1,
 *     "cacheStatus": "MISS",
 *     "traceId": "trace123",
 *     "spanId": "span456"
 *   }
 * }
```

### Response Format

The API will respond with a `200` acknowledging that the log has been stored and will be available on your dashboard soon.
