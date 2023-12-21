# 📃 Custom Metadata

You can send custom metadata along with your API requests in Portkey, which can be later used for auditing or filtering logs. You can pass any number of keys, all values should be of type with max-length as 128 characters.

### Proxy Metadata

To include metadata in the proxy requests, you can add an `x-portkey-metadata` header with a JSON string containing your metadata. Portkey will parse the JSON object and make it available for filtering.

```
{    
    "x-portkey-metadata": JSON.stringify({
        "_user": "userid123",
        "environment": "production",
        "organisation": "orgid123",
        "prompt": "summarisationPrompt",
        "foo": "abc",
        "bar": "def"
    })
}
```

{% hint style="info" %}
**\_user is a special key**

This key is used to provide user analytics in the analytics section.\
\
If you pass the `user` key in the OpenAI request body, it will be automatically stored in `_user`. If both the OpenAI request body `user` key and the metadata `_user` key are passed, the metadata `_user` key will be stored.
{% endhint %}

### **🖥️ Portkey Dashboard Guide**

You can filter your logs with the predefined keys (`_environment`, `_user`, `_organisation`, `_prompt)` easily on the Portkey logs.

<figure><img src="../../.gitbook/assets/Metadata 1.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/Metadata 2.png" alt=""><figcaption></figcaption></figure>
