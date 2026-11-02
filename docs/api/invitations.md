# Invitations

An invitation is everything that happened to one customer for one order: the messages sent, the customer's feedback and the review that followed.

## The invitation object

| Field | Type | Description |
|---|---|---|
| `id` | string | Unique and increasing. Used as the pagination cursor. |
| `status` | string | Furthest step reached: `sent` (at least one message went out), `clicked` (the customer opened the review page), `reviewed` (a rating was left). It never goes back: a rated invitation stays `reviewed`. |
| `source` | string | Where the invitation came from. A store or CRM integration, a file or batch (`gsheets`, `sftp`, `aws_s3`, `file`, `batch`), a scanned QR code (`qr_code`), or the API (`api`). |
| `qr_code` | object, nullable | The scanned QR code, when `source` is `qr_code`. Fields: `id`, `name`. |
| `customer` | object | The person contacted. Each field may be `null`: `name`, `email`, `phone` (E.164, for example `+33612345678`), `country` (ISO 3166-1 alpha-2, for example `FR`), `language` (ISO 639-1, for example `fr`). |
| `order` | object, nullable | The order behind the invitation. `null` for file batches and QR codes. Fields: `id`, `number`, `created_at`, `products` (array of `external_id` and `title`). |
| `location` | object, nullable | Point of sale, for multi-location accounts: `location_id`, `location_name`. |
| `messages` | array | Messages handed to the channel, in order. Delivery status is not included. |
| `sent_at` | datetime | Date of the initial message. |
| `clicked_at` | datetime, nullable | Last opening of the review page. It is overwritten on every visit, including the one where the rating is left. |
| `review` | object, nullable | The feedback left on Review Collect: `rating` (1 to 5), `comment` (free text, may be `null`), `submitted_at`. `null` until the customer rates. |
| `routing` | object, nullable | What followed the feedback, as configured in your Review Routing settings. `null` until the customer rates. |
| `created_at` | datetime | Creation of the invitation. |
| `updated_at` | datetime | Latest event on the invitation. It drives the `since` filter. See below. |

### Messages

Each message has a `step`, a `channel` and a `sent_at`.

- `step`: `initial`, `reminder_1`, `reminder_2` or `reminder_3`.
- `channel`: `email`, `sms` or `whatsapp`. Accounts sending WhatsApp through their own provider see the provider name instead.

### Routing

| Field | Type | Description |
|---|---|---|
| `type` | string | `redirect` (the customer was taken to a public review page), `notification` (you were notified), `help_page` (the customer was shown your help page) or `none` (nothing followed). |
| `destination` | string, nullable | Depends on `type`. For `redirect`: `trustpilot`, `google`, `judge_me`, `app_store`, `google_play`, `custom_url`, `indeed`, `glassdoor`, `poulpeo`. For `notification`: `email`, `sms`, `slack`, `zendesk`, `gorgias`, `crisp`. |
| `published` | boolean, nullable | Trustpilot only. `true`: the review was found on Trustpilot. `false`: not found so far. `null`: not applicable, or the Trustpilot API is not connected. |
| `published_review` | object, nullable | The Trustpilot review, once found: `id`, `rating`, `title`, `text`, `url`, `published_at`, `reply`, `replied_at`. |

`published` goes from `false` to `true` once the review is matched. It goes back to `false` only if the review disappears from Trustpilot.

`published` and `published_review` are filled only if the Trustpilot API is connected to Review Collect, which requires Trustpilot's paid Connect module. Reviews are matched to invitations by customer email, with up to 24 hours of delay.

### What moves `updated_at`

`updated_at` moves on creation, on each message sent, on each opening of the review page, on the rating and on the Trustpilot match. A reply on Trustpilot does not move it.

### Full example

