# Common Errors & Resolutions

Since Portkey functions as a gateway - you may encounter Portkey-related, as well as non-Portkey related erros while using our services.

### Identifying the error source

1. Errors exclusively originating from Portkey are **prefixed with "Portkey Error".**
2. While errors originating from your LLM providers (or frameworks) are **returned as they are**, without any transformations.

### How to verify if it's Portkey error

You can quickly verify if the problem is originating from Portkey by **running the same request without Portkey**. If it executes successfully, then it's likely that the error is with Portkey or its integration.

### Common Portkey Errors

1. **Errors related to Missing Mandatory Headers**: This is a common error where certain mandatory headers might be missing from the request. Make sure that all the necessary headers as specified in the respective feature documentation are included in your requests.
2. **Errors related to Invalid Header Values**: At times, an incorrect or unsupported value might be passed in a header, causing this error. Cross-check the values provided against the allowed ones mentioned in our documentation.

