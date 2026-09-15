# oshieteo (教えてお)

> *"oshiete yo, oshiete yo sono shikumi wo..."* ☕🩸

**oshieteo** is a zero-configuration, all-in-one terminal inspector and Swiss Army knife.

Stop memorizing flags for `tar`, parsing JSON manually, searching for how to inspect systemd daemons, or looking up terminal image viewers. Hand whatever you have to `oshieteo` — it detects what it is and explains it to you instantly.

---

## ⚡ Quick Demo

```bash
$ oshieteo oshieteo
sono shikumi wo

$ oshieteo 1000-7
1000 - 7 = 993

$ oshieteo zxc
# Launches Dota 2 via Steam (steam://run/570)
```

---

## 🚀 Features & Behaviors

| Input | What it does | Tool used |
|---|---|---|
| *(no arguments)* | Full system & hardware summary | `fastfetch` / `neofetch` |
| `script.sh`, `config.conf` | Syntax highlighted file viewer | `bat` (fallback `cat`) |
| `/var/log/`, `path/to/dir/` | Rich directory listing with icons | `eza` (fallback `ls -la`) |
| `repo/` *(with `.git`)* | Git repository stats & commit graph | `onefetch`, `git status`, `git log` |
| `archive.tar.gz`, `.zip`, `.7z` | Lists contents inside archive without extracting | `tar`, `unzip -l`, `7z l`, `bsdtar` |
| `photo.png`, `avatar.jpg` | High-res pixel-art render inside terminal | `chafa` (fallback `catimg`) |
| `music.flac`, `video.mp4` | Complete audio/video track & codec inspection | `mediainfo` / `ffprobe` |
| `document.pdf` | Metadata, page count & text preview | `pdfinfo`, `pdftotext` |
| `data.json` | Formatted and colorized JSON tree | `jq` (fallback `python3 json.tool`) |
| `README.md` | Formatted rich Markdown rendering | `glow` (fallback `bat`) |
| `https://domain.com/file.zip` | Smart download | `wget` / `curl -O` |
| `https://example.com/article` | Terminal reader mode | `lynx -dump` / `links` / `curl` |
| `8.8.8.8` | Network reachability test | `ping -c 4` |
| `google.com` | WHOIS and DNS record lookup | `whois` / `dig` |
| `nginx`, `sshd`, `docker` | Real-time systemd service status | `systemctl status` |
| Any executable binary | Automatic help inspection | `--help` / `-h` |

---

## 💀 Lore & Easter Eggs

- **`oshieteo 1000-7`** / **`oshieteo 993-7`** / **`oshieteo 986`**:
  Calculates the next step in the sequence `(X + 1) % 7 == 0`. When reaching `<= 0`, prints `dead inside.`.
- **`oshieteo zxc`**:
  Activates ZXC mode and launches Dota 2 directly through Steam.
- **`oshieteo sono`** / **`oshieteo shikumi`**:
  `BOKU NO NAKA NI DARE GA IRU NO?`
- **Object not found**:
  `❌ boku no naka ni dare ga iru no? (object not found: '...')`

---

## 📦 Installation

Clone the repository and symlink `oshieteo` to your `$PATH`:

```bash
git clone https://github.com/YOUR_USERNAME/oshiheteo.git
cd oshiheteo
chmod +x oshieteo

# Install locally to ~/.local/bin
mkdir -p ~/.local/bin
ln -s "$(pwd)/oshieteo" ~/.local/bin/oshieteo
ln -s "$(pwd)/oshieteo" ~/.local/bin/oshiheteo
```

Make sure `~/.local/bin` is in your `PATH` (usually present in `~/.bashrc` or `~/.zshrc`):
```bash
export PATH="$HOME/.local/bin:$PATH"
```

### Recommended Packages

`oshieteo` uses standard fallback tools (`cat`, `ls`, `curl`, `file`), but works best with modern CLI utilities:

**Arch Linux:**
```bash
sudo pacman -S fastfetch bat eza jq glow chafa mediainfo ffmpeg poppler bind whois
```

**Debian / Ubuntu:**
```bash
sudo apt update && sudo apt install -y fastfetch bat eza jq chafa mediainfo ffmpeg poppler-utils dnsutils whois
```

---

## 📜 License

[GPL-3.0 / MIT](LICENSE)