```json
{
  "object": "invitation",
  "id": "8448604",
  "status": "reviewed",
  "source": "shopify",
  "qr_code": null,
  "customer": {
    "name": "Marie Dupont",
    "email": "marie@example.com",
    "phone": null,
    "country": "FR",
    "language": "fr"
  },
  "order": {
    "id": "6507661234",
    "number": "650766",
    "created_at": "2026-07-25T14:31:32Z",
    "products": [{ "external_id": "sku-e3-1", "title": "Matelas 140x190" }]
  },
  "location": null,
  "messages": [
    { "step": "initial", "channel": "whatsapp", "sent_at": "2026-07-28T14:34:43Z" },
    { "step": "reminder_1", "channel": "email", "sent_at": "2026-07-30T09:00:00Z" }
  ],
  "sent_at": "2026-07-28T14:34:43Z",
  "clicked_at": "2026-07-28T14:36:00Z",
  "review": {
    "rating": 5,
    "comment": "Livraison rapide, matelas top.",
    "submitted_at": "2026-07-28T14:36:00Z"
  },
  "routing": {
    "type": "redirect",
    "destination": "trustpilot",
    "published": true,
    "published_review": {
      "id": "68a1f2c3d4e5f60012ab34cd",
      "rating": 5,
      "title": "Très bon matelas",
      "text": "Livraison rapide, matelas très confortable.",
      "url": "https://fr.trustpilot.com/reviews/68a1f2c3d4e5f60012ab34cd",
      "published_at": "2026-07-29",
      "reply": "Merci Marie !",
      "replied_at": "2026-07-30"
    }
  },
  "created_at": "2026-07-28T14:34:43Z",
  "updated_at": "2026-07-30T06:12:00Z"
}
```

## List invitations

Returns the invitations of the account, one per customer contacted for an order, or per contact for file batches and QR codes, sorted by `id` ascending.

```
GET /api:v1/invitations
```

### Query parameters

All parameters are optional and can be combined. Dates are ISO 8601: `2026-10-01` means midnight UTC, and `2026-10-01T08:00:00+02:00` works too.

| Parameter | Type | Description |
|---|---|---|
| `limit` | integer | Objects per page, 1 to 1000. Default 100. |
| `cursor` | string | The `next_cursor` of the previous page. |
| `since` | datetime | Invitations updated on or after this instant. For incremental loads. |
| `from` | datetime | Invitations sent on or after this instant. |
| `to` | datetime | Invitations sent strictly before this instant. |
| `status` | string | `sent`, `clicked` or `reviewed`. |
| `channel` | string | Channel of the initial message: `email`, `sms` or `whatsapp`. |
| `source` | string | The source of the invitation, for example `shopify`, `prestashop`, `magento`, `gsheets`, `sftp`, `qr_code` or `api`. |
| `rating` | integer | Rating left on Review Collect, 1 to 5. |
| `order_number` | string | Exact order number as shown in the store. |

### Examples

Everything that changed since yesterday, for an incremental sync:

```bash
curl "https://api.review-collect.com/api:v1/invitations?since=2026-10-21T00:00:00Z&limit=1000" \
  -H "X-Api-Key: $RC_API_KEY"
```

Invitations sent in September that got a rating:

```bash
curl "https://api.review-collect.com/api:v1/invitations?from=2026-09-01&to=2026-10-01&status=reviewed" \
  -H "X-Api-Key: $RC_API_KEY"
```

One order, for a support lookup:

```bash
curl "https://api.review-collect.com/api:v1/invitations?order_number=650766" \
  -H "X-Api-Key: $RC_API_KEY"
```

### Response

```json
{
  "object": "list",
  "has_more": true,
  "next_cursor": "8448610",
  "data": [
    {
      "object": "invitation",
      "id": "8448604",
      "status": "reviewed",
      "source": "shopify",
      "sent_at": "2026-07-28T14:34:43Z",
      "review": { "rating": 5, "comment": "Livraison rapide, matelas top.", "submitted_at": "2026-07-28T14:36:00Z" }
    }
  ]
}
```

Each item is an invitation object. Paginate as described in [Pagination](pagination.md).

## Incremental sync

**First run.** Walk all pages with `limit=1000` until `has_more` is `false`. 100,000 invitations take about 100 calls.

**Next runs.** Pass the start time of the previous run, minus five minutes for clock differences, as `since`. It filters on `updated_at`, so an invitation sent in April and rated in October comes back in October's run. An invitation updated while a run is in progress gets an `updated_at` after that run's start, so the next run picks it up.

**Upsert on `id`.** The same invitation comes back each time it changes, and the overlap between runs returns some rows twice. Writing them again is harmless.

`from` and `to` filter on the sending date. Use them for reporting, not for syncing.
