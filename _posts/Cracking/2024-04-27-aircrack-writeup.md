---
layout: post
title: "Aircrack-ng"
date: 2024-04-27
categories: Cracking
---

## Introduction to WiFi Cracking with Aircrack-ng

Aircrack-ng is a comprehensive suite of tools designed for auditing wireless networks, known for its efficiency in cracking WEP and WPA/WPA2-PSK passwords. This guide assumes no prior knowledge and begins with the fundamentals of wireless network security before progressing to practical cracking techniques.

### Understanding Wireless Network Security

Wireless networks use various protocols to secure communications, the most common being WEP, WPA, and WPA2. Each protocol has its vulnerabilities and protective mechanisms:

- **WEP (Wired Equivalent Privacy)**: Once common, now obsolete due to significant vulnerabilities.
- **WPA (WiFi Protected Access)**: Introduced as a replacement for WEP, improving security but still susceptible to attacks.
- **WPA2**: Enhances WPA security, currently the standard for securing WiFi networks.

### What Makes Wireless Security Complex?

Wireless security involves several components that interact to protect data:

- **SSID (Service Set Identifier)**: The network name visible to users.
- **BSSID (Basic Service Set Identifier)**: The MAC address of the access point.
- **Encryption**: Protects data transmitted between devices and access points.

The complexity arises from the need to balance accessibility and security, ensuring users can connect easily while protecting the network from unauthorized access.

### Role of Aircrack-ng

Aircrack-ng facilitates the exploitation of weaknesses in wireless security protocols. It includes tools like `airmon-ng` for monitoring networks, `airodump-ng` for capturing packets, and `aircrack-ng` for cracking passwords. This process often targets the WPA/WPA2's 4-way handshake, a mechanism that confirms that both the client and access point possess the correct credentials without revealing the password.

## Cracking WPA/WPA2 with Aircrack-ng

Aircrack-ng is versatile for both beginners and advanced users. This section delves into the basics of Aircrack-ng's functionality and provides guidelines on how to effectively use this tool suite for cracking WiFi passwords.

#### Aircrack-ng Basic Syntax

- **airodump-ng**: Monitors WiFi networks and captures traffic.
- **aircrack-ng**: Analyzes captured traffic and cracks passwords.
- **airmon-ng**: Manages monitoring mode on wireless NICs.

#### Monitor Mode

- **Checking for Interfering Processes**:
  - Before enabling monitor mode, identify and kill any processes that could interfere with the operation of your wireless card. Use the command:
    - `airmon-ng check kill`
      - 'NetworkManager restart' after check kill to restart NAT/Wifi

- **Enabling Monitor Mode**:
  - `airmon-ng start wlan0`

- **Disabling Monitor Mode**:
  - `airmon-ng stop wlan0`

### Packet Sniffing with Airodump-ng

Packet sniffing is a method used to monitor and capture data packets traveling over wireless networks. `airodump-ng` is particularly adept at this task, providing the ability to capture packets on specified channels or frequency bands.

#### Sniffing Frequency Bands

- **2.4 GHz Band**:
  - This band is widely used and often congested, but many devices are compatible with it.
  - **Command**: `airodump-ng [wireless adapter name]`
- **5 GHz Band**:
  - Typically less congested and offers faster data transmission, though not all devices support this band.
  - **Command**: `airodump-ng --band a [wireless adapter name]`
- **Dual-Band Monitoring**:
  - Monitoring both 2.4 GHz and 5 GHz bands can provide comprehensive coverage but may slow down the capture process due to the increased data volume.
  - **Command**: `airodump-ng --band abg [wireless adapter name]`

#### Using Airodump-ng for Targeted Sniffing

To focus on a specific network and capture data related to it, you can specify the BSSID and the channel. This targeted approach is useful for reducing data noise and improving the quality of the capture.

- **Targeting a Specific Network**:
  - **Command**: `airodump-ng --bssid [bssid] --channel [channel] [wireless adapter name]`
  - Captures packets from a particular network identified by its BSSID on the specified channel.

