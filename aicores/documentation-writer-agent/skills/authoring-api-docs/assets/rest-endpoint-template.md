---
title: "[Endpoint name]"
description: "[One sentence: what this endpoint does]"
audience: developer
doc-type: reference
last-updated: [YYYY-MM-DD]
version: [X.X]
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
