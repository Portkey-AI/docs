# Metadata

You can send custom metadata along with your API requests in Portkey, which can be later used for auditing or filtering logs. Portkey provides four predefined keys:&#x20;

* `_environment`,&#x20;
* `_user`,
* `_organisation`,&#x20;
* and `_prompt`.&#x20;

These predefined keys are indexed and allow for filtering data in Portkey analytics and logs sections. You can still pass any other metadata key you desire, but these four predefined keys will be indexed and will be available for filtering data in Portkey.

{% hint style="info" %}
All predefined keys should be of type String, with max-length as 128 characters.
{% endhint %}

### Gateway Metadata

To include metadata in your requests, you can add an `x-portkey-metadata` header with a JSON string containing your metadata. Portkey will parse the JSON object and make it available for filtering.

```json
{    
    "x-portkey-metadata": JSON.stringify({
        "_environment": "production",
        "_user": "userid123",
        "_organisation": "orgid123",
        "_prompt": "summarisationPrompt",
        "foo": "abc",
        "bar": "def"
    })
}
```

{% hint style="info" %}
**When using the \_user predefined key, the following behaviour applies:**

If you pass the `user` key in the OpenAI request body, it will be automatically stored in `_user`. If both the OpenAI request body `user` key and the metadata `_user` key are passed, the metadata `_user` key will be stored.
{% endhint %}

You can also pass it as request config parameter when using the Portkey or OpenAI SDKs.

{% tabs %}
{% tab title="NodeJS" %}
```javascript
const requestOptions = { metadata: {"_user": "432erf6"} }
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
    metadata = {"_user": "39e2a60c-b47c-45d8"}
).chat.completions.create(
    messages = [{ "role": 'user', "content": 'Say this is a test' }],
    model = 'gpt-3.5-turbo'
})
```
{% endtab %}

{% tab title="OpenAI NodeJS" %}
<pre class="language-javascript"><code class="lang-javascript">const reqHeaders = {headers: <a data-footnote-ref href="#user-content-fn-1">createHeaders</a>({metadata: {"_user": "432erf6"}})}
const chatCompletion = await openai.chat.completions.create({
    messages: [{ role: 'user', content: 'Say this is a test' }],
    model: 'gpt-3.5-turbo',
}, reqHeaders);
</code></pre>
{% endtab %}

{% tab title="OpenAI Python" %}
```python
req_headers = createHeaders(metadata = {"_user": "39e2a60c-b47c-45d8"})
chat_complete = client.with_options(headers=req_headers).chat.completions.create(
    model="gpt-4",
    messages=[{"role": "user", "content": "Say this is a test"}],
)
```
{% endtab %}
{% endtabs %}

### **🖥️ Portkey Dashboard Guide**

You can filter your logs with the predefined keys (`_environment`, `_user`, `_organisation`, `_prompt)` easily on the the analytics and logs dashboards.

[^1]: Imported from the Portkey SDK
