# VPS 自建节点完全指南

> 本仓库为 2026 年 VPS 自建科学上网节点提供全面指导，涵盖从服务商选择、配置部署到日常维护的完整流程。无论你是 Linux 新手还是有一定基础的用户，都能在这里找到适合你的方案。目标是让你用最少的钱，搭建最稳定快速的独享节点。

---

## 📌 目录

- [VPS vs 机场：该如何选择？](#vps-vs-机场该如何选择)
- [VPS 服务商推荐（2026）](#vps-服务商推荐2026)
- [核心配置参数解读](#核心配置参数解读)
- [地区选择与线路分析](#地区选择与线路分析)
- [购买与初始化全流程](#购买与初始化全流程)
- [主流代理协议部署指南](#主流代理协议部署指南)
- [进阶优化：BBR/CC/WireGuard](#进阶优化bbwrccwireguard)
- [Docker 容器化部署](#docker-容器化部署)
- [安全防护与日常维护](#安全防护与日常维护)
- [常见问题与故障排查](#常见问题与故障排查)
- [免责声明](#免责声明)

---

## VPS vs 机场：该如何选择？

在开始之前，你需要明确自己的需求。VPS 自建和机场订阅各有优劣：

| 维度 | VPS 自建 | 机场订阅 |
|------|---------|---------|
| **技术要求** | 需要基础 Linux 知识和命令行操作能力 | 零门槛，订阅即用 |
| **月均成本** | 年付约 $2~15/月，折算更低 | 月付 ¥10~80 |
| **流量** | 绝大多数情况下无限流量 | 通常有月流量限制 |
| **带宽** | 独享带宽（低配通常 1Gbps） | 共享带宽，晚高峰可能降速 |
| **节点数量** | 1~3 个固定节点 | 20~200+ 节点随时切换 |
| **IP 被封风险** | 需自行处理（换 IP/换服务商） | 服务商统一管理 |
| **维护精力** | 需自行维护、更新、排查问题 | 全托管，省心 |
| **适用人群** | 有技术基础、大流量用户 | 普通用户、懒得折腾的人 |
| **灵活性** | 完全自定义，可部署多个服务 | 受限于套餐规定 |

**什么时候选 VPS？**
- 你有大流量需求（长期下载、PT、直播推流等）
- 你有技术基础，或愿意学习基础 Linux 操作
- 你需要更稳定的独享带宽，不希望被他人抢占
- 你希望有更高的隐私保护（流量不经过第三方）
- 你想学习网络知识，自己掌控节点

**什么时候选机场？**
- 你是纯小白，不想折腾
- 流量需求一般（日常浏览、视频观看）
- 需要大量节点灵活切换
- 懒得维护，希望出问题有人帮你搞定

本仓库专注于帮助有**自建意愿**的用户，从零到一完成 VPS 部署。

---

## VPS 服务商推荐（2026）

### 🥇 第一梯队：推荐主力使用

#### 1. RackNerd（性价比之王）

| 参数 | 配置 |
|------|------|
| **价格** | 年付 $10.99 起（1核/1GB/18GB SSD） |
| **网络** | 1Gbps 共享带宽，部分套餐 10Gbps |
| **机房** | 洛杉矶、纽约、亚特兰大、圣何塞、达拉斯 |
| **付款** | PayPal、信用卡、支付宝（部分） |
| **退款政策** | 30天退款保证 |
| **特点** | 价格极低，网络稳定，促销频繁 |

适合人群：预算有限、流量需求一般的入门用户。

#### 2. 搬瓦工（BandwagonHost）

| 参数 | 配置 |
|------|------|
| **价格** | 年付 $49.99 起（CN2 GIA 线路） |
| **网络** | CN2 GIA / 香港 HKmediaVM |
| **机房** | 香港、洛杉矶、弗里蒙特、纽约等 |
| **付款** | PayPal、信用卡、支付宝 |
| **退款政策** | 退余款（非全额退款） |
| **特点** | 网络质量好，支持支付宝，中文社区活跃 |

适合人群：追求网络质量、愿意多花一点钱获得 CN2 GIA 优化的用户。

#### 3. CloudCone

| 参数 | 配置 |
|------|------|
| **价格** | 月付 $1.80 起（促销时） |
| **网络** | 1Gbps，洛杉矶机房 |
| **机房** | 洛杉矶 |
| **付款** | PayPal、信用卡 |
| **退款政策** | 按小时计费，可随时销毁退款 |
| **特点** | 按小时计费，灵活性高 |

适合人群：需要灵活短期使用、不想被长期套餐绑定的用户。

### 🥈 第二梯队：特定场景推荐

#### 4. 恒创科技（HengHost）

| 参数 | 配置 |
|------|------|
| **价格** | 月付 ¥29 起 |
| **网络** | 优化线路，BGP 多线接入 |
| **机房** | 香港、日本、美国 |
| **付款** | 支付宝、微信、PayPal |
| **特点** | 支持支付宝，中文客服，适合国内用户 |

#### 5. DMIT

| 参数 | 配置 |
|------|------|
| **价格** | 年付 $30 起 |
| **网络** | CN2 GIA / IPLC 专线 |
| **机房** | 香港、日本、美国 |
| **特点** | 高端线路，稳定性极佳 |

#### 6. HostDare

| 参数 | 配置 |
|------|------|
| **价格** | 季付 $14.99 起 |
| **网络** | CN2 GIA Premium / 优化线路 |
| **机房** | 洛杉矶 |
| **特点** | 小众精品，网络质量不错 |

---

## 核心配置参数解读

购买 VPS 时，你需要关注以下几个核心参数：

### 🖥️ CPU（处理器）

| 类型 | 说明 | 适合场景 |
|------|------|---------|
| **Intel Xeon** | 稳定，通用场景 | 大多数用户首选 |
| **AMD EPYC** | 多核性能强，高并发 | 需要处理大量并发连接 |
| **AMD Ryzen** | 性价比高 | 普通代理用途足够 |

> 对于科学上网代理来说，CPU 通常不是瓶颈，1~2 核足够使用。

### 💾 内存（RAM）

| 内存大小 | 能做什么 |
|---------|---------|
| **512MB** | 跑一个简单代理（不推荐） |
| **1GB** | 跑单节点代理+轻量 Web |
| **2GB** | 跑代理+Docker + 小型数据库 |
| **4GB+** | 跑多个服务 + 脚本处理 |

> 建议起步 **1GB**，有条件上 **2GB**，长期使用建议 **2GB+**。

### 💿 硬盘（Storage）

| 类型 | 说明 |
|------|------|
| **HDD** | 便宜，读写慢，不推荐 |
| **SSD** | 速度快，推荐 |
| **NVMe SSD** | 极速，适合高 IO 场景 |

> 科学上网代理对磁盘 IO 要求不高，20~40GB SSD 足够。

### 🌐 带宽与流量

| 参数 | 说明 |
|------|------|
| **带宽** | 通常是共享带宽（如 1Gbps），代表上限 |
| **流量** | 月流量限制 vs 无限流量 |
| **超量处理** | 超出流量后降速/额外计费/暂停服务 |

> 尽量选择**无限流量**或**大流量套餐**，避免月月超量的尴尬。

### 🗺️ 机房地区选择

这是最影响使用体验的参数，详见下一节。

---

## 地区选择与线路分析

### 📊 各地区 VPS 特点对比

| 地区 | 延迟 | 速度 | 价格 | 推荐指数 | 适合场景 |
|------|------|------|------|---------|---------|
| **香港** | 20~60ms | 极快 | 高（¥100+/月） | ⭐⭐⭐⭐⭐ | 全场景首选，延迟最低 |
| **日本** | 50~100ms | 快 | 中（$5~15/月） | ⭐⭐⭐⭐⭐ | 视频/游戏性价比之选 |
| **新加坡** | 80~150ms | 较快 | 中（$8~15/月） | ⭐⭐⭐⭐ | 东南亚方向首选 |
| **美国洛杉矶** | 150~250ms | 中 | 低（$3~10/月） | ⭐⭐⭐⭐ | 大流量/长连接 |
| **美国其他** | 180~300ms | 一般 | 低 | ⭐⭐⭐ | 备用节点 |
| **韩国** | 60~120ms | 快 | 中 | ⭐⭐⭐⭐ | 兼顾速度与价格 |
| **欧洲（荷兰/德国）** | 200~350ms | 一般 | 中 | ⭐⭐⭐ | 欧洲方向需求 |

### 🏆 线路类型详解

#### CN2 GIA（强烈推荐）

中国电信精品优化线路，单独占用国际出口带宽，不经过普通公网，延迟低、速度快、晚高峰不卡顿。是国内用户连接海外 VPS 的最优选择。

**判断方法：** 服务商明确标注"CN2 GIA"字样，或购买前在官网确认。

#### CN2 GT

CN2 Global Transit，比 CN2 GIA 低一档，经过部分公网，晚高峰可能有影响。性价比尚可。

#### 163 骨干网（普通线路）

最常见的国际出口，经过公众互联网，晚高峰容易拥堵，速度不稳定。不推荐作为主力。

#### IPLC 专线

通过物理光纤直连，带宽独享，但成本极高，通常只有机场服务商会大规模使用，个人用户购买较少。

### 🎯 选购建议

```
国内用户首选：
1. 香港 CN2 GIA / HKmediaVM（最贵，速度最快）
2. 日本 CN2 GIA（性价比最高）
3. 洛杉矶 CN2 GIA（便宜，大流量）

按需求选：
- 低延迟优先 → 香港 > 日本 > 韩国
- 大流量省钱 → 美国洛杉矶 > 欧洲
- 兼顾两者 → 日本东京大阪
```

---

## 购买与初始化全流程

### Step 1：注册账户

以 RackNerd 为例：
1. 访问 racknerd.com
2. 点击 Sign Up，填写邮箱、密码
3. 邮箱激活后登录控制面板

> 💡 建议使用真实邮箱，便于接收服务通知。

### Step 2：选择套餐并支付

1. 选择目标机房（推荐洛杉矶 DC9 / DC3）
2. 选择套餐配置（推荐 $10.99/年 起步配置）
3. 添加到购物车
4. 结账时选择付款方式（PayPal/信用卡）
5. 支付成功后，VPS 在 15 分钟内开通

### Step 3：登录控制面板

RackNerd 使用 **KiwiVM** 控制面板：
- 登录后找到 "Client Main" → "My Services"
- 点击对应 VPS → 进入控制面板

常用功能：
| 功能 | 说明 |
|------|------|
| **Root Password Modification** | 修改 root 密码 |
| **Install New OS** | 重装系统（建议 Debian 11/12 或 Ubuntu 22.04） |
| **Control Panel** | 查看 VPS IP、端口等信息 |
| **Firewall** | 端口防火墙管理 |

### Step 4：连接 VPS（SSH）

**Windows 用户：使用 PowerShell 或 Terminal**

```powershell
ssh root@你的VPS_IP
```

**首次连接会提示确认密钥，输入 `yes` 并回车，然后输入 root 密码。**

> ⚠️ 密码输入时屏幕不会显示字符，输入完回车即可。

**Mac/Linux 用户：直接使用终端**

```bash
ssh root@你的VPS_IP
```

### Step 5：初始化服务器

新装机后，建议执行以下操作：

```bash
# 更新系统
apt update && apt upgrade -y

# 安装基础工具
apt install -y curl wget git vim unzip ca-certificates gnupg lsb-release

# 设置时区（中国用户）
ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime

# 关闭 SSH 密码登录（后续配好密钥后）
# 编辑 /etc/ssh/sshd_config
# PasswordAuthentication no
# 然后 systemctl restart sshd
```

---

## 主流代理协议部署指南

### 方案一：Xray（推荐，综合最优）

Xray 是 V2Ray 的分支，性能更好，配置更灵活，支持 VLESS + XTLS / Trojan 等协议。

**安装脚本（一键）：**
```bash
bash <(curl -Ls https://raw.githubusercontent.com/mhsanaei/anti-replay/main/install.sh)
```

**或者使用 mack-a 大神的脚本（功能更全）：**
```bash
bash <(curl -s -L https://raw.githubusercontent.com/mack-a/v2ray-agent/master/install.sh)
```

安装后访问管理面板（通常是 65432 端口），配置节点信息。

### 方案二：Hysteria2（高速低延迟）

Hysteria2 是基于 QUIC 的高速协议，在丢包率高的情况下仍能保持较高速度。

**安装（官方脚本）：**
```bash
curl -fsSL https://get.hy2.sh | bash
hysteria server -c /etc/hysteria/config.yaml --mode smart
```

### 方案三：WireGuard（安全高效）

WireGuard 是新一代 VPN 协议，内核级集成，性能极佳，配置简单。

**服务端安装：**
```bash
apt install -y wireguard
wg genkey | tee privatekey | wg pubkey > publickey
```

**客户端配置示例：**
```ini
[Interface]
PrivateKey = <客户端私钥>
Address = 10.0.0.2/24
DNS = 8.8.8.8

[Peer]
PublicKey = <服务端公钥>
Endpoint = 你的VPS_IP:51820
AllowedIPs = 0.0.0.0/0
PersistentKeepalive = 25
```

### 方案四：Shadowsocks + v2ray-plugin

经典协议，适合兼容老设备。

**安装：**
```bash
apt install -y shadowsocks-libev
```

---

## 进阶优化：BBR/CC/WireGuard

### 🚀 启用 BBR 加速

BBR 是 Google 开发的 TCP 拥塞控制算法，可显著提升网络吞吐量。

```bash
# 检查当前拥塞控制算法
sysctl net.ipv4.tcp_congestion_control

# 启用 BBR
echo "net.core.default_qdisc=fq" >> /etc/sysctl.conf
echo "net.ipv4.tcp_congestion_control=bbr" >> /etc/sysctl.conf
sysctl -p

# 验证
sysctl net.ipv4.tcp_congestion_control
# 应输出：net.ipv4.tcp_congestion_control = bbr
```

### 🔧 优化系统参数

在 `/etc/sysctl.conf` 中添加：

```bash
# 最大文件描述符
fs.file-max = 1000000

# 网络参数优化
net.core.rmem_max = 16777216
net.core.wmem_max = 16777216
net.core.netdev_max_backlog = 65535
net.core.somaxconn = 65535

# TCP 参数优化
net.ipv4.tcp_max_syn_backlog = 65535
net.ipv4.tcp_fastopen = 3
net.ipv4.tcp_max_tw_buckets = 65535
net.ipv4.tcp_fin_timeout = 10
net.ipv4.tcp_keepalive_time = 1200
net.ipv4.tcp_max_syn_backlog = 65535
net.ipv4.tcp_tw_reuse = 1
net.ipv4.tcp_slow_start_after_idle = 0

sysctl -p
```

### 🛡️ 配置防火墙（ufw）

```bash
# 安装
apt install -y ufw

# 默认规则
ufw default deny incoming
ufw default allow outgoing

# 开放 SSH 端口（重要！）
ufw allow 22/tcp

# 开放代理端口（假设是 443）
ufw allow 443/tcp

# 启用防火墙
ufw enable
```

---

## Docker 容器化部署

Docker 让代理服务部署和迁移更加便捷。

### 安装 Docker

```bash
curl -fsSL https://get.docker.com | sh
systemctl enable docker
```

### Docker Compose 部署 Xray

创建 `docker-compose.yml`：

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

### Docker 部署 Hysteria2

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

## 安全防护与日常维护

### 🔒 必做安全措施

#### 1. 修改 SSH 端口

```bash
# 编辑 /etc/ssh/sshd_config
Port 22022  # 改为非默认端口

systemctl restart sshd
```

#### 2. 禁用 root 直接登录

```bash
# 创建新用户
adduser admin
usermod -aG sudo admin

# 编辑 /etc/ssh/sshd_config
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes

# 添加公钥到 ~/.ssh/authorized_keys
systemctl restart sshd
```

#### 3. 配置 Fail2Ban 防暴力破解

```bash
apt install -y fail2ban
systemctl enable fail2ban
systemctl start fail2ban
```

#### 4. 定期更新系统

```bash
# 每月至少一次
apt update && apt upgrade -y
```

### 📅 定期维护清单

| 频率 | 操作 |
|------|------|
| 每天 | 检查服务是否正常运行 |
| 每周 | 查看系统日志，检查异常登录 |
| 每月 | 更新系统、软件包，备份配置 |
| 季度 | 检查账单，评估是否需要升级 |
| 遇到问题时 | 查看日志，排查原因，必要时重装 |

### 📋 常用维护命令

```bash
# 查看服务状态
systemctl status xray

# 查看实时日志
journalctl -u xray -f

# 重启服务
systemctl restart xray

# 查看资源使用
htop

# 查看端口监听
ss -tlnp

# 查看 IP 连接数
netstat -an | grep ESTABLISHED | wc -l
```

---

## 常见问题与故障排查

### ❌ VPS 连不上，SSH 报错 "Connection refused"

可能原因：
1. IP 被封 → 检查能否 ping 通，尝试更换 IP
2. 端口不对 → 确认 SSH 端口（默认 22）
3. 防火墙阻止 → 检查 ufw/iptables 规则
4. 服务未启动 → `systemctl status sshd`

### ❌ 代理速度很慢

排查步骤：
1. 执行 `speedtest-cli` 测试服务器本地上下行
2. 如果服务器本身速度正常，问题在本地网络
3. 尝试切换节点线路（CN2 vs 普通线路）
4. 检查是否开启了 BBR 加速
5. 检查客户端配置是否正确（TLS/XTLS 配置错误会导致降速）

### ❌ 代理频繁掉线

可能原因：
1. 服务商网络不稳定 → 联系客服或换服务商
2. 内存不足导致 OOM → 升级配置或优化内存使用
3. IP 被墙 → 更换 IP（多数服务商收费 $2~5）

### ❌ Xray/Hysteria2 启动失败

排查步骤：
1. `journalctl -u xray -xe` 查看详细错误日志
2. 检查配置文件语法（JSON 格式是否正确）
3. 检查端口是否被占用：`ss -tlnp | grep 443`
4. 检查日志文件权限

### ❌ 流量很快就用完了

- 检查是否有异常蹭网（查看连接日志）
- 开启防火墙仅允许白名单 IP
- 考虑升级到无限流量套餐

---

## 免责声明

⚠️ **重要声明**

1. 本仓库仅提供 VPS 技术配置教程，所有信息用于学习与研究目的。
2. 在购买和使用 VPS 服务前，请自行了解并遵守当地法律法规以及服务商的条款。
3. 本仓库不对因配置错误、服务商问题或任何不可抗力导致的损失承担责任。
4. 使用科学上网工具时，请勿用于任何违法活动。
5. IP 被封、账户被封等风险由用户自行承担。
6. 建议通过正规渠道购买服务，选择有口碑的服务商，避免因贪图便宜而上当受骗。

---

## 📚 相关资源

| 资源类型 | 链接 |
|---------|------|
| 🛡️ VPS 安全防护指南 | [CG-spring/vps-security-pro](https://github.com/CG-spring/vps-security-pro) |
| 📊 VPS 基准测试工具 | [CG-spring/vps-benchmark-tools](https://github.com/CG-spring/vps-benchmark-tools) |
| 🧭 机场推荐导航 | [nav.clashvip.net](https://nav.clashvip.net) |
| 🔧 Clash 全平台配置 | [CG-spring/clash-client-setup](https://github.com/CG-spring/clash-client-setup) |
| 💬 社区论坛 | [bbs.clashhub.net](https://bbs.clashhub.net) |

---

<div align="center">

**觉得有帮助？记得 ⭐ Star 支持！**

</div>

---

*最后更新：2026 年 9 月 | 本仓库内容基于公开资料与实践经验整理，欢迎提交 Issue 或 PR 完善内容*
