# 📃 Custom Metadata

You can send custom metadata along with your API requests in Portkey, which can be later used for auditing or filtering logs. Portkey provides four predefined keys: `_environment`, `_user`, `_organisation`, and `_prompt`. These predefined keys are indexed and allow for filtering data in Portkey analytics and logs sections. You can still pass any other metadata key you desire, but these four predefined keys will be indexed and will be available for filtering data in Portkey.

{% hint style="info" %}
All predefined keys should be of type String, with max-length as 128 characters.
{% endhint %}

### Proxy Metadata

To include metadata in the proxy requests, you can add an `x-portkey-metadata` header with a JSON string containing your metadata. Portkey will parse the JSON object and make it available for filtering.

```
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
**When using the \_user predefined key, the following behavior applies:**

If you pass the `user` key in the OpenAI request body, it will be automatically stored in `_user`. If both the OpenAI request body `user` key and the metadata `_user` key are passed, the metadata `_user` key will be stored.
{% endhint %}

### **🖥️ Portkey Dashboard Guide**

You can filter your logs with the predefined keys (`_environment`, `_user`, `_organisation`, `_prompt)` easily on the Portkey logs.

<figure><img src="../.gitbook/assets/Metadata 1.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/Metadata 2.png" alt=""><figcaption></figcaption></figure>
