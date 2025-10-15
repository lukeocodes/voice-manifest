# Testing Your Manifest

**Learn**: Verify your voice manifest works correctly.

## Validation

### 1. JSON Syntax

Ensure valid JSON:

```bash
cat voice-manifest.json | jq .
```

If no errors, JSON is valid.

### 2. Schema Validation

Validate against schema:

```bash
npm install -g ajv-cli
ajv validate -s schema/0.0.1/voice-manifest.schema.json -d voice-manifest.json
```

### 3. Accessibility

Check manifest is accessible:

```bash
curl -I https://yoursite.com/voice-manifest.json
```

Should return `200 OK` and `Content-Type: application/json`.

## HTML Link

Verify `<link>` tag exists:

```bash
curl https://yoursite.com | grep voice-manifest
```

Should show:

```html
<link rel="voice-manifest" href="/voice-manifest.json" />
```

## External References

Test external file loading:

```bash
# If using relative paths
curl https://yoursite.com/system-prompt.txt

# Should return file contents
```

## MCP Endpoints

Test MCP server connectivity:

```bash
curl -X POST https://api.yoursite.com/mcp \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc": "2.0", "method": "initialize", "params": {}, "id": 1}'
```

Should return MCP initialization response.

### List Tools

```bash
curl -X POST https://api.yoursite.com/mcp \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc": "2.0", "method": "tools/list", "params": {}, "id": 2}'
```

Should return list of available tools.

## Voice Provider Endpoints

Test provider availability (if specified):

```bash
# Deepgram example
curl -I https://agent.deepgram.com/v1/agent/converse
```

Should return valid response (not 404).

## Browser DevTools

1. Open your site in browser
2. Open DevTools (F12)
3. Go to Network tab
4. Filter for "voice-manifest"
5. Reload page
6. Verify manifest loads successfully

## Common Issues

### 404 Not Found

**Problem**: Manifest file not accessible
**Solution**:

- Check file exists at specified path
- Verify web server serves JSON files
- Check file permissions

### CORS Error

**Problem**: Cross-origin blocked
**Solution**: Add CORS headers:

```
Access-Control-Allow-Origin: *
```

### Invalid JSON

**Problem**: Syntax error
**Solution**: Use JSON validator or `jq` to find error location

### Schema Validation Fails

**Problem**: Property doesn't match schema
**Solution**: Check error message, fix property type/value

### MCP Connection Fails

**Problem**: Can't reach MCP server
**Solution**:

- Verify URL is correct and accessible
- Check firewall rules
- Ensure HTTPS is configured

## Automated Testing

Create a test script:

```javascript
// test-manifest.js
const Ajv = require("ajv");
const fs = require("fs");

const ajv = new Ajv();
const schema = JSON.parse(
  fs.readFileSync("./schema/0.0.1/voice-manifest.schema.json")
);
const manifest = JSON.parse(fs.readFileSync("./voice-manifest.json"));

const valid = ajv.validate(schema, manifest);

if (!valid) {
  console.error("Validation errors:", ajv.errors);
  process.exit(1);
}

console.log("✓ Manifest is valid");
```

Run in CI/CD:

```yaml
# .github/workflows/test.yml
name: Test
on: [push]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: actions/setup-node@v2
      - run: npm install ajv
      - run: node test-manifest.js
```

## Manual Testing Checklist

- [ ] JSON syntax valid
- [ ] Schema validation passes
- [ ] Manifest accessible via URL
- [ ] HTML link tag present
- [ ] External files load (if used)
- [ ] MCP endpoints respond (if used)
- [ ] Provider endpoints accessible (if specified)
- [ ] No credentials in manifest
- [ ] CORS headers configured
- [ ] HTTPS used for all URLs

---

**Previous**: [Schema Validation](./Schema%20Validation.md) | **Next**: [Security Considerations](./Security%20Considerations.md)
