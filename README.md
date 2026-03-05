# Tanmatsu SSH Client

A standalone SSH terminal client for the [Tanmatsu](https://tanmatsu.cloud/) badge, built on ESP-IDF.

Connect to remote servers over SSH directly from your badge with a full interactive terminal session, host key verification, and persistent connection profiles.

## Features

- Multiple saved SSH connection profiles (host, port, username, password)
- Interactive terminal emulator with ANSI escape sequence support
- Host key fingerprint verification (SHA256) with NVS-based persistence
- Man-in-the-middle attack detection (warns on host key changes)
- Keyboard backlight and display brightness controls during sessions
- Optional background images from SD card (`/sd/bg/`)
- xterm-256color terminal emulation

## Requirements

- [Tanmatsu](https://tanmatsu.cloud/) badge (ESP32-P4 based)
- WiFi network with access to SSH servers
- ESP-IDF v5.5.1 (included as submodule)

## Quick Start

```bash
# Clone with submodules
git clone --recursive <repo-url>
cd tanmatsu-template-ssh

# Set up ESP-IDF toolchain (first time only)
make prepare

# Build
make build

# Flash to device
make flash
```

The output binary is `build/tanmatsu/tanmatsu-ssh.bin`.

## Usage

### SSH Connection Menu

| Key | Action |
|-----|--------|
| F3 | Add new connection |
| F4 | Edit selected connection |
| F5 | Delete selected connection |
| Enter | Connect to selected server |
| F1 / ESC | Return to launcher |
| F2 | Toggle keyboard backlight |

### During SSH Session

| Key | Action |
|-----|--------|
| F1 | Disconnect and return to menu |
| F2 | Toggle keyboard backlight |
| F3 | Cycle display brightness |
| Arrow keys | Cursor movement |
| Tab | Tab completion |
| Ctrl+key | Send control characters |
| Volume +/- | Adjust font size |

### Host Key Verification

On first connection to a server, the SHA256 fingerprint is displayed and you are prompted to trust the host. Once accepted, the fingerprint is saved in NVS (non-volatile storage) and silently verified on subsequent connections. If a host key changes, a warning is shown to alert you to a possible man-in-the-middle attack.

## Project Structure

```
main/
  main.c              - App entry point (BSP, WiFi, display init)
  menu_ssh.c/h        - SSH connection list menu
  menu_ssh_edit.c/h   - Connection profile editor
  util_ssh.c/h        - SSH session handling (libssh2)
  settings_ssh.c/h    - NVS persistence for connection profiles and host keys
  textedit.c/h        - Text input UI
  message_dialog.c/h  - Dialog boxes
  icons.c/h           - Embedded keyboard key icons (PNG)
  common/
    display.c/h       - Display abstraction layer
    theme.c/h         - UI theme
  icons/              - Embedded icon PNG assets
components/
  gui/                - Menu and UI widget library
```

## Build Targets

```bash
make prepare    # Set up ESP-IDF toolchain
make build      # Build for Tanmatsu (default)
make flash      # Flash to device via USB
make monitor    # Serial monitor
make clean      # Clean build artifacts
```

Build for a specific device:

```bash
make build DEVICE=tanmatsu
make build DEVICE=konsool
```

## Dependencies

Managed components (automatically downloaded):

| Component | Purpose |
|-----------|---------|
| `badgeteam/badge-bsp` | Board support package |
| `nicolaielectronics/tanmatsu-wifi` | WiFi via radio coprocessor |
| `nicolaielectronics/wifi-manager` | WiFi network management |
| `skuodi/libssh2_esp` | SSH client library (libssh2) |
| `badgeteam/terminal-emulator` | Terminal emulator engine |
| `robotman2412/pax-gfx` | Graphics library |
| `robotman2412/pax-codecs` | PNG decoder |

## License

See [LICENSE](LICENSE) for details.
