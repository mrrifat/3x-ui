# China Network Optimization Guide

## Performance Settings

### 1. Enable BBR Congestion Control
Run from x-ui menu:
```bash
x-ui
# Select option 23: Enable BBR
```

### 2. Transport Protocols Priority (Best to Worst)
1. **gRPC** - Best reliability and speed
2. **Reality** - Best stealth
3. **WebSocket** - Best CDN compatibility
4. **TCP** - Good for direct connections
5. **mKCP** - For unstable connections

### 3. Multiplexing (Mux)
- Enable for multiple connections
- Reduces latency for many small requests
- Configure in subscription settings

### 4. Recommended Settings for Speed
- **Protocol**: VLESS + gRPC + TLS
- **Port**: 443
- **BBR**: Enabled
- **Mux**: Enabled (concurrency: 8)
- **DNS**: Split DNS (China domestic + ControlD foreign)

### 5. Recommended Settings for Reliability
- **Protocol**: VLESS + Reality
- **Port**: 443
- **BBR**: Enabled
- **Flow**: xtls-rprx-vision
- **Fallback**: www.microsoft.com:443

## Quick Setup Steps

1. Install 3x-ui:
   ```bash
   bash <(curl -Ls https://raw.githubusercontent.com/mrrifat/3x-ui/main/install.sh)
   ```

2. Enable BBR (Menu option 23)

3. Update geo files including China (Menu option 24, then option 5)

4. Import protocol template (use files from config/templates/)

5. Configure your domain and certificates

6. Test connection and enjoy!

## Troubleshooting

### Slow speeds?
- Enable BBR
- Try gRPC protocol
- Check if routing rules are correct (China direct, foreign proxy)

### Connection unstable?
- Try Reality protocol
- Use standard port 443
- Enable Flow control (xtls-rprx-vision)

### Detected/Blocked?
- Switch to Reality protocol
- Change destination domain
- Use random ports

---

## DNS over HTTPS (DoH) and DNS over TLS (DoT)

### Why Use DoH/DoT?

**Problems with Traditional DNS:**
- ❌ DNS queries sent in plain text
- ❌ Easy to intercept and poison
- ❌ No encryption
- ❌ Can be blocked by ISP
- ❌ Subject to man-in-the-middle attacks

**Benefits of DoH/DoT:**
- ✅ Encrypted DNS queries
- ✅ Prevents DNS poisoning
- ✅ Bypasses DNS blocking
- ✅ Privacy protection
- ✅ Faster resolution (often)
- ✅ Better for streaming and sensitive sites

---

### DoH vs DoT: Which to Use?

| Feature | DoH (DNS over HTTPS) | DoT (DNS over TLS) |
|---------|---------------------|-------------------|
| Protocol | HTTPS (Port 443) | TLS (Port 853) |
| Detection | Harder to detect | Easier to detect |
| Performance | Slightly slower | Slightly faster |
| Compatibility | Better (uses HTTPS) | May be blocked |
| **Recommended for China** | ✅ **YES** | ⚠️ May be blocked |

**Recommendation for China: Use DoH** (harder to detect and block)

---

### Default Configuration

3x-ui is **pre-configured with DoH by default**:

**Server DNS** (`web/service/config.json`):
- Chinese domains → 223.5.5.5 (AliDNS)
- Foreign domains → https://223.5.5.5/dns-query (DoH)
- Fallback → https://1.1.1.1/dns-query (Cloudflare DoH)
- Final fallback → 8.8.8.8 (Google DNS)

**Client DNS** (`sub/default.json`):
- Same configuration as server
- Works automatically with subscriptions

---

### Available DoH/DoT Templates

Pre-configured templates are in `config/templates/`:

#### 1. **dns-doh-cloudflare.json**
**Best for: Global access, privacy**
- Provider: Cloudflare
- DoH endpoints: https://1.1.1.1/dns-query, https://1.0.0.1/dns-query
- Fast, reliable, privacy-focused
- Works well in China

