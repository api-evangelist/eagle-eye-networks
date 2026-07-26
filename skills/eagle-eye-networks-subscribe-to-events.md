---
name: Subscribe to events via webhook
description: Discover event types and create a persistent webhook subscription (with filters) on the Eagle Eye Video API Platform.
api: Eagle Eye Video API Platform (v3)
operations: [listeventtypes, createeventsubscription, createeventsubscriptionfilter, listevents]
generated: '2026-07-18'
method: generated
source: https://developer.eagleeyenetworks.com/reference
---

# Subscribe to events via webhook

Use this skill to receive Eagle Eye events (motion, device status, LPR, analytics) at your endpoint.

## Authenticate
- OAuth 2.0 Bearer token (scope `vms.all`); call the per-account `httpsBaseUrl` host.

## Steps
1. **Discover event types** — `listeventtypes` (`GET /eventTypes`) to see supported types.
2. **Create the subscription** — `createeventsubscription` (`POST /eventSubscriptions`).
   Choose a **persistent** webhook subscription with your `deliveryConfig` URL, or a
   **temporary** server-sent-events subscription with `timeToLiveSeconds`.
3. **Narrow with filters** — `createeventsubscriptionfilter`
   (`POST /eventSubscriptions/{eventSubscriptionId}/filters`) to limit to specific actors
   or event types.
4. **Backfill / verify** — `listevents` (`GET /events`) with `actor` (or `actor__in`) and a
   time range to confirm the events you expect; use `include` to expand data schemas.

## Rules
- Respond to every delivered webhook message with `200 OK`. If Eagle Eye cannot deliver
  for 90 continuous days, the subscription is disabled.
- Temporary SSE subscriptions are auto-deleted after `timeToLiveSeconds` with no subscribers.
- `actor` or `actor__in` is required on `listevents`.
