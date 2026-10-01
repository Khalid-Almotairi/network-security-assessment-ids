# Network Security Assessment & Intrusion Detection

A hands-on network security project focused on assessing, attacking, monitoring, and hardening a small lab network environment.

The project demonstrates practical security techniques across network reconnaissance, traffic interception, host-based firewall enforcement, and intrusion detection using Suricata.

---

## Overview

This project simulates a small network environment containing a client, an attacker/security testing machine, and a server.

The assessment follows a defensive security workflow:

1. Build and validate the lab network
2. Perform network reconnaissance and port scanning
3. Analyze network traffic using Wireshark
4. Demonstrate ARP spoofing and traffic interception
5. Configure and validate host-based firewall controls
6. Deploy and test Suricata as an Intrusion Detection System (IDS)
7. Analyze remaining security risks
8. Propose architectural improvements

---

## Objectives

* Assess the security of a controlled network environment
* Identify exposed network services and attack surfaces
* Perform TCP and stealth reconnaissance
* Analyze network traffic with Wireshark
* Demonstrate the impact of ARP spoofing in a flat LAN
* Implement host-based access control using `iptables`
* Validate firewall rules through Nmap scanning
* Monitor suspicious network activity using Suricata
* Identify residual risks and propose security improvements

---

## Lab Environment

The project uses a controlled virtual network environment consisting of:

| Component  | Role                                |
| ---------- | ----------------------------------- |
| Kali Linux | Security testing / Red Team machine |
| Client     | Network client                      |
| Server     | Target / Research Data Server       |
| Wireshark  | Network traffic analysis            |
| Nmap       | Network reconnaissance              |
| iptables   | Host-based firewall                 |
| Suricata   | Intrusion Detection System          |

The environment is isolated and used strictly for authorized security testing.

---

## Project Phases

### Phase 1 — Lab Environment

The network environment was configured and connectivity was verified between the participating machines.

Basic connectivity was tested using ICMP:

```text
Kali → Server
Client → Server
```

This phase established the baseline network communication before performing security testing.

---

### Phase 2 — Network Security Assessment

#### A. Red Team Reconnaissance

Network reconnaissance was performed using Nmap.

Two scanning techniques were evaluated:

* Full TCP connection scan
* TCP SYN / stealth scan

The resulting traffic was also examined using Wireshark to understand how reconnaissance activity appears at the packet level.

#### B. Traffic Interception

ARP spoofing was demonstrated within the controlled lab environment.

The client ARP table was inspected to verify the poisoning state, followed by packet analysis using Wireshark.

This demonstrates how a malicious host on the same Layer 2 network can position itself between communicating hosts and intercept traffic.

---

### Phase 3 — Firewall Implementation

Host-based access control was implemented using `iptables`.

The firewall was configured to restrict unauthorized access to selected services, including:

```text
FTP  - TCP/21
Telnet - TCP/23
```

Firewall effectiveness was validated using Nmap.

Blocked ports were observed as filtered during scanning.

Firewall logs were also inspected to verify that unauthorized connection attempts were detected and recorded.

Example log entries identified blocked SYN probes originating from the testing machine.

---

### Phase 4 — Intrusion Detection

Suricata was deployed as an Intrusion Detection System (IDS).

The system was tested against reconnaissance activity targeting the server.

The IDS successfully detected and logged probing activity against the SSH service on:

```text
TCP/22
```

This demonstrates the difference between:

* **Prevention** — firewall rules blocking unwanted traffic
* **Detection** — Suricata identifying and logging suspicious activity

---

## Security Findings

The assessment demonstrated that multiple layers of defense can work together:

```text
                Network Activity
                       │
                       ▼
              ┌─────────────────┐
              │   Recon / Scan  │
              └────────┬────────┘
                       │
                       ▼
              ┌─────────────────┐
              │    Firewall     │
              │    iptables     │
              └────────┬────────┘
                       │
                 Allowed Traffic
                       │
                       ▼
              ┌─────────────────┐
              │     Server      │
              └────────┬────────┘
                       │
                       ▼
              ┌─────────────────┐
              │    Suricata     │
              │      IDS        │
              └─────────────────┘
```

However, the assessment also identified a significant architectural weakness: the lab operates within a single flat Layer 2 broadcast domain.

Because the network is not segmented, an internal attacker can potentially perform Layer 2 attacks such as:

* ARP spoofing
* MAC flooding
* Man-in-the-Middle attacks
* Local traffic interception

Host-based firewall rules and signature-based IDS controls do not eliminate this underlying Layer 2 weakness.

---

## Recommended Improvements

### 1. Network Segmentation

Separate sensitive servers into dedicated VLANs.

For example:

```text
                ┌─────────────────┐
                │     Clients     │
                │     VLAN 10     │
                └────────┬────────┘
                         │
                         ▼
                 ┌───────────────┐
                 │ Router / NGFW │
                 └───────┬───────┘
                         │
                         ▼
                ┌─────────────────┐
                │  Data Servers   │
                │     VLAN 20     │
                └─────────────────┘
```

This forces traffic between network zones through a routing or firewall layer instead of allowing unrestricted Layer 2 communication.

### 2. Dynamic ARP Inspection

Deploy Dynamic ARP Inspection (DAI) together with DHCP Snooping.

DAI can validate ARP packets against trusted DHCP/IP-MAC bindings and drop illegitimate ARP responses.

These measures address the Layer 2 weaknesses identified during the assessment.

---

## Tools & Technologies

* **Kali Linux**
* **Nmap**
* **Wireshark**
* **iptables**
* **Suricata**
* **Linux**
* **TCP/IP**
* **ARP**
* **Network Security**
* **Intrusion Detection**

---

## Key Takeaways

This project demonstrates a layered approach to network security:

```text
Reconnaissance
      ↓
Traffic Analysis
      ↓
Attack Simulation
      ↓
Firewall Prevention
      ↓
IDS Detection
      ↓
Risk Assessment
      ↓
Security Improvements
```

The assessment shows that blocking network services alone is not sufficient to secure a network. Network architecture and Layer 2 protections are also important when defending against internal threats.

---

## Repository Structure

```text
.
├── README.md
├── docs/
│   └── Network-Security-Project.pdf
├── screenshots/
├── configs/
└── .gitignore
```

---

## Disclaimer

This project was conducted in an isolated, authorized laboratory environment for educational purposes.

The techniques demonstrated in this repository should only be used against systems and networks where explicit authorization has been granted.

---

## Author

**Network Security Project Team**
Abdulrahman Alzahrani 
Khalid Almutairi 
Mohammed Alsayed 

