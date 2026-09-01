# Complete VPS Self-Hosted Node Guide

> This repository provides comprehensive guidance on self-hosting a scientific internet access node using a VPS in 2026, covering the full process from provider selection and deployment to daily maintenance. Whether you're a Linux beginner or an experienced user, you'll find a suitable solution here. The goal: the most stable and fastest dedicated node at the lowest cost.

---

## Table of Contents

- [VPS vs Airport: Which Should You Choose?](#vps-vs-airport-which-should-you-choose)
- [Recommended VPS Providers (2026)](#recommended-vps-providers-2026)
- [Understanding Core Configuration Parameters](#understanding-core-configuration-parameters)
- [Region Selection & Route Analysis](#region-selection--route-analysis)
- [Full Purchase & Initialization Walkthrough](#full-purchase--initialization-walkthrough)
- [Deploying Popular Proxy Protocols](#deploying-popular-proxy-protocols)
- [Advanced Optimization: BBR, TCP Tuning, WireGuard](#advanced-optimization-bbr-tcp-tuning-wireguard)
- [Docker Containerized Deployment](#docker-containerized-deployment)
- [Security Hardening & Maintenance](#security-hardening--maintenance)
- [Troubleshooting Common Issues](#troubleshooting-common-issues)
- [Disclaimer](#disclaimer)

---

## VPS vs Airport: Which Should You Choose?

Before diving in, clarify your actual needs. VPS self-hosting and airport subscriptions each have distinct advantages:

| Dimension | VPS Self-Hosted | Airport Subscription |
|-----------|----------------|---------------------|
| **Technical Requirement** | Basic Linux knowledge and CLI competency | Zero barrier — subscribe and go |
| **Monthly Cost** | ~$2~15/month (annual plans cheaper) | ¥10~80/month |
| **Data Allowance** | Almost always unlimited | Usually has a monthly cap |
| **Bandwidth** | Dedicated (typically 1Gbps) | Shared — may throttle during peak |
| **Node Count** | 1~3 fixed nodes | 20~200+ nodes with instant switching |
| **IP Ban Risk** | You handle it (replace IP / switch provider) | Managed by the provider |
| **Maintenance Effort** | You maintain, update, and troubleshoot | Fully managed — zero effort |
| **Flexibility** | Fully customizable, deploy multiple services | Limited by plan terms |
| **Best For** | Tech-savvy users, heavy downloaders | Casual users, plug-and-play preference |

**When to choose VPS?**
- You have heavy data needs (sustained downloads, PT, live streaming, etc.)
- You have technical skills or are willing to learn basic Linux operations
- You need a stable dedicated bandwidth that won't be affected by others
- You want stronger privacy (traffic doesn't pass through a third party)
- You want to learn networking and have full control over your node

**When to choose an airport?**
- You're a pure beginner who doesn't want any hassle
- Your data needs are moderate (daily browsing, video streaming)
- You need a large pool of nodes to switch between freely
- You prefer to let someone else handle maintenance and troubleshooting

This repository focuses on helping users with a **self-hosting mindset** — building from zero to a fully operational VPS deployment.

---

## Recommended VPS Providers (2026)

### 🥇 Tier 1: Primary Recommendations

#### 1. RackNerd — Best Value

| Parameter | Details |
|-----------|---------|
| **Price** | Starting at $10.99/year (1 vCPU / 1GB RAM / 18GB SSD) |
| **Network** | 1Gbps shared, some plans at 10Gbps |
| **Locations** | Los Angeles, New York, Atlanta, San Jose, Dallas |
| **Payment** | PayPal, Credit Card, Alipay (some) |
| **Refund Policy** | 30-day money-back guarantee |
| **Highlights** | Extremely low prices, stable network, frequent promotions |

Best for: Budget-conscious beginners with moderate data needs.

#### 2. BandwagonHost (搬瓦工)

| Parameter | Details |
|-----------|---------|
| **Price** | Starting at $49.99/year (CN2 GIA routes) |
| **Network** | CN2 GIA / HKmediaVM |
| **Locations** | Hong Kong, Los Angeles, Fremont, New York, etc. |
| **Payment** | PayPal, Credit Card, Alipay |
| **Refund Policy** | Refund of remaining balance (not full refund) |
| **Highlights** | Excellent network quality, Alipay support, active Chinese community |

Best for: Users who prioritize network quality and are willing to pay a bit more for CN2 GIA optimization.

#### 3. CloudCone

| Parameter | Details |
|-----------|---------|
| **Price** | Starting at $1.80/month (during promotions) |
| **Network** | 1Gbps, Los Angeles datacenter |
| **Locations** | Los Angeles |
| **Payment** | PayPal, Credit Card |
| **Refund Policy** | Hourly billing — destroy anytime for refund |
| **Highlights** | Hourly billing, high flexibility |

Best for: Users needing short-term flexibility without being locked into annual commitments.

### 🥈 Tier 2: Specialized Recommendations

#### 4. HengHost (恒创科技)

| Parameter | Details |
|-----------|---------|
| **Price** | Starting at ¥29/month |
| **Network** | Optimized routes, BGP multi-line |
| **Locations** | Hong Kong, Japan, US |
| **Payment** | Alipay, WeChat Pay, PayPal |
| **Highlights** | Chinese-language support, ideal for Chinese users |

#### 5. DMIT

| Parameter | Details |
|-----------|---------|
| **Price** | Starting at $30/year |
| **Network** | CN2 GIA / IPLC Dedicated |
| **Locations** | Hong Kong, Japan, US |
| **Highlights** | High-end routes, exceptional stability |

#### 6. HostDare

| Parameter | Details |
|-----------|---------|
| **Price** | Starting at $14.99/quarter |
| **Network** | CN2 GIA Premium / Optimized |
| **Locations** | Los Angeles |
| **Highlights** | Niche boutique, solid network quality |

---

## Understanding Core Configuration Parameters

When purchasing a VPS, focus on these core parameters:

### 🖥️ CPU

| Type | Description | Use Case |
|------|-----------|---------|
| **Intel Xeon** | Stable, versatile | Best choice for most users |
| **AMD EPYC** | Strong multi-core, high concurrency | High concurrent connection workloads |
| **AMD Ryzen** | Great value | Sufficient for standard proxy usage |

> For proxy services, CPU is rarely the bottleneck. 1~2 cores is enough.

### 💾 Memory

| RAM Size | Capability |
|---------|-----------|
| **512MB** | Running a simple proxy (not recommended) |
| **1GB** | Single-node proxy + lightweight web |
| **2GB** | Proxy + Docker + small databases |
| **4GB+** | Multiple services + scripting workloads |

> Recommendation: Start with **1GB**, go for **2GB** if possible, **2GB+** for long-term use.

### 💿 Storage

| Type | Description |
|------|-------------|
| **HDD** | Cheap, slow read/write — not recommended |
| **SSD** | Fast — recommended |
| **NVMe SSD** | Ultra-fast, for high IO scenarios |

> Proxy services have modest disk IO requirements. 20~40GB SSD is sufficient.

### 🌐 Bandwidth & Data

| Parameter | Description |
|----------|-------------|
| **Bandwidth** | Usually shared (e.g., 1Gbps) — represents the ceiling |
| **Data** | Monthly cap vs. unlimited |
| **Overage Handling** | Throttle / charge extra / suspend after cap |

> Try to choose **unlimited** or **high-cap** plans to avoid running into limits monthly.

---

## Region Selection & Route Analysis

### 📊 VPS Region Comparison

| Region | Latency | Speed | Price | Rating | Best For |
|--------|---------|-------|-------|--------|---------|
| **Hong Kong** | 20~60ms | Excellent | High (¥100+/mo) | ⭐⭐⭐⭐⭐ | All-around best, lowest latency |
| **Japan** | 50~100ms | Fast | Moderate ($5~15/mo) | ⭐⭐⭐⭐⭐ | Video streaming / gaming — best value |
| **Singapore** | 80~150ms | Good | Moderate ($8~15/mo) | ⭐⭐⭐⭐ | Southeast Asia preferred |
| **US Los Angeles** | 150~250ms | Moderate | Low ($3~10/mo) | ⭐⭐⭐⭐ | Heavy downloads / long sessions |
| **US Other** | 180~300ms | Average | Low | ⭐⭐⭐ | Backup nodes |
| **South Korea** | 60~120ms | Fast | Moderate | ⭐⭐⭐⭐ | Balance of speed and cost |
| **Europe (NL/DE)** | 200~350ms | Average | Moderate | ⭐⭐⭐ | European target |

### 🏆 Route Type Deep Dive

#### CN2 GIA (Strongly Recommended)

China Telecom's premium optimized route — occupies dedicated international exit bandwidth, bypassing regular public internet congestion. Offers low latency, high speed, and smooth performance during peak hours. The top choice for Chinese users connecting to overseas VPS.

**How to identify:** Provider explicitly markets "CN2 GIA," or confirm on the provider's website before purchasing.

#### CN2 GT

CN2 Global Transit — one tier below CN2 GIA. Traffic partially traverses public internet, which may cause evening peak slowdowns. Still decent value.

#### 163 Backbone (Standard Routes)

The most common international exit — traverses the public internet. Congestion during evening peaks is common, speeds are inconsistent. Not recommended as your primary choice.

#### IPLC Dedicated Line

Physical fiber optic cross-border connection with dedicated bandwidth. Extremely high cost, typically used by airport providers at scale. Less common for individual users.

### 🎯 Purchasing Recommendations

```
Best picks for users in China:
1. Hong Kong CN2 GIA / HKmediaVM (most expensive, fastest)
2. Japan CN2 GIA (best value for money)
3. Los Angeles CN2 GIA (cheap, high data allowance)

By use case:
- Prioritize low latency → Hong Kong > Japan > South Korea
- Heavy downloads on a budget → US Los Angeles > Europe
- Balance both → Japan Tokyo / Osaka
```

---

## Full Purchase & Initialization Walkthrough

### Step 1: Register an Account

Using RackNerd as an example:
1. Visit racknerd.com
2. Click Sign Up, fill in email and password
3. Activate via email and log in to the control panel

> 💡 Use a real email address — you'll receive service notifications there.

### Step 2: Choose a Plan and Pay

1. Select your target datacenter (recommend Los Angeles DC9 / DC3)
2. Choose your configuration (recommend starting at the $10.99/year plan)
3. Add to cart
4. Checkout — select PayPal or credit card
5. VPS is provisioned within 15 minutes of payment

### Step 3: Access the Control Panel

RackNerd uses the **KiwiVM** control panel:
- Log in → "Client Main" → "My Services"
- Click the corresponding VPS → Control Panel

Common functions:

| Function | Purpose |
|---------|---------|
| **Root Password Modification** | Change root password |
| **Install New OS** | Reinstall OS (recommend Debian 11/12 or Ubuntu 22.04) |
| **Control Panel** | View VPS IP, port info |
| **Firewall** | Manage port firewall rules |

### Step 4: Connect via SSH

**Windows: Use PowerShell or Windows Terminal**

```powershell
ssh root@你的VPS_IP
```

> ⚠️ Password characters won't appear as you type — just enter your password and press Enter.

**Mac/Linux: Use Terminal directly**

```bash
ssh root@你的VPS_IP
```

### Step 5: Initialize the Server

After a fresh OS install, execute the following:

```bash
# Update the system
apt update && apt upgrade -y

# Install basic tools
apt install -y curl wget git vim unzip ca-certificates gnupg lsb-release

# Set timezone (for Chinese users)
ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
```

---

## Deploying Popular Proxy Protocols

### Option 1: Xray (Recommended — Best All-Rounder)

Xray is a V2Ray fork with better performance and more flexible configuration, supporting VLESS + XTLS / Trojan and more.

**Quick install script:**
```bash
bash <(curl -Ls https://raw.githubusercontent.com/mhsanaei/anti-replay/main/install.sh)
```

**Or mack-a's feature-rich script:**
```bash
bash <(curl -s -L https://raw.githubusercontent.com/mack-a/v2ray-agent/master/install.sh)
```

After installation, access the management panel (usually port 65432) to configure node information.

### Option 2: Hysteria2 (High Speed, Low Latency)

Hysteria2 is a QUIC-based high-speed protocol that maintains excellent throughput even in high packet-loss environments.

**Install (official script):**
```bash
curl -fsSL https://get.hy2.sh | bash
hysteria server -c /etc/hysteria/config.yaml --mode smart
```

### Option 3: WireGuard (Secure & Efficient)

WireGuard is the next-generation VPN protocol with kernel-level integration — exceptional performance and simple configuration.

**Server-side install:**
```bash
apt install -y wireguard
wg genkey | tee privatekey | wg pubkey > publickey
```

**Client configuration example:**
```ini
[Interface]
PrivateKey = <client private key>
Address = 10.0.0.2/24
DNS = 8.8.8.8

[Peer]
PublicKey = <server public key>
Endpoint = your_VPS_IP:51820
AllowedIPs = 0.0.0.0/0
PersistentKeepalive = 25
```

### Option 4: Shadowsocks + v2ray-plugin

A classic protocol that works well with legacy devices.

```bash
apt install -y shadowsocks-libev
```

---

## Advanced Optimization: BBR, TCP Tuning, WireGuard

### 🚀 Enable BBR Acceleration

BBR (Bottleneck Bandwidth and Round-trip propagation time) is Google's TCP congestion control algorithm that dramatically improves network throughput.

```bash
# Check current congestion control algorithm
sysctl net.ipv4.tcp_congestion_control

# Enable BBR
echo "net.core.default_qdisc=fq" >> /etc/sysctl.conf
echo "net.ipv4.tcp_congestion_control=bbr" >> /etc/sysctl.conf
sysctl -p

# Verify
sysctl net.ipv4.tcp_congestion_control
# Should output: net.ipv4.tcp_congestion_control = bbr
```

### 🔧 Tune System Parameters

Add the following to `/etc/sysctl.conf`:

```bash
# Maximum file descriptors
fs.file-max = 1000000

# Network parameter optimization
net.core.rmem_max = 16777216
net.core.wmem_max = 16777216
net.core.netdev_max_backlog = 65535
net.core.somaxconn = 65535

# TCP optimization
net.ipv4.tcp_max_syn_backlog = 65535
net.ipv4.tcp_fastopen = 3
net.ipv4.tcp_max_tw_buckets = 65535
net.ipv4.tcp_fin_timeout = 10
net.ipv4.tcp_keepalive_time = 1200
net.ipv4.tcp_tw_reuse = 1
net.ipv4.tcp_slow_start_after_idle = 0

sysctl -p
```

### 🛡️ Configure Firewall (ufw)

```bash
# Install
apt install -y ufw

# Default rules
ufw default deny incoming
ufw default allow outgoing

# Open SSH port (critical!)
ufw allow 22/tcp

# Open proxy port (assuming 443)
ufw allow 443/tcp

# Enable firewall
ufw enable
```

---

## Docker Containerized Deployment

Docker makes deploying and migrating proxy services significantly easier.

### Install Docker

```bash
curl -fsSL https://get.docker.com | sh
systemctl enable docker
```

### Deploy Xray with Docker Compose

Create `docker-compose.yml`:

```yaml
version: '3'
services:
  xray:
    image: teddysun/xray
    container_name: xray
    restart: always
    ports:
      - "443:443"
      - "443:443/udp"
    volumes:
      - /etc/xray/config.json:/etc/xray/config.json
    network_mode: host
```

```bash
docker-compose up -d
```

### Deploy Hysteria2 with Docker

```yaml
services:
  hysteria:
    image: tommylau625/hysteria2
    container_name: hysteria2
    restart: always
    ports:
      - "443:443/udp"
      - "443:443"
    volumes:
      - ./config.yaml:/etc/hysteria/config.yaml
    network_mode: host
```

---

## Security Hardening & Maintenance

### 🔒 Essential Security Measures

#### 1. Change the SSH Port

```bash
# Edit /etc/ssh/sshd_config
Port 22022  # Change to a non-default port

systemctl restart sshd
```

#### 2. Disable Direct Root Login

```bash
# Create a new user
adduser admin
usermod -aG sudo admin

# Edit /etc/ssh/sshd_config
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes

# Add your public key to ~/.ssh/authorized_keys
systemctl restart sshd
```

#### 3. Install Fail2Ban Against Brute Force

```bash
apt install -y fail2ban
systemctl enable fail2ban
systemctl start fail2ban
```

#### 4. Regular System Updates

```bash
# At least once a month
apt update && apt upgrade -y
```

### 📅 Maintenance Checklist

| Frequency | Action |
|-----------|--------|
| Daily | Verify services are running |
| Weekly | Review system logs, check for unauthorized access |
| Monthly | Update system and packages, back up configurations |
| Quarterly | Review bills, assess if an upgrade is needed |
| On issues | Check logs, diagnose root cause, reinstall if necessary |

### 📋 Common Maintenance Commands

```bash
# Check service status
systemctl status xray

# Real-time logs
journalctl -u xray -f

# Restart service
systemctl restart xray

# Resource usage
htop

# Port listening
ss -tlnp

# Active connections count
netstat -an | grep ESTABLISHED | wc -l
```

---

## Troubleshooting Common Issues

### ❌ VPS unreachable, SSH shows "Connection Refused"

Possible causes:
1. IP blocked → check if ping works, try replacing the IP
2. Wrong port → confirm SSH port (default 22)
3. Firewall blocking → check ufw/iptables rules
4. Service not running → `systemctl status sshd`

### ❌ Proxy is very slow

Troubleshooting steps:
1. Run `speedtest-cli` to test server-side up/down speeds
2. If the server itself is fast, the issue is on your local network
3. Try switching route types (CN2 vs standard routes)
4. Check if BBR acceleration is enabled
5. Verify client configuration (incorrect TLS/XTLS settings cause speed drops)

### ❌ Proxy disconnects frequently

Possible causes:
1. Unstable provider network → contact support or switch providers
2. Out of memory causing OOM → upgrade plan or optimize memory usage
3. IP blocked → replace IP (most providers charge $2~5)

### ❌ Xray / Hysteria2 fails to start

Troubleshooting steps:
1. `journalctl -u xray -xe` — read detailed error logs
2. Verify configuration file syntax (is the JSON valid?)
3. Check if the port is already in use: `ss -tlnp | grep 443`
4. Check log file permissions

### ❌ Data allowance exhausted too quickly

- Review connection logs for unauthorized usage
- Enable firewall with IP whitelist only
- Consider upgrading to an unlimited plan

---

## Disclaimer

⚠️ **Important Notice**

1. This repository provides only VPS technical configuration tutorials. All information is intended for learning and research purposes.
2. Before purchasing and using VPS services, research and comply with local laws, regulations, and the provider's terms of service.
3. This repository assumes no responsibility for losses arising from configuration errors, provider issues, or force majeure events.
4. Do not use scientific internet access tools for any illegal activities.
5. Risks such as IP bans and account suspensions are the user's responsibility.
6. Purchase services through legitimate channels and choose reputable providers. Avoid being scammed by unrealistically cheap deals.

---

## 📚 Related Resources

| Resource Type | Link |
|-------------|------|
| 🛡️ VPS Security Hardening Guide | [CG-spring/vps-security-pro](https://github.com/CG-spring/vps-security-pro) |
| 📊 VPS Benchmark Tools | [CG-spring/vps-benchmark-tools](https://github.com/CG-spring/vps-benchmark-tools) |
| 🧭 Airport Navigator | [nav.clashvip.net](https://nav.clashvip.net) |
| 🔧 Clash Cross-Platform Setup | [CG-spring/clash-client-setup](https://github.com/CG-spring/clash-client-setup) |
| 💬 Community Forum | [bbs.clashhub.net](https://bbs.clashhub.net) |

---

<div align="center">

**Found this useful? Remember to ⭐ Star to show your support!**

</div>

---

*Last updated: September 2026 | This repository's content is compiled from public resources and practical experience. Issues and PRs are welcome to improve the content.*
