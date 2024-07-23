---
layout: post
title: "Packet Analysis with Wireshark"
date: 2024-04-28
---

## Introduction to Wireshark

Wireshark is an essential tool for network administrators and security professionals for monitoring and analyzing network traffic. This comprehensive guide provides an overview of Wireshark's capabilities for packet analysis, alongside a detailed look at the network protocols commonly encountered during network traffic analysis.

### Getting Started with Wireshark

Upon launching Wireshark, the main interface presents options for selecting network interfaces and applying filters to manage the traffic you wish to capture. The activity level of each interface is visually represented, helping to identify active interfaces suitable for capturing traffic.

#### Live Packet Captures and PCAP Analysis

To start a live packet capture:

1. Navigate to the "Manage Capture Filters" section from the green ribbon to explore and set desired filters. These filters reduce the volume of captured data and organize the packets more efficiently.
2. Initiate a capture by double-clicking an interface or using the right-click menu to select "Start Capture".
3. To stop capturing, click the red square and begin analyzing the collected data.

Alternatively, to analyze previously captured data, navigate to `File > Open` and select the desired PCAP file.

#### Basic Packet Analysis Interface

In the packet analysis interface, Wireshark displays vital information about each packet:

- Packet Number
- Timestamp
- Source and Destination Addresses
- Protocol
- Packet Length

Wireshark also uses color coding to differentiate between packet types and potential issues, aiding in quicker identification of important packets.

### Methods for Collecting PCAP Files

Before attempting live captures, consider:

- Starting with a sample capture to verify setup accuracy.
- Ensuring your system has adequate computational power and storage for the expected traffic volume.

#### Network Taps and Other Collection Techniques

For live packet capture, various methods such as network taps, MAC flooding, and ARP poisoning are used. Network taps, including inline taps like the Throwing Star LAN Tap, are commonly utilized for their ability to copy traffic between two points transparently.

### Filtering and Analyzing Packets

Effective packet filtering is crucial when dealing with large volumes of data. Wireshark offers two types of filters: capture filters and display filters. Display filters are applied after capturing the packets and are accessible through the 'Analyze' tab or the filter bar at the top of the interface.

#### Basic Filtering Examples

- **Filter by IP Address**: `ip.addr == <IP Address>`
- **Filter by Source and Destination**: `ip.src == <Source IP> and ip.dst == <Destination IP>`
- **Filter by Protocol**: `tcp.port eq <Port Number> or <Protocol Name>`

### Understanding Protocols Through OSI Model Layers

Wireshark's ability to dissect network traffic through the lens of the OSI (Open Systems Interconnection) model is instrumental in conducting thorough network analyses. This model divides network communication into seven distinct layers, each playing a specific role in the handling of data as it travels through a network. By examining packets at each OSI layer, Wireshark helps users pinpoint the origin of network issues and understand the flow of data through different network protocols.

#### Overview of the OSI Model Layers in Wireshark

1. **Physical Layer (Layer 1)**:
   - **Responsibilities**: Transmission and reception of raw bit streams over a physical medium.
   - **Wireshark Utility**: Shows the physical details like frame synchronization, bit rate control, and physical network interfaces used, such as Ethernet frames.

2. **Data Link Layer (Layer 2)**:
   - **Responsibilities**: Provides node-to-node data transfer—a link between two directly connected nodes. It handles framing, error detection and correction, and MAC addressing.
   - **Wireshark Utility**: Displays Ethernet headers, MAC addresses, and errors such as frame loss or detection issues, which are vital for troubleshooting network access issues and analyzing traffic flow.

3. **Network Layer (Layer 3)**:
   - **Responsibilities**: Handles packet forwarding including routing through intermediate routers. This layer is where Internet Protocol (IP) operates.
   - **Wireshark Utility**: Captures and displays packet routing information such as source and destination IP addresses, subnet masks, and routing protocols involved in packet distribution across networks.

