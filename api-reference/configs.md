---
description: API for configs
---

# Configs

## List all Configs

<mark style="color:green;">`GET`</mark> `/v1/configs`

API to list all active configs

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
            "id": "61feea09-6e3c-4c08-9867-6c2416639f52",
            "name": "whatever",
            "slug": "pc-whatev-680e74",
            "organisation_id": "079ca61e-69b1-4f2b-8451-abd5848ec759",
            "is_default": 0,
            "status": "active",
            "owner_id": "259df0b0-90de-47da-9893-683c0d836f3e",
            "updated_by": "259df0b0-90de-47da-9893-683c0d836f3e",
            "created_at": "2024-05-24T08:49:16.000Z",
            "last_updated_at": "2024-05-24T08:49:16.000Z"
        },
        {
            "id": "f822b223-f21d-4db6-9520-b135616cf019",
            "name": "Prompt Partials",
            "slug": "pc-prompt-2f3fc2",
            "organisation_id": "079ca61e-69b1-4f2b-8451-abd5848ec759",
            "is_default": 0,
            "status": "active",
            "owner_id": "259df0b0-90de-47da-9893-683c0d836f3e",
            "updated_by": "259df0b0-90de-47da-9893-683c0d836f3e",
            "created_at": "2024-02-27T03:40:05.000Z",
            "last_updated_at": "2024-02-27T03:40:05.000Z"
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

## Get a Config

<mark style="color:green;">`GET`</mark> `/v1/configs/<config-slug>`

API to get a particular config by slug.

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
        "config": {
            "retry": {
                "attempts": 3
            },
            "cache": {
                "mode": "simple"
            }
        }
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

## Create a new Config

<mark style="color:green;">`POST`</mark> `/v1/configs`

API to create a new config.

**Headers**

| Name              | Value               |
| ----------------- | ------------------- |
| Content-Type      | `application/json`  |
| x-portkey-api-key | `<PORTKEY_API_KEY>` |

**Body**

| Name     | Type   | Description                                             |
| -------- | ------ | ------------------------------------------------------- |
| `name`   | string | Name of the config                                      |
| `config` | Object | [Config JSON schema](config-object.md) compliant Object |

**Response**

{% tabs %}
{% tab title="200" %}
```json
{
    "success": true,
    "data": {
        "slug": "slug_of_new_config" // eg: pc-conf-abc"
    }
}
```
{% endtab %}

{% tab title="400" %}
```json
{
  "error": "Invalid request"
}
```
{% endtab %}
{% endtabs %}



## Update a config

<mark style="color:green;">`PUT`</mark> `/v1/configs/<config-slug>`

API to update an existing config by slug.

**Headers**

| Name              | Value               |
| ----------------- | ------------------- |
| Content-Type      | `application/json`  |
| x-portkey-api-key | `<PORTKEY_API_KEY>` |

**Body**

| Name   | Type   | Description                                             |
| ------ | ------ | ------------------------------------------------------- |
| `name` | string | Name of the config                                      |
| `age`  | Object | [Config JSON schema](config-object.md) compliant Object |
| status | string | can be either "active" OR "archived"                    |

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

## Delete a config

<mark style="color:red;">`DELETE`</mark> `/v1/configs/<config-slug>`

\<Description of the endpoint>

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
