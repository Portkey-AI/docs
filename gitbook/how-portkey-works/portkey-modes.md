# 📪 Portkey Modes

Portkey's goal is to simplify and enhance your experience when deploying and managing Generative AI applications. It can be integrated into your applications in two distinctive ways, each designed to cater to your specific needs:

1.  **Middleware Mode**: This mode is ideal for clients who want to maintain their existing LLM setup, while adding the robust capabilities that Portkey provides. Here's how it works:

    * The client modifies their LLM base path to route all requests via Portkey.
    * Portkey acts as a reverse proxy, a mediator that intercepts the client's requests.
    * Once a request is received, Portkey forwards it to the original LLM URL, becoming a 'hop' in the process.
    * Throughout this process, Portkey offers its extensive suite of features such as request tracing, caching, automatic retries, custom metadata, load balancing, and observability.
    * The responses from the LLM are then passed back to the client through Portkey, maintaining the integrity and transparency of the process.


2. **Managed Models Mode**: This mode is for customers who prefer a hands-off approach, allowing Portkey to handle the intricacies of managing the LLMs:
   * Customers define and save their LLM prompts and all the associated parameters on Portkey.
   * Portkey then creates an API endpoint specific to this model.
   * Users can directly call this API, which handles all the interactions with the LLM behind the scenes.
   * This model not only simplifies the process of working with LLMs but also ensures that the customers can leverage all the features of Portkey without worrying about the complexities of directly handling the LLM.

These integration methods have been designed to offer flexibility and ease of use, ensuring that you can make the most of your Generative AI applications with minimal hassle.
