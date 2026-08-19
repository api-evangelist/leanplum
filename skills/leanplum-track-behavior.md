---
name: leanplum-track-behavior
description: >-
  Send server-side session, event, state and profile data into Leanplum — start a
  session, track events and states, set user attributes — and batch it correctly so
  you are not billed several times for the same user. Use for backend telemetry that
  the mobile SDK cannot see.
api: leanplum:leanplum-api
operations:
  - start
  - track
  - advance
  - heartbeat
  - pauseSession
  - resumeSession
  - stop
  - setUserAttributes
  - setDeviceAttributes
  - setTrafficSourceInfo
  - multi
  - getMultiResults
generated: '2026-08-13'
method: generated
source: openapi/leanplum-api-openapi.json + https://docs.leanplum.com/reference/api-introduction
---

# Track behaviour into Leanplum from your backend

Everything below is a **production** `clientKey` method except `multi` and
`getMultiResults`, which take the **development** key.

## The session model

Leanplum organises data into sessions — one use of the app by one user.

1. `start` opens a session and returns the variables presented to that user.
2. `track` records an event (something the user did, with optional `value` and `params`).
3. `advance` records a state (a part of the app the user is in — an event with duration);
   `pauseState` / `resumeState` bracket it.
4. `pauseSession` / `resumeSession` bracket the session itself.
5. `heartbeat` keeps a session alive.
6. `stop` ends it.

Sessions time out after **2 hours** of inactivity, or **30 minutes** after being
paused.

## Tracking outside a session

Some things happen when the user is not in the app. `track` accepts
`disposition=passive`, and `setUserAttributes` works outside a session too. Use those
rather than opening a session you do not mean.

## Profile updates

- `setUserAttributes` — user attributes and behavioural counters (lifetime event
  occurrences). Last-write-wins, so it is safe to retry.
- `setDeviceAttributes` — device attributes, including push tokens imported from a
  previous vendor.
- `setTrafficSourceInfo` — attribution.

## Whether a missing profile gets created

`createDisposition` controls it, and **the default differs per method**:

- `CreateIfNeeded` — create the profile if it does not exist.
- `CreateNever` — require it to exist; otherwise the call is **skipped with a warning**
  on an HTTP 200.

If your pipeline expects Leanplum to create users, set `createDisposition=CreateIfNeeded`
explicitly rather than relying on a default.

## Batch correctly — this is a billing decision

Each **unique user lookup** in a request is one billable API call. `multi` allows up to
**50 users / 500 actions**; over that the call is ignored with a **403**.

Batch **per user first**, then across users:

```
multi request:
  track (user1)
  setUserAttributes (user1)
  track (user2)
  setUserAttributes (user2)
```

That is 4 actions and **2** billable calls. Splitting it into a "track batch" and a
"setUserAttributes batch" is the same work for **4** billable calls.

While a `multi` runs, other requests (API or SDK) for those users are queued behind it.

## Never fire concurrent calls for one user

Leanplum enforces strict device locking: concurrent GET/POST for the same `userId` are
serialized, and the loser gets a **429** with
`"Request failed due to concurrent requests to the same profile ID"`. Fold per-user work
into one `multi` instead.

## Verify, then retry

`200` + `success: true` only means the request was **received**. Check
`response[].warning` and `response[].error` on every response.

Retry only on **429** and **5xx**, with exponential backoff (2s, 4s, 8s with jitter).

`track` and `advance` are **append-only and not idempotent** — Leanplum publishes no
idempotency key, so a blind retry records the event twice. Deduplicate on your side.
