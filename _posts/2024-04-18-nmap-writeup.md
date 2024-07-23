---
layout: post
title: "Network Scanning with Nmap"
date: 2024-04-18
---

## Table of Contents
1. [Active and Passive Network Scanning](#active-and-passive-network-scanning)
    1. [Passive Scanning](#passive-scanning)
    2. [Active Scanning](#active-scanning)
    3. [Integrating Scanning Types](#integrating-scanning-types)
2. [Determining Network Range](#determining-network-range)
    1. [Step 1: Convert Subnet Mask to CIDR Notation](#step-1-convert-subnet-mask-to-cidr-notation)
    2. [Step 2: Determine the Network Address](#step-2-determine-the-network-address)
    3. [Step 3: Determine the Broadcast Address](#step-3-determine-the-broadcast-address)
    4. [Step 4: Determine the Range of Usable IP Addresses](#step-4-determine-the-range-of-usable-ip-addresses)
    5. [Step 5: Use Nmap to Scan the Network](#step-5-use-nmap-to-scan-the-network)
3. [Nmap Switches](#nmap-switches)
4. [Scan Types Overview](#scan-types-overview)
    1. [Basic Scan Types](#basic-scan-types)
    2. [Advanced TCP Scan Types](#advanced-tcp-scan-types)
    3. [Network Scanning: ICMP](#network-scanning-icmp)
5. [TCP Connect Scans](#tcp-connect-scans)
6. [SYN Scans](#syn-scans)
7. [UDP Scans](#udp-scans)
8. [NULL, FIN, and Xmas Scans](#null-fin-and-xmas-scans)
9. [ICMP Network Scanning](#icmp-network-scanning)
10. [ARP Scans](#arp-scans)
11. [NSE Scripts Overview](#nse-scripts-overview)
12. [Firewall Evasion](#firewall-evasion)
13. [Glossary](#glossary)

## Active and Passive Network Scanning

Network scanning can be broadly categorized into two types: active and passive. Each type has its own methodologies and tools, suited for different scenarios and objectives. Understanding both types allows a security professional to choose the appropriate approach based on the environment and the goal of the assessment.

### Passive Scanning

Passive scanning involves monitoring network traffic without sending any data to the network. This method is non-intrusive and stealthy, as it does not alert the network or its devices to the presence of the scanner.

- **Tools**: One of the most popular tools for passive scanning is `airodump-ng`. This tool is part of the Aircrack-ng suite and is commonly used for capturing raw 802.11 frames in monitor mode. It's particularly useful for gathering information about Wi-Fi networks without joining them.
  
- **Usage**: The basic command to start capturing traffic with `airodump-ng` is:

  ```bash
  airodump-ng wlan0
  ```

  Here, `wlan0` is the name of your wireless network interface in monitor mode. This command captures all visible Wi-Fi data packets, which can then be analyzed to gather information about the networks and devices in range.

- **Advantages**: Passive scanning is highly discreet, making it ideal for initial reconnaissance where you wish to avoid detection. It allows the collection of data such as SSIDs, MAC addresses, and encryption types, without interacting with the network directly.

- **Limitations**: Since passive scanning only captures ongoing traffic, it may not provide a complete picture of the network if the traffic is sparse or encrypted. Additionally, some devices or networks may not be detectable if they are not actively transmitting data.

### Active Scanning

In contrast, active scanning involves sending packets or requests to the network and analyzing the responses received. This approach is more direct and often provides more comprehensive data about the network.

- **Tools**: `Nmap` is one of the most versatile active scanning tools available, offering a wide range of scanning techniques as detailed earlier. It can discover hosts, services, operating systems, and network devices along with their configurations and vulnerabilities.

- **Usage**: A simple example of an active scan using Nmap to discover live hosts on a network is:

  ```bash
  nmap -sn 192.168.1.0/24
  ```

  This command performs a ping sweep to identify which hosts are up without scanning ports.

- **Advantages**: Active scanning can quickly provide detailed information about the network structure, open ports, running services, and potential vulnerabilities. It is more likely to discover all active devices and services on a network.

- **Limitations**: Because active scanning sends traffic to the network, it is more easily detected by intrusion detection systems, firewalls, or network monitoring tools. This can lead to potential blocks or security alerts.

### Integrating Scanning Types

For thorough network reconnaissance, combining passive and active scanning techniques can be highly effective. Begin with passive scanning to observe the network without raising alarms, then follow up with active scanning for a deeper dive into the network's architecture and vulnerabilities. This phased approach helps in maintaining stealth while gradually escalating the intensity of the scan according to the sensitivity and security posture of the network.

## Determining Network Range

To determine the network range using an example IP address 172.22.17.130 and subnet mask 255.255.240.0, follow these steps:

### Step 1: Convert Subnet Mask to CIDR Notation

The subnet mask 255.255.240.0 can be converted to CIDR notation. This involves counting the number of 1s in the binary representation of the subnet mask:

```
255.255.240.0 in binary is:
11111111.11111111.11110000.00000000
```

Counting the 1s gives us 20, so the CIDR notation is /20.

### Step 2: Determine the Network Address

To find the network address, perform a bitwise AND operation between the IP address and the subnet mask:

```
IP Address:        172.22.17.130 -> 10101100.00010110.00010001.10000010
Subnet Mask:       255.255.240.0  -> 11111111.11111111.11110000.00000000
Network Address:   172.22.16.0    -> 10101100.00010110.00010000.00000000
```

### Step 3: Determine the Broadcast Address

The broadcast address can be found by setting all the host bits to 1 (the bits not covered by the subnet mask):

```
Network Address:   172.22.16.0    -> 10101100.00010110.00010000.00000000
Host Bits to 1:    00000000.00000000.00001111.11111111
Broadcast Address: 172.22.31.255  -> 10101100.00010110.00011111.11111111
```

### Step 4: Determine the Range of Usable IP Addresses

The range of usable IP addresses is from one more than the network address to one less than the broadcast address:

- **Network Address**: 172.22.16.0
- **Usable Range**: 172.22.16.1 to 172.22.31.254
- **Broadcast Address**: 172.22.31.255

### Step 5: Use Nmap to Scan the Network

Now that we have the network range, we can use Nmap to scan it:

```bash
nmap 172.22.16.0/20
```

This command will scan all IP addresses from 172.22.16.0 to 172.22.31.255 and list the active hosts. This approach is effective for discovering devices on the network without performing intrusive scans.

## Nmap Switches

Nmap is a powerful network scanning tool that uses various switches (options) to customize its behavior. Below are some commonly used switches and their functionalities:

#### Switch Descriptions

- **-A**:
  - Activates comprehensive scanning features including OS detection (-O), version detection (-sV), script scanning (-sC), and traceroute, providing in-depth analysis of the target.

- **-F**:
  - Performs a fast scan by scanning the top 100 most common ports instead of the default top 1000 ports, significantly reducing scan time while still targeting commonly used ports.

- **-oA**:
  - Saves scan results in all major output formats (normal, XML, and grepable) at once, providing a comprehensive set of files for analysis and reporting.

- **-oG**:
  - Saves results in a "grepable" format, useful for easy parsing and analysis via text processing tools.

- **-oN**:
  - Saves scan results in a normal, human-readable format.

- **-p**:
  - Directs Nmap to scan specific ports or ranges of ports, allowing targeted scanning based on input format (e.g., -p 1-1000 for a range or -p 80,443,8080 for specific ports).

- **-Pn**:
  - Skips the host discovery phase, assuming the target is online. Useful for targets that don't respond to ping requests, avoiding unnecessary preliminary checks.

- **-sC**:
  - Runs a set of default Nmap Scripting Engine (NSE) scripts, which are designed to provide additional information about the target, often focusing on services associated with the open ports detected during the scan.

- **-T**:
  - Specifies timing template (from 0 for "paranoid" to 5 for "insane"), controlling speed and stealth of the scan by adjusting how aggressively packets are sent.

- **-v**:
  - Increases verbosity level of Nmap's output to provide more detailed information about the scanning process.

- **-vv**:
  - Further increases verbosity to a "very verbose" level, giving granular details about the scan's progress.

#### Example Commands

Switches (options) can be placed in any order in the command line.

```bash
nmap -vv -A -T2 -p- <target>
```

This configuration prioritizes comprehensiveness, making it suitable for situations where you have time to gather information and are more concerned about being stealthy.

```bash
nmap <target> -vv -Pn -n -sS -F -T5 --min-parallelism 10 
```

This configuration prioritizes speed, making it suitable for situations where you need information quickly and are less concerned about being stealthy.

## Scan Types Overview

Nmap offers a variety of scan types for different purposes. Understanding each type's mechanics and use case can greatly enhance the effectiveness of your reconnaissance in penetration testing.

#### Basic Scan Types

- **TCP Connect Scans (`-sT`)**:
  - This scan type attempts to establish a full TCP connection with each targeted port. It completes the three-way handshake and is logged by most systems. This is the most basic form of TCP scanning but can be easily detected.

- **SYN "Half-open" Scans (`-sS`)**:
  - Often referred to as stealth scanning, it sends a SYN packet and waits for a response, but does not complete the handshake (does not send the final ACK). This method is less likely to be logged by the target system, making it preferable for avoiding detection.

- **UDP Scans (`-sU`)**:
  - UDP scanning involves sending UDP packets to different ports and observing the response. This type of scan is essential for discovering "open" UDP ports as there is no handshake protocol like TCP, making it more challenging and slower to execute accurately.

#### Advanced TCP Scan Types

- **TCP Null Scans (`-sN`)**:
  - This scan type sends a TCP packet with no flags set. Most operating systems will respond to these packets with a reset response if the port is closed, but no response is expected for open ports. This is useful for evading firewall rules that expect specific flag combinations.

- **TCP FIN Scans (`-sF`)**:
  - Similar to the Null Scan, a FIN scan sends TCP packets with only the FIN flag set, designed to bypass non-stateful firewalls and some IDS (Intrusion Detection Systems) configurations. Closed ports respond with a reset, while open ports do not respond at all.

- **TCP Xmas Scans (`-sX`)**:
  - This scan sends packets with the FIN, PSH, and URG flags set, lighting up the packet like a Christmas tree. It operates under the same principle as Null and FIN scans, where closed ports send a reset, and open ports are silent.

#### Network Scanning: ICMP

- **ICMP (or "ping") Scanning**:
  - ICMP scanning involves sending echo request messages to network devices and monitoring echo replies (ping responses). This type of scan is typically used to discover which hosts are up in a network, forming a foundational step in network mapping.

Understanding the nuances of these scan types allows you to tailor your approach to the network environment and the security measures in place. While TCP Connect, SYN, and UDP scans cover most needs, specialized scans like Null, FIN, and Xmas provide tools for more specific contexts, especially in tightly secured or monitored networks.

## TCP Connect Scans

The TCP Connect scan is the default TCP scan method used by Nmap when executed without root privileges. This scan type completes the full TCP three-way handshake process with each target port, directly interacting with the TCP/IP stack of the operating system.

#### Command Usage

```bash
nmap -sT <target>
```

#### Three-Way Handshake

The three-way handshake involves:

1. **SYN Sent**: The client (attacker's machine) sends a SYN packet to initiate a connection.
2. **SYN/ACK Received**: If the port is open, the server responds with a SYN/ACK packet.
3. **ACK Sent**: The client completes the handshake by sending an ACK packet.

This handshake mechanism confirms whether a TCP port is open and able to accept connections.

#### Responses and Port Status

- **Closed Ports**: According to RFC 9293, a closed port responds to unsolicited SYN packets with an RST (Reset) packet. This allows Nmap to accurately determine that the port is closed.
- **Open Ports**: An open port responds to a SYN with a SYN/ACK, prompting Nmap to mark the port as open after completing the handshake with an ACK.
- **Filtered Ports**: If a port is behind a firewall that drops packets (a common configuration), Nmap receives no response, leading it to categorize the port as filtered. Alternatively, firewalls can be configured to send a TCP RST packet, misleading Nmap into recording the port as closed.

#### Firewall Considerations

Configuring a firewall to respond with a TCP reset packet can be done simply on Linux using IPtables:

```bash
iptables -I INPUT -p tcp --dport <port> -j REJECT --reject-with tcp-reset
```

This configuration complicates the accurate assessment of port status, as it can make an open port appear closed.

## SYN Scans

SYN scans, indicated by `-sS`, are a common method for exploring the TCP ports of a target. These scans are distinctive from full TCP scans, being sometimes referred to as "Half-open" or "Stealth" scans.

#### Command for SYN Scan

```bash
nmap -sS <target>
```

#### How SYN Scans Work

In a SYN scan, the scanner sends a SYN packet and, upon receiving a SYN/ACK in response, it returns a RST packet. This sequence avoids the completion of the three-way handshake that characterizes a full TCP scan, thus:

1. A SYN packet is sent to the target port.
2. If the port is open, the target responds with a SYN/ACK.
3. The scanner then sends a RST packet to prevent a full connection establishment.

#### Advantages

SYN scans offer several benefits:

- **Stealth Mode**: They are less likely to be detected by older Intrusion Detection Systems (IDS) that monitor for completed handshakes.
- **No Logs on Target**: Many applications log connections only upon a completed handshake, so SYN scans may not be recorded.
- **Speed**: These scans are faster as they do not establish and then close a full connection for each checked port.

#### Disadvantages

However, there are drawbacks:

- **Root Privileges Required**: SYN scans need raw packet capabilities, which are typically only granted to root users, to function correctly.
- **Potential Service Disruption**: Poorly configured or unstable services might crash under the unusual network load created by SYN scans.

#### Use in Nmap

SYN scans are the default method in Nmap when executed with root privileges. Without root access, Nmap reverts to using the slower TCP Connect scan.

#### Port Responses

- **Closed Ports**: Respond with a RST packet, similar to TCP Connect scans.
- **Filtered Ports**: Are either non-responsive or send a spoofed RST packet, indicating firewall filtering.

Despite some limitations, the advantages of using SYN scans typically outweigh the disadvantages, making them a favored choice for initial reconnaissance in network security assessments.

## UDP Scans

#### UDP Scan Command

```bash
nmap -sU <target>
```

Scanning UDP ports, which are stateless, with Nmap's UDP scan option.

## NULL, FIN, and Xmas Scans

#### NULL, FIN, and Xmas Scan Commands

- NULL Scan:

  ```bash
  nmap -sN <target>
  ```

- FIN Scan:

  ```bash
  nmap -sF <target>
  ```

- Xmas Scan:

  ```bash
  nmap -sX <target>
  ```

These scans can be used to stealthily scan a target without establishing a full TCP connection.

## ICMP Network Scanning

In network security assessments, especially in black box settings where the internal structure of a network is unknown, it's crucial to first identify active hosts. This process, known as a "ping sweep," involves scanning IP addresses to detect which ones are online.

#### Command for ICMP Echo Scan

```bash
nmap -sn <target>
```

#### How ICMP Echo Scans Work

Nmap's ICMP echo scan, performed using the `-sn` switch, sends an ICMP echo request to each IP address within a specified range to check for host availability. For example, to scan the 192.168.0.x network, you could use:

- `nmap -sn 192.168.0.1-254`
- `nmap -sn 192.168.0.0/24`

This type of scan is designed to map out which IP addresses are active without scanning any ports.

#### Ping Sweep Mechanics

The `-sn` option in Nmap does the following:

- **No Port Scanning**: It specifically instructs Nmap not to perform port scanning.
- **ICMP Echo Requests**: Sends ICMP packets, commonly known as "ping" requests, to determine if a host is online.
- **Additional Packets**: On local networks, if run with root privileges, it utilizes ARP requests. On all networks, it sends a TCP SYN packet to port 443 and a TCP ACK packet (or TCP SYN if not run as root) to port 80 to further verify host availability.

#### Advantages and Limitations

- **Advantages**:
  - **Quick Mapping**: Quickly identifies active hosts without the overhead of port scanning.
  - **Low Profile**: Less intrusive compared to full port scans, potentially evading some basic security measures.

- **Limitations**:
  - **Potential Inaccuracy**: Some hosts may be configured to not respond to ICMP or ARP requests, leading to false negatives.
  - **Firewall Filtering**: Firewalls blocking ICMP or specific TCP packets can prevent accurate detection of live hosts.

While ICMP echo scans provide a foundational overview of network structure by identifying live hosts, they should be supplemented with other scanning techniques for a comprehensive security audit. This method is particularly useful for initial reconnaissance in environments where detailed intelligence on the network is limited.

## ARP Scans

ARP (Address Resolution Protocol) scans are used to discover active hosts on a local network. This type of scan is effective in local area networks (LANs) because it relies on the ARP protocol, which is used for mapping IP network addresses to the hardware (MAC) addresses used by a data link protocol.

### Command for ARP Scan

To perform an ARP scan using Nmap, you can use the following command:

```bash
sudo nmap -PR -sn MACHINE_IP/24
```

### How ARP Scans Work

- **ARP Requests**: Nmap sends ARP requests to each IP address in the specified range. ARP requests ask for the MAC address corresponding to an IP address.
- **ARP Replies**: Active devices on the network respond with their MAC addresses, allowing Nmap to identify which IP addresses are currently in use.

### Advantages of ARP Scans

- **Accuracy**: ARP scans are highly accurate for local networks because they directly query devices at the data link layer, avoiding issues like IP filtering.
- **Speed**: These scans are very fast, as ARP requests and replies are processed quickly by most devices.
- **Stealth**: ARP requests are a normal part of network traffic, making ARP scans relatively stealthy and less likely to be flagged by intrusion detection systems.

### Usage Example

To perform an ARP scan on the 192.168.1.0/24 network, the command would be:

```bash
sudo nmap -PR -sn 192.168.1.0/24
```

This command will identify all active devices on the specified subnet by sending ARP requests and listening for ARP replies.

### Considerations

- **Local Network Only**: ARP scans are only applicable for local networks. For remote networks, other types of scans, such as ICMP or TCP, must be used.
- **Permissions**: ARP scanning typically requires root or administrative privileges, as it involves low-level network interactions.

## NSE Scripts Overview

An overview of Nmap Scripting Engine (NSE) which allows users to write (or use existing) scripts to automate a wide variety of networking tasks.

#### Running Scripts with Nmap

```bash
nmap --script=<script-name> <target>
```

#### Search for NSE Scripts

```bash
ls /usr/share/nmap/scripts | grep <search-term>
```

Locate scripts by name or functionality, and how to use them in scans.

### How `-sC` Works

```bash
nmap -sC <target>
```

1. **Initial Scan**:
   - Nmap first performs a basic scan to identify which ports are open on the target host.

2. **Script Selection**:
   - Based on the open ports and the services detected running on those ports, Nmap selects and runs relevant default scripts to gather more information.
   - These default scripts are categorized under safe and useful scripts that provide further insights without being intrusive.

### Benefits of Running `-sC`

- **Comprehensive Information**: By running default scripts, Nmap can provide a more comprehensive picture of the target’s network configuration and potential vulnerabilities.
- **Automated Process**: You don’t need to manually specify each script; `-sC` automatically selects and runs the relevant ones based on the services detected.

## Firewall Evasion

#### Firewall Evasion Techniques

- Packet Fragmentation:

  ```bash
  nmap -f <target>
  ```

- Decoy Scans:

  ```bash
  nmap -D RND:10 <target>
  ```

- Source Port:

  ```bash
  nmap --source-port 53 <target>
  ```

Techniques to evade firewalls and intrusion detection systems during scanning.

### Disclaimer

**Legal Considerations:** Always ensure you have permission to perform enumeration on the network you are targeting to avoid violating legal or ethical boundaries.

## Glossary

Below is a comprehensive list of Nmap options and their descriptions.

| Argument                         | Description                                                              |
|----------------------------------|--------------------------------------------------------------------------|
| `-6`                             | Enable IPv6 scanning                                                    |
| `-A`                             | Enable OS detection, version detection, script scanning, and traceroute  |
| `-D <decoy1,decoy2,...>`         | Use decoys                                                              |
| `-e <iface>`                     | Use specified interface                                                 |
| `-F`                             | Fast scan                                                               |
| `-f`                             | Fragment packets                                                        |
| `-g <port>` / `--source-port <port>` | Use specified source port                                              |
| `-O`                             | Enable OS detection                                                     |
| `-oA <basename>`                 | Output in all formats (normal, XML, grepable) with a common basename     |
| `-oG <file>`                     | Output scan results in a grepable format                                 |
| `-oN <file>`                     | Output scan results to a normal file                                     |
| `-oX <file>`                     | Output scan results to an XML file                                       |
| `-PA <portlist>`                 | TCP ACK discovery on specified ports                                    |
| `-p <port ranges>`               | Specify ports to scan                                                   |
| `-Pn`                            | Treat all hosts as online, skip host discovery                          |
| `-PR`                            | ARP discovery                                                           |
| `-PS <portlist>`                 | TCP SYN discovery on specified ports                                    |
| `-PU <portlist>`                 | UDP discovery on specified ports                                        |
| `-sA`                            | ACK scan                                                                |
| `-sC`                            | Run default NSE scripts                                                 |
| `-sF`                            | FIN scan                                                                |
| `-sM`                            | Maimon scan                                                             |
| `-sN`                            | NULL scan                                                               |
| `-sP` / `-sn`                    | Ping scan                                                               |
| `-sS`                            | SYN scan                                                                |
| `-sT`                            | TCP connect scan                                                        |
| `-sU`                            | UDP scan                                                                |
| `-sV`                            | Version detection                                                       |
| `-sW`                            | Window scan                                                             |
| `-T<0-5>`                        | Set timing template (0: slowest, 5: fastest)                            |
| `-v` / `-vv`                     | Increase verbosity / Very verbose output                                |
| `--append-output`                | Append to output files                                                  |
| `--data-length <number>`         | Append random data to packets                                           |
| `--iflist`                       | List network interfaces and routes                                      |
| `--ip-options <options>`         | Send packets with specified IP options                                  |
| `--max-parallelism <number>`     | Maximum number of parallel probes                                       |
| `--max-rate <number>`            | Maximum packet send rate                                                |
| `--max-retries <number>`         | Maximum number of retries                                               |
| `--max-rtt-timeout <time>`       | Maximum round trip time timeout                                         |
| `--min-parallelism <number>`     | Minimum number of parallel probes                                       |
| `--min-rate <number>`            | Minimum packet send rate                                                |
| `--min-rtt-timeout <time>`       | Minimum round trip time timeout                                         |
| `--open`                         | Show only open ports                                                    |
| `--osscan-guess`                 | Guess OS aggressively                                                   |
| `--osscan-limit`                 | Limit OS detection to promising targets                                 |
| `--packet-trace`                 | Show all packets sent/received                                          |
| `--reason`                       | Display reason for port state                                           |
| `--resume <filename>`            | Resume an aborted scan                                                  |
| `--script=<scripts>`             | Specify NSE scripts to run                                              |
| `--script-args=<args>`           | Provide arguments to scripts                                            |
| `--script-trace`                 | Show data sent/received during script execution                         |
| `--top-ports <number>`           | Scan the most common ports                                              |
| `--traceroute`                   | Trace the path packets take to the target                               |
| `--version-all`                  | Aggressive version scan                                                 |
| `--version-intensity <level>`    | Set version detection intensity level (0-9)                             |
| `--version-light`                | Light version scan                                                      |
