---
name: authoring-api-docs
description: "Use when producing API reference documentation — REST endpoints, SDK/library references, CLI command references, or documentation generated from OpenAPI/Swagger specs. Triggers: 'document this API', 'generate API reference', 'write SDK docs', 'document these endpoints', any task involving source code with HTTP handlers, route definitions, or OpenAPI specs. Always load authoring-technical-docs first."
---

# Authoring API Docs Action

Produces precise, exhaustive API reference documentation — the information-oriented quadrant of the Diátaxis framework.

**Load `authoring-technical-docs` first** for the multi-pass workflow, style rules, and quality framework. This action provides the templates and API-specific rules.

---

## Template: REST API endpoint

```markdown
---
title: "[Endpoint name]"
description: "[One sentence: what this endpoint does]"
audience: developer
doc-type: reference
last-updated: [YYYY-MM-DD]
---

# [HTTP Method] [Path]

[One paragraph: what this endpoint does and when to use it.]

## Authentication

[Required auth: API key, OAuth token, etc. Include header format.]

## Request

### Path parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `id` | string | Yes | The unique identifier of the resource. |

### Query parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `limit` | integer | 20 | Maximum number of results to return. |

### Request body

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `name` | string | Yes | Display name for the resource. |

#### Example request

```bash
curl -X POST https://api.example.com/v1/resources \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "My Resource"
  }'
```

## Response

### Success response (200 OK)

| Field | Type | Description |
|-------|------|-------------|
| `id` | string | Unique identifier of the created resource. |
| `name` | string | The display name. |
| `created_at` | string (ISO 8601) | Timestamp of creation. |

#### Example response

```json
{
  "id": "res_abc123",
  "name": "My Resource",
  "created_at": "2026-01-15T10:30:00Z"
}
```

### Error responses

| Status | Code | Description | Resolution |
|--------|------|-------------|------------|
| 400 | `invalid_request` | Request body is malformed. | Check required fields are present and correctly typed. |
| 401 | `unauthorized` | API key is missing or invalid. | Verify your API key in the Authorization header. |
| 429 | `rate_limited` | Too many requests. | Wait and retry with exponential backoff. |

## Rate limits

[Rate limits, headers indicating remaining quota, retry strategy.]

## Related endpoints

- [Link to related endpoints]
```

---

## Template: SDK / library function

```markdown
## `functionName(param1, param2, options?)`

[One sentence: what this function does.]

### Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `param1` | `string` | Yes | — | Description. |
| `options` | `object` | No | `{}` | Configuration options. |

#### Options

| Name | Type | Default | Description |
|------|------|---------|-------------|
| `timeout` | `number` | `5000` | Request timeout in milliseconds. |

### Returns

`Promise<Result>` — An object containing:

| Field | Type | Description |
|-------|------|-------------|
| `data` | `object` | The response payload. |
| `status` | `number` | HTTP status code. |

### Errors

| Error | When | How to handle |
|-------|------|---------------|
| `TimeoutError` | Request exceeds timeout | Increase timeout or check network. |

### Example

```javascript
const result = await functionName("hello", 42, { timeout: 10000 });
console.log(result.data);
```
```

---

## API-specific rules

1. **Every parameter must have a type.** No exceptions. Complex types link to schema definitions.
2. **Every endpoint must have a working example.** Copy-pasteable with clearly marked placeholders (`YOUR_API_KEY`).
3. **Document all error codes.** Include the code string, description, and resolution.
4. **Show both request and response.** Always pair them.
5. **Use consistent field naming.** Match the API's convention exactly — never translate.
6. **Document authentication once at the top**, then reference from individual endpoints.
7. **Include rate limit information** for every rate-limited endpoint.
8. **Version your docs** if the API has versions.

---

## Generating from OpenAPI specs

When an OpenAPI/Swagger spec is the input:

1. Parse all paths, methods, parameters, and schemas
2. Group endpoints by tag or resource
3. Populate the endpoint template for each
4. Create a "Data types" section for schemas referenced by `$ref`
5. Use `description` fields as starting points — rewrite for clarity
6. Use `example` fields in code examples
7. Document `security` section as auth requirements at the top

Save output to `docs/api/`.
