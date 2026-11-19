# originium-sunshine

Read-only fork of [Sunshine](https://github.com/LizardByte/Sunshine) used by
[Originium Stream](https://github.com/robinmaomao/originium-stream-webui).

## Policy

- **No pushes, no pull requests, no issues against upstream.** The `upstream`
  remote exists only for `git fetch` / `git rebase`.
- All work lives on the `originium` branch.
- Releases in this repository correspond 1:1 to source tags (GPL-3.0 compliance).

## Branch strategy

```
upstream/master ──●────●────●────●──►  (fetch only)
                   \    \    \
originium        ──●────●────●────●──►  (rebase + patches)
```

```bash
git remote add upstream https://github.com/LizardByte/Sunshine.git
git fetch upstream --tags
git rebase upstream/master
```

## Patches

| ID | Title | Status |
|---|---|---|
| 0001 | [Session status API](./patches/0001-session-events.md) | implemented |
| 0002 | [Trim embedded WebUI](./patches/0002-trim-webui.md) | design |

Patch documents describe the intended change, touched files, API contract and
tests. Binary patches will be added as they are implemented and compiled in CI.

## Build (Windows)

Sunshine's Windows build uses **MSYS2 UCRT64 + MinGW**, not MSVC:

```bash
pacman -S --needed git mingw-w64-ucrt-x86_64-toolchain \
  mingw-w64-ucrt-x86_64-cmake mingw-w64-ucrt-x86_64-ninja \
  mingw-w64-ucrt-x86_64-{boost,curl-winssl,openssl,opus,onevpl,miniupnpc,cppwinrt,qt6-static,MinHook,nsis}
export TOOLCHAIN=ucrt-x86_64
cmake -B build -G Ninja -S .
ninja -C build
cpack -G ZIP --config ./build/CPackConfig.cmake
```

The resulting `Sunshine-Windows-AMD64-lite.zip` (or the CPack ZIP artifact) is
consumed by the application component manager.

## License

GPL-3.0-or-later. See [LICENSE](./LICENSE).
