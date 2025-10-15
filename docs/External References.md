# External References

**Learn**: Reference external files for large content.

## Why Use External Files

- **System prompts** > 100 words
- **Configuration files** with complex settings
- **Separate concerns** - keep JSON clean

## Syntax

```json
{
  "property": {
    "$ref": "./path/to/file.txt"
  }
}
```

## System Prompt Example

**voice-manifest.json**:

```json
{
  "name": "Healthcare Portal",
  "system_prompt": {
    "$ref": "./system-prompt.txt"
  }
}
```

**system-prompt.txt**:

```
You are a HIPAA-compliant healthcare assistant.

[... detailed prompt content ...]
```

## Provider Config Example

**voice-manifest.json**:

```json
{
  "agent": {
    "provider": {
      "stt": {
        "name": "deepgram",
        "config": {
          "$ref": "./stt-config.json"
        }
      }
    }
  }
}
```

**stt-config.json**:

```json
{
  "model": "nova-2-medical",
  "language": "en-US",
  "keywords": ["prescription", "medication"],
  "punctuate": true,
  "diarize": false
}
```

## Path Rules

**Relative paths**: From manifest location

- `./file.txt` - same directory
- `./config/file.json` - subdirectory
- `../shared/prompt.txt` - parent directory

**Absolute URLs**: Remote files

- `https://example.com/prompts/system.txt`

## Best Practices

**File naming**: Use descriptive names

- ✅ `healthcare-system-prompt.txt`
- ❌ `prompt.txt`

**Version control**: Commit referenced files with manifest

**File types**: Use appropriate extensions

- `.txt` for plain text prompts
- `.json` for structured config

**Size**: Keep files reasonable (<100KB)

## Voice Client Behavior

Voice clients must:

1. Fetch referenced files
2. Use same authentication as manifest (if hosted on same domain)
3. Handle fetch failures gracefully
4. Cache appropriately

## See Also

- [04-with-mcp](../examples/04-with-mcp/) - External system prompt
- [06-with-composite-pipeline](../examples/06-with-composite-pipeline/) - External config

---

**Previous**: [Voice Providers](./Voice%20Providers.md) | **Next**: [Schema Validation](./Schema%20Validation.md)
