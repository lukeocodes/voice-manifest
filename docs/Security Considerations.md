# Security Considerations

**Learn**: Keep your voice manifest and integrations secure.

## Never Include Credentials

**Never** put API keys, secrets, or tokens in the manifest:

```json
❌ {
  "mcp": {
    "servers": {
      "backend": {
        "url": "https://api.example.com/mcp",
        "headers": {
          "Authorization": "Bearer sk_live_abc123..."
        }
      }
    }
  }
}
```

**Instead**, use placeholders:

```json
✅ {
  "mcp": {
    "servers": {
      "backend": {
        "url": "https://api.example.com/mcp",
        "headers": {
          "Authorization": "Bearer ${API_KEY}"
        }
      }
    }
  }
}
```

Voice clients should prompt users for credentials or use OAuth.

## HTTPS Only

All endpoints must use HTTPS:

```json
✅ "url": "https://api.example.com/mcp"
❌ "url": "http://api.example.com/mcp"
```

## CORS Headers

If manifest is hosted separately from main site, configure CORS:

```
Access-Control-Allow-Origin: *
Access-Control-Allow-Methods: GET
```

For MCP servers:

```
Access-Control-Allow-Origin: *
Access-Control-Allow-Methods: POST, OPTIONS
Access-Control-Allow-Headers: Content-Type, Authorization
```

## Authentication Patterns

### User-Provided Keys

Voice client prompts user for API key, stores securely, injects at runtime.

### OAuth Flow

```json
{
  "mcp": {
    "servers": {
      "backend": {
        "url": "https://api.example.com/mcp",
        "authentication": {
          "type": "oauth2",
          "authorization_url": "https://auth.example.com/oauth/authorize",
          "token_url": "https://auth.example.com/oauth/token"
        }
      }
    }
  }
}
```

Voice client handles OAuth flow, stores tokens securely.

## Rate Limiting

Implement rate limiting on MCP endpoints:

```javascript
// Express example
const rateLimit = require("express-rate-limit");

const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 100, // limit each IP to 100 requests per windowMs
});

app.use("/mcp", limiter);
```

## Input Validation

Validate all tool inputs on server side:

```typescript
server.setRequestHandler(CallToolRequestSchema, async (request) => {
  // Validate input
  if (!isValidEmail(request.params.arguments.email)) {
    throw new Error("Invalid email");
  }

  // Sanitize input
  const sanitized = sanitizeInput(request.params.arguments.query);

  // Execute
  return await executeQuery(sanitized);
});
```

## Privacy

Never log sensitive information:

```typescript
// ❌ Bad
console.log("User query:", request.params.arguments);

// ✅ Good
console.log("Tool called:", request.params.name);
```

Consider adding privacy metadata:

```json
{
  "privacy": {
    "data_retention": "Voice data retained for 30 days",
    "recording_consent": true,
    "pii_handling": "encrypt",
    "privacy_policy": "https://example.com/privacy"
  }
}
```

## Content Security

Use Content Security Policy headers:

```
Content-Security-Policy: default-src 'self'; connect-src 'self' https://api.example.com
```

## Regular Updates

- Monitor MCP server logs for unusual activity
- Keep dependencies updated
- Rotate API keys regularly
- Review access logs

## Checklist

- [ ] No credentials in manifest
- [ ] HTTPS for all endpoints
- [ ] CORS configured properly
- [ ] Authentication implemented
- [ ] Rate limiting enabled
- [ ] Input validation on all tools
- [ ] Sensitive data not logged
- [ ] Privacy policy linked
- [ ] CSP headers configured
- [ ] Regular security reviews

---

**Previous**: [Testing Your Manifest](./Testing%20Your%20Manifest.md) | **See Also**: [Progressive Enhancement](./Progressive%20Enhancement.md)
