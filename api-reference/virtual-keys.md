---
description: APIs for Virtual Keys.
---

# Virtual Keys

## List All Virtual Keys

<mark style="color:green;">`GET`</mark> `/v1/virtual-keys`

API to get of list all active virtual keys.

**Headers**

| Name              | Value               |
| ----------------- | ------------------- |
| Content-Type      | `application/json`  |
| x-portkey-api-key | `<PORTKEY_API_KEY>` |

**Response**

{% tabs %}
{% tab title="200" %}
```json
{
    "success": true,
    "data": [
        {
            "name": "Key 1 Open AI",
            "note": "Used by Team A",
            "status": "active",
            "created_at": "2024-05-29T01:56:42.000Z",
            "slug": "key-1-open-ai-8876b9",
            "model_config": {},
            "credit_limit": null,
            "provider": "openai"
        },
        {
            "name": "Key 2 Open AI",
            "note": "Used by Team B",
            "status": "active",
            "created_at": "2024-05-29T00:22:20.000Z",
            "slug": "key-2-open-ai-fe8abc",
            "model_config": {},
            "credit_limit": null,
            "provider": "openai"
        }
    ]
}
```
{% endtab %}

{% tab title="401" %}
```json
{
    "success": false,
    "data": {
        "message": "Unauthorised Request"
    }
}
```
{% endtab %}
{% endtabs %}

## Get a Virtual Key

<mark style="color:green;">`GET`</mark> `/v1/virtual-keys/<virtual-key-slug>`

API to get a particular Virtual Key by slug. For security reasons, the key returned in masked.

**Headers**

| Name              | Value               |
| ----------------- | ------------------- |
| Content-Type      | `application/json`  |
| x-portkey-api-key | `<PORTKEY_API_KEY>` |

**Response**

{% tabs %}
{% tab title="200" %}
```json
{
    "success": true,
    "data": {
        "model_config": {},
        "key": "op*****est",
        "name": "Key 1 Open AI",
        "credit_limit": null,
        "status": "active",
        "note": "randomness",
        "created_at": "2024-05-29T01:56:42.000Z",
        "provider": "openai"
    }
}
```
{% endtab %}

{% tab title="401" %}
```json
{
    "success": false,
    "data": {
        "message": "Unauthorised Request"
    }
}
```
{% endtab %}
{% endtabs %}

## Create a Virtual Key

<mark style="color:green;">`POST`</mark> `/v1/virtual-keys`

API to create a new Virtual Key. Returns slug of the new virtual key created on success.

**Headers**

| Name              | Value               |
| ----------------- | ------------------- |
| Content-Type      | `application/json`  |
| x-portkey-api-key | `<PORTKEY_API_KEY>` |

**Body**

| Name       | Type   | Description               |
| ---------- | ------ | ------------------------- |
| `name`     | string | Name of the virtual key   |
| `provider` | string | openai, azure-openai, etc |
| `key`      | string | Actual API Key            |
| `note`     | string | Description (optional)    |

**Response**

{% tabs %}
{% tab title="200" %}
```json
{
    "success": true,
    "data": {
        "slug": "slug_of_new_created_key" //eg: key-1-open-ai-abcd
    }
}
```
{% endtab %}

{% tab title="401" %}
```json
{
    "success": false,
    "data": {
        "message": "Unauthorised Request"
    }
}
```
{% endtab %}
{% endtabs %}

## Update a Virtual Key

<mark style="color:green;">`PUT`</mark> `/v1/virtual-keys/<virtual-key-slug>`

API to update an existing Virtual Key by slug

**Headers**

| Name              | Value               |
| ----------------- | ------------------- |
| Content-Type      | `application/json`  |
| x-portkey-api-key | `<PORTKEY_API_KEY>` |

**Body**

| Name   | Type   | Description             |
| ------ | ------ | ----------------------- |
| `name` | string | Name of the virtual key |
| `key`  | string | Actual API Key          |
| note   | string | Description (optional)  |

**Response**

{% tabs %}
{% tab title="200" %}
```json
{
    "success": true,
    "data": {}
}
```
{% endtab %}

{% tab title="401" %}
```json
{
    "success": false,
    "data": {
        "message": "Unauthorised Request"
    }
}
```
{% endtab %}
{% endtabs %}

## Delete a Virtual Key

<mark style="color:red;">`DELETE`</mark> `/v1/virtual-keys/<virtual-key-slug>`

API to delete an existing Virtual Key by slug.

**Headers**

| Name              | Value               |
| ----------------- | ------------------- |
| Content-Type      | `application/json`  |
| x-portkey-api-key | `<PORTKEY_API_KEY>` |

**Response**

{% tabs %}
{% tab title="200" %}
```json
{
    "success": true,
    "data": {}
}
```
{% endtab %}

{% tab title="401" %}
```json
{
    "success": false,
    "data": {
        "message": "Unauthorised Request"
    }
}
```
{% endtab %}
{% endtabs %}
