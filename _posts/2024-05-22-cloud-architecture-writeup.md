---
layout: post
title: "Network Design - Cloud Architecture"
date: 2024-05-22
---

## Network Design Overview

Creating a robust enterprise network involves strategic planning, careful selection of hardware and software, and meticulous implementation. This architecture showcases the development of an enterprise network using high-quality hardware, a new network configuration, and optimized departmental segmentation.

### Initial Budget Allocation and Hardware Selection

The allocated budget covered both the initial setup costs and the yearly operational expenses. The hardware selected ensures reliability, scalability, and performance.

#### Upfront Costs

| Item Name | Price | Quantity | Total Cost |
| --- | --- | --- | --- |
| [Cat6 Ethernet Cable (150ft)](https://www.amazon.com/Cable-Matters-Snagless-Ethernet-Black/dp/B00B3UTRWI/ref=sr_1_3?crid=1FGIJIVBAX429&keywords=cat6%2Bethernet%2Bcable%2B150%2Bft&qid=1701453230&sprefix=cat6%2Bethernet%2Bcable%2B150%2Caps%2C174&sr=8-3&th=1) | $30.00 | 100 | $3,000.00 |
| [Cisco Catalyst 9000](https://www.amazon.com/Cisco-Catalyst-C9300-48Un-A-Switch-Renewed/dp/B09DT9VXYR/ref=sr_1_4?crid=PBTIU71W0X51&keywords=cisco+catalyst+9000&qid=1701363788&sprefix=Cisco+Catalyst+9000%2Caps%2C163&sr=8-4) | $2,200.00 | 2 | $4,400.00 |
| [Cisco Catalyst 1200](https://www.amazon.com/Cisco-Catalyst-1200-24FP-4G-Protection-C1200-24FP-4G/dp/B0CJ3VNWS7/ref=sr_1_2_sspa?crid=JFFPQSPXWUC9&keywords=Cisco+Catalyst+3000&qid=1701445036&s=electronics&sprefix=cisco+catalyst+3000%2Celectronics%2C149&sr=1-2-spons&ufe=app_do%3Aamzn1.fos.ac2169a1-b668-44b9-8bd0-5ec63b24bcb5&sp_csd=d2lkZ2V0TmFtZT1zcF9hdGY&psc=1) | $800.00 | 7 | $5,600.00 |
| [Cisco C1111X-8P Integrated Services Router](https://www.amazon.com/dp/B0C8TPKTHC/ref=dp_cr_wdg_tit_rfb) | $950.00 | 1 | $950.00 |
| [FORTINET FortiGate](https://www.amazon.com/FORTINET-FORTIGATE-Next-Firewall-FG-40F/dp/B084HKDKM9/ref=sr_1_3?crid=26UB58A0LW0EU&keywords=fortinet%2Bfortigate&qid=1701444625&sprefix=Fortinet%2BFortiGate%2Caps%2C152&sr=8-3&ufe=app_do%3Aamzn1.fos.ac2169a1-b668-44b9-8bd0-5ec63b24bcb5&th=1) | $287.00 | 2 | $574.00 |
| [CyberPower UPS - 1500VA/1000W](https://www.amazon.com/CyberPower-CP1500PFCLCD-Sinewave-Outlets-Mini-Tower/dp/B00429N19W/ref=sr_1_1_sspa?crid=2RKOPCKC0IKIU&keywords=APC%2BSmart-UPS&qid=1701465779&sprefix=apc%2Bsmart-ups%2Caps%2C166&sr=8-1-spons&ufe=app_do%3Aamzn1.fos.18ed3cb5-28d5-4975-8bc7-93deae8f9840&sp_csd=d2lkZ2V0TmFtZT1zcF9hdGY&th=1) | $220.00 | 2 | $440.00 |

#### Yearly Costs

| Item Name | Price | Quantity | Total Cost |
| --- | --- | --- | --- |
| Google Cloud | $32,700.00 | 1 | $32,700.00 |
| [Google Fiber (ISP)](https://www.switchful.com/compare/internet/clearwave-fiber-vs-google-fiber) | $1,200.00 | 1 | $1,200.00 |

### Network Architecture Overview

The network is designed for high performance and reliability, with a focus on ensuring high availability and redundancy. The architecture leverages sophisticated hardware components, cloud services, and robust security measures.

#### Network Diagram

The following diagram illustrates the network configuration:

```bash
                                 Internet
                                      |
                                 [ISP Router]
                                   10.0.0.1
                                      |
                                    WAN1
                                      |
                                 [Firewall]
                                   10.0.0.2
                                      |
                                 Ports 1, 2, 3
                                      |
                                 [Core Switch - L3]
                                    10.0.0.3
         +------------+-----------+-----------+-----------+---------------+
         |            |           |           |           |               |
   Ports 4/1          5/2         6/3         10/4        11/5            12/6
         |            |           |           |           |               |
         [Finance]    [HR]        [Sales]     [IT]        [Development]   [Operations]
         10.0.1.0/27  10.0.2.0/28 10.0.3.0/28 10.0.4.0/26 10.0.5.0/26     10.0.6.0/26
```

### Detailed Network Configuration

#### Subnet Allocation and IP Addressing

| Department | Network Address | CIDR Notation | Subnet Mask | Default Gateway | DHCP Address Pool |
| --- | --- | --- | --- | --- | --- |
| Finance | 10.0.1.0 | /27 | 255.255.255.224 | 10.0.1.1 | 10.0.1.2 - 10.0.1.30 |
| Human Resources | 10.0.2.0 | /28 | 255.255.255.240 | 10.0.2.1 | 10.0.2.2 - 10.0.2.14 |
| Sales | 10.0.3.0 | /28 | 255.255.255.240 | 10.0.3.1 | 10.0.3.2 - 10.0.3.14 |
| IT & Support | 10.0.4.0 | /26 | 255.255.255.192 | 10.0.4.1 | 10.0.4.2 - 10.0.4.62 |
| Development | 10.0.5.0 | /26 | 255.255.255.192 | 10.0.5.1 | 10.0.5.2 - 10.0.5.62 |
| Operations | 10.0.6.0 | /26 | 255.255.255.192 | 10.0.6.1 | 10.0.6.2 - 10.0.6.62 |

### Network Hardening for Security

To protect against various security threats, several measures were implemented to harden the network and systems:

1. **Firewall Deployment**:
   - The FortiGate firewall filters incoming and outgoing traffic, blocking unauthorized access and mitigating threats such as DDoS attacks.
2. **Intrusion Detection and Prevention Systems (IDPS)**:
   - Integrated IDPS within the firewall and core switch to detect and respond to malicious activities in real-time.
3. **VLAN Segmentation**:
   - Departmental VLANs prevent lateral movement of threats within the network, isolating sensitive data and limiting access based on departmental roles.
4. **Access Control Lists (ACL)**:
   - Configured ACLs on the Layer 3 switches to control the flow of traffic and restrict access to critical resources.
5. **Regular Security Audits and Updates**:
   - Scheduled security audits and routine updates to firmware and software to address vulnerabilities and enhance protection.

### Ensuring Network Reliability and Redundancy

To ensure continuous operation and minimize downtime, the following measures are implemented:

- **Redundant Hardware**: Spare switches and firewalls are available for quick deployment in case of failure.
- **Redundant Connections**: Critical network devices are connected using multiple network paths to ensure redundancy.
- **Redundant Power Supplies**: UPS units provide backup power, ensuring network stability during power outages.
- **Cloud Services**: GCP’s high availability guarantees reduce the risk.