4. **Transport Layer (Layer 4)**:
   - **Responsibilities**: Provides transparent transfer of data between end systems, or host-to-host communication. This layer includes protocols like TCP (Transmission Control Protocol) and UDP (User Datagram Protocol), which are responsible for error recovery, and flow control.
   - **Wireshark Utility**: Shows ports used, sequence numbers, acknowledgments, and flags that indicate the status of the communication (such as SYN and ACK flags in TCP), essential for diagnosing connectivity issues and ensuring data packets are transmitted reliably.

5. **Session Layer (Layer 5)**:
   - **Responsibilities**: Manages sessions between end-users, controlling the setup, management, and termination of connections. Protocols like NFS (Network File System) operate at this level.
   - **Wireshark Utility**: Rarely detailed in packet captures as modern applications often bypass this layer; however, when present, it can show how different applications establish, manage, and terminate sessions.

6. **Presentation Layer (Layer 6)**:
   - **Responsibilities**: Ensures that data is presented correctly, translating between various syntax and semantics and encrypting/decrypting data when necessary. Formats such as JPEG, ASCII, and EBCDIC are handled here.
   - **Wireshark Utility**: Displays data transformation information, such as encryption, compression, and format conversions, aiding in the debugging of data formatting issues.

7. **Application Layer (Layer 7)**:
   - **Responsibilities**: Closest to the end user, this layer interacts with software applications that implement a communicating component. Protocols like HTTP, FTP, SMTP, and DNS operate at this level.
   - **Wireshark Utility**: Provides the most granular details about protocols and data exchanges seen in user applications. It allows users to see the content of the communications, such as website URLs, emails, and other data passed over the network, essential for application-specific troubleshooting and monitoring.

#### Practical Application of OSI Model in Wireshark

Understanding how Wireshark categorizes data across these layers can significantly enhance troubleshooting capabilities. For instance:

- Network administrators can monitor the physical and data link layers to ensure hardware and Ethernet connectivity are functioning correctly.
- System engineers might focus on the transport and network layers to optimize routing and manage traffic efficiently across corporate networks.
- Application developers often look at the application layer to debug protocol implementations and optimize application networking code.

### ARP Overview

The Address Resolution Protocol (ARP) is a critical Layer 2 protocol used primarily to associate IP addresses with their corresponding MAC addresses on a local area network. This association is vital for facilitating communication between devices on the same network segment. ARP operates through two types of messages: REQUEST and REPLY, which help devices discover the MAC address of a target host given its IP address.

#### Understanding ARP Messages

1. **ARP Request**:
   - **Purpose**: Broadcasted by a device to find the MAC address of another device on the same network segment with a known IP address.
   - **Operation Code**: 1 (Request)
   - **Wireshark View**: Displays as a broadcast message to all devices (MAC address: ff:ff:ff:ff:ff:ff) asking who has a specific IP address.

2. **ARP Reply**:
   - **Purpose**: Sent unicast by the device that owns the requested IP address, providing its MAC address.
   - **Operation Code**: 2 (Reply)
   - **Wireshark View**: Shows the MAC and IP address of the responding device, confirming it recognizes the requested IP address as its own.

#### Practical Use of ARP in Wireshark

Wireshark provides capabilities to visualize these ARP interactions:

- **Packet Details**: The ARP packets in Wireshark will list details such as the sender's and target's MAC and IP addresses, allowing users to trace network activity and validate device communications.
- **Name Resolution**: Enabling the "Resolve Physical Addresses" feature under the `View > Name Resolution` menu makes it easier to read the packet captures by replacing MAC addresses with manufacturer names or other identifiers.

#### ARP Traffic Analysis

Analyzing ARP traffic can offer insights into the network's operational state and highlight potential security issues:

