# Protocol Templates for China

These templates are optimized for reliability and speed in China's network environment.

## Templates Available

### 1. VLESS + Reality (vless-reality-vision.json)
- **Best for**: Maximum stealth and anti-detection
- **Use case**: Main protocol for bypassing censorship
- **Port**: 443 (standard HTTPS)
- **Setup**: Requires valid destination domain (e.g., www.microsoft.com)

### 2. VMess + WebSocket + TLS (vmess-ws-tls.json)
- **Best for**: CDN compatibility (CloudFlare, etc.)
- **Use case**: When you need CDN protection
- **Port**: 443, 8443, 2053, 2083, 2087, 2096
- **Setup**: Requires valid TLS certificate

### 3. Trojan + WebSocket + TLS (trojan-ws-tls.json)
- **Best for**: Reliable connection with good disguise
- **Use case**: General purpose proxy
- **Port**: 443
- **Setup**: Requires valid TLS certificate

### 4. VLESS + gRPC + TLS (vless-grpc-tls.json)
- **Best for**: Maximum reliability and speed
- **Use case**: When stability is critical
- **Port**: 443
- **Setup**: Requires valid TLS certificate

## Configuration Tips

### For Maximum Reliability:
1. Use VLESS + Reality or VLESS + gRPC + TLS
2. Enable BBR congestion control
3. Use standard ports (443)

### For CDN Support:
1. Use VMess/Trojan + WebSocket + TLS
2. Use CDN-friendly ports: 443, 8443, 2053, 2083, 2087, 2096
3. Configure proper SNI and Host headers

### DNS Configuration:

**Default (Smart Split DNS):**
- Chinese domains (geosite:cn): 223.5.5.5, 119.29.29.29 (Traditional DNS - ~10ms)
- International domains: ControlD DoH, Cloudflare DoH (Encrypted - ~50ms)
- Fallback: 8.8.8.8, 1.1.1.1

**Benefits:**
- ✅ Chinese sites: Fast (10ms) - Traditional DNS
- ✅ International sites: Secure (50ms) - DoH/DoT
- ✅ Best of both worlds

**Alternative DNS Presets:**
1. **Hybrid China** - AliDNS + Cloudflare DoH (default)
2. **ControlD Custom** - AliDNS + ControlD (custom filtering)
3. **AliDNS DoH** - Full AliDNS with DoH for international
4. **Cloudflare DoH** - Privacy-focused
5. **Google DoH** - Reliability-focused
6. **Cloudflare DoT** - Maximum privacy (DoT)
7. **Google DoT** - Secure DNS
8. **Traditional China** - Fastest (no encryption)

See [DNS Configuration Guide](../docs/CHINA-OPTIMIZATION.md#dns-configuration) for details.

### Routing:
- Chinese domains/IPs: Direct connection (bypass proxy)
- International: Through proxy
- Private IPs: Blocked
