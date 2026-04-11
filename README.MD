<div align="center">
  <img src="assets/icon.png" width="128" height="128" alt="Meridian icon" />
  <h1>Meridian</h1>
  <p><strong>Native macOS Wine launcher with a bundled runtime</strong></p>
  <p>
    <img src="https://img.shields.io/badge/macOS-13.0%2B-black?style=flat-square&logo=apple" />
    <img src="https://img.shields.io/badge/Apple_Silicon-Recommended-blue?style=flat-square" />
    <img src="https://img.shields.io/badge/Swift-5.9-orange?style=flat-square&logo=swift" />
    <img src="https://img.shields.io/badge/License-MIT-green?style=flat-square" />
  </p>
</div>

---

Meridian is a native **SwiftUI** app for running Windows applications and games on Apple Silicon. It manages isolated Wine bottles, ships a complete runtime inside the `.app` bundle, and wraps everything in a polished macOS-native interface — no Homebrew, no Terminal, no external Wine install required.

## Screenshots

> *Coming soon*

## Features

### Bottle Management
- Create, rename, duplicate, import, and export Wine bottles
- Per-bottle icon — choose an emoji, a letter, or a custom color
- Isolated `WINEPREFIX` per bottle, stored in `~/Library/Application Support/Meridian/`
- Automatic `wineboot --init` on bottle creation with optimal defaults applied out of the box

### Runtime
- Full runtime bundled inside `Meridian.app/Contents/Resources/Runtime/`
- Includes **Wine**, **DXVK**, **vkd3d-proton**, **D3DMetal**, **GStreamer**, and supporting libraries
- No external install required — works on a clean macOS system

### Graphics & Compatibility
| Setting | Options |
|---|---|
| Graphics API | D3DMetal *(default)*, DXVK, vkd3d-proton |
| Sync Method | msync *(default)*, esync |
| Retina Mode | On / Off — written to Wine registry |
| DLSS / XeSS | Experimental upscaling hook |

- DLL overrides applied automatically on API switch
- `wineserver` restarted when settings change so they take effect immediately

### App Library
- Built-in catalog of popular Windows launchers, runtimes, and tools
- Installer cache — downloaded `.exe` files are stored locally and reused
- Smart download sheet with circular progress, speed, ETA, and live install log

### Program Management
- Launchpad-style icon grid inside each bottle
- Real icon extraction from `.exe` PE headers (+ Homebrew `icoutils` when available)
- Auto-scan `drive_c/Program Files` for installed programs
- Per-program launch arguments, rename, and remove

### Diagnostics
- **Runtime Doctor** — checks Wine binary, DXVK, vkd3d, D3DMetal, and GStreamer paths
- **Process Monitor** — live Wine process list with CPU / RAM and per-process kill
- **Disk usage** per bottle
- Full `wineboot` and install log viewer

### macOS Integration
- Menu bar icon with quick-launch for every pinned program
- OS notifications on install complete and bottle ready
- `/Applications` shortcuts with extracted icons
- Export / import bottles as `.bottle.zip`
- Onboarding wizard on first launch

### Localization
- English and Russian UI

## Requirements

| | |
|---|---|
| macOS | 13.0 Ventura or later |
| Architecture | Apple Silicon (M1 and later) recommended |
| Disk space | ~2 GB for the bundled runtime |
| External dependencies | None required. `icoutils` via Homebrew enables better icon extraction. |

## Installation

1. Download the latest `Meridian.dmg` from [Releases](../../releases)
2. Drag `Meridian.app` to your Applications folder
3. Launch — the onboarding wizard will verify the bundled runtime and create your first bottle

> Meridian ships its own runtime. You do not need to install Wine, Homebrew, or any other tool to get started.

## Building from Source

```bash
git clone https://github.com/yourname/Meridian.git
cd Meridian
open Meridian.xcodeproj
```

**Required setup in Xcode:**
- Target → Signing & Capabilities → **remove App Sandbox**
- Hardened Runtime → enable **Disable Library Validation** and **Allow Unsigned Executable Memory**
- Add your `Runtime/` folder to **Copy Bundle Resources** using *Create folder references* (blue icon)

```
Runtime/
├── bin/         wine, wineserver
├── dxvk/        x32/, x64/   (.dll)
├── vkd3d/       x86/, x64/   (.dll)
├── lib/          *.dylib, wine/, GStreamer.framework, external/ (d3dmetal)
└── share/        wine/
```

## Known Limitations

- **32-bit programs** — `WINEARCH=win64` is enforced; 32-bit-only executables will not run
- **Anti-cheat** — games using kernel-level anti-cheat (Valorant, EasyAntiCheat on some titles) are not supported
- **DLSS / XeSS** — experimental; results vary by game and driver version
- **Localization** — a few secondary screens and service messages remain English-only in this release
- **Controller support** and engine patch tooling are functional but not validated across a wide game matrix

## Acknowledgements

Meridian is built on top of the work of the Wine and GPTK communities:

- [Wine](https://www.winehq.org/) — Windows compatibility layer
- [Gcenx / macOS_Wine_builds](https://github.com/Gcenx/macOS_Wine_builds) — macOS Wine builds
- [DXVK](https://github.com/doitsujin/dxvk) — DirectX 9/10/11 → Vulkan
- [vkd3d-proton](https://github.com/HansKristian-Work/vkd3d-proton) — DirectX 12 → Vulkan (Valve fork)
- Apple [Game Porting Toolkit](https://developer.apple.com/games/) — D3DMetal and Metal translation layer

## License

MIT — see [LICENSE](LICENSE) for details.