- **Normal ARP Traffic**: Involves periodic requests and replies as devices communicate or verify the presence of others on the network.
- **Suspicious ARP Activity**: An excessive number of ARP requests from an unknown source could indicate ARP poisoning or a spoofing attack aimed at intercepting or disrupting normal network communications.

### Examining ARP Packets

In a typical Wireshark capture, ARP packets are relatively straightforward to analyze due to their simple structure:

- **Request Packet Analysis**:
  - Look for packets with an Opcode of 1.
  - Check the target IP address to see which device is being queried.
  - Observe the broadcast nature of the request, indicative of the ARP discovery process.

- **Reply Packet Analysis**:
  - Identify packets with an Opcode of 2.
  - Note the source MAC and IP addresses—these belong to the device responding to the ARP request.
  - The unicast nature of the reply typically confirms a device's presence and its network address.

By paying close attention to these details, users can use ARP data to verify network configuration, diagnose connectivity issues, or detect unusual network behavior that might signify security threats. ARP's simplicity makes it one of the easier protocols to understand and monitor with Wireshark, providing valuable network insights from even a basic packet capture.

### ICMP Overview

The Internet Control Message Protocol (ICMP) is an integral part of the Internet Protocol Suite, used primarily for sending error messages and operational information indicating, for example, that a requested service is not available or that a host or router could not be reached. ICMP is utilized by network devices, like routers, to send error messages and operational information indicating why certain services may not be available or why packets cannot reach their destination.

#### How ICMP Works

ICMP messages are made up of types and codes that help identify the specific reason for the message. These messages are crucial for network diagnosis and are implemented by tools such as `ping` for testing reachability, and `traceroute` to show the route packets take to a particular network destination.

#### Common ICMP Messages

- **Echo Request and Echo Reply**: These are used by the ping tool. An Echo Request (type 8) solicits an Echo Reply (type 0) from the destination node, confirming network reachability and round-trip time.
- **Destination Unreachable**: Various codes under this type identify reasons why a service or host might not be reachable (e.g., network failure, port unreachable).
- **Time Exceeded**: Indicates that a packet has been discarded because it exceeded its time to live (TTL) in the routing process, typically seen in traceroute operations.

### ICMP Traffic Analysis with Wireshark

Analyzing ICMP traffic with Wireshark can provide valuable insights into network health and configuration issues.

#### Analyzing ICMP Request and Reply Packets

When examining ICMP packets in Wireshark:

- **ICMP Request**:
  - **Type**: 8 (Echo Request)
  - **Code**: Usually 0 for Echo Request
  - **Details to Note**: Look at the timestamp to confirm when the request was sent and check the data payload, which usually includes a sequence of bytes used to measure the round-trip time.

- **ICMP Reply**:
  - **Type**: 0 (Echo Reply)
  - **Code**: Consistently 0 for Echo Reply
  - **Key Differences**: Primarily in the type and code; the reply acknowledges receipt of the Echo Request.

#### Signs of Suspicious ICMP Activity

- **Irregular ICMP Types and Codes**: Unusual types or codes not typical for the expected network communications can indicate misconfigurations or malicious activities.
- **Frequent Unreachable Messages**: Frequent Destination Unreachable messages might suggest incorrect routing or denial of service attacks.
- **Modified Data in Echo Requests**: Alterations in the ICMP data payload beyond what is standard for tools like ping may suggest data exfiltration or other illicit activities.

### Practical Applications

In practical network management, ICMP is used for:

- **Network Troubleshooting**: By confirming the reachability of hosts and diagnosing network issues.
- **Security Assessments**: Monitoring ICMP traffic can help identify potential security threats, such as network scanning or denial of service (DoS) attacks.
- **Operational Management**: Ensuring that network pathways and nodes are functioning as expected, and identifying routes that packets take across the network.

ICMP plays a crucial role in network maintenance and management, providing the tools necessary to diagnose and resolve issues efficiently. Understanding and analyzing ICMP traffic with Wireshark allows network administrators and security professionals to maintain optimal network performance and security.

