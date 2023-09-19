# ⛅ Improve LLM success rate

Working with Large Language Models (LLMs) often presents unique challenges. When dealing with LLMs at scale, you may encounter various operational issues. These might include:

1. **Timeouts:** A request to an LLM may not receive a response within the expected timeframe, causing a timeout.
2. **Rate Limiting Errors:** When the number of requests from a specific source exceeds the limit defined by the LLM provider, it results in rate limiting errors.
3. **Random 500s:** Server-side issues can cause random 500 error responses, disrupting the service flow.

These issues pose significant challenges for businesses and developers relying on LLMs for their operations.

### The Solution: Portkey's Automatic Retries

Portkey provides a powerful feature to combat these challenges – automatic retries. By implementing this, Portkey will automatically retry failed requests, applying exponential backoff between retries to distribute the load and avoid hammering the server, thus increasing the chances of request success.

The implementation of automatic retries can have a dramatic impact on the success rate of your LLM requests. Our data has shown that by using this feature, you can see up to a 30% reduction in rate limit errors, timeouts, and random 500s.
