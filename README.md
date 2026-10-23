# Enterprise Network, Firewall & Server Setup

## Project Overview
Engineered and deployed a complete IT infrastructure and secure network for an enterprise delivery center.

---

##  Network Architecture Diagram

```mermaid
flowchart TD
    subgraph WAN["External Networks & Remote Offices"]
        ISP1["Primary ISP Uplink<br/>198.51.100.0/30"]
        ISP2["Secondary ISP Backup<br/>203.0.113.0/29"]
        Remote["Remote Office (Country X)<br/>IPsec Gateway"]
    end

    subgraph Edge["Perimeter Security Layer"]
        FW["FortiGate Next-Gen Firewall<br/>• Dual-WAN Failover (SD-WAN)<br/>• Route-Based IPsec VPN Tunnel<br/>• Web/App Filtering & IPS Inspection<br/>• FSSO Active Directory Integration"]
    end

    subgraph Core["Core Switching Layer"]
        SW["Enterprise Managed L2/L3 Switch<br/>• 802.1Q VLAN Trunking<br/>• Spanning Tree Protocol (STP)<br/>• Port Security"]
    end

    subgraph Internal["Internal Network Segments"]
        subgraph V10["VLAN 10: Production LAN"]
            Clients["Workstations & Laptops<br/>• Microsoft Entra ID Joined<br/>• Standard User (Least Privilege)<br/>• Full Disk Encryption<br/>(10.100.10.0/24)"]
        end

        subgraph V40["VLAN 40: Wireless Network"]
            AP["Wireless Access Point<br/>(10.100.40.0/24)"]
        end

        subgraph ServerInfra["Server & Storage Subnet"]
            Server["Dell PowerEdge Server<br/>• Windows Server (AD DS / DNS)<br/>• Centralized Secure Storage<br/>• FSSO Identity Agent"]
            iDRAC["iDRAC Management Port<br/>• Out-of-Band Hardware Control"]
        end
    end

    ISP1 --> FW
    ISP2 --> FW
    FW <== "Encrypted IPsec Tunnel" ==> Remote
    FW === SW
    SW --- V10
    SW --- V40
    SW --- ServerInfra
```

---

##  Key Highlights
- **Edge Security & Firewall:** Configured FortiGate NGFW with dual-ISP automatic failover and security filtering policies.
- **Site-to-Site VPN:** Built secure IPsec VPN tunnels connecting the delivery center to international offices (Country X).
- **Identity & Access Management:** Integrated Windows Active Directory with Fortinet Single Sign-On (FSSO) for role-based security.
- **Core Server & Storage:** Configured Dell PowerEdge server (Windows Server) hosting centralized storage with iDRAC remote management.
- **Endpoint Security:** Enforced Microsoft Entra ID, standard user privileges (least privilege), and full-disk encryption on client laptops.

---

##  Logical Network Scheme (Sanitized)

| Segment / VLAN | Description | Gateway / Subnet |
| :--- | :--- | :--- |
| **VLAN 10** | Production Workstations | `10.100.10.1/24` |
| **VLAN 40** | Wireless Network (WLAN) | `10.100.40.1/24` |
| **VLAN 99** | Server & iDRAC Management | `10.100.99.1/24` |
| **WAN 1** | Primary High-Speed ISP | `198.51.100.10/30` |
| **WAN 2** | Secondary Backup ISP | `203.0.113.42/29` |
