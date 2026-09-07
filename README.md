# CCNA Inter-VLAN Routing Lab: Comparative Analysis

A practical implementation comparing **3 different Inter-VLAN Routing techniques** in Cisco Packet Tracer:
1. Legacy Inter-VLAN Routing (Separate Physical Links)
2. Router-on-a-Stick (802.1Q Sub-interfaces)
3. Layer 3 Switch Routing (Switched Virtual Interfaces - SVI)

---

## 📐 Topology & Addressing Plan

<img width="1502" height="542" alt="topology" src="https://github.com/user-attachments/assets/531f5b0a-ad17-4deb-8e19-79d72a27de2e" />


### VLAN & Subnet Scheme
| VLAN | Name | Subnet Network | Default Gateway |
| :--- | :--- | :--- | :--- |
| **VLAN 10** | HR | `192.168.1.0/24` | `192.168.1.100` |
| **VLAN 20** | IT | `192.168.2.0/24` | `192.168.2.100` |

---

## 🛠 Configuration Details

### A. Legacy Inter-VLAN Routing (`RTR-LGC`)
- **Router Interfaces:** `Fa0/0` (VLAN 10 - `192.168.1.100/24`), `Fa0/1` (VLAN 20 - `192.168.2.100/24`)
- **Switch Ports:** VLAN 10 (`Fa0/1, Fa0/2, Fa0/5`), VLAN 20 (`Fa0/3, Fa0/4, Fa0/6`)

### B. Router-on-a-Stick (`RTR-ROAS`)
- **Trunk Port:** `Fa0/5` on switch
- **Sub-interfaces:**
```text
interface FastEthernet0/0.10
 encapsulation dot1Q 10
 ip address 192.168.1.100 255.255.255.0

interface FastEthernet0/0.20
 encapsulation dot1Q 20
 ip address 192.168.2.100 255.255.255.0
```
- **Access Ports:** VLAN 10 (`Fa0/1, Fa0/2`), VLAN 20 (`Fa0/3, Fa0/4`)

### C. Multilayer Switch Routing (`MLS-SVI`)
- **Enabled L3 Routing:** `ip routing`
- **SVI Configuration:**
```text
interface Vlan10
 ip address 192.168.1.100 255.255.255.0

interface Vlan20
 ip address 192.168.2.100 255.255.255.0
```
- **Access Ports:** VLAN 10 (`Fa0/1, Fa0/2`), VLAN 20 (`Fa0/3, Fa0/4`)

---

## 🔧 Troubleshooting Note (Personal Log)
> **Issue faced during testing:** 
> Initially, PCs in VLAN 10 could not ping VLAN 20 on the Layer 3 Switch. 
> 
> **Root Cause:** Inter-VLAN routing was inactive because `ip routing` global command was missing on `MLS-SVI`. 
> 
> **Fix:** Executed `ip routing` in global configuration mode, restoring cross-VLAN connectivity.

---

## 📁 Repository Files
- `InterVLAN_3_Methods.pkt` - Cisco Packet Tracer lab file
- `topology.png` - Network topology diagram
- `configs/` - Device running configurations (`RTR-LGC.txt`, `RTR-ROAS.txt`, `MLS-SVI.txt`)
