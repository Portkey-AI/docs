# 🔧 Continuous Fine-Tuning \[Beta]

With Portkey, it becomes very easy to 1️) Curate & create data for fine-tuning, 2) Schedule fine-tuning jobs on OpenAI or Anyscale, and 3) Use the fine-tuned models!

## Fine-Tuning Process

1. **Initialization:** Set up your task with a name, description, AI provider key, model, and epoch count.
2. **Training Data:** Use logs from Portkey or upload a JSONL file for fine-tuning. View essential details like cost per epoch, and more.
3. **Validation Data (Optional):** Add optional validation data using Portkey logs or JSONL upload.
4. **Review and Execution:** Review the final cost estimates, and schedule the fine-tuning job! Use the fine-tuned model with Portkey playground or replace existing models in your code with the fine-tuned models.

## Supported Models

<table><thead><tr><th>Model</th><th>Provider</th><th data-hidden>Model</th></tr></thead><tbody><tr><td><ul><li>gpt-3.5-turbo</li><li>gpt-4 (<code>if you have access</code>)</li></ul></td><td>OpenAI</td><td><ul><li>gpt-3.5-turbo</li><li>gpt-4 (if your key has access)</li></ul></td></tr><tr><td><ul><li>Llama2-7b</li><li>Llama2-13b</li><li>Llama2-70b</li></ul></td><td>Anyscale</td><td><ul><li>Llama-2-7b</li><li>Llama-2-13b</li></ul></td></tr></tbody></table>

## Join the Beta

This feature is under development, but we are opening it up to select orgs that are excited to try it out. To get access, please [**join the Portkey Discord**](https://discord.gg/F5RRVKvxmN) and send a **"Hi"** to anyone on **Team Portkey**, you'll be invited to the private channel for the beta.

## **Upcoming Enhancements**

1. Better cost calculations
2. Simplified Portkey slugs instead of long OpenAI model slugs
3. Auto split logs into training & validation
4. Loss charts while training
5. Fetch fine-tuned models that are not on Portkey, and let you further fine-tune them
