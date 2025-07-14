# 💓 High Availability Setup using Heartbeat

This project demonstrates **High Availability (HA)** using the `heartbeat` service on Linux servers. It ensures one Load Balancer takes over when another fails.

## 🧱 Architecture Overview

- Two load balancer VMs
- Shared virtual IP: `192.168.0.10`
- Backend servers behind the load balancer

## ⚙️ Configuration Steps

### 1. Install Heartbeat
```bash
sudo apt install heartbeat
```

### 2. Configure /etc/ha.d/ha.cf
```
logfacility local0
keepalive 2
deadtime 10
warntime 5
initdead 20
udpport 694
bcast eth0
node lb1
node lb2
```

### 3. Configure /etc/ha.d/haresources
```
lb1 192.168.0.10 nginx
```

### 4. Configure /etc/ha.d/authkeys
```
auth 1
1 crc
```
```bash
chmod 600 /etc/ha.d/authkeys
```

### 5. Start Heartbeat
```bash
sudo systemctl start heartbeat
```

## 🧪 Failure Test

1. Turn off nginx on `lb1`
2. Heartbeat will automatically assign virtual IP to `lb2`
3. User will still be able to access the backend — HA working ✅
