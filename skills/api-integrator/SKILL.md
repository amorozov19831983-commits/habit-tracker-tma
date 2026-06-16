---
name: "api-integrator"
description: "Guidelines and patterns for interacting with external REST APIs via curl and CLI tools."
---

# API Integrator Skill

This skill provides a standardized approach to making, debugging, and managing requests to external REST APIs.

## Core Principles
- **Tooling**: Always use `curl.exe` on Windows to avoid PowerShell's `Invoke-WebRequest` alias issues.
- **Security**: Never hardcode API keys. Use environment variables or prompt the user for a secret.
- **Resilience**: Implement basic retry logic for 5xx errors and respect `Retry-After` headers.
- **Validation**: Always validate the response status code before parsing the body.

## Implementation Patterns

### 1. Basic Request (GET)
```powershell
curl.exe -s "https://api.example.com/v1/resource" `
     -H "Accept: application/json" `
     -H "Authorization: Bearer $API_TOKEN"
```

### 2. Sending Data (POST/PUT)
Use `-d` for data and specify the content type.
```powershell
curl.exe -s -X POST "https://api.example.com/v1/resource" `
     -H "Content-Type: application/json" `
     -H "Authorization: Bearer $API_TOKEN" `
     -d '{"key": "value"}'
```

### 3. Debugging
When a request fails, use `-v` (verbose) or `-i` (include headers) to diagnose the issue.
```powershell
curl.exe -v -s "https://api.example.com/v1/resource"
```

## Error Handling Checklist
- [ ] **401/403**: Check token validity and permissions.
- [ ] **404**: Verify the endpoint URL and resource ID.
- [ ] **429**: Check rate limits and implement a wait period.
- [ ] **5xx**: Temporary server issue; retry with exponential backoff.

## Integration Workflow
1. **Discovery**: Read API documentation (web_search/web_fetch).
2. **Test**: Run a minimal `curl.exe` request to verify connectivity.
3. **Implement**: Build the full logic within the session or as a script.
4. **Audit**: Ensure no secrets are leaked in logs or files.
