---
name: leanplum-send-message
description: >-
  Send a Leanplum push notification, newsfeed or in-app message to a specific user
  or device from a server, with template values, and verify it was actually
  accepted. Use when a backend event (not a Leanplum-side trigger) should fire a
  message.
api: leanplum:leanplum-api
operations:
  - getMessages
  - getMessage
  - sendMessage
generated: '2026-08-13'
method: generated
source: openapi/leanplum-api-openapi.json + https://docs.leanplum.com/reference/responses
---

# Send a Leanplum message from your server

Leanplum triggers most messages itself, from user activity. Use this flow only when
the trigger lives on your side — an order shipped, a balance dropped, a job finished.

## Before you start

- Base URL is `https://api.leanplum.com/api`. Every call carries `appId`,
  `clientKey`, `apiVersion=1.0.6`, and the method name in `action`.
- `sendMessage` requires the **production** `clientKey`. `getMessages` and
  `getMessage` are read-only methods and take the **content read-only** key.
- The message must already exist in the Leanplum dashboard. This flow sends an
  existing message; it does not author one.

## Step 1 — find the message ID

```http
GET https://api.leanplum.com/api?action=getMessages&appId=APP_ID&clientKey=READONLY_KEY&apiVersion=1.0.6
```

`messageId` is also visible in the dashboard URL:
`www.leanplum.com/dashboard#/{APP_ID}/messaging/{MESSAGE_ID}`.

Confirm the one you want with `getMessage`.

## Step 2 — decide who receives it

From `selecting-a-user`:

- **`userId` only** — sends to *all devices* of that user. This is the normal case.
- **`deviceId` only** — sends to that device only; the user is inferred.
- **both** — that user on that device.

## Step 3 — send

```http
POST https://api.leanplum.com/api?action=sendMessage
Content-Type: application/json

{
  "appId": "APP_ID",
  "clientKey": "PROD_KEY",
  "apiVersion": "1.0.6",
  "userId": "USER_ID",
  "messageId": 101001,
  "values": { "name": "Donna", "points": 5 }
}
```

`values` overrides template variables in the message body. In the dashboard the
message can reference them as `{{Value "name"}}`, `{{"points"}}` or
`{{value['points']}}`.

Messages are queued — they are sent *after* the request completes.

## Step 4 — verify it was accepted (do not skip this)

A `200` is **not** success. Leanplum returns `200` with `success: true` even when the
action was skipped. Read `response[0]`:

| Body | Meaning | Do |
|---|---|---|
| `{"success":true}` | Sent | Done |
| `{"success":true,"warning":{"message":"User not found; request skipped."}}` | **Not sent** | Fix the userId, or pass `createDisposition=CreateIfNeeded` |
| `{"success":true,"error":{"message":"..."}}` | **Not sent** | Read the message; do not blind-retry |

## Failure handling

- **429** — either concurrent calls for the same `userId` (strict device locking) or
  the devMode 1 rps cap. Serialize per user, then retry: 2s ± rand(0,1), 4s ± rand(0,2),
  8s ± rand(0,4).
- **451** — the userId is blocked via the `block` method. Stop sending to that user.
- **5xx** — retry with the same backoff, bounded.
- **"Invalid access key"** — you used the wrong key class. `sendMessage` needs the
  production key.

## Idempotency warning

Leanplum publishes **no idempotency key**. A retried `sendMessage` sends the message
again. Record your own dedupe key before you call, and only retry on 429/5xx — never
on a 200 that carried a warning or error.

## Sending to many users

There is no bulk send parameter. Batch with `multi` (max 50 users / 500 actions per
call), batching **per user first** — each unique user lookup is one billable API call.
