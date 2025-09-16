# transmission Installation Guide

transmission is a free and open-source BitTorrent client. Transmission provides a fast, easy, and free BitTorrent client with web interface

## Table of Contents
1. [Prerequisites](#prerequisites)
2. [Supported Operating Systems](#supported-operating-systems)
3. [Installation](#installation)
4. [Configuration](#configuration)
5. [Service Management](#service-management)
6. [Troubleshooting](#troubleshooting)
7. [Security Considerations](#security-considerations)
8. [Performance Tuning](#performance-tuning)
9. [Backup and Restore](#backup-and-restore)
10. [System Requirements](#system-requirements)
11. [Support](#support)
12. [Contributing](#contributing)
13. [License](#license)
14. [Acknowledgments](#acknowledgments)
15. [Version History](#version-history)
16. [Appendices](#appendices)

## 1. Prerequisites

- **Hardware Requirements**:
  - CPU: 1 core minimum
  - RAM: 256MB minimum
  - Storage: 1GB for downloads
  - Network: BitTorrent protocol
- **Operating System**: 
  - Linux: Any modern distribution (RHEL, Debian, Ubuntu, CentOS, Fedora, Arch, Alpine, openSUSE)
  - macOS: 10.14+ (Mojave or newer)
  - Windows: Windows Server 2016+ or Windows 10
  - FreeBSD: 11.0+
- **Network Requirements**:
  - Port 9091 (default transmission port)
  - Peer ports 51413
- **Dependencies**:
  - See official documentation for specific requirements
- **System Access**: root or sudo privileges required


## 2. Supported Operating Systems

This guide supports installation on:
- RHEL 8/9 and derivatives (CentOS Stream, Rocky Linux, AlmaLinux)
- Debian 11/12
- Ubuntu 20.04/22.04/24.04 LTS
- Arch Linux (rolling release)
- Alpine Linux 3.18+
- openSUSE Leap 15.5+ / Tumbleweed
- SUSE Linux Enterprise Server (SLES) 15+
- macOS 12+ (Monterey and later) 
- FreeBSD 13+
- Windows 10/11/Server 2019+ (where applicable)

## 3. Installation

### RHEL/CentOS/Rocky Linux/AlmaLinux

```bash
# Install EPEL repository if needed
sudo dnf install -y epel-release

# Install transmission
sudo dnf install -y transmission

# Enable and start service
sudo systemctl enable --now transmission

# Configure firewall
sudo firewall-cmd --permanent --add-port=9091/tcp
sudo firewall-cmd --reload

# Verify installation
transmission --version
```

### Debian/Ubuntu

```bash
# Update package index
sudo apt update

# Install transmission
sudo apt install -y transmission

# Enable and start service
sudo systemctl enable --now transmission

# Configure firewall
sudo ufw allow 9091

# Verify installation
transmission --version
```

### Arch Linux

```bash
# Install transmission
sudo pacman -S transmission

# Enable and start service
sudo systemctl enable --now transmission

# Verify installation
transmission --version
```

### Alpine Linux

```bash
# Install transmission
apk add --no-cache transmission

# Enable and start service
rc-update add transmission default
rc-service transmission start

# Verify installation
transmission --version
```

### openSUSE/SLES

```bash
# Install transmission
sudo zypper install -y transmission

# Enable and start service
sudo systemctl enable --now transmission

# Configure firewall
sudo firewall-cmd --permanent --add-port=9091/tcp
sudo firewall-cmd --reload

# Verify installation
transmission --version
```

### macOS

```bash
# Using Homebrew
brew install transmission

# Start service
brew services start transmission

# Verify installation
transmission --version
```

### FreeBSD

```bash
# Using pkg
pkg install transmission

# Enable in rc.conf
echo 'transmission_enable="YES"' >> /etc/rc.conf

# Start service
service transmission start

# Verify installation
transmission --version
```

### Windows

```bash
# Using Chocolatey
choco install transmission

# Or using Scoop
scoop install transmission

# Verify installation
transmission --version
```

## Initial Configuration

### Basic Configuration

```bash
# Create configuration directory
sudo mkdir -p /etc/transmission

# Set up basic configuration
# See official documentation for detailed configuration options

# Test configuration
transmission --version
```

## 5. Service Management

### systemd (RHEL, Debian, Ubuntu, Arch, openSUSE)

```bash
# Enable service
sudo systemctl enable transmission

# Start service
sudo systemctl start transmission

# Stop service
sudo systemctl stop transmission

# Restart service
sudo systemctl restart transmission

# Check status
sudo systemctl status transmission

# View logs
sudo journalctl -u transmission -f
```

### OpenRC (Alpine Linux)

```bash
# Enable service
rc-update add transmission default

# Start service
rc-service transmission start

# Stop service
rc-service transmission stop

# Restart service
rc-service transmission restart

# Check status
rc-service transmission status
```

### rc.d (FreeBSD)

```bash
# Enable in /etc/rc.conf
echo 'transmission_enable="YES"' >> /etc/rc.conf

# Start service
service transmission start

# Stop service
service transmission stop

# Restart service
service transmission restart

# Check status
service transmission status
```

### launchd (macOS)

```bash
# Using Homebrew services
brew services start transmission
brew services stop transmission
brew services restart transmission

# Check status
brew services list | grep transmission
```

### Windows Service Manager

```powershell
# Start service
net start transmission

# Stop service
net stop transmission

# Using PowerShell
Start-Service transmission
Stop-Service transmission
Restart-Service transmission

# Check status
Get-Service transmission
```

## Advanced Configuration

### Speed and Connection Limits
```json
{
    "alt-speed-down": 50,
    "alt-speed-enabled": false,
    "alt-speed-time-begin": 540,
    "alt-speed-time-day": 127,
    "alt-speed-time-enabled": false,
    "alt-speed-time-end": 1020,
    "alt-speed-up": 50,
    "speed-limit-down": 100,
    "speed-limit-down-enabled": false,
    "speed-limit-up": 100,
    "speed-limit-up-enabled": false,
    "peer-limit-global": 200,
    "peer-limit-per-torrent": 50
}
```

### Queue Management
```json
{
    "download-queue-enabled": true,
    "download-queue-size": 5,
    "queue-stalled-enabled": true,
    "queue-stalled-minutes": 30,
    "seed-queue-enabled": true,
    "seed-queue-size": 10
}
```

### Bandwidth Scheduling
```json
{
    "idle-seeding-limit": 30,
    "idle-seeding-limit-enabled": true,
    "ratio-limit": 2.0,
    "ratio-limit-enabled": true
}
```

### Blocklist Configuration
```json
{
    "blocklist-enabled": true,
    "blocklist-url": "http://john.bitsurge.net/public/biglist.p2p.gz"
}
```

### DHT and Peer Exchange
```json
{
    "dht-enabled": true,
    "lpd-enabled": true,
    "pex-enabled": true,
    "utp-enabled": true
}
```

### Encryption Settings
```json
{
    "encryption": 2
}
```
Where: 0 = Prefer unencrypted, 1 = Prefer encrypted, 2 = Require encrypted

### Script Hooks
```json
{
    "script-torrent-done-enabled": true,
    "script-torrent-done-filename": "/opt/transmission/scripts/complete.sh"
}
```

Example completion script:
```bash
#!/bin/bash
# /opt/transmission/scripts/complete.sh
# Called when download completes
# Environment variables:
# TR_TORRENT_DIR, TR_TORRENT_NAME, TR_TORRENT_ID, TR_TORRENT_HASH

logger "Transmission: Download completed - $TR_TORRENT_NAME"

# Send notification
if command -v notify-send &> /dev/null; then
    notify-send "Download Complete" "$TR_TORRENT_NAME"
fi

# Move to media library
if [[ "$TR_TORRENT_DIR" == *"/downloads/movies"* ]]; then
    mv "$TR_TORRENT_DIR/$TR_TORRENT_NAME" "/media/movies/"
elif [[ "$TR_TORRENT_DIR" == *"/downloads/tv"* ]]; then
    mv "$TR_TORRENT_DIR/$TR_TORRENT_NAME" "/media/tv/"
fi
```

## Reverse Proxy Setup

### nginx Configuration

```nginx
upstream transmission_backend {
    server 127.0.0.1:9091;
}

server {
    listen 80;
    server_name transmission.example.com;
    return 301 https://$server_name$request_uri;
}

server {
    listen 443 ssl http2;
    server_name transmission.example.com;

    ssl_certificate /etc/ssl/certs/transmission.example.com.crt;
    ssl_certificate_key /etc/ssl/private/transmission.example.com.key;

    location / {
        proxy_pass http://transmission_backend;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

### Apache Configuration

```apache
<VirtualHost *:80>
    ServerName transmission.example.com
    Redirect permanent / https://transmission.example.com/
</VirtualHost>

<VirtualHost *:443>
    ServerName transmission.example.com
    
    SSLEngine on
    SSLCertificateFile /etc/ssl/certs/transmission.example.com.crt
    SSLCertificateKeyFile /etc/ssl/private/transmission.example.com.key
    
    ProxyRequests Off
    ProxyPreserveHost On
    
    ProxyPass / http://127.0.0.1:9091/
    ProxyPassReverse / http://127.0.0.1:9091/
</VirtualHost>
```

### HAProxy Configuration

```haproxy
frontend transmission_frontend
    bind *:80
    bind *:443 ssl crt /etc/ssl/certs/transmission.pem
    redirect scheme https if !{ ssl_fc }
    default_backend transmission_backend

backend transmission_backend
    balance roundrobin
    server transmission1 127.0.0.1:9091 check
```

## Security Configuration

### Basic Security Setup

```bash
# Set appropriate permissions
sudo chown -R transmission:transmission /etc/transmission
sudo chmod 750 /etc/transmission

# Configure firewall
sudo firewall-cmd --permanent --add-port=9091/tcp
sudo firewall-cmd --reload

# Enable SELinux policies (if applicable)
sudo setsebool -P httpd_can_network_connect on
```

## Database Setup

See official documentation for database configuration requirements.

## Performance Optimization

### System Tuning

```bash
# Basic system tuning
echo 'net.core.somaxconn = 65535' | sudo tee -a /etc/sysctl.conf
echo 'net.ipv4.tcp_max_syn_backlog = 65535' | sudo tee -a /etc/sysctl.conf
sudo sysctl -p
```

## Monitoring

### Basic Monitoring

```bash
# Check service status
sudo systemctl status transmission

# View logs
sudo journalctl -u transmission -f

# Monitor resource usage
top -p $(pgrep transmission)
```

## 9. Backup and Restore

### Backup Script

```bash
#!/bin/bash
# Basic backup script
BACKUP_DIR="/backup/transmission"
DATE=$(date +%Y%m%d_%H%M%S)

mkdir -p "$BACKUP_DIR"
tar -czf "$BACKUP_DIR/transmission-backup-$DATE.tar.gz" /etc/transmission /var/lib/transmission

echo "Backup completed: $BACKUP_DIR/transmission-backup-$DATE.tar.gz"
```

### Restore Procedure

```bash
# Stop service
sudo systemctl stop transmission

# Restore from backup
tar -xzf /backup/transmission/transmission-backup-*.tar.gz -C /

# Start service
sudo systemctl start transmission
```

## 6. Troubleshooting

### Common Issues

1. **Service won't start**:
```bash
# Check logs
sudo journalctl -u transmission -n 100
sudo tail -f /var/log/transmission/transmission.log

# Check configuration
transmission --version

# Check permissions
ls -la /etc/transmission
```

2. **Connection issues**:
```bash
# Check if service is listening
sudo ss -tlnp | grep 9091

# Test connectivity
telnet localhost 9091

# Check firewall
sudo firewall-cmd --list-all
```

3. **Performance issues**:
```bash
# Check resource usage
top -p $(pgrep transmission)

# Check disk I/O
iotop -p $(pgrep transmission)

# Check connections
ss -an | grep 9091
```

## Integration Examples

### Docker Compose Example

```yaml
version: '3.8'
services:
  transmission:
    image: transmission:latest
    ports:
      - "9091:9091"
    volumes:
      - ./config:/etc/transmission
      - ./data:/var/lib/transmission
    restart: unless-stopped
```

## Maintenance

### Update Procedures

```bash
# RHEL/CentOS/Rocky/AlmaLinux
sudo dnf update transmission

# Debian/Ubuntu
sudo apt update && sudo apt upgrade transmission

# Arch Linux
sudo pacman -Syu transmission

# Alpine Linux
apk update && apk upgrade transmission

# openSUSE
sudo zypper update transmission

# FreeBSD
pkg update && pkg upgrade transmission

# Always backup before updates
tar -czf /backup/transmission-pre-update-$(date +%Y%m%d).tar.gz /etc/transmission

# Restart after updates
sudo systemctl restart transmission
```

### Regular Maintenance

```bash
# Log rotation
sudo logrotate -f /etc/logrotate.d/transmission

# Clean old logs
find /var/log/transmission -name "*.log" -mtime +30 -delete

# Check disk usage
du -sh /var/lib/transmission
```

## Additional Resources

- Official Documentation: https://docs.transmission.org/
- GitHub Repository: https://github.com/transmission/transmission
- Community Forum: https://forum.transmission.org/
- Best Practices Guide: https://docs.transmission.org/best-practices

---

**Note:** This guide is part of the [HowToMgr](https://howtomgr.github.io) collection. Always refer to official documentation for the most up-to-date information.