### TCP Overview

Transmission Control Protocol (TCP) is a fundamental protocol within the Internet Protocol Suite that ensures reliable, ordered delivery of a data stream between applications. It manages data packets between computers over a network, which includes packet sequencing, error handling, and ensuring data integrity, availability, and delivery.

#### Key Features of TCP

- **Reliable Delivery**: Guarantees the delivery of data packets in the order they were sent, without duplication.
- **Error Checking**: Includes checks for errors through checksums to ensure that data transfers across the network are free of errors.
- **Flow Control**: Manages data flow to prevent network congestion, adjusting the rate based on the receiver's ability to process data.
- **Congestion Control**: Adjusts the data sending rate when network congestion is detected.

### Analyzing TCP Traffic with Wireshark

Wireshark is particularly adept at visualizing TCP communication dynamics, from connection establishment to termination. Its color-coding feature helps to quickly identify packet types and potential issues in the communication stream.

#### TCP Handshake Analysis

The TCP handshake is critical for establishing a connection between two hosts:

- **SYN Packet**: Initiates the connection.
- **SYN-ACK Packet**: Acknowledges the connection request.
- **ACK Packet**: Finalizes the connection establishment.

This sequence is crucial for setting up a reliable connection where both sides synchronize the sequence numbers used during the TCP session.

#### Common TCP Packet Types

- **RST, ACK Packet**: Indicates that a port is closed or a connection is being reset. For instance, during an Nmap scan, a RST, ACK packet response to a SYN packet typically indicates that the scanned port is closed.
- **FIN Packet**: Used to close a connection, indicating that there is no more data to be sent.

### Practical TCP Packet Analysis

When using Wireshark to analyze TCP traffic, certain elements are key to understanding the nature of the traffic and diagnosing issues:

- **Sequence and Acknowledgment Numbers**: These numbers are critical for identifying the order of packets and ensuring that all packets are accounted for.
- **TCP Flags**: Such as SYN, ACK, and RST, among others, indicate the status of the connection and help to diagnose common issues like premature connection closures or external interruptions.
- **Window Size**: Reflects the amount of data that can be sent before needing an acknowledgment, vital for understanding flow control issues.

#### Advanced TCP Analysis Techniques

For a deeper analysis, particularly when dealing with a large volume of packets, you may need to use additional tools:

- **RSA NetWitness**: Useful for advanced analysis and visualization of large data sets.
- **NetworkMiner**: Can be employed for parsing captured traffic and reconstructing files and certificates.

### TCP Analysis in Security Contexts

TCP traffic analysis is not just about performance and troubleshooting; it's also critical in security assessments:

- **Identifying Suspicious Patterns**: Such as unusual sequences of RST packets, which might indicate port scanning or denial of service attacks.
- **Understanding Attack Signatures**: Like those seen in SYN flood attacks, where attackers attempt to overwhelm a target by repeatedly sending SYN packets to consume server resources.

TCP's design for reliable communication combined with Wireshark's powerful analysis capabilities provides a comprehensive view into network behaviors, both in regular operations and in detecting and diagnosing network anomalies. Understanding the intricacies of TCP and how to analyze its data with Wireshark are crucial skills for network administrators and security professionals alike.

### DNS Overview

Domain Name System (DNS) is a foundational Internet service that translates human-readable domain names (like <www.example.com>) into machine-readable IP addresses, facilitating the routing of data across the Internet. DNS operates primarily using the User Datagram Protocol (UDP) to maximize efficiency and speed, though it can utilize TCP for larger query responses or for certain types of administrative transactions.

#### Key Components of DNS Operations

- **Query-Response Mechanism**: DNS transactions are based on a query-response model where a client sends a DNS query request, and the server responds with the corresponding IP address.
- **DNS Servers**: These are specialized servers that handle DNS queries. They can be local network servers or public servers provided by ISPs.
- **UDP as the Primary Protocol**: DNS commonly uses UDP on port 53 for queries due to its lower overhead compared to TCP.

