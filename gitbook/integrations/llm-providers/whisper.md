# ➡ Whisper

Portkey makes logging Whisper requests easy and effortless with its integration.&#x20;

## Node SDK

Integrate Portkey in OpenAI's Node SDK in just two steps:

**Step 1**: In openai configuration, set `baseURL` to Portkey proxy (`https://api.portkey.ai/v1/proxy`)

**Step 2**: Add Portkey API key & mode in `defaultHeaders`.

Here's how:

{% tabs %}
{% tab title="Node SDK" %}
```javascript
import fs from "fs";
import OpenAI from "openai";

const configuration = {
  apiKey: '<OPENAI_API_KEY>',

  // Portkey specific additions
  baseURL: 'https://api.portkey.ai/v1/proxy', // Change base URL to Portkey
  defaultHeaders: {
    'x-portkey-api-key': '<PORTKEY_API_KEY>', // Add Portkey API key
    'x-portkey-mode': 'proxy openai' // Setting mode to use OpenAI via Portkey
  }
};

const openai = new OpenAI(configuration);

async function main() {
  const transcription = await openai.audio.transcriptions.create({
    file: fs.createReadStream("audio.mp3"),
    model: "whisper-1",
  });

  console.log(transcription.text);
}
main();
```
{% endtab %}
{% endtabs %}

## Rest API

Portkey supports Whisper endpoints with HTTP-based requests as well:

#### Step 1: **Set Up the Request URL**

* Base Portkey URL: `https://api.portkey.ai/v1/proxy`
* Whisper Endpoint: `/audio/transcriptions`&#x20;

Concatenate the Base Portkey URL with the Whisper Endpoint:

```bash
https://api.portkey.ai/v1/proxy/audio/transcriptions
```

#### Step 2: **Configure Portkey Headers**

Add the following default Portkey headers to your request:

`x-portkey-api-key` :  Your Portkey API Key

`x-portkey-mode` : `proxy openai` (For Whisper integration)

#### Example:

{% tabs %}
{% tab title="cURL" %}
```sh
# Set the API
OPENAI_API_KEY="<OPENAI_API_KEY>"
PORTKEY_API_KEY="<PORTKEY_API_KEY>"

# Define Portkey Headers. 
# These can be customized to enable different features
PORTKEY_HEADERS=(
-H "x-portkey-api-key: $PORTKEY_API_KEY"
-H "x-portkey-mode: proxy openai"
-H "x-portkey-trace-id: whisper_portkey"
)

# Make a curl request to Portkey and append the Whisper endpoint at the end
curl https://api.portkey.ai/v1/proxy/audio/transcriptions \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -H "Content-Type: multipart/form-data" \
  "${PORTKEY_HEADERS[@]}" \ # Include the defined Portkey headers
  -F file="@./audio.mp3" \
  -F model="whisper-1"
```
{% endtab %}
{% endtabs %}

## Additional Features

Refer to the [features.md](../../introduction/features.md "mention") to leverage advanced Portkey production features such as semantic caching, custom metadata, retries, and more.
