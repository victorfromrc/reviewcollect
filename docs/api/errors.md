# Errors

Errors use HTTP status codes and a JSON body with a readable message.

| Status | Code | When |
|---|---|---|
| `400` | `ERROR_CODE_BAD_REQUEST` | A query parameter is invalid. The message names it. |
| `401` | `ERROR_CODE_UNAUTHORIZED` | Missing or unknown API key. |
| `403` | `ERROR_CODE_ACCESS_DENIED` | The account is inactive. |
| `404` | `ERROR_CODE_NOT_FOUND` | Unknown path or resource. |
| `429` | `ERROR_CODE_TOO_MANY_REQUESTS` | Over 300 requests per minute. See [Rate limits](rate-limits.md). |

## Example

```json
{
  "code": "ERROR_CODE_BAD_REQUEST",
  "message": "limit must be an integer between 1 and 1000."
}
```
