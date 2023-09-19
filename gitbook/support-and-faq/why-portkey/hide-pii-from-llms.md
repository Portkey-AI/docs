# 🚫 Hide PII form LLMs

When working with Large Language Models (LLMs), it's crucial to respect and protect users' Personal Identifiable Information (PII). Often, users may unknowingly or unintentionally input PII into a prompt, which is then sent to the LLM for processing. This information can include names, addresses, contact details, and more. Sending such data directly to the LLMs for processing can present serious privacy and compliance issues.

### The Solution: Portkey's Redaction Engine

Portkey provides an effective and efficient solution to this challenge with its redaction engine. This powerful tool preprocesses requests, identifies potential PII within the prompt string, and redacts it before the request is sent to the LLM.

This way, the information processed by the LLM never contains any personal data, helping ensure user privacy and maintain compliance with data protection regulations.

### How the Redaction Engine Works

The redaction engine employs advanced pattern recognition and machine learning techniques to identify PII within the user's input. It can recognize a variety of PII types, including:

* Names
* Addresses
* Phone numbers
* Email addresses
* Social Security numbers
* Credit card numbers

Once potential PII is identified, the engine replaces it with generic placeholders, effectively "redacting" the sensitive information from the prompt. The redacted prompt is then passed to the LLM for processing, ensuring that no PII ever reaches the LLM.



With Portkey's redaction engine, you can significantly enhance the privacy and security of your generative AI applications. By ensuring that no PII is sent to the LLM, you can maintain user trust, achieve regulatory compliance, and avoid potential data breaches.

{% hint style="info" %}
**Please note:** that PII redaction is currently a custom enterprise features. If you're interested in deploying it, reach out to our support team for a discussion.
{% endhint %}