### Analyzing DNS Traffic

When examining DNS packets in Wireshark, certain elements are critical for identifying whether the traffic is legitimate or potentially suspicious:

#### DNS Query Analysis

- **Protocol Use**: DNS queries typically use UDP on port 53. Observing a DNS query over TCP can be normal for cases where the DNS response data exceeds the UDP message size limit or in DNSSEC (DNS Security Extensions) operations, which can significantly increase the size of DNS responses. However, consistent DNS traffic over TCP without such justifications should be scrutinized.
- **Content of Queries**: The domains being requested in DNS queries can indicate normal activity or suggest malicious intent, such as queries for known malicious domains.

#### DNS Response Analysis

- **Matching Responses**: Responses should directly correspond to queries, providing the requested IP address. Responses not matching any prior query are suspicious and might indicate a spoofed DNS reply.
- **Answer Section**: This part of the DNS response contains the actual mapping of domain names to IP addresses. A mismatch in this section compared to expected results can suggest DNS poisoning or other DNS-related attacks.

### Practical Tips for DNS Packet Analysis

Analyzing DNS packets effectively requires an understanding of typical network behavior and the ability to distinguish between usual and unusual DNS traffic:

- **Consistency in Protocol Use**: Regularly monitor the use of UDP and TCP for DNS and understand the context in which TCP is used.
- **Domain Name Review**: Look at the frequency and pattern of domain lookups. High volumes of requests for the same domain, especially if not linked to user activity, could suggest command and control (C&C) communications for malware.
- **Geographic and Server Consistency**: DNS requests should typically be resolved by servers that are logically and geographically consistent with the network configuration. Requests outside these norms warrant further investigation.

### Suspicious DNS Traffic Indicators

- **Unusual Response Codes**: Such as SERVFAIL or REFUSED, which could indicate DNS server configuration issues or deliberate interference.
- **Frequent NXDOMAIN Responses**: Excessive queries that return non-existent domain responses might indicate malware trying to communicate with a C&C server via algorithmically generated domain names (DGA).
- **Unexpected Remote DNS Servers**: DNS queries being answered by unexpected, possibly malicious, remote servers can be a sign of DNS hijacking.

By understanding these elements, network administrators and security analysts can use tools like Wireshark to effectively monitor DNS traffic, ensuring network integrity and security against potential DNS-based threats.

### HTTP Overview

Hypertext Transfer Protocol (HTTP) is foundational for the web, enabling the transfer of data between web servers and clients. HTTP operates through clear-text requests and responses, making it less secure than HTTPS, which encrypts the exchange. Understanding HTTP is essential for web developers, security professionals, and network administrators, particularly for identifying common vulnerabilities like SQL injections and unauthorized web shells.

#### Key HTTP Concepts

- **GET and POST Requests**: The most common HTTP methods, where GET retrieves data from a server and POST sends data to a server.
- **Statelessness**: HTTP does not inherently maintain state between different requests, which has implications for session management and security.

### HTTP Traffic Analysis

Analyzing HTTP traffic involves understanding the structure of HTTP requests and responses and recognizing how data is exchanged in a web environment.

#### Practical HTTP Packet Analysis

Using tools like Wireshark, analysts can inspect individual HTTP packets for a detailed view of web communications:

- **Request URI**: Shows the specific address of the resource being requested.
- **File Data**: Indicates the data being transmitted in a POST request or the outcome of a GET request.
- **Server Headers**: Provide information about the server, such as software type and supported technologies, which can be critical for identifying server vulnerabilities.

#### Example of HTTP Packet Analysis

In a typical HTTP packet capture, you might examine specific packets to gather crucial data:

