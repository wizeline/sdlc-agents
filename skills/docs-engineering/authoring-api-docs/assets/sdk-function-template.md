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
