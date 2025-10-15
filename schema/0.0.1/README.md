# Voice Manifest Schema v0.0.1

This directory contains version 0.0.1 of the Voice Manifest JSON Schema.

## Schema File

- **`voice-manifest.schema.json`** - The complete JSON Schema definition

## Schema ID

```
https://voicemanifest.org/voice-manifest/schema/0.0.1/voice-manifest.schema.json
```

## Usage in Voice Manifests

Reference this schema in your `voice-manifest.json`:

```json
{
  "$schema": "https://voicemanifest.org/voice-manifest/schema/0.0.1/voice-manifest.schema.json",
  "name": "My Website",
  ...
}
```

## Version Info

- **Version**: 0.0.1
- **Released**: 2025-10-15
- **Status**: Initial release

## What's in v0.0.1

### Core Features

- Basic metadata (name, description, lang, etc.)
- Display configuration (activation phrase, suggested prompts, branding)
- System prompt (inline or external file)
- MCP server endpoints (HTTP/SSE only)
- Voice agent configuration (all-in-one or composite STT/LLM/TTS)

### External References

This schema references:

- **Web App Manifest**: For common fields like `name`, `short_name`, `description`, `lang`, `dir`, `scope`, `start_url`, and display properties
- **Source**: `https://json.schemastore.org/web-manifest-combined.json`

### Design Principles

1. **No execution environment assumed** - Declarative document only
2. **HTTP/SSE only for MCP** - No stdio/local execution
3. **Progressive enhancement** - Start minimal, add features as needed
4. **Browser fallbacks** - Voice clients provide defaults for all optional features

## See Also

- [Changelog](../../CHANGELOG.md)
- [Examples](../../examples/)
- [Schema References](../../docs/Schema%20References.md)
