# Example 1: Absolute Minimum

The absolute bare minimum voice manifest - just one field.

## What's Included

- ✅ `name` - The only required field

## What's Missing

- No display hints
- No system prompt
- No MCP configuration
- No voice providers

## What Voice Clients Can Do

With just this manifest, voice clients can:

- Discover your site is voice-enabled
- Show it in their voice UI
- Provide generic voice interaction using their own capabilities

## Use Case

Perfect for:

- Testing voice manifest discovery
- Verifying your `<link rel="voice-manifest">` setup works
- Learning the absolute basics

## HTML Integration

```html
<link rel="voice-manifest" href="/voice-manifest.json" />
```

## Next Step

See [02-with-display](../02-with-display/) to add display hints for better UX.
