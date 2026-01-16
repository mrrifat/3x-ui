# Quick Start Guide - China Edition

## Installation (One Command)

```bash
bash <(curl -Ls https://raw.githubusercontent.com/mrrifat/3x-ui/main/install.sh)
```

**What happens:**
1. ✅ Installs 3x-ui panel
2. ✅ Downloads China geo-files automatically
3. ✅ Configures smart split DNS (AliDNS + ControlD DoH)
4. ✅ Sets up optimized routing rules
5. ✅ Installs protocol templates

**Installation time:** ~2-5 minutes

---

## First Login

### 1. Get Panel Credentials

```bash
x-ui status
```

**Output:**
```
3x-ui is running
Web panel: http://YOUR_SERVER_IP:2053
Username: admin
Password: admin
```

### 2. Login to Web Panel

1. Open browser: `http://YOUR_SERVER_IP:2053`
2. Login with credentials (change password immediately!)
3. Panel language: English (switch if needed)

---

## Quick Configuration (3 Steps)

### Step 1: Create Inbound (Protocol)

1. Navigate to: **Inbounds** → **Add Inbound**
2. Choose protocol template:
   - **VLESS Reality** (Recommended - stealth)
   - **VMess WS+TLS** (CDN compatible)
   - **Trojan WS+TLS** (reliable)
   - **VLESS gRPC** (fast)

3. Click **Add** → Done!

**Your inbound link is ready** - copy it from the panel

### Step 2: Verify DNS Configuration

1. Navigate to: **Panel Settings** → **Xray Settings** → **DNS**
2. Verify default config:

```json
{
  "servers": [
    { "address": "223.5.5.5", "domains": ["geosite:cn"] },
    { "address": "119.29.29.29", "domains": ["geosite:cn"] },
    { "address": "https://1.1.1.1/dns-query" },
    { "address": "8.8.8.8" }
  ]
}
```

✅ **If you see this** - DNS is correctly configured!  
❌ **If different** - use "DNS Provider Selector" to choose "Hybrid China"

### Step 3: Test Connection

1. Import inbound link to your client (v2rayN, Clash, etc.)
2. Configure your client to use the proxy (typically SOCKS5 on port 10808 or HTTP on port 10809)
3. Test Chinese site:
   ```bash
   # If using SOCKS5 proxy on port 10808
   curl -x socks5://127.0.0.1:10808 baidu.com
   ```
4. Test international site:
   ```bash
   curl -x socks5://127.0.0.1:10808 google.com
   ```

✅ **Both work?** - You're all set!

> [!NOTE]
> The proxy port (10808) may vary depending on your client configuration. Check your client settings for the correct port.

---

## Performance Verification

### Test DNS Speed

```bash
# Test Chinese domain
time curl -H "Accept: application/dns-json" \
  "https://223.5.5.5/dns-query?name=baidu.com&type=A"

# Expected: ~10ms ⚡
```

```bash
# Test international domain  
time curl -H "Accept: application/dns-json" \
  "https://1.1.1.1/dns-query?name=google.com&type=A"

# Expected: ~50ms 🔒
```

### Test Routing

**Chinese Site (Should be fast):**
```bash
curl -o /dev/null -s -w "Time: %{time_total}s\n" \
  -x socks5://127.0.0.1:10808 https://baidu.com

# Expected: ~0.2s (fast direct routing)
```

**International Site (Through proxy):**
```bash
curl -o /dev/null -s -w "Time: %{time_total}s\n" \
  -x socks5://127.0.0.1:10808 https://google.com

# Expected: ~1-2s (through proxy)
```

---

## Troubleshooting

### Issue: Chinese sites are slow

**Check DNS configuration:**
```bash
# View current Xray config
cat /usr/local/x-ui/bin/config.json | grep -A 20 '"dns"'
```

**Expected:** Chinese domains should route to 223.5.5.5 or 119.29.29.29

**Fix:**
1. Go to Panel → Xray Settings → DNS
2. Select "Hybrid China" from DNS Provider dropdown
3. Click Apply → Restart Xray

### Issue: International sites not working

**Check routing rules:**
```bash
# View routing config
cat /usr/local/x-ui/bin/config.json | grep -A 30 '"routing"'
```

**Expected:** International traffic should NOT have "geosite:cn" in rules

**Fix:**
1. Go to Panel → Xray Settings → Routing
2. Verify rule order:
   - Rule 1: geosite:cn → direct
   - Rule 2: geoip:cn → direct  
   - Rule 3: (default) → proxy

### Issue: Geo-files not found

**Update geo-files:**
```bash
# Update all geo-files (includes China files)
x-ui geo

# Or manually:
cd /usr/local/x-ui/bin
curl -sfLRO https://github.com/Loyalsoldier/v2ray-rules-dat/releases/latest/download/geoip.dat
curl -sfLRO https://github.com/Loyalsoldier/v2ray-rules-dat/releases/latest/download/geosite.dat
```

---

## Next Steps

### Optimize Performance

1. **Enable BBR** (congestion control):
   ```bash
   echo "net.core.default_qdisc=fq" >> /etc/sysctl.conf
   echo "net.ipv4.tcp_congestion_control=bbr" >> /etc/sysctl.conf
   sysctl -p
   ```

2. **Use CDN** (if available):
   - CloudFlare: Use VMess/Trojan WS+TLS on ports 443, 8443, 2053, 2083, 2087, 2096

3. **Monitor Performance**:
   - Panel → Dashboard → View traffic statistics
   - Check logs: `x-ui log`

### Customize DNS

**Want different DNS providers?**

1. Panel → Xray Settings → DNS → DNS Provider Selector
2. Choose from 8 presets:
   - Hybrid China (default)
   - ControlD Custom (custom filtering)
   - AliDNS DoH
   - Cloudflare DoH
   - Google DoH
   - Cloudflare DoT
   - Google DoT
   - Traditional China

3. Click Apply → Test → Restart

---

## Useful Commands

```bash
# Panel management
x-ui start          # Start panel
x-ui stop           # Stop panel
x-ui restart        # Restart panel
x-ui status         # Show status
x-ui log            # View logs

# Updates
x-ui update         # Update panel
x-ui geo            # Update geo-files

# Troubleshooting
x-ui logs           # Full logs
systemctl status x-ui.service  # Service status
```

---

## Summary: What You Have Now

✅ **Smart DNS Routing:**
- Chinese domains: ~10ms (AliDNS)
- International: ~50ms (DoH encrypted)

✅ **Optimized Traffic Routing:**
- Chinese traffic: Direct (fast)
- International: Through proxy

✅ **Auto-Updated Geo-files:**
- China databases included
- Updates with `x-ui geo`

✅ **Protocol Templates:**
- VLESS Reality (stealth)
- VMess/Trojan WS+TLS (CDN)
- VLESS gRPC (fast)

✅ **Zero Configuration:**
- Everything works out of the box!

**You're ready to use your optimized 3x-ui panel! 🚀**
