# Pipe-Network-Testnet-Node-Run-Full-Guide
pipe node

-----------

# Hardware Requirements 

## Overview
Cache Node is a high-performance caching service that helps distribute and serve files efficiently across a network

| Component      | Specification                      |
|----------------|------------------------------------|
| CPU            | 4 core Processor                   |
| RAM            | 8 GB (4–8 GB can also run it)     |
| Storage        | 100 GB SSD                         |
| Internet Speed | 25 Mbps Upload / Download          |


# CLI Node Run Full Guide

### Offical Docs by Pipe Network - https://docs.pipe.network/nodes/testnet

----

## 🧰 Prerequisites (Only For Devnet Node Run Users)
	
### Stop Old Node
If you have running a devnet node previously, you need to do these steps first.

### 1. Backup `Node Info File`
You will need to backup old `node_info.json` file which you can find in pipe folder in your `root` directory.

Save the file
```
nano ~/node_info.json
```

### 2. Stop old node
```
sudo systemctl stop pipe
sudo systemctl disable pipe
sudo systemctl daemon-reload
```

open port in google cloud 
| Field                   | Value
| **Name**                | allow-pipe
| **Targets**             | All instances in the network
| **Source IP ranges**    | 0.0.0.0/0` *(allow all incoming)
| **Protocols and ports** | Check "Specified protocols and ports" 80,443

```
sudo useradd -m -s /bin/bash popcache
sudo usermod -aG sudo popcache
```
```
sudo apt update -y && sudo apt upgrade -y
sudo apt install libssl-dev ca-certificates wget -y
```

```
 sudo bash -c 'cat > /etc/sysctl.d/99-popcache.conf << EOL
net.ipv4.ip_local_port_range = 1024 65535
net.core.somaxconn = 65535
net.ipv4.tcp_low_latency = 1
net.ipv4.tcp_fastopen = 3
net.ipv4.tcp_slow_start_after_idle = 0
net.ipv4.tcp_window_scaling = 1
net.ipv4.tcp_wmem = 4096 65536 16777216
net.ipv4.tcp_rmem = 4096 87380 16777216
net.core.wmem_max = 16777216
net.core.rmem_max = 16777216
EOL'
```
```
sudo sysctl -p /etc/sysctl.d/99-popcache.conf
```

```
sudo bash -c 'cat > /etc/security/limits.d/popcache.conf << EOL
*    hard nofile 65535
*    soft nofile 65535
EOL'
```

# Now close ur VPS  Then Open again ur VPS🛑

# 🛑Make Directory (create folder to be used for download cache)🛑
```
sudo ufw allow ssh
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw enable
```
```
sudo mkdir -p /opt/popcache/logs
cd /opt/popcache
```
```
sudo wget https://download.pipe.network/static/pop-v0.3.0-linux-x64.tar.gz
sudo tar -xzf pop-v0.3.0-linux-x64.tar.gz
sudo chmod 755 /opt/popcache/pop
```
```
sudo nano /opt/popcache/config.json
```

```
{
  "pop_name": "username",
  "pop_location": "vps_location",
  "invite_code": "invite_code",
  "server": {
    "host": "0.0.0.0",
    "port": 443,
    "http_port": 80,
    "workers": 40
  },
  "cache_config": {
    "memory_cache_size_mb": 5500,
    "disk_cache_path": "./cache",
    "disk_cache_size_gb": 100,
    "default_ttl_seconds": 86400,
    "respect_origin_headers": true,
    "max_cacheable_size_mb": 1024
  },
  "api_endpoints": {
    "base_url": "https://dataplane.pipenetwork.com"
  },
  "identity_config": {
    "node_name": "username",
    "name": "yourRealname",
    "email": "email",
    "website": "https://your-website.com",
    "discord": "username",
    "telegram": "username",
    "solana_pubkey": "yoursolanawalletaddress"
  }
}


pop_name: Unique name for your node (e.g., MyPipeNode).
pop_location: City and country of your VPS (e.g., Ho Chi Minh, VN).
invite_code: Your invite code from the Pipe Network email.
Inside identity_config:

node_name: Name for your node (e.g., magicstone1412).
name: Your personal or organization name.
email: Your email address.
website: Your website (optional; use a valid URL or leave as is).
discord: Your Discord username (optional).
telegram: Your Telegram handle (optional).
solana_pubkey: Your Solana wallet address for rewards.
```
# 🛑 Save and exit the editor: Ctrl+O, Enter, then Ctrl+X 🛑
```
sudo nano /etc/systemd/system/popcache.service
```

```
[Unit]
Description=POP Cache Node
After=network.target

[Service]
Type=simple
User=root
Group=root
WorkingDirectory=/opt/popcache
ExecStart=/opt/popcache/pop
Restart=always
RestartSec=5
LimitNOFILE=65535
StandardOutput=append:/opt/popcache/logs/stdout.log
StandardError=append:/opt/popcache/logs/stderr.log
Environment=POP_CONFIG_PATH=/opt/popcache/config.json

[Install]
WantedBy=multi-user.target
```

```
 sudo systemctl daemon-reload
sudo systemctl enable popcache
sudo systemctl start popcache
```

```
sudo systemctl status popcache
sudo journalctl -u popcache -f
```


usable commands -

-Check Health Endpoint
curl http://localhost/health

-Open in browser:
https://<your-vps-ip>/state

tail -f /opt/popcache/logs/stdout.log
tail -f /opt/popcache/logs/stderr.log


Participate in PipeQuest
Monitor Rewards: Rewards depend on data cached (1x to 3x multiplier).
Correct Wallet: Ensure solana_pubkey is accurate in your config.
Stay active to earn consistency bonuses across seasons.









