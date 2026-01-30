# Using Anthropic Model in n8n: Resolving 400 Bad Request

## Context
Integrating the Anthropic model in n8n workflows can show a 400 Bad Request error if certain parameters are not correctly configured. Users often encounter this issue when attempting to set both the `temperature` and `top_p` parameters simultaneously. This problem requires attention to ensure smooth operation and accurate predictions.

## Core
When configuring the Anthropic model in n8n, avoid setting both `temperature` and `top_p`. Here’s a simple example of how these parameters should be handled:

```json
{
  "parameters": {
    "temperature": 0.7,
    /* Avoid using 'top_p' when 'temperature' is set */
  }
}
```

Instead of setting both, choose one based on the model's desired behavior. This configuration avoids conflicts leading to errors.

## Insight
Upon inspecting the official documentation and error messages, it’s clear that specifying both `temperature` and `top_p` for a single model leads to a conflict, triggering the 400 Bad Request error. Addressing this involves understanding the specific impacts of each parameter and selecting one that best aligns with the intended outcome. This insight helps in configuring models more effectively, ensuring that workflows in n8n remain robust and efficient.