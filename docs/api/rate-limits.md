# Rate limits

Each API key can make **300 requests per minute**.

Every response carries the limit in the `X-RateLimit-Limit` header.

Above the limit, requests get a `429` with a `Retry-After` header, in seconds. Wait that long and retry: the window is one minute and nothing is lost.

```http
HTTP/2 429
X-RateLimit-Limit: 300
Retry-After: 60

{
  "code": "ERROR_CODE_TOO_MANY_REQUESTS",
  "message": "Rate limit exceeded: 300 requests per minute per API key. Retry in a minute."
}
```

A full history stays well within the limit. For example, 100,000 invitations are 100 pages of 1000, loaded one after the other in a few minutes.
