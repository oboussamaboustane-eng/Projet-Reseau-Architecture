#  Network Architecture: Switching, Routing & WAN

![Cisco Packet Tracer](https://img.shields.io/badge/Technology-Cisco%20Packet%20Tracer-blue)
![Status](https://img.shields.io/badge/Status-Completed-success)

## Project Overview
This repository contains the technical resources and documentation for a simulated enterprise network infrastructure project. It implements a hierarchical network design interconnecting a headquarters site with remote locations over a WAN link.

* **Author:** [Boustane Oussama](https://www.linkedin.com/in/oussama-boustane-22a990298/)
* **Context:** Networks & Systems Module
* **Academic Year:** 2025/2026

---

## Technical Objectives
The primary goal is to demonstrate the implementation of the following technologies:
1. **Switching:** Segmentation using VLANs, trunking protocols (802.1Q), and link aggregation (LACP).
2. **Inter-VLAN Routing:** Configuration of a "Router-on-a-Stick" for cross-subnet communication.
3. **WAN Interconnection:** Serial links and routing protocols for remote site access.
4. **Optimization:** Route Summarization to reduce routing table overhead.

---

## Topology & Architecture
The network is built around a core router (R1) handling inter-VLAN routing and WAN access, supported by a distribution/access layer of interconnected switches.

![Global Topology Diagram](images/topologie_globale.png)

### Simulated Hardware Inventory

| Device | Model | Primary Role |
| :--- | :--- | :--- |
| Router R1 | Cisco 2811 | LAN Gateway & WAN Edge |
| Router R2 | Cisco 2811 | Transit Router (ISP/WAN) |
| Router R3 | Cisco 2811 | Remote Destination (Simulated Internet) |
| Switch S1 | Cisco 2960 | Distribution & Aggregation |
| Switch S2 | Cisco 2960 | Client & Management Access |

---

## IP Addressing Plan (VLSM)
An optimized addressing scheme was applied to conserve IPv4 addresses, especially on point-to-point links (/30).

| Device | Interface | IP Address | Mask (CIDR) | Description |
| :--- | :--- | :--- | :--- | :--- |
| R1 | Fa0/0.10 | 172.18.10.14 | /28 | Gateway VLAN 10 |
| R1 | Fa0/0.20 | 172.18.20.14 | /28 | Gateway VLAN 20 |
| R1 | Fa0/0.30 | 172.18.30.14 | /28 | Gateway VLAN 30 |
| R1 | S0/0/0 | 10.0.30.177 | /30 | Link to R2 |
| R2 | S0/0/0 | 10.0.30.178 | /30 | Link to R1 |
| R3 | Loopback0 | 10.0.30.129 | /32 | Target IP (Test) |
| S2 | Vlan60 | 172.18.60.2 | /28 | Management Interface |

---

## Key Configuration Points

### 1. Link Aggregation (EtherChannel)
Configuration of LACP (Link Aggregation Control Protocol) between switches S1 and S2 for redundancy and increased bandwidth.

```
! S1 Configuration Snippet
interface range FastEthernet0/21-22
 channel-group 1 mode active
!
interface Port-channel1
 switchport mode trunk
 switchport trunk native vlan 50
```

### 2. Inter-VLAN Routing
Utilization of sub-interfaces on router R1 with 802.1Q encapsulation.

```
! R1 Configuration Snippet
interface FastEthernet0/0.10
 encapsulation dot1Q 10
 ip address 172.18.10.14 255.255.255.240
```

---

## Validation & Testing

### Connectivity Test (Ping & Traceroute)
The following test validates the complete packet path from the local network (VLAN 10) to the simulated remote network (R3), traversing the R1 gateway and the WAN.

```
PC1> ping 10.0.30.129

Pinging 10.0.30.129 with 32 bytes of data:

Reply from 10.0.30.129: bytes=32 time=1ms TTL=126
Reply from 10.0.30.129: bytes=32 time=2ms TTL=126
...
```

### EtherChannel Verification
The `show etherchannel summary` command confirms that the Port-Channel is active (SU) and that physical ports are aggregated via the LACP protocol (P).

```
S1# show etherchannel summary
Flags:  D - down        P - bundled in port-channel
...
```

---

## Repository Structure

```
├── configs/                 # Configuration files (.txt)
├── images/                  # Topology diagrams and test screenshots
├── Architecture_Reseau.pkt  # Cisco Packet Tracer simulation file
└── README.md                # This file
```

---

## License
This project is for educational purposes as part of an academic module.

Academic Project - Boustane Oussama 2025/2026
```