- **Sorting Captured Data**:
  - During a capture session, you can press the **s** key to bring up the sort interface, allowing you to organize the displayed networks by power, beacon count, or any other available metric.
  - Press **Enter** after selecting your sorting preference to update the display order. This feature helps in pinpointing networks with stronger signals or more frequent activity.

- **Optimize Location**: Physical proximity to the target can greatly improve the quality of the capture. Position yourself within a reasonable range of the access point to capture more packets.

#### Capturing a 4-Way Handshake

To crack a WPA/WPA2 password, capturing the 4-way handshake is crucial.

- **Capturing Packets**:
  - `airodump-ng --bssid [target BSSID] -c [channel] --write output wlan0mon`

#### Cracking the Password

Once the handshake is captured, use `aircrack-ng` to crack the password:

- **Basic Command**:
  - `aircrack-ng -w [path to wordlist] output-01.cap`
    - Find the file:
      - `/usr/share/wordlists`
    - Decompress the file:
      - `gunzip rockyou.txt.gz`

This command uses a wordlist to perform a dictionary attack against the handshake file captured by `airodump-ng`.

### Advanced Techniques

For more sophisticated approaches, consider:

- **Deauthentication Attack**:
  - Forces devices to reconnect, capturing new handshakes.
  - `aireplay-ng --deauth 5 -a [routers MAC/BSSID] -c [client's MAC/STATION] wlan0`
- **Using GPU Acceleration**:
  - Convert the capture to a hashcat format for faster cracking with GPUs.
  - `aircrack-ng -J output hashcat_output.hccapx`

### Utilizing Aircrack-ng's Capabilities

To maximize the effectiveness of Aircrack-ng in cracking WPA/WPA2 passwords, it's important to tailor the tool's options and strategies to the specifics of the network and the environment. Here are some guidelines:

- **Customizing Options**:
  - Adjust performance settings and use optimal commands for your specific scenario.
  
- **Network Specifics**:
  - Always consider the target network's security settings, such as the type of encryption and the strength of the deployed passwords.

- **Monitoring and Managing Attacks**:
  - Keep track of your cracking attempts and adjust strategies as needed based on feedback from the tools.

Cracking WPA/WPA2 passwords with Aircrack-ng is a powerful method in a security professional's toolkit, particularly useful in penetration testing and ethical hacking. It highlights the importance of strong wireless security policies and the need for continuous network monitoring.

**Workflow Summary**:

1. **Prepare the Environment**:
   - Use `airmon-ng check kill` to stop services that might interfere with your wireless card’s monitor mode. This step ensures that no other processes will disrupt the monitoring and packet capturing process.

2. **Enable Monitor Mode**:
   - Activate monitor mode on your wireless interface with the command `airmon-ng start wlan0`. This enables your network interface card (NIC) to listen to all wireless traffic on supported frequencies.

3. **Start General Packet Capture**:
   - Begin capturing packets with `airodump-ng` to record ongoing network traffic. This is crucial for identifying active networks and their details. Use commands based on the frequency band of interest:
     - For 2.4 GHz only: `airodump-ng wlan0`
     - For 5 GHz only: `airodump-ng --band a wlan0`
     - For both bands: `airodump-ng --band abg wlan0`

4. **Refine to Targeted Sniffing**:
   - Once you identify the target network and its operating channel from the general capture data, switch to targeted sniffing to focus on capturing data from this specific network:
     - Command: `airodump-ng --bssid [target BSSID] --channel [channel] --write [filename] wlan0`
     - This command focuses the capture on the target network by its BSSID on the specified channel and saves the data to a file, making it easier to manage the data and use it for further analysis or password cracking.

5. **Initiate Deauthentication Attack**:
   - Use `aireplay-ng` to send deauthentication packets to the target, prompting devices connected to the network to reconnect. This forces the network to generate new 4-way handshakes, which are crucial for cracking WPA/WPA2 passwords.
     - Command: `aireplay-ng --deauth 5 -a [routers MAC/BSSID] -c [client's MAC/STATION] wlan0`
     - This attack can help you capture the 4-way handshake if it was not captured during the initial targeted sniffing.

### Disclaimer

**Legal Considerations:** Always ensure you have permission to monitor and capture data on the network you are targeting to avoid legal issues.
