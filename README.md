# Simulation_reseau_Entreprise
# Secure Enterprise Network Infrastructure Simulation

A simulated mid-size enterprise network built in Cisco Packet Tracer, covering VLAN segmentation, inter-VLAN routing, centralized network services, and multiple network-security hardening mechanisms.

![Network Topology](images/topology-overview.png)

## Overview

This project designs and simulates a secure enterprise network for a company with several departments and a dedicated server infrastructure. It reproduces the kind of environment a small/mid-size business network would run: segmented departments, centralized services (DHCP, DNS, Web, FTP), controlled internet access, and layered security controls rather than a flat, unsecured LAN.

## Objectives

- Design a complete enterprise network topology
- Segment the network using VLANs
- Configure inter-VLAN routing
- Deploy centralized network services
- Provide secured internet access
- Implement network security mechanisms
- Simulate a realistic, production-like infrastructure

## Topology

| Device | Role |
|---|---|
| R-CORE | Core router — inter-VLAN routing, DHCP relay, NAT/PAT |
| R-ISP | Simulated ISP router — internet uplink |
| SwitchACCESS | Central access switch |
| SW-HR / SW-IT / SW-SALES / SW-SERVERS | Department access switches |
| 4x Server-PT | DHCP, DNS, Web, and File(FTP) servers |

## VLAN Architecture

| VLAN | Name | Subnet | Department |
|---|---|---|---|
| 10 | HR | 192.168.10.0/24 | Human Resources |
| 20 | IT | 192.168.20.0/24 | IT Department |
| 30 | SALES | 192.168.30.0/24 | Sales |
| 40 | SERVERS | 192.168.40.0/24 | Centralized services |
| 99 | MANAGEMENT | 192.168.99.0/24 | Switch/device management |

![VLAN Brief](images/vlan-brief.png)

## Inter-VLAN Routing

Routing between VLANs uses the **Router-on-a-Stick** model: a single physical link from R-CORE to the access switch, split into subinterfaces with `encapsulation dot1Q` per VLAN.

![Router Interface Brief](images/interface-brief.png)

## DHCP Infrastructure

A centralized DHCP server in the SERVERS VLAN provides automatic IP addressing, default gateways, and DNS assignment for all department VLANs. R-CORE relays DHCP requests from remote VLANs to the server using `ip helper-address` on each subinterface.

![DHCP Server Configuration](images/dhcp-config.png)

## Network Services

- **DNS** — resolves internal hostnames (`www.company.local`, `files.company.local`, `dhcp.company.local`) so users don't need to remember server IPs.
- **Web (HTTP)** — hosts an internal company site and internal-facing services.
- **FTP** — centralized file storage and sharing with user-based access.

![DNS Records](images/dns-records.png)

## Security Mechanisms

Security wasn't an afterthought — it's built into three layers:

**Access Control Lists (ACLs)**
Restrict inter-VLAN traffic to only what's needed (e.g. only the IT department VLAN is permitted SSH access to R-CORE; other VLANs are denied).

**DHCP Snooping**
Enabled on the access switch across all VLANs (10, 20, 30, 40, 99) to block rogue DHCP servers and enforce trusted/untrusted port roles, preventing attackers from handing out malicious IP configs.

![DHCP Snooping Verification](images/dhcp-snooping.png)

**Dynamic ARP Inspection (DAI)**
Detects ARP spoofing attempts and protects the network against Man-in-the-Middle attacks by validating ARP packets against the DHCP snooping binding table.

## Internet Access

Internet connectivity is provided through:
- NAT/PAT translation on R-CORE
- A default route toward the simulated ISP router
- A simulated internet-side server representing external connectivity

## Testing & Validation

The following were tested and confirmed working:
- Inter-VLAN communication
- DHCP address assignment across all VLANs
- DNS resolution
- Web server access
- FTP connectivity
- Internet access
- ACL enforcement
- NAT/PAT translation

## Challenges Encountered

- ASA firewall configuration issues within Packet Tracer's simulation limits
- DHCP relay misconfiguration during initial setup
- VLAN trunking errors between switches
- Routing misconfigurations
- ACL rule ordering/logic errors

Each issue was diagnosed and resolved during the deployment and testing phases — see the troubleshooting notes in the project file for details.

## Skills Demonstrated

`VLAN segmentation` `Inter-VLAN routing (Router-on-a-Stick)` `DHCP & DHCP relay` `DNS` `Web/FTP services` `ACLs` `DHCP Snooping` `Dynamic ARP Inspection` `NAT/PAT` `Network troubleshooting`

## Requirements

- [Cisco Packet Tracer](https://www.netacad.com/courses/packet-tracer) (free with a Cisco Networking Academy account) to open and explore the `.pkt` file.

## Author

Cybersecurity student — built as part of a hands-on portfolio of network and security projects.