- **Host**: Identifies the web server a request is directed to.
- **User-Agent**: Reveals the client software making the request, useful for understanding client behavior and detecting potentially malicious activity.
- **Requested URI**: Shows the exact path requested, which can be crucial for detecting attempts to access unauthorized areas.

### Utilizing Wireshark for HTTP Analysis

Wireshark offers several features that enhance the analysis of HTTP traffic:

- **Protocol Hierarchy**: Allows analysts to view the distribution of protocols within a capture, helping to identify unexpected protocols that may indicate suspicious activity.
- **Export HTTP Object**: Facilitates the organization of HTTP data, such as images, scripts, and stylesheets, which can be exported for deeper analysis.
- **Endpoints Feature**: Provides a comprehensive list of all network endpoints communicated with during the capture, which is vital for mapping network activity and identifying potential external threats.

#### Steps for Analyzing HTTP with Wireshark

1. **Open a PCAP File**: Begin by loading a file containing HTTP traffic into Wireshark.
2. **Inspect Individual Packets**: Look at the details of HTTP requests and responses to gather intelligence about the nature of the traffic.
3. **Use Wireshark’s Features**: Navigate through `Statistics > Protocol Hierarchy` and `File > Export Objects > HTTP` for organized analysis.

### Application in Security

HTTP traffic analysis is crucial for security purposes:

- **Threat Hunting**: Analysts can spot anomalies in HTTP requests that may indicate a breach, such as unusual request patterns or unknown host connections.
- **Forensic Analysis**: In the event of a security incident, HTTP data can provide evidence about the sources of traffic and the nature of any data exfiltration.

While HTTPS has become more prevalent due to its encryption capabilities, HTTP is still widely used and requires careful analysis to ensure security. Understanding HTTP's basic functioning and how to analyze its traffic with tools like Wireshark is essential for maintaining the integrity and security of web communications.

### HTTPS Overview

Hypertext Transfer Protocol Secure (HTTPS) is the secure version of HTTP, using encryption to enhance security for online communications. The protocol involves complex interactions that establish a secure channel over an insecure network—making packet analysis more challenging but crucial for security assessments.

#### Key Components of HTTPS

- **Protocol Version Agreement**: The client and server agree on which version of the protocol to use (e.g., TLS 1.2, TLS 1.3).
- **Cryptographic Algorithms Selection**: They select algorithms that best suit their security requirements and capabilities.
- **Authentication**: Optionally, both parties can authenticate each other to confirm that they are communicating with the intended entity.
- **Secure Tunnel Creation**: Utilizing public key cryptography, they establish a secure communication tunnel.

### Analyzing HTTPS Traffic

The analysis of HTTPS traffic begins with examining the handshake process:

#### TLS/SSL Handshake Analysis

1. **Client Hello**: Initiates the handshake by suggesting cryptographic algorithms, supported SSL/TLS versions, and other necessary session parameters.
2. **Server Hello**: Responds to the Client Hello, agreeing on the protocol version and cryptographic algorithms, and provides the server’s SSL certificate.
3. **Client Key Exchange**: The client sends key information which will be used to establish encryption keys.
4. **Server Confirmation**: The server confirms the key exchange and finalizes the secure tunnel setup.

From this point forward, all data transmitted between the client and server is encrypted, making it invisible to those without the encryption key.

### Practical HTTPS Packet Analysis

Analyzing HTTPS packets requires access to the encryption keys to decrypt the secure traffic, which can be a major obstacle in network monitoring and threat analysis.

#### Decrypting HTTPS Traffic in Wireshark

To decrypt HTTPS in Wireshark, you need the server’s private key. Here’s how you can load it:

1. **Prepare the Key**: Ensure you have the server’s private RSA key file. It must be in a format that Wireshark can read (typically a `.pem` file).
2. **Load the Key into Wireshark**:
   - Navigate to `Edit > Preferences > Protocols > TLS` (or SSL for older versions).
   - Under 'RSA Keys List', add the IP address of the server, the port number (usually 443 for HTTPS), and the path to your RSA key file.

