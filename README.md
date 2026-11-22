<div align="center">

# ✦ Abdelrahman AutoScript ✦

**Install and manage a multi-protocol VPN server from one colorful terminal menu.**

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Debian](https://img.shields.io/badge/Debian-11%20%7C%2012-A81D33?logo=debian&logoColor=white)](#compatibility-intended-targets)
[![Ubuntu](https://img.shields.io/badge/Ubuntu-20.04%20%7C%2022.04-E95420?logo=ubuntu&logoColor=white)](#compatibility-intended-targets)
[![Shell](https://img.shields.io/badge/Built%20with-Bash%20%2B%20Go-2EA44F?logo=gnu-bash&logoColor=white)](#features)

*Developed by Eng. Abdelrahman Rabie*

</div>

AutoscriptXRAY is a collection of Bash installers and small Go services that set up SSH over WebSocket-style/SSL transports, an Xray core (VMess, VLESS, Trojan, Shadowsocks), WireGuard and UDP tunnels on a fresh VPS, and manage them through a single interactive menu. Every screen shares one violet-to-magenta theme, defined once in `theme.sh`.

> **Status:** personal project under active development. The feature list describes what the scripts are designed to install and manage. No independent installation or connectivity validation has been performed on this code, so treat the tables below as design intent, not a support guarantee.

## Features

| Category    | Features                                                                          |
| ----------- | --------------------------------------------------------------------------------- |
| SSH WS      | OpenSSH, Dropbear, SSH WS, SSH SSL WS, Enhanced Payload, UDP Custom, BadVPN UDPGW |
| XRAY Core   | VMess, VLESS, Trojan, Shadowsocks, gRPC, NGINX Reverse Proxy, Auto SSL            |
| WireGuard   | WireGuard VPN, Client Generator, QR Code                                          |
| UDP Tunnel  | UDP Custom, ZIVPN UDP, BadVPN UDPGW                                               |
| Management  | Backup, Domain Manager, Speedtest, Service Checker, Traffic Monitor               |
| Dashboard   | Interactive Panel, Service Status, User Statistics, Traffic Statistics            |
| Go Services | Native Dropbear WS, Native Stunnel WS, Multi-Connection Support                   |

Open the menu any time after installation:

```bash
menu
```

## Quick Install

> **Note:** this script must be run as root, on a fresh VPS.

```bash
# update
apt update -y && apt upgrade -y
apt install git curl screen sudo -y

# disable ipv6
sysctl -w net.ipv6.conf.all.disable_ipv6=1
sysctl -w net.ipv6.conf.default.disable_ipv6=1

# clone the repo
git clone https://github.com/0xArxploit/AutoscriptXRAY.git
cd AutoscriptXRAY

# run the main installer
chmod +x setup.sh
screen -S setup ./setup.sh
```

## Default Ports

| Service        | Port    |
| -------------- | ------- |
| OpenSSH        | 22      |
| Dropbear       | 109,143 |
| SSH WS         | 2082    |
| SSH SSL WS     | 2096    |
| BadVPN UDPGW   | 7300    |
| UDP Custom     | 1-65535 |
| VMess TLS      | 443     |
| VMess None TLS | 80      |
| VLESS TLS      | 443     |
| Trojan TLS     | 443     |
| Shadowsocks WS | 443     |

## Transport note: SSH "WebSocket" services

The SSH WS services (`sshws/ws-dropbear.py`, `sshws/ws-stunnel.py` and the Go variants in `internal/go/`) implement a minimal HTTP `101 Switching Protocols` upgrade tunnel, the kind injector-style clients such as HTTP Injector or KPN Tunnel expect. They do not implement RFC 6455 WebSocket framing, so standards-compliant WebSocket clients are not interoperable with them. The Xray protocols are served by Xray-core behind NGINX with proper upgrade handling and are unaffected by this limitation.

## Debugging

Check listening ports:

```bash
ss -tulpn
```

Check NGINX:

```bash
nginx -t
systemctl status nginx
```

Check XRAY:

```bash
xray -test -config /etc/xray/config.json
systemctl status xray
```

Check SSH WS:

```bash
systemctl status ws-dropbear
systemctl status ws-stunnel
```

Check the UDP tunnels:

```bash
systemctl status udp-custom
systemctl status udpgw
```

## Compatibility (intended targets)

These are the installer's intended targets, as designed. They are not independently verified: behavior can vary between provider images and configurations.

| OS           | Intended status |
| ------------ | --------------- |
| Debian 12    | ⭐ Recommended   |
| Debian 11    | ✅ Intended      |
| Ubuntu 22.04 | ✅ Intended      |
| Ubuntu 20.04 | ⚠ Limited       |
| Debian 10    | ❌ Deprecated    |
| OpenVZ       | ❌ Unsupported   |
| KVM          | ✅ Recommended   |
| VMware       | ✅ Recommended   |

## Notes

* Recommended on a fresh VPS, minimum 1 GB RAM
* A domain is required for XRAY TLS
* Cloudflare supported
* Enhanced payload supported

## Theme

All screens share one palette defined in `theme.sh` (installed to `/etc/autoscriptvpn/theme.sh`).
Change the colours once and every menu follows. Requires a 256-colour terminal.

## Credits

This project stands on the shoulders of:

* XTLS / Xray-core
* BadVPN
* WireGuard
* acme.sh
* NGINX

## License

Copyright (c) 2026 Eng. Abdelrahman Rabie. Based on AutoscriptXRAY (MIT), see `LICENSE` and `NOTICE`.
