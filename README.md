# Leanplum (leanplum)

Leanplum is a mobile marketing and multichannel customer engagement platform offering push notifications, in-app and email messaging, behavioral event tracking and analytics, A/B testing, and remotely configurable content variables. Leanplum was acquired by **CleverTap in 2022** and now operates as "Leanplum by CleverTap"; the brand and its documented REST API remain active while customers are migrated onto the CleverTap platform (CleverTap has wrapped its own methods behind the Leanplum API surface to smooth that transition). All API requests are made to `https://api.leanplum.com/api` and are dispatched by an `action` parameter, authenticated with an `appId` plus a permission-scoped `clientKey` (production, development, data export, or content read-only) and an `apiVersion`.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/leanplum/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/leanplum/refs/heads/main/apis.yml)

## Tags

- Mobile Marketing
- Customer Engagement
- Push Notifications
- Messaging
- A/B Testing
- Analytics
- CleverTap

## Timestamps

- **Created:** 2026-07-03
- **Modified:** 2026-07-03

## APIs

### Leanplum Events & Tracking API

Server-side behavioral tracking. Start and manage user sessions (`start`, `pauseSession`, `resumeSession`, `heartbeat`, `stop`) and record events and application states with `track` and `advance`. Requires the production clientKey.

- **Human URL:** [https://docs.leanplum.com/reference/tracking-analytics-data-via-api](https://docs.leanplum.com/reference/tracking-analytics-data-via-api)
- **Base URL:** `https://api.leanplum.com/api`

### Leanplum User & Device Attributes API

Set and manage the user and device profile attributes that drive segmentation and audience targeting - `setUserAttributes`, `setDeviceAttributes`, `setTrafficSourceInfo` - and remove users with `deleteUser`.

- **Human URL:** [https://docs.leanplum.com/reference/post_api-action-setuserattributes](https://docs.leanplum.com/reference/post_api-action-setuserattributes)
- **Base URL:** `https://api.leanplum.com/api`

### Leanplum Messaging API

Trigger and inspect outbound messages. `sendMessage` delivers a push notification, news-feed, in-app, or other message to a user; `getMessages` and `getMessage` retrieve message campaign definitions and metadata.

- **Human URL:** [https://docs.leanplum.com/reference/post_api-action-sendmessage](https://docs.leanplum.com/reference/post_api-action-sendmessage)
- **Base URL:** `https://api.leanplum.com/api`

### Leanplum A/B Tests API

Read A/B test experiments and their variants - `getAbTests`, `getAbTest`, and `getVariant`. Uses the content read-only clientKey.

- **Human URL:** [https://docs.leanplum.com/reference/api-methods](https://docs.leanplum.com/reference/api-methods)
- **Base URL:** `https://api.leanplum.com/api`

### Leanplum Content & Variables API

Manage remotely configurable content. `getVars` returns the variables presented to a user or device, `setVars` defines the variable set in the content management system, and `downloadFile` retrieves a managed file asset.

- **Human URL:** [https://docs.leanplum.com/reference/get_api-action-getvars](https://docs.leanplum.com/reference/get_api-action-getvars)
- **Base URL:** `https://api.leanplum.com/api`

### Leanplum Data Export API

Bulk export of raw analytics and reporting data - `exportData`, `exportUsers`, `exportUser`, `exportReport` - plus `getMultiResults` to check the status of an asynchronous job. Requires the data export clientKey.

- **Human URL:** [https://docs.leanplum.com/reference/get_api-action-exportdata](https://docs.leanplum.com/reference/get_api-action-exportdata)
- **Base URL:** `https://api.leanplum.com/api`

### Leanplum Postbacks & Batch API

Operational plumbing - `addPostback` registers an HTTP postback (webhook) callback, and `multi` batches up to 50 users and/or 500 actions into a single request (over-limit batches return 403).

- **Human URL:** [https://docs.leanplum.com/reference/batching-requests](https://docs.leanplum.com/reference/batching-requests)
- **Base URL:** `https://api.leanplum.com/api`

## Common Properties

- [GitHub Organization](https://github.com/Leanplum)
- [LinkedIn](https://www.linkedin.com/company/leanplum)
- [Website](https://www.leanplum.com)
- [Documentation](https://docs.leanplum.com)
- [Plans](plans/leanplum-plans-pricing.yml)
- [Rate Limits](rate-limits/leanplum-rate-limits.yml)
- [Fin Ops](finops/leanplum-finops.yml)

## Operating Status

**Active (legacy / migrating).** Leanplum was acquired by CleverTap in 2022 and operates as "Leanplum by CleverTap." Documentation and the REST API at `api.leanplum.com` remain live and SDKs are still being updated, but CleverTap is migrating Leanplum customers onto the CleverTap platform via a manual, Customer Success-driven process, and the public `status.leanplum.com` page is inactive. New customers are generally directed to CleverTap; existing Leanplum integrations continue to function.

## Maintainers

**FN:** Kin Lane
**Email:** kin@apievangelist.com
