# API reference

The Review Collect API gives programmatic access to the data of your Review Collect account, from your own tools, warehouse or scripts.

The API is organised around resources. Each resource has its own object definition and endpoints. Requests go over HTTPS and responses are JSON.

| | |
|---|---|
| **Base URL** | `https://api.review-collect.com/api:v1` |
| **Format** | JSON. Timestamps are ISO 8601 in UTC, ids are strings. |
| **Versioning** | `v1` is in the base URL. Fields and endpoints can be added without a version change, so ignore the fields you do not know. |
| **OpenAPI** | An OpenAPI 3.1 specification is available. Import it into Postman or generate a client. See https://developers.review-collect.com/ |

## Quickstart

1. **Get your API key** from your account manager. It is bound to your account: keep it server side.
2. **Make a call.** The key goes in the `X-Api-Key` header.

```bash
curl "https://api.review-collect.com/api:v1/invitations?limit=100" \
  -H "X-Api-Key: $RC_API_KEY"
```

3. **Read the response.** `data` holds the [invitation objects](invitations.md). `has_more` and `next_cursor` drive [pagination](pagination.md).

## Guides

- [Authentication](authentication.md)
- [Pagination](pagination.md)
- [Rate limits](rate-limits.md)
- [Errors](errors.md)

## Resources

- [Invitations](invitations.md): the invitation object, the list endpoint and incremental sync.

The latest reference is always at https://developers.review-collect.com/
