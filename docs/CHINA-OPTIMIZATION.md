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

## Using DNS Provider UI

The 3x-ui web panel now includes a user-friendly DNS provider selector that allows you to configure DNS settings without manual JSON editing.

### Quick Setup Guide

1. Navigate to **Xray Settings** → **DNS** tab in the web panel
2. Enable DNS by toggling the "Enable DNS" switch
3. Find the **DNS Provider Selector** panel
4. Select a provider from the dropdown menu
5. Click **Apply** to configure the DNS servers
6. Click **Test** to verify the configuration
7. Click **Save** at the top to apply changes

### Available DNS Providers

| Provider | Type | Description | Use Case |
|----------|------|-------------|----------|
| **Hybrid China (Default)** | Hybrid | Mixed DoH/DoT/Traditional | Best all-around performance for China |
| **AliDNS DoH** | DoH | Alibaba Cloud DNS over HTTPS | China-optimized, fast and reliable |
| **Cloudflare DoH** | DoH | Cloudflare DNS over HTTPS | Privacy-focused, global coverage |
| **Google DoH** | DoH | Google DNS over HTTPS | Reliable and fast worldwide |
| **Cloudflare DoT** | DoT | Cloudflare DNS over TLS | Maximum privacy and security |
| **Google DoT** | DoT | Google DNS over TLS | Secure DNS queries |
| **Traditional China DNS** | Traditional | Standard DNS servers | Maximum speed, lowest latency |
| **Custom Configuration** | Custom | Use existing configuration | Manual control |

### Provider Type Tags

- **DoH (DNS over HTTPS)**: Encrypted DNS queries via HTTPS protocol
- **DoT (DNS over TLS)**: Encrypted DNS queries via TLS protocol
- **Traditional**: Standard unencrypted DNS queries
- **Hybrid**: Combination of multiple DNS types for optimal performance

### Step-by-Step Instructions

#### For China Users (Recommended)
1. Select **"Hybrid China (Default)"** for best performance
2. This configuration uses:
   - AliDNS DoH for Chinese domains
   - Cloudflare DoH for international domains
   - Traditional DNS as fallback
3. Click **Apply** then **Save**

#### For Privacy-Focused Users
1. Select **"Cloudflare DoH"** or **"Cloudflare DoT"**
2. Provides encrypted DNS queries
3. Prevents DNS hijacking and tracking
4. Click **Apply** then **Save**

#### For Maximum Speed
1. Select **"Traditional China DNS"**
2. Uses multiple fast Chinese DNS servers
3. Lowest latency but not encrypted
4. Click **Apply** then **Save**

### Testing Your Configuration

After applying a DNS provider:
1. Click the **Test** button to verify configuration
2. The system will show a success message if DNS is working
3. Save your settings using the **Save** button at the top
4. Restart Xray using the **Restart** button

### Viewing Active DNS Servers

After applying a provider, scroll down to the **DNS** panel to see:
- List of active DNS servers
- Server addresses (DoH/DoT URLs or IP addresses)
- Domain routing rules
- Expected IP ranges

### Custom Configuration

If you need manual control:
1. Select **"Custom Configuration"** from the dropdown
2. Use the **DNS** panel below to manually add/edit DNS servers
3. Click the **+** button to add new DNS servers
4. Configure advanced options like domains and expectIPs

### Troubleshooting

**DNS not working?**
- Make sure DNS is enabled (toggle switch)
- Try a different provider
- Click Test to verify configuration
- Check Xray logs for errors

**Slow DNS resolution?**
- Try "Traditional China DNS" for lowest latency
- Make sure BBR is enabled
- Check your network connection

**Privacy concerns?**
- Use DoH or DoT providers (Cloudflare/Google)
- Avoid Traditional DNS
- Enable DNS encryption in Xray settings

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
