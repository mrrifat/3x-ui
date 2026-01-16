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
