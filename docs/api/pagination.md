# Pagination

List endpoints share one envelope and two parameters.

| Parameter | Description |
|---|---|
| `limit` | Number of objects per page, from 1 to 1000. |
| `cursor` | The `next_cursor` of the previous page. |

## Response envelope

```json
{
  "object": "list",
  "data": [],
  "has_more": true,
  "next_cursor": "8448610"
}
```

Call again with `cursor` while `has_more` is `true`.

The cursor is the last id of the page, so a walk stays consistent while new objects are being created. The same filters apply to every page.
