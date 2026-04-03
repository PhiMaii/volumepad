# VolumePad

VolumePad is a hardware + software stack for physical volume control: firmware on a custom device, a Windows desktop companion app, and an optional Stream Deck remote plugin.

This repository is the umbrella project that ties everything together and pins each part via Git submodules.

## Repository Structure

| Path | Type | Function | Upstream Repo |
| --- | --- | --- | --- |
| `docs/` | Root docs | Global architecture, protocol, and layout documentation shared across all projects. | - |
| `software/desktop-app/` | Git submodule | Windows companion app ("VolumePad Link"): device connection/runtime, audio control integration, and UI. | [PhiMaii/volumepad-link](https://github.com/PhiMaii/volumepad-link.git) |
| `firmware/` | Git submodule | ESP32-S3 firmware: input handling, haptics, LED rendering, serial protocol server, and settings persistence. | [PhiMaii/volumepad-firmware](https://github.com/PhiMaii/volumepad-firmware.git) |
| `electronics/` | Git submodule | Hardware design files (KiCad) for mainboard and supporting boards. | [PhiMaii/volumepad-electronics](https://github.com/PhiMaii/volumepad-electronics.git) |
| `software/streamdeck-plugin/` | Git submodule | Stream Deck plugin for remote volume control and backend/device status actions. | [PhiMaii/volumepad-streamdeck-plugin](https://github.com/PhiMaii/volumepad-streamdeck-plugin.git) |
| `cad/` | Local folder | Mechanical/CAD assets (currently placeholder). | - |

## Clone And Initialize

```bash
git clone --recurse-submodules https://github.com/PhiMaii/volumepad.git
cd volumepad
```

If you already cloned without submodules:

```bash
git submodule update --init --recursive
```

To pull the latest submodule commits referenced by this repo:

```bash
git submodule sync --recursive
git submodule update --init --recursive
```

## Build Quickstart

Desktop app (`software/desktop-app`):

```powershell
dotnet build VolumePadLink.slnx
dotnet test tests/VolumePadLink.Tests/VolumePadLink.Tests.csproj
```

Firmware (`firmware`):

```powershell
pio run
pio device monitor
```

Stream Deck plugin (`software/streamdeck-plugin`):

```powershell
npm install
npm run build
```

Electronics (`electronics`):

- Open KiCad projects in `mainboard/`, `touch_board/`, and `usb_hub_board/`.

## Documentation Policy

The root [`docs/`](docs/) folder is the only source of truth for shared/global documentation.

- General architecture and layout docs must live in root `docs/`.
- Submodule-local `docs/` folders should only contain project-specific documentation.
- When docs conflict, prefer root `docs/` for global behavior and contracts.

## Core Global Docs

- [`docs/app-summary.md`](docs/app-summary.md): product/app scope and runtime behavior.
- [`docs/protocol_v2.md`](docs/protocol_v2.md): core protocol and settings contract.
- [`docs/streamdeck_protocol.md`](docs/streamdeck_protocol.md): Stream Deck integration API/events.
