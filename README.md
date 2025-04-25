# GG3-Homelab

## TL;DR
This repository contains the **full infrastructure blueprint** for the **GG3-DevNet R&D Homelab**:  
- Rack hardware layout (modem, firewall, servers, switch, UPS)
- VLAN network design and IP reservations
- Equipment roles, cable types, power planning
- Expansion notes for future upgrades  
> Full technical breakdown below. YAML file included for architecture reference.

---

## Overview
This repository contains the infrastructure blueprint for the **GG3-DevNet R&D Homelab**.  
It outlines planned rack hardware, network VLAN architecture, port assignments, and future expansion plans.

---

## Rack Layout

| Unit | Equipment | Type | Notes |
|:----:|:---------:|:----:|:-----:|
| 1 | Ubiquiti UCI Modem | Modem | DOCSIS 3.1, rack-mountable |
| 2 | Patch Panel (Optional) | Cable Management | Clean routing and cable management |
| 3 | Ubiquiti USW-24-POE | Switch | 24-port managed PoE, VLAN-capable |
| 4 | Qotom pfSense Box | Firewall/Router | Atom C3808, 4x SFP+, 5x 2.5GbE |
| 5 | Ventilation Gap | Spacer | Airflow between firewall and servers |
| 6 | Supermicro Node 1 | 1U Server | XCP-ng Hypervisor Host |
| 7 | Supermicro Node 2 | 1U Server | Backup/Secondary Services |
| 8 | Ventilation Gap | Spacer | Airflow between servers and power units |
| 9 | Tripp Lite RS1215-RA | PDU | 12-outlet horizontal PDU |
| 10-11 | CyberPower CP1500PFCRM2U | UPS | 1500VA/1000W UPS, short depth |

---

## Equipment Roles and Power Draw

| Equipment | Role | Power Draw (Watts) | Cable Type |
|:---------:|:----:|:------------------:|:----------:|
| Qotom pfSense Box | Firewall/Router | 20 | CAT6, SFP+ |
| Supermicro Node 1 | XCP-ng Host | 80 | CAT6 |
| Supermicro Node 2 | Backup Server | 75 | CAT6 |
| Ubiquiti USW-24-POE | Core Switch | 25 (no PoE load) | CAT6 |
| Tripp Lite RS1215-RA | Power Strip | 0 | N/A |
| CyberPower CP1500PFCRM2U | UPS | 0 | N/A |
| Ubiquiti UCI Modem | WAN Modem | 10 | Coax + CAT6 |

---

## Network VLAN Structure

| VLAN ID | Name | Subnet | Purpose |
|:-------:|:----:|:------:|:-------:|
| 10 | Main LAN | 10.3.10.0/24 | Trusted devices |
| 20 | Lab VLAN | 10.3.20.0/24 | Homelab and XCP-ng |
| 30 | Guest VLAN | 10.3.30.0/24 | Guest Wi-Fi and family |
| 99 | VPN VLAN | 10.99.99.0/24 | WireGuard access via Raspberry Pi |

---

## Port Assignments

**pfSense Ports**
- Port 1: WAN (connected to modem)
- Port 2: LAN (VLAN trunk to switch for VLANs 10, 20, 30, 99)
- Port 3: Management/OOB (optional)
- Port 4: Future expansion

**Switch Ports**
- Ports 1–4: Access Points (VLAN-tagged SSIDs)
- Ports 5–12: Servers and lab devices (VLAN 20)
- Ports 13–16: Main LAN devices (VLAN 10)
- Ports 17–20: Guest devices (VLAN 30)
- Ports 21–24: Spare / VPN / PoE Pi (VLAN 99)

---

## Static IP Reservations

| Device | IP Address | VLAN | Role |
|:------:|:----------:|:----:|:----:|
| pfSense Firewall | 10.3.10.1 | 10 | Gateway |
| Supermicro Node 1 | 10.3.20.10 | 20 | XCP-ng Hypervisor |
| Supermicro Node 2 | 10.3.20.11 | 20 | Backup/Services |
| Mac Mini | 10.3.10.66 | 10 | Dev Machine |
| WireGuard Pi | 10.99.99.66 | 99 | VPN Gateway |
| Access Point 1 | 10.3.10.100 | 10 | Wi-Fi Uplink |

---

## Wireless Setup

| Access Point | Type | VLANs Supported | Notes |
|:------------:|:----:|:---------------:|:-----:|
| Ubiquiti U7-Pro | Wi-Fi 7 AP | 10, 20, 30 | Tri-band AP with VLAN-tagged SSIDs |

---

## Expansion Notes
- Available rack units: **1U**
- Future plans:
  - Add NAS storage node (rack-mount or shelf)
  - Install UniFi Cloud Key or host UniFi Controller on VM
  - Dedicated out-of-band management device
  - Install rack environmental monitor or fans
  - Test fiber uplink or external DMZ segmentation

---

## Notice
- IP addresses, hardware notes, and network configurations shown here are for planning and architectural purposes.
- Live operational environments will be documented separately under the **GG3-DevNet** project.
