# Smart Split DNS Routing

## Overview

The Smart Split DNS feature intelligently routes DNS queries based on the geographic location of the target domain. This provides optimal performance and security by using the best DNS server for each type of domain.

## How It Works

### Chinese Domains (geosite:cn)
- **Route to:** AliDNS (223.5.5.5, 119.29.29.29)
- **Type:** Traditional DNS (UDP port 53)
- **Speed:** ~10ms (fastest possible)
- **Why:** Chinese DNS servers are geographically closest and provide the fastest resolution for Chinese domains

**Examples:** baidu.com, taobao.com, qq.com, jd.com, etc.

### International Domains (geosite:geolocation-!cn)
- **Route to:** DoH/DoT providers (Cloudflare, Google, ControlD, etc.)
- **Type:** DNS over HTTPS (DoH) or DNS over TLS (DoT)
- **Speed:** ~50ms
- **Why:** Encrypted DNS prevents DNS hijacking and provides privacy

**Examples:** google.com, youtube.com, facebook.com, etc.

## Available Presets

### 1. Hybrid China (Default) ⭐
**Best for:** Most users in China
```json
{
  "servers": [
    { "address": "223.5.5.5", "domains": ["geosite:cn"], "expectIPs": ["geoip:cn"] },
    { "address": "119.29.29.29", "domains": ["geosite:cn"], "expectIPs": ["geoip:cn"] },
    { "address": "https://223.5.5.5/dns-query", "domains": ["geosite:geolocation-!cn"] },
    { "address": "https://1.1.1.1/dns-query" },
    { "address": "tls://1.1.1.1" },
    { "address": "1.1.1.1" },
    { "address": "8.8.8.8" }
  ]
}
```

### 2. ControlD Custom (Smart Split)
**Best for:** Users who want custom filtering for international sites
```json
{
  "servers": [
    { "address": "223.5.5.5", "domains": ["geosite:cn"], "expectIPs": ["geoip:cn"] },
    { "address": "119.29.29.29", "domains": ["geosite:cn"], "expectIPs": ["geoip:cn"] },
    { "address": "https://dns.controld.com/20m7ehw60mm", "domains": ["geosite:geolocation-!cn"] },
    { "address": "20m7ehw60mm.dns.controld.com" },
    { "address": "https://1.1.1.1/dns-query" },
    { "address": "1.1.1.1" }
  ]
}
```

### 3. AliDNS DoH (China Optimized)
**Best for:** Users who trust Alibaba's DoH service
- Chinese domains → AliDNS traditional (~10ms)
- International domains → AliDNS DoH

### 4. Cloudflare DoH (Split DNS)
**Best for:** Privacy-focused users
- Chinese domains → AliDNS traditional (~10ms)
- International domains → Cloudflare DoH (1.1.1.1)

### 5. Google DoH (Split DNS)
**Best for:** Users who prefer Google's DNS
- Chinese domains → AliDNS traditional (~10ms)
- International domains → Google DoH (8.8.8.8)

### 6. Cloudflare DoT (Split DNS)
**Best for:** Users who prefer DNS over TLS
- Chinese domains → AliDNS traditional (~10ms)
- International domains → Cloudflare DoT

### 7. Google DoT (Split DNS)
**Best for:** Users who prefer Google's DoT
- Chinese domains → AliDNS traditional (~10ms)
- International domains → Google DoT

### 8. Traditional China DNS
**Best for:** Users who want maximum speed without encryption
- All domains → Traditional DNS
- No encryption, fastest possible

## Query Flow Examples

### Chinese Domain Query (baidu.com)
```
User requests baidu.com
  ↓
Xray checks: geosite:cn? YES
  ↓
Routes to: 223.5.5.5 (AliDNS)
  ↓
Response: 8ms ⚡
  ↓
Fast!
```

### International Domain Query (google.com)
```
User requests google.com
  ↓
Xray checks: geosite:cn? NO
  ↓
Routes to: https://1.1.1.1/dns-query (Cloudflare DoH)
  ↓
Response: 50ms (encrypted) 🔒
  ↓
Secure & fast!
```

### Adult Site Query (example.xxx)
```
User requests example.xxx
  ↓
Xray checks: geosite:cn? NO
  ↓
Routes to: https://dns.controld.com/20m7ehw60mm
  ↓
ControlD applies custom filtering
  ↓
Response: 48ms (encrypted) 🔒
  ↓
Works with filtering!
```

## Benefits

✅ **Best of both worlds**
- Fast Chinese sites (~10ms) using local DNS
- Secure international sites (~50ms) using encrypted DNS

✅ **No overhead on domestic traffic**
- Chinese sites get traditional DNS (fastest)
- Only international sites use encrypted DNS

✅ **Custom filtering available**
- ControlD preset allows custom filtering for international domains
- Chinese sites remain unaffected

✅ **Privacy & Security**
- Encrypted DNS prevents DNS hijacking
- ISP cannot see what international sites you're visiting

## Performance Summary

| Domain Type | DNS Server | Protocol | Average Latency |
|-------------|------------|----------|-----------------|
| Chinese | AliDNS (223.5.5.5) | Traditional (UDP) | ~10ms ⚡ |
| International | Cloudflare DoH | HTTPS | ~50ms 🔒 |
| International | Google DoH | HTTPS | ~50ms 🔒 |
| International | ControlD DoH | HTTPS | ~50ms 🔒 |

## Visual Indicators

The UI displays clear indicators for each DNS server:

- **🇨🇳 CN** - Server handles Chinese domains only
- **🌍 International** - Server handles international domains only
- **DoH** (Blue badge) - DNS over HTTPS
- **DoT** (Green badge) - DNS over TLS
- **UDP** (Gray badge) - Traditional DNS

## Configuration

1. Navigate to **Xray Settings** → **DNS** tab
2. Click **Add DNS Server** or select from **DNS Presets**
3. Choose a preset that matches your needs
4. Click **Install** to apply the configuration
5. Save and restart Xray

## Notes

- All presets include fallback DNS servers for redundancy
- The `expectIPs` field ensures Chinese domains return Chinese IPs
- Split DNS requires Xray-core with geosite/geoip data files
- For custom domains, you can manually configure routing rules