#### 2. **dns-doh-google.json**
**Best for: Reliability, speed**
- Provider: Google
- DoH endpoints: https://8.8.8.8/dns-query, https://8.8.4.4/dns-query
- Very fast, reliable
- May be slower in China

#### 3. **dns-doh-alidns.json**
**Best for: China users** ⭐ RECOMMENDED
- Provider: Alibaba Cloud
- DoH endpoints: https://223.5.5.5/dns-query, https://223.6.6.6/dns-query
- Optimized for China network
- Fast domestic resolution
- Includes Cloudflare fallback for foreign sites

#### 4. **dns-dot-cloudflare.json**
**Best for: Maximum privacy**
- Provider: Cloudflare
- DoT endpoints: tls://1.1.1.1, tls://1.0.0.1
- More private than DoH
- ⚠️ Port 853 may be blocked in some regions

#### 5. **dns-dot-google.json**
**Best for: Speed**
- Provider: Google
- DoT endpoints: tls://8.8.8.8, tls://8.8.4.4
- Fast resolution
- ⚠️ Port 853 may be blocked in some regions

#### 6. **dns-hybrid-china.json**
**Best for: Maximum reliability in China** ⭐ DEFAULT
- Combines traditional DNS + DoH + DoT
- Chinese domains: Traditional DNS (fastest)
- Foreign domains: DoH (secure)
- Multiple fallbacks
- **This is the default configuration**

---

### How to Use DoH/DoT Templates

#### Method 1: Apply to Server Configuration

1. **Navigate to templates folder:**
   ```bash
   cd /usr/local/x-ui/config/templates
   ```

2. **View available templates:**
   ```bash
   ls dns-*.json
   ```

3. **Apply template to server config:**
   ```bash
   # Backup current config
   cp /usr/local/x-ui/web/service/config.json /usr/local/x-ui/web/service/config.json.backup
   
   # Copy DNS section from template
   # Example: Use AliDNS DoH
   cat dns-doh-alidns.json
   # Then manually copy the "dns" section to config.json
   ```

4. **Restart x-ui:**
   ```bash
   x-ui restart
   ```

#### Method 2: Apply to Client Subscriptions

1. **Edit subscription template:**
   ```bash
   nano /usr/local/x-ui/sub/default.json
   ```

2. **Replace "dns" section** with template content

3. **Save and restart:**
   ```bash
   x-ui restart
   ```

4. **Clients will get new DNS config** on next subscription update

#### Method 3: Web Panel (Future Enhancement)

In future versions, you'll be able to:
- Select DoH/DoT provider from dropdown
- Toggle between traditional DNS and DoH/DoT
- Test DNS performance
- (This feature is planned)

---

### Testing DoH/DoT Configuration

#### Test if DoH is Working:

```bash
# Check DNS queries in logs
x-ui log | grep "dns query"

# Should show HTTPS queries instead of plain DNS
```

#### Test DNS Resolution Speed:

```bash
# Test domain resolution
nslookup google.com
nslookup baidu.com
nslookup www.ixxx.com

# Should resolve in < 500ms
```

#### Test from Client:

1. Connect to VPN
2. Visit: https://1.1.1.1/help
3. Check if "Using DNS over HTTPS (DoH)" shows ✅
4. Check if "Using DNS over TLS (DoT)" shows ✅ (if using DoT)

---

### Troubleshooting DoH/DoT

#### Issue: DNS still timing out

**Solution:**
1. Check if DoH endpoint is accessible:
   ```bash
   curl -v https://1.1.1.1/dns-query
   ```

2. If blocked, switch to different provider:
   - Try AliDNS: https://223.5.5.5/dns-query
   - Try Google: https://8.8.8.8/dns-query

3. Add traditional DNS fallback:
   ```json
   {
     "servers": [
       "https://1.1.1.1/dns-query",
       "1.1.1.1",
       "8.8.8.8"
     ]
   }
   ```

