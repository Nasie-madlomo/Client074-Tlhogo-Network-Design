# CMPG 325 – Network Design Project

## Client: Tlhogo Roofing & Construction (Vryburg)

| **Client ID** | CLI-074 |
|---------------|---------|
| **Industry** | Construction |
| **Assigned Challenge** | Port Security (switchport access control) |
| **Addressing Block** | 192.168.37.0/24 |
| **Design Constraint** | Customer records are confidential — access must be controlled |
| **Change Request (CR5)** | User numbers grow by 25% — addressing plan must absorb this without renumbering |

---

## 📁 Repository Contents

| File / Folder | Description |
|---------------|-------------|
| `Tlhogo_Roofing_Construction.pkt` | Cisco Packet Tracer simulation file |
| `Physical_Topology.png` | Physical network topology diagram |
| `Logical_Topology.png` | Logical topology showing VLANs and IP addressing |
| `IP_Addressing_Plan.md` | IP addressing and subnet plan |
| `Testing_Evidence/` | Screenshots of all connectivity and security tests |
| `Configuration_Files/` | Device configuration exports |
| `README.md` | This file |

---

## 🗺️ Network Design Overview

### Physical Topology

- **1 × Router 2911** (R1-Tlhogo) — Inter-VLAN routing
- **4 × Switch 2960** (SW-Core, SW-Admin, SW-Projects, SW-Finance)
- **3 × PC** (PC-Admin, PC-Projects, PC-Finance)

### Logical Topology (VLANs)

| VLAN ID | Department | Subnet | Gateway |
|---------|------------|--------|---------|
| 10 | Admin | 192.168.37.0/27 | 192.168.37.1 |
| 20 | Projects | 192.168.37.32/27 | 192.168.37.33 |
| 30 | Finance | 192.168.37.64/28 | 192.168.37.65 |

### IP Addressing Plan

| Device | IP Address | Subnet Mask | Gateway |
|--------|-----------|-------------|---------|
| PC-Admin | 192.168.37.2 | 255.255.255.224 | 192.168.37.1 |
| PC-Projects | 192.168.37.34 | 255.255.255.224 | 192.168.37.33 |
| PC-Finance | 192.168.37.66 | 255.255.255.240 | 192.168.37.65 |

---

## 🔐 Assigned Challenge: Port Security

Port Security was configured on **SW-Admin Fa0/2** (the port connecting to PC-Admin).

### Configuration Applied

### Why This Configuration?

- **Maximum 1 MAC address:** Only one device allowed per port.
- **Violation mode restrict:** Blocks unauthorised traffic while keeping the port active.
- **Sticky MAC:** Automatically learns and locks the MAC address of the first device.

### Verification

- ✅ Sticky MAC learned for PC-Admin
- ✅ Rogue PC blocked — violation count incremented to 6
- ✅ Legitimate PC-Admin reconnected and working

---

## 🧪 Testing Evidence

| Test | Description | Result | Screenshot |
|------|-------------|--------|------------|
| 1 | PC-Admin → Gateway | 0% loss | `test1_ping_gateway.png` |
| 2 | PC-Admin → PC-Projects | 0% loss | `test2_ping_projects.png` |
| 3 | PC-Admin → PC-Finance | 0% loss | `test3_ping_finance.png` |
| 4 | Port Security — sticky MAC learned | ✅ Verified | `port_security_address.png` |
| 5 | Port Security — rogue PC blocked | Violation count = 6 | `port_security_violation.png` |

---

## 🔧 Troubleshooting

| Issue | Cause | Resolution |
|-------|-------|------------|
| Packet loss on VLAN 20 | Unused port Fa0/4 flapping in auto mode | Shut down Fa0/4 |
| VLAN 30 unreachable | ARP not resolving | Restarted sub-interface and switches; rebuilt topology |
| STP blocking on trunk | Native VLAN mismatch after port changes | Reconfigured native VLAN 1 on both ends |

---

## 📈 Change Request CR5 — 25% Growth

The addressing plan uses **VLSM** and includes reserved growth room:

| VLAN | Current Users | Subnet Size | Available | Growth Room |
|------|---------------|-------------|-----------|-------------|
| 10 | 24 | /27 (30 hosts) | 30 | 25% |
| 20 | 24 | /27 (30 hosts) | 30 | 25% |
| 30 | 10 | /28 (14 hosts) | 14 | 40% |

**Additional reserved space:** 192.168.37.160 – 192.168.37.255 (96 addresses) for future VLANs.

---

## 👤 Author

**Nasiphi Mlambo**  
Student Number: 41555597  
Module: CMPG 325 – Computer Networks  
North-West University

---

## 📚 References

- Cisco Networking Academy (2026) *CCNA: Introduction to Networks*.
- Cisco (2026) *Packet Tracer Documentation*.
- Kurose, J.F. and Ross, K.W. (2021) *Computer Networking: A Top-Down Approach*. 8th edn. Pearson.
