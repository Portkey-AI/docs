# ➡ Whisper

Portkey makes logging Whisper requests absolutely easy. Whisper endpoints are supported with HTTP-based requests. Follow the steps below to integrate:

### Step 1: **Set Up the Request URL**

* Base Portkey URL: `https://api.portkey.ai/v1/proxy`
* Whisper Endpoint: `/audio/transcriptions`&#x20;

Concatenate the Base Portkey URL with the Whisper Endpoint:

```bash
https://api.portkey.ai/v1/proxy/audio/transcriptions
```

### Step 2: **Configure Portkey Headers**

Add the following default Portkey headers to your request:

`x-portkey-api-key` :  Your Portkey API Key

`x-portkey-mode` : `proxy openai` (For Whisper integration)

#### Additional Features

Refer to the [features.md](../../introduction/features.md "mention") to leverage advanced Portkey production features such as semantic caching, custom metadata, retries, and more.

### Code Examples

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

{% tab title="Python" %}
```python
import requests

# Set the API keys
OPENAI_API_KEY = "<OPENAI_API_KEY>"
PORTKEY_API_KEY = "<PORTKEY_API_KEY>"

# Set up the URL
url = "https://api.portkey.ai/v1/proxy/audio/transcriptions"

# Define Headers including Portkey headers
headers = {
    "Authorization": f"Bearer {OPENAI_API_KEY}",
    "x-portkey-api-key": PORTKEY_API_KEY,
    "x-portkey-mode": "proxy openai",
    "x-portkey-trace-id": "whisper_portkey",
}

# Set up the file and model data
file_path = "./audio.mp3"
model = "whisper-1"

# Making a POST request to Portkey
with open(file_path, 'rb') as f:
    response = requests.post(url, headers=headers, files={"file": f}, data={"model": model})

# Handle the response
if response.status_code == 200:
    print("Request was successful!")
    print(response.json())
else:
    print(f"Failed with status code: {response.status_code}")
    print(response.text)
```
{% endtab %}

{% tab title="Node" %}
```javascript
const axios = require('axios');
const fs = require('fs');
const FormData = require('form-data');

// Set the API keys
const OPENAI_API_KEY = "<OPENAI_API_KEY>";
const PORTKEY_API_KEY = "<PORTKEY_API_KEY>";

// Set up the URL
const url = "https://api.portkey.ai/v1/proxy/audio/transcriptions";

// Define Portkey Headers
const headers = {
    Authorization: `Bearer ${OPENAI_API_KEY}`,
    "x-portkey-api-key": PORTKEY_API_KEY,
    "x-portkey-mode": "proxy openai",
    "x-portkey-trace-id": "whisper_portkey",
};

// Create a new FormData object
let form = new FormData();

// Append the file and model to the form object
form.append('file', fs.createReadStream('./audio.mp3'));
form.append('model', 'whisper-1');

// Make the request
axios.post(url, form, { headers: { ...form.getHeaders(), ...headers } })
  .then(response => console.log(response.data))
  .catch(error => {
    console.error(`Failed with status code: ${error.response.status}`);
    console.error(error.response.data);
  });
```
{% endtab %}
{% endtabs %}
