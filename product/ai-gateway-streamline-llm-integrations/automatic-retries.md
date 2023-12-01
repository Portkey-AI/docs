# Automatic Retries

Automatically reprocess any unsuccessful API requests **`upto 5`** times. We apply an **`exponential backoff`** strategy, which spaces out retry attempts to prevent network overload.

You can use the "retry" parameter of a [config object](../../api-reference/config-object.md) to enable retries.

```
"retry": {
    "attempts": 5,
    "on_status_codes": [429, 504]
}
```

