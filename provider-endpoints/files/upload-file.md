# Upload File

## Example Usage

{% tabs %}
{% tab title="OpenAI Node" %}
```typescript
import fs from "fs";
import OpenAI from 'openai';
import { PORTKEY_GATEWAY_URL, createHeaders } from 'portkey-ai'

const client = new OpenAI({
  baseURL: PORTKEY_GATEWAY_URL,
  defaultHeaders: createHeaders({
    apiKey: "PORTKEY_API_KEY",
    virtualKey: "PROVIDER_VIRTUAL_KEY"
  })
});

async function main() {
  const file = await client.files.create({
    file: fs.createReadStream("mydata.jsonl"),
    purpose: "batch",
  });

  console.log(file);
}

main();
```
{% endtab %}

{% tab title="OpenAI Python" %}
```python
from openai import OpenAI
from portkey_ai import PORTKEY_GATEWAY_URL, createHeaders

client = OpenAI(
    base_url=PORTKEY_GATEWAY_URL,
    default_headers=createHeaders(
        api_key="PORTKEY_API_KEY",
        virtual_key="PROVIDER_VIRTUAL_KEY"
    )
)

upload = client.files.create(
  file=open("mydata.jsonl", "rb"),
  purpose="batch"
)
```
{% endtab %}

{% tab title="Portkey Node" %}
```typescript
import fs from "fs";
import Portkey from 'portkey-ai';

const client = new Portkey({
  apiKey: 'PORTKEY_API_KEY',
  virtualKey: 'PROVIDER_VIRTUAL_KEY'
});

async function main() {
  const file = await client.files.create({
    file: fs.createReadStream("mydata.jsonl"),
    purpose: "batch",
  });

  console.log(file);
}

main();
```
{% endtab %}

{% tab title="Portkey Python" %}
```python
from portkey_ai import Portkey

client = Portkey(
  api_key = "PORTKEY_API_KEY",
  virtual_key = "PROVIDER_VIRTUAL_KEY"
)

upload = client.files.create(
  file=open("mydata.jsonl", "rb"),
  purpose="batch"
)
```
{% endtab %}

{% tab title="CURL" %}
```bash
curl https://api.portkey.ai/v1/files \
  -H "x-portkey-api-key: $PORTKEY_API_KEY" \
  -H "x-portkey-virtual-key: $PORTKEY_PROVIDER_VIRTUAL_KEY" \
  -F purpose="fine-tune" \
  -F file="@mydata.jsonl"
```
{% endtab %}
{% endtabs %}

{% swagger src="https://raw.githubusercontent.com/Portkey-AI/openapi/master/openapi.yaml" path="/files" method="post" %}
[https://raw.githubusercontent.com/Portkey-AI/openapi/master/openapi.yaml](https://raw.githubusercontent.com/Portkey-AI/openapi/master/openapi.yaml)
{% endswagger %}
