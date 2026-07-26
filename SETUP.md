# Setup Guide

## Building & Installing

### Prerequisites

- GTK 4 and Libadwaita development libraries
- ClamAV (`clamdscan` and/or `clamscan`)
- Meson >= 1.0.0
- A C compiler (GCC or Clang)

On Debian/Ubuntu:
```sh
sudo apt install libgtk-4-dev libadwaita-1-dev clamav clamav-daemon meson
```

On Fedora:
```sh
sudo dnf install gtk4-devel libadwaita-devel clamav clamav-update meson
```

On Arch:
```sh
sudo pacman -S gtk4 libadwaita clamav meson
```

### Build

```sh
meson setup build --prefix=/usr
sudo ninja -C build install
```

### Uninstall

```sh
sudo ninja -C build uninstall
```

---

## ClamAV Configuration (Recommended)

By default, `clamd` runs as the `clamav` user, which **cannot read most user files**. This means `clamdscan` will return "Permission denied" errors on files owned by other users.

To allow WuMing to scan all files on the system, configure `clamd` to run as root.

### 1. Override the systemd service

```sh
sudo mkdir -p /etc/systemd/system/clamav-daemon.service.d
sudo tee /etc/systemd/system/clamav-daemon.service.d/override.conf << 'EOF'
[Service]
User=root
Group=root
EOF
```

### 2. Update clamd.conf

```sh
sudo sed -i 's/^User clamav/User root/' /etc/clamav/clamd.conf
```

### 3. Ensure clamd's temp directory exists

```sh
sudo mkdir -p /etc/clamav/tmp
```

> **Note:** WuMing will also attempt to create this directory automatically if it is missing.

### 4. Restart clamd

```sh
sudo systemctl daemon-reload
sudo systemctl restart clamav-daemon
```

### 5. Verify

```sh
ps -o user,comm -p $(pgrep clamd)
# Should show: root clamd
```

---

## Troubleshooting

### "ClamAV daemon is not running" toast

WuMing falls back to `clamscan` (slower, non-daemon) when `clamd` is not running. Make sure the service is active:

```sh
systemctl status clamav-daemon
```

### Permission denied errors during scan

If you see permission errors, either:
- Configure `clamd` to run as root (see above), or
- Run WuMing as root (not recommended for regular use)

### Signature database is outdated

Update virus definitions manually:

```sh
sudo freshclam
```

Or use the update button in WuMing's overview page.

### clamd fails to start

Check that the temp directory exists:

```sh
ls -la /etc/clamav/tmp
# If missing: sudo mkdir -p /etc/clamav/tmp
```

Check clamd logs:

```sh
journalctl -u clamav-daemon --no-pager -n 50
```

---

## Keyboard Shortcuts

| Shortcut | Action |
|----------|--------|
| `Ctrl+O` | Scan a file |
| `Ctrl+Shift+O` | Scan a folder |
| `Ctrl+U` | Update signatures |
| `?` | Show all shortcuts |
