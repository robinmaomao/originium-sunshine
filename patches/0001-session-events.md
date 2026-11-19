# 0001 — Session status API

**Status:** implemented (v1, polling endpoint)

## Motivation

Originium Stream needs to know whether the host is currently streaming and which
application is running, without parsing logs.

## Contract

```
GET /api/session
Authorization: Basic …
```

```json
{
  "running": true,
  "appId": 3,
  "appName": "Desktop",
  "state": "streaming"
}
```

- `running=false`, `appId=0`, `appName=""`, `state="idle"` when idle.
- Authentication reuses `confighttp::authenticate`.
- Implemented in `src/confighttp.cpp` (`getSessionStatus`), using
  `proc::proc.running()` and `proc::proc.get_last_run_app_name()`.

## Touched files

| File | Change |
|---|---|
| `src/confighttp.cpp` | `getSessionStatus` handler + `^/api/session$` GET route |

## Future work

- SSE variant (`GET /api/session/events`) with live fps/bitrate samples.
- Client identity and per-session statistics.

## Capability

`session-events` in `components.lock.json`.

## Compatibility

Official upstream binaries do not expose the endpoint. The app falls back to
`/api/logs` polling and the paired-client list.
