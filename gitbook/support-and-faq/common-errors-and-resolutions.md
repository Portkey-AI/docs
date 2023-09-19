# ⁉ Common Errors and Resolutions

While using Portkey, you may encounter various types of errors. An essential part of troubleshooting is to accurately identify the source of the error.

### Identifying Source of Error

Errors can originate either from the Portkey platform itself or from the Language Learning Models (LLMs) provider. **To make this distinction, remember that any error originating from Portkey is prefixed with "Portkey Error:".** If you encounter an error message without this prefix, it is likely that the error has occurred at the LLM provider's end.

### Troubleshooting Tip for Middleware Mode

If you are facing issues specifically when using the middleware mode, a good troubleshooting step would be to try running the same request without Portkey. If the request executes successfully without Portkey, then the error is likely due to the integration with Portkey.

### Common Portkey Errors

While the nature of errors could be quite diverse, we'll focus on a couple of common errors that users encounter:

1. **Errors related to Missing Mandatory Headers**: This is a common error where certain mandatory headers might be missing from the request. Make sure that all the necessary headers as specified in the respective feature documentation are included in your requests.
2. **Errors related to Invalid Header Values**: At times, an incorrect or unsupported value might be passed in a header, causing this error. Cross-check the values provided against the allowed ones mentioned in our documentation.
