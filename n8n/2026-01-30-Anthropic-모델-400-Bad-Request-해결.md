# Use of Anthropic Model in n8n

## Context
While working with Anthropic models in n8n, I encountered a `400 Bad Request` error. It was problematic as the error message was not explicitly clear about the exact issue. I realized that the error occurred when both `temperature` and `top_p` parameters were set simultaneously in the model configuration.

## Core
To resolve the issue, ensure that only one of the two parameters, `temperature` or `top_p`, is set at a time within the n8n Anthropic model configuration.

```json
{
  "temperature": 0.7
  // "top_p": 0.8  // Comment out or remove this line
}
```

## Insight
After troubleshooting, I found that this limitation is stated in Anthropic's official documentation. When configuring the models, setting both `temperature` and `top_p` is not allowed, as it leads to parameter conflict, resulting in a `400 Bad Request` error. Removing or commenting out one of these parameters resolves the issue effectively.