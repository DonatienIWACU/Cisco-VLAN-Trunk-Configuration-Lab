# 🧠 Cisco VLAN & Trunk Configuration Lab

### 📘 Overview
This lab demonstrates the configuration of **VLAN segmentation** and **trunk interconnection** between two Cisco switches using **Packet Tracer**.  
Each switch hosts end devices assigned to different VLANs (10, 20, and 30), and both switches are connected via a trunk link that carries traffic for all VLANs.

---

## ⚙️ Network Topology

```
[ PC1 ]---(Fa0/1) SW1 (Fa0/4)====(Fa0/4) SW2 (Fa0/1)---[ PC4 ]
[ PC2 ]---(Fa0/2)        |               (Fa0/2)---[ PC5 ]
[ PC3 ]---(Fa0/3)        |               (Fa0/3)---[ PC6 ]
                         |
                  VLAN Trunk Link (802.1Q)
```

---

## 🧩 Objectives

1. Create and name VLANs 10, 20, and 30 on both switches.  
2. Assign switch access ports to their respective VLANs.  
3. Configure a trunk link between the two switches on FastEthernet0/4.  
4. Verify VLAN and trunk operation.  
5. Enable inter-switch communication within the same VLAN.

---

## 🧱 VLAN Table

| VLAN ID | VLAN Name | Purpose / Department |
|----------|------------|----------------------|
| 10 | VLAN10 | Engineering |
| 20 | VLAN20 | HR |
| 30 | VLAN30 | Finance |

---

## 🖥️ Switch 1 (SW1) Configuration

```bash
hostname SW1

! Create VLANs
vlan 10
 name VLAN10
vlan 20
 name VLAN20
vlan 30
 name VLAN30

! Assign access ports
interface fa0/1
 switchport mode access
 switchport access vlan 10
 no shutdown

interface fa0/2
 switchport mode access
 switchport access vlan 20
 no shutdown

interface fa0/3
 switchport mode access
 switchport access vlan 30
 no shutdown

! Configure trunk link to SW2
interface fa0/4
 description Link_to_SW2
 switchport mode trunk
 switchport trunk allowed vlan 10,20,30
 no shutdown

! Save configuration
end
write memory
```

---

## 🖥️ Switch 2 (SW2) Configuration

```bash
hostname SW2

! Create VLANs
vlan 10
 name VLAN10
vlan 20
 name VLAN20
vlan 30
 name VLAN30

! Assign access ports
interface fa0/1
 switchport mode access
 switchport access vlan 10
 no shutdown

interface fa0/2
 switchport mode access
 switchport access vlan 20
 no shutdown

interface fa0/3
 switchport mode access
 switchport access vlan 30
 no shutdown

! Configure trunk link to SW1
interface fa0/4
 description Link_to_SW1
 switchport mode trunk
 switchport trunk allowed vlan 10,20,30
 no shutdown

! Save configuration
end
write memory
```

---

## 🔍 Verification Commands

### VLAN verification
```bash
show vlan brief
```
✅ Confirms VLANs 10, 20, 30 exist and ports Fa0/1–Fa0/3 are assigned correctly.

### Trunk verification
```bash
show interfaces trunk
```
✅ Displays Fa0/4 as an **active trunk** carrying VLANs 10, 20, 30.

### Interface status
```bash
show interfaces status
```
✅ Shows connected PCs and trunk port as “trunk”.

### Connectivity test
- PCs within the same VLAN (e.g., VLAN10 on SW1 and VLAN10 on SW2) should **ping each other successfully**.
- PCs in different VLANs **cannot communicate** without a router (Inter-VLAN routing).

---

## 🧾 Results Summary

| Test | Expected Result | Status |
|------|------------------|--------|
| VLANs created successfully | VLANs 10, 20, 30 active on both switches | ✅ |
| Access ports assigned correctly | Fa0/1–Fa0/3 in VLANs 10, 20, 30 | ✅ |
| Trunk link between switches | Fa0/4 trunking with VLANs 10,20,30 | ✅ |
| Same VLAN communication | PCs in same VLAN ping successfully | ✅ |
| Inter-VLAN isolation | PCs in different VLANs cannot ping | ✅ |

---

## 📚 Notes

- The `switchport trunk encapsulation dot1q` command is not available on **2960-series** switches in Packet Tracer because they support **only 802.1Q** encapsulation by default.
- For labs requiring the `encapsulation` command, use **Cisco Catalyst 3560** or **3550** models.
- Save configs using:
  ```bash
  write memory
  ```
  or
  ```bash
  copy running-config startup-config
  ```

---

## 🧑‍💻 Author

**Donatien N**  
Cisco Networking Academy Lab • Packet Tracer Simulation  
GitHub: [yourusername](https://github.com/https://github.com/DonatienIWACU/)
