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

### 5. Create New Admin User
```bash
super-password level 15 cipher Huawei@2025 simple Huawei@2025
# or
user name admin2 password cipher YourStrongPass123 level 15
```

### 6. Create Smart VLAN
```bash
smart-vlan 4000
 vlan attrib 4000 smart
 quit
```

### 7. Create DBA Profile (Bandwidth control)
```bash
dba-profile add profile-id 10 name "VSOL-1G" type4 max 1048576    # 1G profile
dba-profile add profile-id 11 name "VSOL-500M" type4 max 524288
dba-profile add profile-id 12 name "VSOL-100M" type4 max 102400
```

### 8. Create ONT Line Profile
```bash
ont-lineprofile gpon profile-id 10
 gemport 1 mapping-mode vlan
 gem add 1 eth 1                             # GEM port for internet
 dba-profile-id 10                           # 1G profile
 ont-port eth 8 pots 1                       # 8 LAN + 1 POTS (if any)
 fec upstream enable                         # Enable FEC upstream (recommended for V-Sol)
 commit
 quit
```

### 9. Create ONT Service Profile (VLAN & Service)
```bash
ont-srvprofile gpon profile-id 10
 ont-port eth 8                              # Allow 8 LAN ports
 commit
 quit
```

### 10. Enable GPON Interface 
```bash
interface gpon 0/1                         # Example: frame 0 / slot 1
 port 0 ont-auto-find enable                # Auto discover ONU
 quit
```

### 11. Discover & auto find ONU and add
```bash
display ont autofind all                    # Show discovered ONUs
# Example output: SN: 56534F4C12345678 (V-SOL)

# Add ONU
interface gpon 0/1/0
 ont add 0 sn-auth 56534F4C12345678 password 12345678 omcc no-onu-profile line-profile-id 10 srv-profile-id 10 desc "Customer Name"
 ont confirm 0 sn-auth 56534F4C12345678 password 12345678
```