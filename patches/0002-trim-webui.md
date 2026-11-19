# 0002 — Trim embedded WebUI

**Status:** design (not yet applied)

## Motivation

Originium Stream replaces Sunshine's WebUI. Shipping the embedded Vue bundle
adds ~2 MB, requires Node.js/Vite during native builds, and exposes a second UI
surface that can drift from the app.

## Change

1. Remove `src_assets/common/assets/web` from the build (CMake resource target).
2. Keep all `/api/*` handlers and static image routes.
3. Remove Node.js/Vite/Rolldown from the native build dependencies.
4. Return `404` for SPA routes; keep `/api` behavior unchanged.

## Touched files

| File | Change |
|---|---|
| `cmake/` | drop web asset target |
| `src/confighttp.cpp` | remove SPA fallback (keep API routes) |
| `.github/workflows/ci.yml` | drop Node setup |

## Capability

`no-webui` in `components.lock.json`.

## Compatibility

The API is unchanged, so the application and official Moonlight clients work
identically. Users who need the upstream WebUI can install the official build.
