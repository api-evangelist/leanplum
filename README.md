# Leanplum (leanplum)

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

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
