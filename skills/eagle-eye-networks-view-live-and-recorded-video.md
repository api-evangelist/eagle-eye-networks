---
name: View live and recorded camera video
description: Authenticate, find a camera, and pull a live snapshot or recorded imagery from the Eagle Eye Video API Platform.
api: Eagle Eye Video API Platform (v3)
operations: [listcameras, getcamera, getliveimage, listmedia, getrecordedimage]
generated: '2026-07-18'
method: generated
source: https://developer.eagleeyenetworks.com/reference
---

# View live and recorded camera video

Use this skill to locate a camera and retrieve live or recorded imagery.

## Authenticate
- Obtain an OAuth 2.0 access token from `https://auth.eagleeyenetworks.com/oauth2/token`
  (authorization-code or client-credentials flow, scope `vms.all`).
- Read the `httpsBaseUrl` returned in the token response and use that per-account host
  (e.g. `api.cNNN.eagleeyenetworks.com`) for all subsequent calls.
- Send `Authorization: Bearer <access_token>` on every request.

## Steps
1. **List cameras** — `listcameras` (`GET /cameras`). Filter by `locationId`, `bridgeId`,
   or `tags`; page with `pageSize` + `pageToken`. Note `totalSize` is the total available,
   not the count matching your query.
2. **Inspect a camera** — `getcamera` (`GET /cameras/{cameraId}`) to confirm status and
   capabilities.
3. **Live snapshot** — `getliveimage` (`GET /media/liveImage.jpeg`) for a fresh image from
   the device (the call waits until an image is available).
4. **Find recordings** — `listmedia` (`GET /media`) to get the intervals for which
   recordings exist; timestamps are ISO 8601.
5. **Recorded image** — `getrecordedimage` (`GET /media/recordedImage.jpeg`) at a chosen
   ISO 8601 timestamp, `type=main` or `type=preview`.

## Rules
- `type=main` recorded/live images are rate-limited — do not call in quick succession;
  stagger requests. A 404 means no recording exists at that timestamp.
- Handle 401 (expired token → refresh), 403 (missing role/permission), 429 (back off).
