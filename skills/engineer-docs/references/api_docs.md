# API Documentation Reference

This reference covers how to produce API reference documentation — the information-oriented quadrant of the Diátaxis framework. API docs must be precise, exhaustive, and machine-readable.

## Template: API Endpoint

Use this structure for every endpoint:

```markdown
---
title: [Endpoint name]
description: [One sentence describing what this endpoint does]
audience: developer
last-updated: [YYYY-MM-DD]
---

# [HTTP Method] [Path]

[One paragraph explaining what this endpoint does and when to use it.]

## Authentication

[Describe required auth: API key, OAuth token, etc. Include header format.]

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
| `id` | string | The unique identifier of the created resource. |
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
| 400 | `invalid_request` | The request body is malformed. | Check that all required fields are present and correctly typed. |
| 401 | `unauthorized` | The API key is missing or invalid. | Verify your API key in the Authorization header. |
| 429 | `rate_limited` | Too many requests. | Wait and retry with exponential backoff. |

## Rate limits

[Describe rate limits, headers that indicate remaining quota, retry strategy.]

## Related endpoints

- [Link to related endpoints]
```

## Template: SDK / Library Reference

For documenting functions, classes, and methods:

```markdown
## `functionName(param1, param2, options?)`

[One sentence: what this function does.]

### Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `param1` | `string` | Yes | — | Description of param1. |
| `param2` | `number` | Yes | — | Description of param2. |
| `options` | `object` | No | `{}` | Configuration options. See below. |

#### Options

| Name | Type | Default | Description |
|------|------|---------|-------------|
| `timeout` | `number` | `5000` | Request timeout in milliseconds. |
| `retries` | `number` | `3` | Number of retry attempts on failure. |

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
| `AuthError` | Invalid credentials | Verify API key. |

### Example

```javascript
const result = await functionName("hello", 42, { timeout: 10000 });
console.log(result.data);
```
```

## Rules Specific to API Docs

1. **Every parameter must have a type.** No exceptions. If the type is complex, link to its schema definition.
2. **Every endpoint must have a working example.** The example should be copy-pasteable — with placeholder values clearly marked (e.g., `YOUR_API_KEY`).
3. **Document all error codes.** Not just the common ones. Include the error code string, a human-readable description, and a resolution.
4. **Show both request and response.** Always pair a request example with its expected response.
5. **Use consistent field naming.** If the API uses `snake_case`, the docs use `snake_case`. Never translate field names.
6. **Document authentication once at the top** of the API reference, then reference it from individual endpoints.
7. **Include rate limit information** for every endpoint that has limits.
8. **Version your API docs.** If the API has versions (v1, v2), the docs must make clear which version each endpoint belongs to.

## Generating from OpenAPI Specs

When an OpenAPI/Swagger spec is provided as input:

1. Parse the spec to extract all paths, methods, parameters, and schemas
2. Group endpoints by tag or resource
3. For each endpoint, populate the template above
4. For schemas referenced by `$ref`, create a dedicated "Data types" section
5. Extract the `description` fields from the spec — these are your starting point, but rewrite for clarity
6. Check for `example` fields in the spec and use them in your code examples
7. If the spec has a `security` section, document the auth requirements at the top
