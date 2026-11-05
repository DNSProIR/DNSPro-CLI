# DNSPro CLI

A simple command-line tool to switch your Linux DNS to **DNSPro** (`80.244.11.244`) and restore your previous settings whenever you need.

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Python](https://img.shields.io/badge/python-3.8%2B-blue.svg)](https://www.python.org/)
[![Platform](https://img.shields.io/badge/platform-Linux-lightgrey.svg)](https://github.com/DNSProIR/DNSPro-CLI)
[![GitHub](https://img.shields.io/badge/GitHub-DNSProIR%2FDNSPro--CLI-black.svg)](https://github.com/DNSProIR/DNSPro-CLI)

---

## Features

- Set system DNS to DNSPro with a single command
- Roll back to your previous DNS configuration
- Check the current nameservers at any time
- Zero dependencies — uses the Python standard library only
- Works across common Linux distributions (Ubuntu, Debian, Fedora, Arch, and others)

---

## Requirements

- Linux
- Python 3.8 or newer
- Root privileges (`sudo`) for `set` and `rollback`

---

## Quick start

```bash
git clone https://github.com/DNSProIR/DNSPro-CLI.git
cd DNSPro-CLI

# Apply DNSPro DNS
sudo python3 dnspro.py set

# Check status
python3 dnspro.py status

# Restore previous DNS
sudo python3 dnspro.py rollback
```

---

## Usage

### Set DNSPro DNS

```bash
sudo python3 dnspro.py set
```

Example output:

```text
Backup saved to /etc/resolv.conf.dnspro.bak
DNS set to DNSPro (80.244.11.244)
```

### Show current status

```bash
python3 dnspro.py status
```

Example output:

```text
DNSPro CLI v1.0.0
Target DNS: 80.244.11.244
Backup: yes

Current nameservers:
  nameserver 80.244.11.244
```

### Roll back to previous DNS

```bash
sudo python3 dnspro.py rollback
```

Example output:

```text
DNS restored from backup.
```

### Show version

```bash
python3 dnspro.py version
```

### Help

```bash
python3 dnspro.py help
```

---

## How it works

DNSPro CLI keeps things intentionally simple:

1. **`set`** copies `/etc/resolv.conf` to `/etc/resolv.conf.dnspro.bak` (once), then writes DNSPro as the system nameserver.
2. **`rollback`** restores the backup file and removes it.
3. **`status`** reads the current nameserver entries from `/etc/resolv.conf`.

If `/etc/resolv.conf` is a symlink (common with `systemd-resolved`), the tool replaces it with a regular file when applying DNSPro settings so the change takes effect.

---

## Troubleshooting

| Issue | What to do |
| --- | --- |
| `Error: this command requires root` | Re-run with `sudo` for `set` and `rollback`. |
| `Error: no backup found` | Run `set` at least once before `rollback`. |
| DNS changes back after reboot or reconnect | NetworkManager or another network service may rewrite `/etc/resolv.conf`. Re-run `sudo python3 dnspro.py set`, or adjust your network manager DNS settings. |
| Permission denied writing `/etc/resolv.conf` | Ensure you are using `sudo` and that the file is writable by root. |

---

## Contributing

Contributions are welcome. Feel free to open an issue or submit a pull request if you find a bug or want to improve the tool.

---

## License

This project is licensed under the [MIT License](LICENSE).
