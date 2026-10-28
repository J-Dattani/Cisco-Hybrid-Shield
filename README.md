
# Cisco Hybrid Shield
### Secure Hybrid Datacenter Network | Cisco Virtual Internship 2026

<p align="center">
  <img src="https://img.shields.io/badge/Cisco-Packet_Tracer-049FD9?style=for-the-badge&logo=cisco&logoColor=white" alt="Cisco Packet Tracer"/>
  <img src="https://img.shields.io/badge/Domain-Cybersecurity-red?style=for-the-badge" alt="Cybersecurity"/>
  <img src="https://img.shields.io/badge/Network-Segmentation-blue?style=for-the-badge" alt="Network Segmentation"/>
  <img src="https://img.shields.io/badge/Internship-2026-success?style=for-the-badge" alt="Internship 2026"/>
</p>

<p align="center">
  A Cisco Packet Tracer project demonstrating secure campus-to-datacenter connectivity through network segmentation, access control, secure administration, and Layer 2 hardening.
</p>

---

## Table of Contents

- [Overview](#overview)
- [Objectives](#objectives)
- [Architecture](#architecture)
- [Technology Stack](#technology-stack)
- [Network Segmentation](#network-segmentation)
- [Security Implementation](#security-implementation)
- [Verification and Testing](#verification-and-testing)
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
- [Limitations](#limitations)
- [Learning Outcomes](#learning-outcomes)
- [Author](#author)

---

## Overview

Modern enterprises increasingly distribute their applications across private datacenters and public cloud environments. Without appropriate segmentation and access controls, a compromised workload may expose other applications, databases, or internal systems.

**Cisco Hybrid Shield** is a simulated secure hybrid datacenter network developed as part of the **Cisco Virtual Internship 2026 — Cyber Security problem statement**.

The project models a campus network connected to a cloud/datacenter environment. It separates users, applications, and databases into dedicated VLANs and applies network security controls to regulate communication between segments.

The implementation focuses on practical networking concepts, security configuration, and connectivity verification using Cisco Packet Tracer.

### Key Highlights

- Seven logical VLANs across campus and datacenter networks.
- Router-on-a-stick inter-VLAN routing.
- Static routing between campus and cloud/datacenter zones.
- Extended ACLs for application and database isolation.
- SSH version 2 for secure device administration.
- Switch port security with sticky MAC learning.
- PortFast and BPDU Guard on host-facing access ports.
- Administrative shutdown of unused switch interfaces.
- Documented configuration and verification evidence.

---

## Objectives

1. Design a secure campus and hybrid datacenter network topology.
2. Separate faculty, student, and administrative traffic.
3. Isolate application and database workloads using VLANs.
4. Enforce traffic restrictions through extended IPv4 ACLs.
5. Protect administrative access using SSH version 2.
6. Apply Layer 2 security controls to reduce endpoint-related risks.
7. Verify permitted connectivity and intentionally blocked traffic.
8. Document the implementation for technical review and demonstration.

---

## Architecture

The topology consists of two primary network zones.

### 1. Campus Network

The campus network contains separate segments for faculty, students, and administrators.

- **R-MU-CAMPUS** — Campus routing and VLAN gateways.
- **SW-MU-CAMPUS** — Campus VLAN switching and access-port assignments.
- **PC-FACULTY** — Faculty endpoint.
- **PC-STUDENT** — Student endpoint.
- **PC-ADMIN** — Administrative endpoint.

### 2. Cloud / Datacenter Network

The simulated datacenter contains isolated application and database workloads.

- **R-CLOUD** — Datacenter routing and ACL enforcement.
- **SW-CLOUD** — Datacenter VLAN switching.
- **APP-A and APP-B** — Application workloads.
- **DB-A and DB-B** — Database workloads.

### Logical Traffic Flow

```text
Faculty / Students / Admin
            |
       SW-MU-CAMPUS
            |
       R-MU-CAMPUS
            |
     Campus–Cloud Transit
        10.10.20.0/24
            |
         R-CLOUD
            |
        SW-CLOUD
            |
     +------+------+
     |             |
 Applications   Databases
 APP-A / APP-B  DB-A / DB-B
```

VLAN gateways are implemented using router subinterfaces and 802.1Q trunking. Static routes provide inter-site reachability, while extended ACLs control selected traffic flows.

---

## Technology Stack

| Technology | Purpose |
|---|---|
| Cisco Packet Tracer | Network topology simulation and testing |
| Cisco IOS | Router and switch configuration |
| VLANs | Logical network segmentation |
| IEEE 802.1Q | VLAN trunking |
| Router-on-a-stick | Inter-VLAN routing |
| IPv4 static routing | Inter-site connectivity |
| Extended IPv4 ACLs | Traffic filtering and workload isolation |
| SSH v2 | Secure administrative access |
| RSA keys | SSH cryptographic setup |
| Switch Port Security | MAC address restrictions |
| PortFast | Faster spanning-tree transition on host-facing ports |
| BPDU Guard | Protection against unexpected BPDUs on protected access ports |
| ICMP / Ping | Connectivity testing |
| IOS Show Commands | Configuration and security verification |

---

## Network Segmentation

The network uses dedicated VLANs and IPv4 subnets to separate campus users and datacenter workloads.

| Network Segment | VLAN ID | Subnet | Default Gateway |
|---|---:|---|---|
| Faculty | 10 | `10.10.10.0/24` | `10.10.10.1` |
| Students | 20 | `10.10.11.0/24` | `10.10.11.1` |
| Administration | 30 | `10.10.12.0/24` | `10.10.12.1` |
| Cloud App-A | 40 | `10.10.30.0/24` | `10.10.30.1` |
| Cloud App-B | 41 | `10.10.31.0/24` | `10.10.31.1` |
| Cloud DB-A | 50 | `10.10.32.0/24` | `10.10.32.1` |
| Cloud DB-B | 51 | `10.10.33.0/24` | `10.10.33.1` |
| Campus–Cloud Transit | — | `10.10.20.0/24` | Routed link |

### Example Endpoint Addresses

| Device | IPv4 Address |
|---|---|
| PC-FACULTY | `10.10.10.10` |
| PC-STUDENT | `10.10.11.11` |
| PC-ADMIN | `10.10.12.12` |
| APP-A | `10.10.30.10` |
| APP-B | `10.10.31.10` |
| DB-A | `10.10.32.10` |
| DB-B | `10.10.33.10` |
| R-MU-CAMPUS Transit Interface | `10.10.20.1` |
| R-CLOUD Transit Interface | `10.10.20.2` |

---

## Security Implementation

### 1. VLAN-Based Segmentation

Separate VLANs are configured for faculty, students, administration, application workloads, and database workloads.

This creates distinct Layer 2 broadcast domains and provides a foundation for applying network access policies.

### 2. Extended Access Control Lists

Two named ACLs are used to enforce traffic restrictions at the cloud routing boundary:

- `CLOUD-APP-ISOLATION`
- `CLOUD-DB-ISOLATION`

The configured policies restrict selected application-to-application and application-to-database communication while allowing designated traffic paths.

### 3. Secure Device Administration

Network devices are configured with:

- SSH version 2.
- Local user authentication.
- RSA key configuration.
- VTY lines restricted to SSH.
- Local administrative access controls.

These measures replace unrestricted Telnet-based remote management with encrypted SSH sessions.

### 4. Switch Port Security

Protected host-facing switch ports use:

- Sticky MAC address learning.
- A maximum of one secure MAC address per protected port.
- Restrict-mode violation handling.

These controls help reduce unauthorized endpoint connections and MAC-address abuse.

### 5. Spanning Tree Protection

PortFast and BPDU Guard are configured on host-facing access ports to reduce the risk of unexpected spanning-tree BPDUs affecting protected edge ports.

### 6. Unused Port Hardening

Unused switch interfaces are administratively disabled and labelled to reduce unnecessary access points.

### 7. Configuration Persistence

The implemented configurations are saved to startup-config so that the settings persist across device restarts within the simulation.

---

## Verification and Testing

The implementation report documents connectivity tests, security-policy checks, interface verification, and SSH access tests.

| Test Case | Expected / Documented Result |
|---|---|
| PC-FACULTY → `10.10.10.1` | Successful gateway connectivity |
| PC-STUDENT → `10.10.11.1` | Successful gateway connectivity |
| PC-ADMIN → `10.10.12.1` | Successful gateway connectivity |
| DB-A → `10.10.32.1` | Successful gateway connectivity |
| APP-A → APP-B | Blocked by isolation policy |
| APP-A → DB-B | Blocked by isolation policy |
| APP-A → DB-A | Permitted by configured policy |
| APP-B → APP-A | Blocked by isolation policy |
| APP-B → DB-A | Blocked by isolation policy |
| APP-B → DB-B | Permitted by configured policy |
| SSH to network devices | Authenticated management access |
| `show access-lists` | ACL rules and match counters |
| `show port-security` | Port-security configuration and status |
| `show interfaces trunk` | Trunk status and allowed VLANs |
| `show interfaces status` | Interface states, including disabled unused ports |

### Useful Verification Commands

```text
show ip interface brief
show ip route
show access-lists
show vlan brief
show interfaces trunk
show port-security
show spanning-tree
show running-config
```

These commands help inspect interface status, routing, VLAN assignments, ACLs, trunk operation, and Layer 2 protections.

Refer to the accompanying PDF report for the documented test checklist and screenshots SS1–SS37.

---

## Project Structure

```text
Cisco-Hybrid-Shield/
│
├── README.md
├── Cisco_Hybrid_Shield.pkt
├── Cisco_Internship_2026_Secure_Hybrid_Datacenter.pdf
│
└── docs/
    └── Additional documentation (optional)
```

| File | Description |
|---|---|
| `README.md` | Project overview and technical documentation |
| `Cisco_Hybrid_Shield.pkt` | Cisco Packet Tracer simulation file |
| `Cisco_Internship_2026_Secure_Hybrid_Datacenter.pdf` | Detailed implementation report, configuration evidence, and verification checklist |

---

## Getting Started

### Prerequisites

- Cisco Packet Tracer installed on your computer.
- The `.pkt` project file downloaded from this repository.

### Run the Project

1. Clone or download this repository.
2. Locate `Cisco_Hybrid_Shield.pkt`.
3. Open the file in Cisco Packet Tracer.
4. Inspect the campus and cloud/datacenter topology.
5. Open the routers and switches to examine their configurations.
6. Use the endpoint command prompts to run the documented connectivity tests.
7. Inspect the ACLs, port-security settings, and VLAN trunks.
8. Refer to the PDF report for the implementation evidence and expected test outcomes.

### Clone the Repository

Replace `YOUR_USERNAME` with your GitHub username.

```bash
git clone https://github.com/YOUR_USERNAME/Cisco-Hybrid-Shield.git
cd Cisco-Hybrid-Shield
```

---

## Limitations

This project is a **Cisco Packet Tracer simulation**, not a production deployment.

- AWS VPCs, IAM, and Security Groups are discussed as architectural concepts. Their isolation principles are represented through simulated VLAN and ACL policies; no native AWS environment is deployed.
- Kubernetes and microservices are considered in the design context, but no Kubernetes cluster is deployed.
- Packet Tracer results validate the simulated topology and configured policies; they do not establish production readiness.
- Real-world deployment would require additional validation of routing, identity management, cloud-native controls, monitoring, and operational security requirements.

---

## Learning Outcomes

Through this implementation, the project demonstrates practical exposure to:

- Enterprise network design and IPv4 addressing.
- VLAN creation and network segmentation.
- 802.1Q trunking and router-on-a-stick.
- Static routing between network zones.
- Extended ACL configuration and traffic filtering.
- SSH-based network administration.
- Switch port security and spanning-tree protections.
- Connectivity testing and configuration verification.
- Security documentation and technical reporting.

---

## Author

**Jaymin Dattani**

Computer Engineering Student | Cybersecurity & Cloud Networking

- **GitHub:** [J-Dattani](https://github.com/J-Dattani)
- **Portfolio:** [jaymindattani.dev](https://jaymindattani.dev)

---

<p align="center">
  <i>Cisco Virtual Internship 2026 · Cyber Security · Secure Hybrid Datacenter Network</i>
</p>
