# 🔖Advanced Tagging - Custom Metadata

Advanced tagigng allows you to track each user interaction in your app in high detail. You can tag requests with **predefined** OR **custom metadata** tags and then audit them with the help of the Portkey dashboard. 

## **🔑 Add to Request Header**
To pass metadata, just add the below params to your API request header. Don't forget to change `base URL` or set `API path` to `Portkey proxy`. (Check here for details).

If you are deploying your prompts through Portkey directly (which we recommend), you can add `metadata headers` to those API requests as well.

### **Enabling Predefined + Custom Keys**
```py
'x-portkey-metadata': json.dumps({"_environment": "production", "_user": "a@b.com", "_organisation": "acme", "_prompt": "prompt_1", "foo": "abc", "bar": "def"})
```

### **🎫 Predefined Keys**
Predefined keys are indexed automatically, so you can filter them in Portkey logs directly.

| Key | Usage | Type | Max Length |
| -- | -- | -- | -- |
| `_environment` | Pass environment name. For example, `staging` or `production` | `string` | 128 chars |
| `_user` | Pass the user name or email | `string` | 128 chars |
| `_organisation` | Pass the org name | `string` | 128 chars | 
| `_prompt` | Pass prompt id from your context | `string` | 128 chars | 

### **🛠️ Custom Keys**

Other than the predefined keys, you can also pass **any custom `{'key':'value'}` pair** to your request. All keys & values should be of type `string`. These keys do not have to be prefixed with **`underscore(_)`**.

Custom keys are not indexed on Portkey logs and hence are not searchable. You can export all the logs to perform analysis.


## **💡 Examples**

### **Implementing Predefined + Custom Keys with `Python`**

#### Portkey Header: Remains same across all providers
```py
import json

openai.api_base = "https://api.portkey.ai/v1/proxy"

portkey_header = {
    'x-portkey-api-key' :  'PORTKEY_API_KEY',
    'x-portkey-mode' : 'proxy openai',
    'x-portkey-metadata': json.dumps({"_environment": "production", "_user": "a@b.com", "_organisation": "acme", "_prompt": "prompt_1", "foo": "abc", "bar": "def"})
}
```
#### Provider: OpenAI, Model: GPT3
```py
response = openai.Completion.create( 
    model="text-davinci-003", 
    prompt="What colour do we get if we mix yellow & blue?",
    headers = portkey_headers, #This is where we pass our Portkey headers
    max_tokens=1024
    )

print (response['choices'][0]['text'])
```

### **Implementing Predefined + Custom Keys with `Javascript` & OpenAI GPT3.5**

```javascript
import { Configuration, OpenAIApi } from "openai";
const configuration = new Configuration({
    organization: "YOUR_ORG_ID",
    apiKey: process.env.OPENAI_API_KEY,
    basePath: "https://api.portkey.ai/v1/proxy", // Replace openai with portkey's endpoint
    baseOptions: {
      headers: {
        "x-portkey-api-key": "PORTKEY_API_KEY", // Can be obtained from your account
        "x-portkey-mode": "proxy openai", // Instructs Portkey to proxy your request to OpenAI
        "x-portkey-metadata": JSON.stringify({"_environment": "production", "foo": "abc", "bar": "def"}) //Enables filtering on _environment
      }
    }
});
const openai = new OpenAIApi(configuration);
```

### **Implementing Predefined + Custom Keys with `Langchain Python Package`**
```py
from langchain.llms import OpenAI
import openai
import json

openai.api_base = "https://api.portkey.ai/v1/proxy"

llm = OpenAI(temperature=0.2, headers={
    "x-portkey-api-key": "PORTKEY_API_KEY",
    "x-portkey-mode": "proxy openai",
    "x-portkey-metadata": json.dumps({"_environment": "production", "foo": "abc", "bar": "def"})  # Enables filtering on _environment
})
text = "Create a 5-day trip plan for Laos."
print(llm(text))
```

## **🖥️ Portkey Dashboard Guide**

Simple cache hits will show up as **"HIT"** & semantic cache hits will show up as **"SEMANTIC HIT"** on your Portkey logs. We also calculate and show the response time and how much money you saved with each hit.

![Metadata 1](./images/Metadata%201.png)
![Metadata 2](./images/Metadata%202.png)