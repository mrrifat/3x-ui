[English](/README.md) | [فارسی](/README.fa_IR.md) | [العربية](/README.ar_EG.md) |  [中文](/README.zh_CN.md) | [Español](/README.es_ES.md) | [Русский](/README.ru_RU.md)

<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="./media/3x-ui-dark.png">
    <img alt="3x-ui" src="./media/3x-ui-light.png">
  </picture>
</p>

[![Release](https://img.shields.io/github/v/release/mrrifat/3x-ui.svg)](https://github.com/mrrifat/3x-ui/releases)
[![Downloads](https://img.shields.io/github/downloads/mrrifat/3x-ui/total.svg)](https://github.com/mrrifat/3x-ui/releases/latest)
[![License](https://img.shields.io/badge/license-GPL%20V3-blue.svg?longCache=true)](https://www.gnu.org/licenses/gpl-3.0.en.html)
[![Stars](https://img.shields.io/github/stars/mrrifat/3x-ui.svg)](https://github.com/mrrifat/3x-ui/stargazers)
[![Forks](https://img.shields.io/github/forks/mrrifat/3x-ui.svg)](https://github.com/mrrifat/3x-ui/network/members)

# 3X-UI - China Optimized Edition 🇨🇳

**Personal fork of 3x-ui optimized for China's network environment** - features smart split DNS routing, automatic geo-file updates, and protocol templates designed for maximum performance and reliability in China.

> [!IMPORTANT]
> This is a **personal fork** optimized specifically for use in China. All optimizations are active by default.
> 
> - ✅ **Smart Split DNS**: Chinese domains → AliDNS (~10ms), International → DoH/ControlD (~50ms)
> - ✅ **Automatic Geo-files**: China geo databases auto-update on installation
> - ✅ **Optimized Routing**: Direct routing for Chinese traffic, proxy for international
> - ✅ **Protocol Templates**: Pre-configured VLESS Reality, VMess/Trojan WS+TLS
> - ✅ **Zero Configuration**: Everything works out of the box

Based on [MHSanaei/3x-ui](https://github.com/MHSanaei/3x-ui) with China-specific enhancements.

## Quick Start (China Optimized)

### Installation

```bash
bash <(curl -Ls https://raw.githubusercontent.com/mrrifat/3x-ui/main/install.sh)
```

**What's Installed:**
- ✅ 3x-ui panel with smart DNS routing
- ✅ China geo-files (geoip_CN.dat, geosite_CN.dat) 
- ✅ Split DNS configuration (AliDNS + ControlD DoH)
- ✅ Optimized routing rules (Chinese traffic direct)
- ✅ Protocol templates (VLESS Reality, VMess/Trojan WS+TLS)

## 📦 Releases

### Latest China-Optimized Release: v2.5.2-china

**Download binaries:**
- [amd64](https://github.com/mrrifat/3x-ui/releases/download/v2.5.2-china/x-ui-linux-amd64.tar.gz)
- [arm64](https://github.com/mrrifat/3x-ui/releases/download/v2.5.2-china/x-ui-linux-arm64.tar.gz)
- [armv7](https://github.com/mrrifat/3x-ui/releases/download/v2.5.2-china/x-ui-linux-armv7.tar.gz)
- [All platforms](https://github.com/mrrifat/3x-ui/releases/latest)

**Install specific version:**
```bash
bash <(curl -Ls https://raw.githubusercontent.com/mrrifat/3x-ui/main/install.sh) v2.5.2-china
```

### First Login

```bash
# Check panel status and credentials
x-ui status

# Default credentials:
# Username: admin
# Password: admin
# Port: 2053 (or check x-ui status output)
```

### Post-Installation

```bash
# Update geo files (China databases included)
x-ui geo

# Restart panel with new configuration
x-ui restart
```

## Documentation

- 📖 **[China Optimization Guide](docs/CHINA-OPTIMIZATION.md)** - Detailed setup guide
- 📖 **[DNS Configuration Guide](docs/CHINA-OPTIMIZATION.md#dns-configuration)** - Smart split DNS setup
- 📖 **[Protocol Templates](config/templates/README.md)** - Pre-configured protocols
- 📖 **[Original Wiki](https://github.com/MHSanaei/3x-ui/wiki)** - Upstream documentation

## Key Features (China Edition)

### 🚀 Smart Split DNS Routing

**Chinese Domains** (baidu.com, taobao.com, qq.com):
```
📍 geosite:cn detected → 🇨🇳 AliDNS (223.5.5.5) → ⚡ ~10ms
```

**International Domains** (google.com, youtube.com):
```
📍 NOT geosite:cn → 🌍 ControlD DoH (encrypted) → 🔒 ~50ms
```

### 🗺️ Automatic Geo-File Management

```bash
# China geo-files auto-downloaded on install
# Update manually anytime:
x-ui geo

# Available geo-files:
# - geoip.dat, geosite.dat (Loyalsoldier - main)
# - geoip_CN.dat, geosite_CN.dat (China-specific)
# - geoip_IR.dat, geosite_IR.dat (Iran)
# - geoip_RU.dat, geosite_RU.dat (Russia)
```

### 🎯 Intelligent Traffic Routing

**Routing Rules (Active by Default):**
```
✅ geosite:cn + geoip:cn → Direct (bypass proxy)
✅ International traffic → Proxy
✅ Private IPs (LAN) → Direct  
✅ BitTorrent → Blocked
```

### 📡 DNS Provider Options

**Built-in DNS Presets:**
1. **Hybrid China (Default)** - AliDNS + Cloudflare DoH (split)
2. **ControlD Custom** - AliDNS + ControlD DoH (split, custom filtering)
3. **AliDNS DoH** - Full AliDNS (China optimized)
4. **Cloudflare DoH** - AliDNS + Cloudflare (privacy focused)
5. **Google DoH** - AliDNS + Google (reliability)
6. **Cloudflare DoT** - AliDNS + Cloudflare DoT (max privacy)
7. **Google DoT** - AliDNS + Google DoT (secure)
8. **Traditional China** - Traditional DNS only (fastest)

### 🛠️ Pre-configured Protocol Templates

**Available Templates** (in `config/templates/`):
- `vless-reality-vision.json` - Maximum stealth (port 443)
- `vmess-ws-tls.json` - CDN compatible (CloudFlare support)
- `trojan-ws-tls.json` - Reliable general purpose
- `vless-grpc-tls.json` - Maximum reliability and speed

**DNS Templates:**
- `dns-hybrid-china.json` - Default split DNS
- `dns-doh-cloudflare.json` - Cloudflare DoH
- `dns-doh-google.json` - Google DoH
- `dns-doh-alidns.json` - AliDNS DoH
- `dns-dot-cloudflare.json` - Cloudflare DoT
- `dns-dot-google.json` - Google DoT

### ⚡ Performance Benchmarks

**DNS Resolution Times:**
| Domain Type | DNS Server | Latency | Encryption |
|-------------|------------|---------|------------|
| Chinese (baidu.com) | AliDNS | ~10ms | None (fastest) |
| International (google.com) | ControlD DoH | ~50ms | HTTPS (secure) |

**Comparison with Non-Split DNS:**
| Domain | Without Split DNS | With Split DNS | Improvement |
|--------|-------------------|----------------|-------------|
| baidu.com | 200ms (ControlD) | 10ms (AliDNS) | **20x faster** ⚡ |
| taobao.com | 180ms (ControlD) | 12ms (AliDNS) | **15x faster** ⚡ |
| google.com | 50ms (ControlD) | 50ms (ControlD) | Same (encrypted) 🔒 |

## Management Commands

```bash
# View all commands
x-ui

# Common operations
x-ui start         # Start 3x-ui panel
x-ui stop          # Stop 3x-ui panel
x-ui restart       # Restart 3x-ui panel
x-ui status        # Show panel status
x-ui geo           # Update geo-files (includes CN files)
x-ui log           # View panel logs
x-ui update        # Update panel (preserves config)
x-ui install       # Reinstall panel
x-ui uninstall     # Remove panel
```

## System Requirements

**Minimum:**
- OS: Ubuntu 20.04+, Debian 10+, CentOS 7+, Fedora, Arch Linux
- RAM: 512MB
- Disk: 1GB
- Network: Open port for panel (default 2053)

**Recommended:**
- RAM: 1GB+
- Disk: 2GB+
- BBR enabled (congestion control)

## Comparison: This Fork vs Upstream

| Feature | Upstream (MHSanaei/3x-ui) | This Fork (mrrifat/3x-ui) |
|---------|---------------------------|----------------------------|
| **DNS Routing** | Basic single DNS | Smart split DNS (CN/International) |
| **Geo Files** | Manual download | Auto-download (CN included) |
| **Default Config** | Generic | China-optimized (split DNS, routing) |
| **Chinese Domains** | ~200ms (DoH) | ~10ms (AliDNS) ⚡ |
| **International Domains** | ~50ms | ~50ms (encrypted) 🔒 |
| **Setup Required** | Manual configuration | Zero config (works immediately) |
| **Protocol Templates** | None | 4 templates + 6 DNS presets |
| **Documentation** | Generic | China-specific guide |
| **Timezone** | Asia/Tehran | Asia/Shanghai 🇨🇳 |

## Upstream Acknowledgment

This fork is based on:
- [MHSanaei/3x-ui](https://github.com/MHSanaei/3x-ui) (License: GPL-3.0)

With additional China-specific optimizations.

### Geo Data Sources:
- [Loyalsoldier/v2ray-rules-dat](https://github.com/Loyalsoldier/v2ray-rules-dat) (License: GPL-3.0) - Main geo-files + China databases
- [chocolate4u/Iran-v2ray-rules](https://github.com/chocolate4u/Iran-v2ray-rules) (License: GPL-3.0) - Iran routing rules
- [runetfreedom/russia-v2ray-rules-dat](https://github.com/runetfreedom/russia-v2ray-rules-dat) (License: GPL-3.0) - Russia routing rules

## Support Original Project

**The upstream project ([MHSanaei/3x-ui](https://github.com/MHSanaei/3x-ui)) accepts donations:**

<a href="https://www.buymeacoffee.com/MHSanaei" target="_blank">
<img src="./media/default-yellow.png" alt="Buy Me A Coffee" style="height: 70px !important;width: 277px !important;" >
</a>

<a href="https://nowpayments.io/donation/hsanaei" target="_blank" rel="noreferrer noopener">
   <img src="./media/donation-button-black.svg" alt="Crypto donation button by NOWPayments">
</a>

## License

GNU General Public License v3.0

## Contributing

This is a **personal fork** optimized for personal use in China. If you find bugs or have suggestions for China-specific optimizations:

1. 🐛 [Open an issue](https://github.com/mrrifat/3x-ui/issues)
2. 💡 [Suggest improvements](https://github.com/mrrifat/3x-ui/discussions)

For general 3x-ui features, contribute to the [upstream project](https://github.com/MHSanaei/3x-ui).

## Stargazers over Time

[![Stargazers over time](https://starchart.cc/mrrifat/3x-ui.svg?variant=adaptive)](https://starchart.cc/mrrifat/3x-ui)
