# Config Object

The `config` object is used to configure API interactions with various providers. It supports multiple modes such as single provider access, load balancing between providers, and fallback strategies.

**Current version:** `2.0`

Here are some sample config objects:

```json
// Simple config with cache and retry
{
  "version": "2.0", // Required*
  "virtualKey": "***", // Your Virtual Key
  "cache": { // Optional
    "mode": "semantic",
    "maxAge": 10000
  },
  "retry": { // Optional
    "count": 5,
    "onStatusCodes": []
  }
}

// Load balancing with 2 OpenAI keys
{
  "version": "2.0",
  "mode": "loadbalance",
  "targets": [
    {
      "provider": "openai",
      "apiKey": "sk-***"
    },
    {
      "provider": "openai",
      "apiKey": "sk-***"
    }
  ]
}
```

You can find more examples of schemas [below](config-object.md#examples).

## Schema Details

| Key Name        | Description                      | Type             | Required                               | Enum Values                                                 | Additional Info                     |
| --------------- | -------------------------------- | ---------------- | -------------------------------------- | ----------------------------------------------------------- | ----------------------------------- |
| \*`version`     | Version of the config schema     | string           | Yes                                    | -                                                           | -                                   |
| `mode`          | Operational mode of the config   | string           | Yes (if no `provider` or `virtualKey`) | "single", "loadbalance", "fallback"                         | -                                   |
| `provider`      | Name of the service provider     | string           | Yes (if no `mode` or `virtualKey`)     | "openai", "anthropic", "azure-openai", "anyscale", "cohere" | -                                   |
| `apiKey`        | API key for the service provider | string           | Yes (if `provider` is specified)       | -                                                           | -                                   |
| `virtualKey`    | Virtual key identifier           | string           | Yes (if no `mode` or `provider`)       | -                                                           | -                                   |
| `cache`         | Caching configuration            | object           | No                                     | -                                                           | See Cache Object Details            |
| `retry`         | Retry configuration              | object           | No                                     | -                                                           | See Retry Object Details            |
| `weight`        | Weight for load balancing        | number           | No                                     | -                                                           | Used in `loadbalance` mode          |
| `onStatusCodes` | Status codes triggering fallback | array of strings | No                                     | -                                                           | Used in `fallback` mode             |
| `targets`       | List of target configurations    | array            | Yes (if `mode` is specified)           | -                                                           | Each item follows the config schema |

### Cache Object Details

| Key Name | Description                   | Type    | Required | Enum Values          | Additional Info |
| -------- | ----------------------------- | ------- | -------- | -------------------- | --------------- |
| `mode`   | Cache mode                    | string  | Yes      | "simple", "semantic" | -               |
| `maxAge` | Maximum age for cache entries | integer | No       | -                    | Optional        |

### Retry Object Details

| Key Name        | Description                     | Type             | Required | Enum Values | Additional Info |
| --------------- | ------------------------------- | ---------------- | -------- | ----------- | --------------- |
| `count`         | Number of retry attempts        | integer          | Yes      | -           | -               |
| `onStatusCodes` | Status codes to trigger retries | array of strings | No       | -           | Optional        |

### Notes

* The `mode` key determines the operational mode of the config. If `mode` is not specified, a single provider mode is assumed, requiring either `provider` and `apiKey` or `virtualKey`.
* In `loadbalance` and `fallback` modes, the `targets` array specifies the configurations for each target.
* The `cache` and `retry` objects provide additional configurations for caching and retry policies, respectively.

## Examples

<details>

<summary>Single Provider with API Key</summary>

```json
{
  "version": "1.0",
  "provider": "openai",
  "apiKey": "sk-***"
}
```

</details>

<details>

<summary>Single Provider with Virtual Key</summary>

```json
{
  "version": "1.0",
  "virtualKey": "***"
}
```

</details>

<details>

<summary>Single Provider with Virtual Key, Cache and Retry</summary>

```json
{
  "version": "1.0",
  "virtualKey": "***",
  "cache": {
    "mode": "semantic",
    "maxAge": 10000
  },
  "retry": {
    "count": 5,
    "onStatusCodes": ["429"]
  }
}
```

</details>

<details>

<summary>Load Balancing with Two OpenAI API Keys</summary>

```json
{
  "version": "2.0",
  "mode": "loadbalance",
  "targets": [
    {
      "provider": "openai",
      "apiKey": "sk-***"
    },
    {
      "provider": "openai",
      "apiKey": "sk-***"
    }
  ]
}
```

</details>

<details>

<summary>Load Balancing and Fallback Combination</summary>

```json
{
  "version": "2.0",
  "mode": "loadbalance",
  "targets": [
    {
      "provider": "openai",
      "apiKey": "sk-***"
    },
    {
      "mode": "fallback",
      "onStatusCodes": ["429", "241"],
      "targets": [
        {
          "virtualKey": "***"
        },
        {
          "virtualKey": "***"
        }
      ]
    }
  ]
}
```

</details>

These examples demonstrate different use cases of the configuration object, helping users to understand how they can structure their own configurations based on the schema. Feel free to add more examples or adjust these as necessary to match your specific use cases and requirements.

## JSON Schema

The following JSON schema is used to validate the config object

<details>

<summary>JSON Schema</summary>

```json
{
    "$schema": "http://json-schema.org/draft-07/schema#",
    "type": "object",
    "properties": {
      "strategy": {
        "type": "object",
        "properties": {
            "mode": {
                "type": "string",
                "enum": ["single", "loadbalance", "fallback"]
            },
            "on_status_codes": {
                "type": "array",
                "items": {"type": "integer"},
                "optional": true
            }
        }
      },
      "provider": {
        "type": "string",
        "enum": ["openai", "anthropic", "azure-openai", "anyscale", "cohere"]
      },
      "override_params": {
        "type": "object"
      },
      "api_key": {
        "type": "string"
      },
      "virtual_key": {
        "type": "string"
      },
      "cache": {
        "type": "object",
        "properties": {
          "mode": {
            "type": "string",
            "enum": ["simple", "semantic"]
          },
          "max_age": {
            "type": "integer",
            "optional": true
          }
        },
        "required": ["mode"]
      },
      "retry": {
        "type": "object",
        "properties": {
          "attempts": {
            "type": "integer"
          },
          "on_status_codes": {
            "type": "array",
            "items": {"type": "string"},
            "optional": true
          }
        },
        "required": ["attempts"]
      },
      "weight": {
        "type": "number"
      },
      "on_status_codes": {
        "type": "array",
        "items": {"type": "integer"}
      },
      "targets": {
        "type": "array",
        "items": {
          "$ref": "#"
        }
      }
    },
    "oneOf": [
      {
        "required": ["provider", "api_key"]
      },
      {
        "required": ["virtual_key"]
      },
      {
        "required": ["strategy", "targets"]
      }
    ],
    "additionalProperties": false
  }
```

</details>

