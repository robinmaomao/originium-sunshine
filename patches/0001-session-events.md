# 0001 — Session events API

**Status:** design (not yet applied)

## Motivation

Originium Stream needs real-time session information to show "this host is being
streamed by N clients" and live bitrate/fps without parsing logs.

## Contract

```
GET /api/session/events
Accept: text/event-stream
Authorization: Basic …
```

Events (SSE `data:` JSON):

```json
{ "type": "session.started", "sessionId": "…", "client": { "name": "Steam Deck", "uuid": "…" }, "app": { "index": 2, "name": "Desktop" }, "at": "2026-11-19T03:12:44Z" }
{ "type": "session.stats", "sessionId": "…", "fps": 120, "bitrateKbps": 80000, "latencyMs": 4 }
{ "type": "session.ended", "sessionId": "…", "reason": "client-disconnect" }
```

- Authentication reuses `confighttp::authenticate`.
- CSRF applies only to browser-origin requests; desktop clients are exempt.
- Endpoint is disabled unless `session_events_enabled = enabled` (default enabled).
- Backpressure: drop `session.stats` events if the client is slow; never drop
  lifecycle events.

## Touched files

| File | Change |
|---|---|
| `src/confighttp.cpp` | route registration, SSE handler, keep-alive comments |
| `src/stream.cpp` | emit lifecycle events on start/stop |
| `src/video.cpp` | periodic stats sample |
| `src/config.cpp` | new `session_events_enabled` option |
| `tests/unit/test_confighttp.cpp` | auth + event framing tests |

## Capability

`session-events` in `components.lock.json`.

## Compatibility

Official upstream binaries do not expose the endpoint. The app falls back to
`/api/logs` polling and the paired-client list.