#### Issue: Slow resolution with DoH

**Solution:**
1. DoH adds ~50-100ms latency
2. Use hybrid config (traditional DNS for China, DoH for foreign)
3. Enable DNS caching:
   ```json
   {
     "dns": {
       "disableCache": false
     }
   }
   ```

#### Issue: Streaming sites still not working

**Solution:**
1. Some streaming services detect DoH/VPN
2. Try different DoH provider
3. Use specific routing rules:
   ```json
   {
     "routing": {
       "rules": [
         {
           "type": "field",
           "domain": ["geosite:netflix", "geosite:youtube"],
           "outboundTag": "proxy"
         }
       ]
     }
   }
   ```

#### Issue: DoT (port 853) blocked

**Solution:**
1. Switch to DoH (uses port 443, harder to block)
2. Use template: `dns-doh-alidns.json` or `dns-doh-cloudflare.json`
3. DoH disguises as HTTPS traffic

---

### Recommended Configurations

#### For Maximum Speed (China):
```json
{
  "dns": {
    "servers": [
      "223.5.5.5",
      "119.29.29.29",
      "https://1.1.1.1/dns-query",
      "1.1.1.1"
    ],
    "queryStrategy": "UseIPv4"
  }
}
```

#### For Maximum Privacy:
```json
{
  "dns": {
    "servers": [
      "https://1.1.1.1/dns-query",
      "tls://1.1.1.1",
      "1.1.1.1"
    ],
    "queryStrategy": "UseIPv4"
  }
}
```

#### For Maximum Reliability (Default):
```json
{
  "dns": {
    "servers": [
      { "address": "223.5.5.5", "domains": ["geosite:cn"] },
      { "address": "119.29.29.29", "domains": ["geosite:cn"] },
      { "address": "https://223.5.5.5/dns-query", "domains": ["geosite:geolocation-!cn"] },
      { "address": "https://1.1.1.1/dns-query" },
      "1.1.1.1",
      "8.8.8.8"
    ],
    "queryStrategy": "UseIPv4",
    "disableFallback": false
  }
}
```

---

### DNS Performance Comparison

| Provider | Type | Latency (China) | Privacy | Reliability | Blocked? |
|----------|------|----------------|---------|-------------|----------|
| AliDNS (223.5.5.5) | Traditional | ~10ms | Low | High | No |
| AliDNS DoH | DoH | ~50ms | High | High | No |
| Cloudflare DoH | DoH | ~100ms | Very High | High | Rare |
| Google DoH | DoH | ~150ms | Medium | High | Sometimes |
| Cloudflare DoT | DoT | ~80ms | Very High | Medium | Sometimes |

**Best for China: AliDNS (traditional) + Cloudflare DoH (fallback)** ← This is the default!

---

### Additional DNS Providers

If default providers don't work, try these alternatives:

#### China DNS Providers:
- **DNSPod**: 119.29.29.29
- **DNSPod DoH**: https://doh.pub/dns-query
- **AliDNS**: 223.5.5.5, 223.6.6.6
- **AliDNS DoH**: https://223.5.5.5/dns-query

#### International DNS Providers:
- **Cloudflare**: 1.1.1.1, 1.0.0.1
- **Cloudflare DoH**: https://1.1.1.1/dns-query
- **Google**: 8.8.8.8, 8.8.4.4
- **Google DoH**: https://8.8.8.8/dns-query
- **Quad9**: 9.9.9.9
- **Quad9 DoH**: https://dns.quad9.net/dns-query
- **ControlD**: p2.freedns.controld.com:5353

---

### Summary

✅ **DoH/DoT is enabled by default** in 3x-ui  
✅ **No manual configuration needed** for most users  
✅ **Multiple fallback DNS servers** for reliability  
✅ **Optimized for China network** with AliDNS  
✅ **Templates available** for easy customization  
✅ **Solves DNS timeout issues** from the logs  

**Your DNS problems should be fixed automatically!** 🎉

