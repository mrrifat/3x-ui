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

---

## DNS Configuration Templates

### DoH (DNS over HTTPS) Templates

#### dns-doh-cloudflare.json
- **Best for**: Privacy, global access
- **Latency**: ~100ms from China
- **Works**: Excellent in most regions
- **Use case**: When privacy is priority

#### dns-doh-google.json
- **Best for**: Speed, reliability
- **Latency**: ~150ms from China
- **Works**: May be slower in China
- **Use case**: General purpose, non-China

#### dns-doh-alidns.json ⭐ RECOMMENDED FOR CHINA
- **Best for**: China users, speed
- **Latency**: ~50ms from China
- **Works**: Optimized for China network
- **Use case**: Primary choice for China-based users

### DoT (DNS over TLS) Templates

#### dns-dot-cloudflare.json
- **Best for**: Maximum privacy
- **Latency**: ~80ms
- **Works**: May be blocked (port 853)
- **Use case**: When DoH is blocked

#### dns-dot-google.json
- **Best for**: Speed
- **Latency**: ~100ms
- **Works**: May be blocked (port 853)
- **Use case**: Alternative to Google DoH

### Hybrid Template

#### dns-hybrid-china.json ⭐ DEFAULT CONFIGURATION
- **Best for**: Maximum reliability
- **Combines**: Traditional DNS + DoH + DoT
- **Works**: Everywhere
- **Use case**: Default for all users

---

## Quick Setup

### Apply DoH Configuration:

```bash
# 1. Navigate to templates
cd /usr/local/x-ui/config/templates

# 2. View template
cat dns-doh-alidns.json

# 3. Copy DNS section to your config
nano /usr/local/x-ui/web/service/config.json
# Paste the "dns" section from template

# 4. Restart
x-ui restart
```

### Test Configuration:

```bash
# Check DNS is working
nslookup google.com
nslookup baidu.com

# Check logs for DoH queries
x-ui log | grep "dns"
```

---

## Performance Tips

1. **Use AliDNS DoH for China** - Fastest resolution
2. **Enable DNS caching** - Reduces repeated queries
3. **Use hybrid config** - Best reliability
4. **Multiple fallbacks** - Prevents timeout
5. **Traditional DNS for CN domains** - Fastest for domestic sites

---

For detailed information, see: `docs/CHINA-OPTIMIZATION.md`

