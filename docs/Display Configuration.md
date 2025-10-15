# Display Configuration

**Learn**: Control how voice UIs show your website.

## Basic Metadata

```json
{
  "name": "My Store",
  "short_name": "Store",
  "description": "Voice shopping assistant",
  "lang": "en-US"
}
```

Borrowed from Web App Manifest. Same semantics.

## Visual Branding

```json
{
  "display": {
    "icon": "/voice-icon.png",
    "background_color": "#1E40AF",
    "theme_color": "#1E40AF"
  }
}
```

Voice UIs use these for visual representation.

## Activation Phrase

```json
{
  "display": {
    "activation_phrase": "Talk to my store"
  }
}
```

Suggested wake phrase. Voice clients may or may not use it.

## Call to Action

```json
{
  "display": {
    "call_to_action": "Ask about products, track orders, or get help"
  }
}
```

Tells users what they can do.

## Suggested Prompts

```json
{
  "display": {
    "suggested_prompts": [
      "Show me wireless headphones",
      "Where's my order?",
      "What's on sale?"
    ]
  }
}
```

Max 10. Guide users on what to ask.

## Complete Example

```json
{
  "name": "Tech Store",
  "short_name": "Tech",
  "description": "Voice-enabled electronics store",
  "lang": "en-US",
  "display": {
    "icon": "/icons/voice.png",
    "background_color": "#0F172A",
    "theme_color": "#0F172A",
    "activation_phrase": "Shop with Tech Store",
    "call_to_action": "Find products, track orders, or ask questions",
    "suggested_prompts": [
      "Show me laptops under $1000",
      "Track order #12345",
      "Compare iPhone vs Samsung"
    ]
  }
}
```

## See Also

- [02-with-display](../examples/02-with-display/)

---

**Previous**: [Getting Started](./Getting%20Started.md) | **Next**: [System Prompts](./System%20Prompts.md)
