# Getting Started

**Learn**: Add voice capabilities to your website in 5 minutes.

## Prerequisites

- A website you control
- Ability to add files and HTML tags

## Steps

### 1. Create the manifest

Create `voice-manifest.json` in your site root:

```json
{
  "name": "My Website",
  "display": {
    "activation_phrase": "Hello website",
    "suggested_prompts": ["What can you help me with?"]
  }
}
```

### 2. Link it

Add to your HTML `<head>`:

```html
<link rel="voice-manifest" href="/voice-manifest.json" />
```

### 3. Test discovery

Voice clients (browsers, extensions, apps) can now discover your site is voice-enabled.

## Next Steps

- [Add a system prompt](./System%20Prompts.md) to customize behavior
- [Connect MCP servers](./MCP%20Integration.md) for actions
- [Configure voice providers](./Voice%20Providers.md) for preferences

---

**Next**: [Display Configuration](./Display%20Configuration.md) - Control how voice UIs show your website