#### Steps to Analyze Encrypted HTTPS Traffic

1. **Open the PCAP File**: Start by opening your packet capture file that contains the HTTPS traffic.
2. **Review the Handshake**: Look at the handshake packets to understand the security parameters established between the client and server.
3. **Decrypt the Traffic**: With the RSA key loaded, Wireshark should be able to decrypt the HTTPS traffic, allowing you to see the underlying HTTP data.

#### Advanced Features for Organized Analysis

- **Protocol Hierarchy**: Use this to view how much of the captured traffic is encrypted HTTPS versus other protocols.
- **Export HTTP Objects**: This feature is useful for extracting resources such as images and scripts from HTTP traffic encapsulated within HTTPS.

### Use Case: Threat Hunting and Network Administration

By decrypting and analyzing HTTPS traffic, network administrators and security analysts can:

- **Identify Suspicious Patterns**: Look for anomalies in the request URIs or unusual user-agent strings that might indicate malicious activity.
- **Verify Compliance**: Ensure that sensitive information is appropriately encrypted and that all server communications meet security policies.

While HTTPS adds a layer of complexity to traffic analysis, understanding its underlying processes and being able to decrypt and examine the content can provide invaluable insights into network security and user behavior. Tools like Wireshark, equipped with the right keys and configuration, make this detailed inspection possible, aiding in everything from compliance checks to advanced threat detection.

## Appendix

### Wireshark --Deauthentication

Using Wireshark to analyze network traffic and verify the success of a deauthentication attack, as well as the subsequent capture of a WPA 4-way handshake, can be effectively done by following these steps:

### 1. **Open the .cap File**

- Start Wireshark and open your `.cap` file that you generated during your test.

### 2. **Filter for Deauthentication Packets**

- To see if the deauthentication attack was successful, use the filter:

     ```
     wlan.fc.type_subtype == 0x0c
     ```

     This filter shows deauthentication frames. You should see packets with the destination address set to the client’s MAC address you targeted (if you targeted a specific client), or a broadcast address for general deauth.

### 3. **Check for Reconnection Attempts**

- After filtering for deauthentication packets, remove the filter and look around the same timeframe for EAPOL packets which are used in the WPA 4-way handshake. Use the filter:

     ```
     eapol
     ```

     This will display all EAPOL frames, which are involved in the handshake process.

### 4. **Analyze the 4-Way Handshake**

- To confirm a complete 4-way handshake, ensure you capture four EAPOL packets between the client and the AP:
  - **Message 1**: from AP to client (contains the ANonce)
  - **Message 2**: from client to AP (contains the SNonce)
  - **Message 3**: from AP to client (confirms the temporal key)
  - **Message 4**: from client to AP (acknowledgment)
- The filter `eapol` should reveal these packets if they were captured. The Info column in Wireshark will detail which part of the handshake each packet belongs to.

### 5. **Verify the Timing**

- Examine the timestamps on the deauthentication packets and the EAPOL messages. There should be a very quick succession of these events if the deauthentication caused the client to reconnect. This timing is crucial to determine if the deauthentication led directly to the handshake capture.

### 6. **Additional Checks**

- **Signal Strength**: Check the radio information, if available, to assess signal strength which might affect packet capture efficacy.
- **Packet Loss**: Consider if there might have been packet loss, which could result in missing parts of the handshake.

Using these steps in Wireshark will allow you to visualize the flow of packets and ascertain both the success of your deauthentication attack and whether it resulted in capturing a new 4-way handshake.

## Acknowledgments

The structure of this guide was inspired by the module created by TryHackMe, available at [https://tryhackme.com/r/room/wireshark](https://tryhackme.com/r/room/wireshark).

## Disclaimer

**Legal Considerations:** Always ensure you have permission to perform packet analysis on the network you are targeting to avoid violating legal or ethical boundaries.
