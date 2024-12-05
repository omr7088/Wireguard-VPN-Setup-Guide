# Wireguard-VPN-Setup-Guide
# A step-by-step guide to creating your own VPN server using DigitalOcean, Docker, and Wireguard.

## Table of Contents
1. DigitalOcean Setup
2. Server Configuration
3. VPN Server Setup
4. Client Setup
5. Proof of Working VPN

## DigitalOcean Setup
1. Create a DigitalOcean Account:
   - Visit(https://www.digitalocean.com)
   - Sign up with your email
   - Add payment method (required for free credit)
   - Apply free credit promotion

2. Create a Droplet:
   - Click "Create" → "Droplets"
   - Choose Ubuntu 22.04 LTS
   - Select Basic Plan
   - Pick $6/month option (1GB RAM/1CPU)
   - Choose a datacenter region
   - Set up SSH or Password authentication
   - Click "Create Droplet"
   ![[1.png]]
   Screenshot: Droplet creation confirmation

## Server Configuration
1. Connect to your server:
```
ssh root@your_server_ip
```

2. Update system and install Docker:
```
# Update system
sudo apt-get update

# Install Docker
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh

# Install Docker Compose
sudo apt-get install docker-compose
```

## VPN Server Setup
1. Create Wireguard directory:
```
mkdir wireguard
cd wireguard
```

2. Create docker-compose.yml:
```
version: "3.8"
services:
  wireguard:
    image: linuxserver/wireguard
    container_name: wireguard
    cap_add:
      - NET_ADMIN
      - SYS_MODULE
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=America/New_York
      - SERVERURL=auto
      - SERVERPORT=51820
      - PEERS=2
      - PEERDNS=auto
      - INTERNAL_SUBNET=10.13.13.0
    volumes:
      - ./config:/config
      - /lib/modules:/lib/modules
    ports:
      - 51820:51820/udp
    sysctls:
      - net.ipv4.conf.all.src_valid_mark=1
    restart: unless-stopped
```

3. Start Wireguard:
```
docker-compose up -d
```

## Client Setup

### PC Client Setup
1. Download Wireguard:
 https://www.wireguard.com/install/

2. Install and Configure:
   - Open Wireguard
   - Click "Add Tunnel"
   - Import configuration from peer1.conf
   - Click "Activate"

### Mobile Client Setup
1. Install Wireguard app:
   - Android: Google Play Store
   - iPhone: App Store

2. Configure:
   - Open Wireguard app
   - Click "+"
   - Scan QR code from server
   - Enable VPN
## Proof of Working VPN

### Before VPN Connection
IP Address check before connecting to VPN:

### After VPN Connection
IP Address check after connecting to VPN:

### Connection Details
- PC Client Status: Connected
- Mobile Client Status: Connected
- VPN Server IP: 104.248.235.89

## Troubleshooting Tips
1. Cannot connect to server:
   - Verify droplet is running
   - Check firewall settings
   - Confirm correct IP address

2. View Wireguard logs:
```
docker-compose logs -f wireguard
```
