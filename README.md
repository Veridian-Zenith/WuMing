# <img src="./data/icons/hicolor/scalable/apps/com.ericlin.wuming.svg" height="64"/> WuMing

**A modern, lightweight GUI frontend for [ClamAV](https://www.clamav.net/)**

WuMing (无名) is a GTK4/Libadwaita desktop application for scanning files and directories for malware using ClamAV. Built for Linux users who want full control over their system's security.

[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)

## Features

| Feature | Description |
|---------|-------------|
| **Fast Scanning** | Uses `clamdscan` (daemon-based) for rapid performance, falls back to `clamscan` when unavailable |
| **Modern UI** | Built with GTK4 and Libadwaita — integrates with GNOME, Phosh, and other libadwaita-based desktops |
| **Manual Control** | No auto-quarantining — you decide what to do with detected threats |
| **Signature Management** | Update ClamAV virus definitions directly from the app |
| **Security Overview** | Dashboard showing signature freshness, last scan time, and daemon status |
| **Drag & Drop** | Drop files or folders onto the window to scan them instantly |
| **Scan Options** | Customize scan behavior: archives, PUA detection, mail scanning, and more |

## Screenshots

<details>
<summary><b>Light Mode</b></summary>

![Security Overview](imgs/overview-light.png)
![Scan Page](imgs/scan-light.png)
![Update Page](imgs/update-light.png)

</details>

<details>
<summary><b>Dark Mode</b></summary>

![Security Overview](imgs/overview-dark.png)
![Scan Page](imgs/scan-dark.png)
![Update Page](imgs/update-dark.png)

</details>

## Requirements

- Linux with GTK 4 and Libadwaita
- [ClamAV](https://www.clamav.net/) (`clamdscan` and/or `clamscan`)
- Meson build system

## Installation

### Build from source

```sh
meson setup build --prefix=/usr
sudo ninja -C build install
```

See [SETUP.md](SETUP.md) for recommended ClamAV configuration (running `clamd` as root for full filesystem access).

### Flatpak

*Coming soon.*

## Usage

Launch `wuming` from your application menu or terminal.

- **Scan a file/folder:** Click the scan button, or drag & drop onto the window
- **Update signatures:** Click the update button on the overview or update page
- **Delete threats:** After a scan, review detected threats and choose to delete or keep each one
- **Keyboard shortcuts:** Press `?` to view available shortcuts

## How it works

```
┌─────────────┐     ┌──────────────┐     ┌────────────┐
│  WuMing UI  │────▶│  clamdscan   │────▶│  clamd     │
│  (GTK4)     │     │  (or clamscan)│    │  (daemon)  │
└─────────────┘     └──────────────┘     └────────────┘
```

WuMing communicates with ClamAV through its command-line tools. When the ClamAV daemon (`clamd`) is running, it uses `clamdscan` for fast, daemon-based scanning. When the daemon is unavailable, it falls back to the slower `clamscan` directly.

## Roadmap

- [x] Update ClamAV signatures
- [x] Scan files and directories
- [x] Take action on infected files (manual control)
- [x] Security overview page
- [x] Settings page
- [x] Scan options customization
- [x] Drag & drop support
- [x] Toast and desktop notifications
- [ ] Scheduled scans
- [ ] Scan history

## Contributing

Contributions are welcome! Please open an issue or pull request on [GitHub](https://github.com/EricLin0509/WuMing).

## License

WuMing is licensed under the [GNU General Public License v3.0](LICENSE).
