# Few-Shot Prompting

LLMs are highly capable of following a given structure. By providing a few examples of how the assistant should respond to a given prompt, the LLM can generate responses that closely follow the format of these examples.

Portkey enhances this capability with the _**raw prompt**_ feature of prompt templates. You can easily add few-shot learning examples to your templates with _raw prompt_ and dynamically update them whenever you want, without needing to modify the prompt templates!

### How does it work?

Let's consider a use case where, given a candidate profile and a job description, the LLM is expected to output candidate notes in a specific JSON format.

#### This is how our raw prompt looks:

{% code overflow="wrap" fullWidth="false" %}
```json
[
    { 
        "role": "system", 
        "message": "You output candidate notes in JSON format when given a candidate profile and a job description.",
    },
    {{few_shot_examples}},
    {
        "role": "user",
        "message": "Candidate Profile: {{profile}} \n Job Description: {{jd}}"
    },
]
```
{% endcode %}

#### Let's define our variables:

As you can see, we have added variables `few_shot_examples`, `profile`, and `jd` in the above examples.

```
profile = "An experienced data scientist with a PhD in Computer Science and 5 years of experience working with machine learning models in the healthcare industry."
jd = "We are seeking a seasoned data scientist with a strong background in machine learning, ideally with experience in the healthcare sector. The ideal candidate should have a PhD or Master's degree in a relevant field and a minimum of 5 years of industry experience."
```

#### And now let's add some examples with the expected JSON structure:

```
few_shot_examples = 
[
 {
  "role": "user",
  "content": "Candidate Profile: Experienced software engineer with a background in developing scalable web applications using Python. Job Description: We’re looking for a Python developer to help us build and scale our web platform.",
 },
 {
  "role": "assistant",
  "content": "{'one-line-intro': 'Experienced Python developer with a track record of building scalable web applications.', 'move-forward': 'Yes', 'priority': 'P1', 'pros': '1. Relevant experience in Python. 2. Has built and scaled web applications. 3. Likely to fit well with the job requirements.', 'cons': 'None apparent from the provided profile.'}",
 },
 { 
  "role": "user",
  "content": "Candidate Profile: Recent graduate with a degree in computer science and a focus on data analysis. Job Description: Seeking a seasoned data scientist to analyze large data sets and derive insights."
 },
 {
  "role": "assistant",
  "content": "{'one-line-intro': 'Recent computer science graduate with a focus on data analysis.', 'move-forward': 'Maybe', 'priority': 'P2', 'pros': '1. Has a strong educational background in computer science. 2. Specialized focus on data analysis.', 'cons': '1. Lack of professional experience. 2. Job requires a seasoned data scientist.' }"
  }
]
```

In this configuration, `{{few_shot_examples}}` is a placeholder for the few-shot learning examples, which are dynamically provided and can be updated as needed. This allows the LLM to adapt its responses to the provided examples, facilitating versatile and context-aware outputs.

### Putting it all together in Portkey's prompt manager:

1. Go to the "Prompts" page on [https://app.portkey.ai/](https://app.portkey.ai/organisation/4e501cb0-512d-4dd3-b480-8b6af7ef4993/9eec4ebc-1c88-41a2-ae5d-ed0610d33b06/collection/17b7d29e-4318-4b4b-a45b-1d5a70ed1e8f) and **Create** a new Prompt template with your preferred AI provider.&#x20;
2. Selecting Chat mode will enable the Raw Prompt feature:

<div align="left">

<figure><img src="../../.gitbook/assets/image (3) (1) (1).png" alt="" width="296"><figcaption></figcaption></figure>

</div>

3. Click on it and paste the [raw prompt code from above](few-shot-prompting.md#this-is-how-our-raw-prompt-would-look). And that's it! You have your **dynamically updatable** few shot prompt template ready to deploy.

### Deploying the Prompt with Portkey

Deploying your prompt template to an API is extremely easy with Portkey. You can use our [Prompt Completions API](../../api-reference/prompts/prompt-completion.md) to use the prompt we created.

{% tabs %}
{% tab title="Python" %}
<pre class="language-python"><code class="lang-python">from portkey_ai import Portkey

client = Portkey(
    api_key="PORTKEY_API_KEY",  # defaults to os.environ.get("PORTKEY_API_KEY")
)

<strong>prompt_completion = client.prompts.completions.create(
</strong><strong>    prompt_id="Your Prompt ID", # Add the prompt ID we just created
</strong><strong>    variables={
</strong><strong>       few_shot_examples: fseObj,
</strong><strong>       profile: "",
</strong><strong>       jd: ""
</strong>    }
)

print(prompt_completion)
</code></pre>

```python
# We can also override the hyperparameters
prompt_completion = client.prompts.completions.create(
    prompt_id="Your Prompt ID", # Add the prompt ID we just created
    variables={
       few_shot_examples: fseObj,
       profile: "",
       jd: ""
    }
    max_tokens=250,
    presence_penalty=0.2
)
print(prompt_completion)
```
{% endtab %}

{% tab title="NodeJS" %}
<pre class="language-javascript"><code class="lang-javascript">import Portkey from 'portkey-ai'

const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY",
})

// Make the prompt creation call with the variables
<strong>const promptCompletion = await portkey.prompts.completions.create({
</strong><strong>    promptID: "Your Prompt ID",
</strong><strong>    variables: {
</strong><strong>       few_shot_examples: fseObj,
</strong><strong>       profile: "",
</strong><strong>       jd: ""
</strong>    }
})
</code></pre>

```javascript
// We can also override the hyperparameters
const promptCompletion = await portkey.prompts.completions.create({
    promptID: "Your Prompt ID",
    variables: {
       few_shot_examples: fseObj,
       profile: "",
       jd: ""
    },
    max_tokens: 250,
    presence_penalty: 0.2
})
```
{% endtab %}

{% tab title="REST" %}
<pre class="language-bash"><code class="lang-bash">curl -X POST "https://api.portkey.ai/v1/prompts/:PROMPT_ID/completions" \
-H "Content-Type: application/json" \
-H "x-portkey-api-key: $PORTKEY_API_KEY" \
<strong>-d '{
</strong><strong>       "variables": {
</strong><strong>              few_shot_examples: fseObj,
</strong><strong>              profile: "",
</strong><strong>              jd: ""
</strong><strong>    },
</strong>    "max_tokens": 250, # Optional
    "presence_penalty": 0.2 # Optional
}'
</code></pre>
{% endtab %}
{% endtabs %}

You can pass your dynamic few shot learning examples with the `few_shot_examples` variable, and start using the prompt template in production!

### Detailed Guide on Few-Shot Prompting

We recommend [this guide](https://www.promptingguide.ai/techniques/fewshot) detailing the research as well as edge cases for few-shot prompting.&#x20;

### Support

Facing an issue? Reach out on support@portkey.ai for a quick resolution.

