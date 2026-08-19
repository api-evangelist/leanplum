---
name: leanplum-export-data
description: >-
  Get data out of Leanplum — submit a raw session/user/report export job, poll it,
  download the files before they expire, or register a postback so events stream to
  your endpoint as they happen. Use for warehouse loads, user-profile lookups, and
  GDPR export requests.
api: leanplum:leanplum-api
operations:
  - exportData
  - exportUsers
  - exportUser
  - exportReport
  - getExportResults
  - addPostback
  - listPostbacks
  - deletePostback
  - downloadFile
generated: '2026-08-13'
method: generated
source: openapi/leanplum-api-openapi.json + https://docs.leanplum.com/reference/export-data
---

# Export data out of Leanplum

Every method here requires the **data export** `clientKey` — not the production key.
Using the wrong class returns `"Invalid access key"`.

## Two shapes: pull (jobs) and push (postbacks)

### Pull — asynchronous export jobs

`exportData`, `exportUsers` and `exportReport` are submit-then-poll. They return a
`jobId` in `response[].jobId`; poll `getExportResults` with it until the file list
appears.

Hard constraints on `exportData`, published by Leanplum:

| Constraint | Value |
|---|---|
| Exports per day | **24** (invalid-argument calls do not count) |
| Data availability | every **2 hours**, complete sessions only |
| How far back you can go | **60 days** |
| Result file lifetime | **24 hours** after the export runs |
| File size | split into roughly **256 MB** parts, unordered |
| Formats | JSON (one JSON-encoded session per line) or CSV |

CSV splits into separate files for sessions, states, events, event parameters and user
attributes. Sessions data can arrive up to **8 days** after a user's last interaction,
so daily exports of the same window can differ — re-export rather than assuming the
first read was final.

Download the files within the 24-hour window. After that the export is gone and you
must resubmit, which costs one of the day's 24 exports.

`exportUser` pulls one profile; use it for subject-access requests rather than
exporting everything.

### Push — postbacks

`addPostback` registers a URL template that Leanplum POSTs to when a message or A/B
test event fires. **Maximum three postbacks per app.**

- `type`: `messageEvents` or `abTestEvents`.
- `channels`: any of `Push Notification`, `Email`, `In-app Message`. If omitted, all
  three are active.
- `postbackUrl`: a curly-brace template. Available values include `User ID`,
  `Device ID`, `Trigger time`, and for message events `Message ID` and `Message event`.

Delivery policy, verbatim from the reference: a 30-second timeout; on a 5xx from your
endpoint Leanplum retries **up to 9 more times** with exponential backoff **starting at
1 hour, up to 10 hours**; if all 10 attempts fail **the data is lost**.

There is **no signature or HMAC** on a postback. If provenance matters, put a secret in
the URL template you register and check it on receipt.

`listPostbacks` shows what is registered; `deletePostback` removes one. With a cap of
three, list before you add.

## Verify every response

`200` + `success: true` means received, not done. Check `response[].error` and
`response[].warning`. Retry only on **429** and **5xx**, with exponential backoff
(2s, 4s, 8s with jitter).

## One exception to the JSON envelope

`downloadFile` returns the file bytes, not the `response[]` envelope.
