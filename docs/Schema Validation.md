# Schema Validation

**Learn**: Validate your voice manifest against the JSON Schema.

## Reference the Schema

Add `$schema` to your manifest:

```json
{
  "$schema": "https://voicemanifest.org/schema/0.0.1/voice-manifest.schema.json",
  "name": "My Website"
}
```

## IDE Support

Most modern IDEs (VS Code, WebStorm, etc.) automatically:

- Validate syntax
- Show errors
- Provide autocomplete
- Display property descriptions

## Command Line Validation

Using [ajv-cli](https://github.com/ajv-validator/ajv-cli):

```bash
npm install -g ajv-cli
ajv validate -s schema/0.0.1/voice-manifest.schema.json -d voice-manifest.json
```

## Common Errors

**Missing required field**:

```
Error: must have required property 'name'
```

Add `"name": "Your Site Name"`.

**Invalid type**:

```
Error: must be string
```

Check property types match schema.

**Invalid URL**:

```
Error: must match format "uri"
```

Ensure URLs are complete with protocol.

## Schema Location

**Official**: `https://voicemanifest.org/schema/0.0.1/voice-manifest.schema.json`

**Local**: `schema/0.0.1/voice-manifest.schema.json` (this repo)

## Versioning

Schema is versioned. Use version that matches your implementation:

- v0.0.1: Initial release

Future versions will be available at:

- `https://voicemanifest.org/schema/0.0.2/voice-manifest.schema.json`
- `https://voicemanifest.org/schema/0.1.0/voice-manifest.schema.json`
- etc.

---

**Previous**: [External References](./External%20References.md) | **Next**: [Testing Your Manifest](./Testing%20Your%20Manifest.md)
