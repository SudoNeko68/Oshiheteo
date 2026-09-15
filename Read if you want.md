# oshieteo

`oshieteo` is a unified command-line dispatcher and resource inspector for Unix-like environments. It determines the underlying type of an arbitrary operand—including filesystem nodes, version control repositories, network addresses, structured data formats, multimedia containers, and system daemons—and transparently delegates execution to the optimal utility or diagnostic tool.

---

## Overview

Unix environments traditionally require distinct utilities and argument structures for inspecting disparate resources (e.g., `tar` for tape archives, `mediainfo` for audiovisual codecs, `systemctl` for daemon states, `jq` for serialized payloads). `oshieteo` standardizes this interface into a single idempotent entry point.

When invoked, the tool sequentially evaluates the input against a prioritized heuristic parser:

1. **Null Operands**: Dispatches system profiling telemetry via `fastfetch` or `neofetch`.
2. **Web Identifiers (HTTP/HTTPS)**: Discriminates between binary endpoints (delegated to `wget`/`curl`) and text/HTML payloads (rendered via `lynx`/`links`/`curl`).
3. **Directory Structures & Version Control**: Identifies Git working trees and emits branch status and commit graphs (`onefetch`, `git status`, `git log`); falls back to structured directory emission (`eza`, `ls`).
4. **Archive Containers**: Queries archive tables of contents without filesystem extraction (`tar`, `unzip`, `7z`, `bsdtar`).
5. **Multimedia Streams**: Performs comprehensive codec, container, and stream metadata extraction (`mediainfo`, `ffprobe`).
6. **Electronic Documents (PDF)**: Extracts document schema (`pdfinfo`) and outputs text streams (`pdftotext`).
7. **Vector and Raster Graphics**: Rasterizes and renders image data directly within the terminal buffer (`chafa`, `catimg`).
8. **Structured Data Serialization**: Validates and formats structured payloads (`jq`, `python3 -m json.tool`, `glow`).
9. **Executable Binaries**: Traverses standard help hooks (`--help`, `-h`, `help`).
10. **Network Endpoints**: Distinguishes IPv4/IPv6 primitives from Domain Name System (DNS) labels, delegating to ICMP reachability probes (`ping`) or registry queries (`whois`, `dig`).
11. **Service Managers**: Resolves systemd service units and outputs state vectors (`systemctl status`).

---

## Dispatch Matrix

| Input Category | Detection Rule | Primary Delegate | Secondary / Fallback |
|---|---|---|---|
| Null | `$# == 0` | `fastfetch` | `neofetch`, `/etc/os-release` |
| Directory (Git) | Directory containing `.git` | `onefetch` | `git status`, `git log` |
| Directory (Generic) | `test -d` | `eza -la --icons` | `ls -la --color=auto` |
| Archive | `.tar.*`, `.zip`, `.rar`, `.7z`, `.iso` | Native table listing | `bsdtar -tvf`, `7z l` |
| Audio Container | `.mp3`, `.flac`, `.wav`, `.ogg`, `.m4a` | `mediainfo` | `ffprobe -hide_banner` |
| Video Container | `.mp4`, `.mkv`, `.avi`, `.mov`, `.webm` | `mediainfo` | `ffprobe -hide_banner` |
| Document | `.pdf` | `pdfinfo` + `pdftotext` | `file` |
| Raster Graphic | `.png`, `.jpg`, `.jpeg`, `.gif`, `.webp` | `chafa` | `catimg`, `file` |
| Data Interchange | `.json` | `jq .` | `python3 -m json.tool` |
| Markdown Text | `.md`, `.markdown` | `glow` | `bat`, `cat` |
| Plain Text / Code | Regular file (`test -f`) | `bat` | `cat` |
| Executable | `test -x` | Native `--help` flag | `-h`, `help` |
| Network (IP) | IPv4 / IPv6 regex pattern | `ping -c 4` | N/A |
| Network (FQDN) | Domain label validation pattern | `whois` | `dig +noall +answer` |
| Daemon / Unit | Matching unit in systemd catalog | `systemctl status` | N/A |
| URL (Binary) | Remote resource with file suffix | `wget` | `curl -O -L` |
| URL (Document) | Remote HTTP/HTTPS page | `lynx -dump` | `links -dump`, `curl -sL` |

---

## Non-Standard Operations

- **Sequence Arithmetic**: Evaluates inputs conforming to $X \pmod 7 \equiv 6$ for $0 \le X \le 1001$ (e.g., `1000-7`, `993-7`, `986`), computing the subsequent scalar in the decrement progression.
- **Application Protocol Handler**: Resolves the `zxc` identifier to trigger the Steam URL protocol hook (`steam://run/570`).

---

## Installation

### Prerequisites

Ensure bash (>= 4.4) is available on the target host. Optional delegates should be installed according to workload requirements.

### Source Installation

```bash
git clone https://github.com/YOUR_USERNAME/oshiheteo.git
cd oshiheteo
chmod +x oshieteo

# Install executable into user environment
mkdir -p "$HOME/.local/bin"
ln -sf "$PWD/oshieteo" "$HOME/.local/bin/oshieteo"
ln -sf "$PWD/oshieteo" "$HOME/.local/bin/oshiheteo"
```

Verify that `$HOME/.local/bin` is populated in the user's `PATH`:

```bash
export PATH="$HOME/.local/bin:$PATH"
```

### Dependency Provisioning

#### Arch Linux
```bash
sudo pacman -S fastfetch bat eza jq glow chafa mediainfo ffmpeg poppler bind whois
```

#### Debian / Ubuntu
```bash
sudo apt update && sudo apt install -y fastfetch bat eza jq chafa mediainfo ffmpeg poppler-utils dnsutils whois
```

---

## License

This software is distributed under the terms of the GNU General Public License v3.0 or MIT License as specified in the LICENSE file.
