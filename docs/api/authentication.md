# Authentication

Every request carries your API key in the `X-Api-Key` header.

```bash
curl "https://api.review-collect.com/api:v1/invitations" \
  -H "X-Api-Key: $RC_API_KEY"
```

A key is bound to one Review Collect account and gives access to that account's data only.

Keep your key server side. Never put it in a browser or in a mobile app.

## Responses

| Status | When |
|---|---|
| `401` | The API key is missing or unknown. |
| `403` | The key belongs to an inactive account. |

To get an API key, ask your account manager.
