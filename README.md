![GitHub](https://img.shields.io/github/license/kaoheng1515/Huawei-OLT-VSolution-Config?style=flat)
![GitHub last commit](https://img.shields.io/github/last-commit/kaoheng1515/Huawei-OLT-VSolution-Config?style=flat)
![ViewCount](https://views.whatilearened.today/views/github/kaoheng1515/Huawei-OLT-VSolution-Config.svg?cache=remove)

# Huawei OLT + V-Solution ONU Configuration Repository  
**Cambodia ISP Standard Configuration (2025 Updated)**  
Tested on: MA5600T, MA5680T, MA5800X2/X7/X15/X17 + V-SOL OLT GPON ONUs  

---

### 1. Login to OLT
```bash
Telnet 10.11.104.2
Username: admin
Password: admin123   (or admin / admin@123 depending on version)
```
After login, enter super user mode

### 2. Confirm and Activate Board
```bash
enable
config
```
### 3. Set Active/Standby Load-Sharing Mode
```bash
active-standby load-sharing enable
```

### Add Management IP
Example: VLAN 4000 for management
```bash
interface vlanif 4000
 ip address 10.11.104.2 255.255.255.0
 quit
```
Add static default route:
```bash
ip route-static 0.0.0.0 0.0.0.0 10.11.104.1    # gateway
```
