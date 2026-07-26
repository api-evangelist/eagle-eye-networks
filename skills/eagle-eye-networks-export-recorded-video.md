---
name: Export and download recorded video
description: Create a video export job, poll it to completion, and download the resulting archive file from the Eagle Eye Video API Platform.
api: Eagle Eye Video API Platform (v3)
operations: [listmedia, createexportjob, getjob, getfiles, downloadfile]
generated: '2026-07-18'
method: generated
source: https://developer.eagleeyenetworks.com/reference
---

# Export and download recorded video

Use this skill to export a time range of recorded video and retrieve the file.

## Authenticate
- OAuth 2.0 Bearer token (scope `vms.all`); call the per-account `httpsBaseUrl` host.

## Steps
1. **Confirm coverage** — `listmedia` (`GET /media`) to verify recordings exist for the
   camera and ISO 8601 time range you want to export.
2. **Start the export** — `createexportjob` (`POST /exports`) with the camera/actor,
   start/end timestamps, and output settings. This returns a job.
3. **Poll the job** — `getjob` (`GET /jobs/{jobId}`) until its state is complete. Filter or
   list with `listjobs` if you lose the id.
4. **Locate the archive** — `getfiles` (`GET /files`) to find the generated archive item(s).
5. **Download** — `downloadfile` (`GET /files/{id}:download`) to save the exported MP4.

## Rules
- Exports are asynchronous — always poll `getjob` rather than assuming completion.
- Timestamps are ISO 8601; handle 404 (no recording in range) before exporting.
- Handle 401 (refresh token), 403 (permission), 429 (back off) on every call.
