# Wireshark Notes and Explanation Part 1

Video : [Wireshark Video](https://www.youtube.com/watch?v=n4muxtqLhN4)

All the explanation in this file is based on the above video posted by Alpha Brains Courses Youtube channel.

---

#  Introduction and Overview

## What is Wireshark?

Wireshark is a network packet analyzer used to capture and inspect network traffic in real time.

It is widely used in:

* Network troubleshooting
* Cybersecurity
* Ethical hacking
* Digital forensics
* Malware analysis
* CTF challenges

Wireshark allows you to see:

* Who is communicating
* Which protocols are being used
* What data is being transferred
* Errors and suspicious traffic

---

#  Getting Traffic

## Switches vs Hubs

### Hub

A hub sends incoming traffic to **all devices** connected to it.

Example:

If PC-A sends data to PC-B:

* Every device connected to the hub receives the packet.

This makes packet sniffing easy.

### Switch

A switch sends packets only to the intended device.

Example:

If PC-A sends data to PC-B:

* Only PC-B receives the packet.

This makes sniffing harder.

---

## Spoofing to Obtain Traffic

In switched networks, attackers may use techniques such as:

### ARP Spoofing

The attacker tricks devices into believing:

* "I am the router"

Traffic is then redirected through the attacker.

### MAC Flooding

The attacker overloads the switch’s MAC table so it behaves like a hub.

Purpose:

* Capture traffic from other machines
* Perform Man-in-the-Middle (MITM) attacks

---

#  Capturing and Viewing Traffic

## Starting a Packet Capture

Steps:

1. Open Wireshark
2. Select the correct network interface

   * Ethernet
   * Wi-Fi
   * VPN
3. Click Start

---

## Capture Options

### Promiscuous Mode

Allows the network card to capture all packets it sees.

Without it:

* Only packets meant for your machine are captured.

---

## Capture Filters

Capture filters reduce traffic before capture starts.

Example:

```bash
port 80
```

Captures only HTTP traffic.

---

## Ring Buffer

Automatically rotates capture files after:

* Certain size
* Certain time

Useful for long captures.

---

## Capturing Wireless Traffic

Wi-Fi capture requires:

### Monitor Mode

Allows capturing all wireless frames nearby.

Can capture:

* Beacon frames
* Probe requests
* Authentication packets

---

## Display Filters vs Capture Filters

### Capture Filter

Applied BEFORE packets are captured.

Example:

```bash
host 192.168.1.1
```

---

### Display Filter

Applied AFTER packets are captured.

Example:

```bash
ip.addr == 192.168.1.1
```

---

## Common Display Filters

### IP Address

```bash
ip.addr == 192.168.1.5
```

### TCP Port

```bash
tcp.port == 443
```

### HTTP Traffic

```bash
http
```

### DNS Traffic

```bash
dns
```

---

## Sorting and Searching

You can sort by:

* Time
* Source
* Destination
* Protocol
* Packet length

### Find Packet

Shortcut:

```text
Ctrl + F
```

Search methods:

* String
* Hex
* Regular expressions

---

## Packet View Panes

Wireshark contains 3 main panes.

### Packet List Pane

Shows packet summary.

### Packet Details Pane

Shows protocol layers.

Example:

```text
Ethernet > IP > TCP > HTTP
```

### Packet Bytes Pane

Raw hexadecimal data.

---

## Customizing the View

You can:

* Add/remove columns
* Change time format
* Add coloring rules

Example:

* TCP retransmissions in red
* HTTP traffic in green

---

#  Streams

## TCP Stream Reassembly

Wireshark can reconstruct full conversations.

Example:

* Login credentials
* Chat messages
* HTTP requests

### Follow TCP Stream

Right-click packet:

```text
Follow > TCP Stream
```

---

## Other Streams

* UDP Stream
* HTTP Stream
* TLS Stream

Useful for:

* Credential extraction
* Malware analysis
* Session reconstruction

---

#  Using Dissectors

## What is a Dissector?

A dissector interprets packet data into readable protocol information.

Example:

* Converts raw bytes into HTTP fields.

---

## Decode As

Used when services run on unusual ports.

Example:

* HTTP traffic running on port 8088

Right-click:

```text
Decode As
```

Then manually assign protocol.

---

#  Name Resolution

## IP Resolution

Converts:

```text
142.250.183.14
```

into:

```text
google.com
```

---

## MAC Address Resolution

Converts MAC addresses into vendor names.

Example:

```text
00:1A:2B
```

may resolve to:

```text
Intel Corporation
```

---

## Port Resolution

Converts:

```text
443
```

into:

```text
HTTPS
```

---

## Warning

DNS lookups create extra traffic.
This can affect captures.

---

#  Saving Captures

## File Formats

### `.pcap`

Older standard format.

### `.pcapng`

Modern format with:

* Metadata
* Multiple interfaces
* Comments

---

#  Capturing From Other Sources

## Remote Capture

Using tools like:

```bash
tcpdump
```

Pipe traffic into Wireshark.

---

## Virtual Interfaces

Capture from:

* VPNs
* Docker
* Virtual machines
* Loopback interfaces

---

## USB Capture

Wireshark can also capture USB traffic.

Useful in:

* Malware analysis
* Device forensics

---

#  Opening Saved Captures

Wireshark supports:

* `.pcap`
* `.pcapng`
---

#  Ring Buffers

## Purpose

Prevent disk overflow during long captures.

Wireshark automatically:

* Creates new files
* Deletes oldest captures

Useful for:

* Servers
* SOC monitoring
* Continuous analysis

---

# Expert Analysis

## Expert Information Panel

Wireshark automatically detects:

* Errors
* Warnings
* Suspicious behavior

---

## Common Issues

### TCP Retransmissions

Packet resent due to failure.

### Duplicate ACKs

Receiver missed packets.

### RST Packets

Connection reset.

### Zero Window

Receiver buffer full.

---

# Applying Dynamic Filters

Right-click any field:

```text
Apply as Filter
```

or

```text
Prepare as Filter
```

This helps create filters quickly.

---

# Filtering Conversations

Filter traffic between two hosts.

Example:

```bash
ip.addr == 192.168.1.5 && ip.addr == 192.168.1.10
```

Useful for:

* Investigations
* Malware traffic isolation

---

#  Investigating Latency

## RTT (Round Trip Time)

Measures delay between:

* Request
* Response

---

## SYN Timing

TCP handshake timing:

```text
SYN -> SYN-ACK -> ACK
```

Used to identify:

* Slow networks
* Delayed servers
---

# Detailed Display Filters

## Comparison Operators

### Equal

```bash
==
```

### Not Equal

```bash
!=
```

### Greater Than

```bash
>
```
---

## Logical Operators

### AND

```bash
&&
```

### OR

```bash
||
```

### NOT

```bash
!
```

---

# Locating Response Codes

## HTTP Response Codes

### Success

```text
200 OK
```

### Redirect

```text
301 Moved Permanently
```

### Forbidden

```text
403 Forbidden
```

### Not Found

```text
404 Not Found
```

### Server Error

```text
500 Internal Server Error
```
---

## Filter Example

```bash
http.response.code == 404
```

---

# Using Expressions in Filters

Wireshark provides:

* Expression Builder GUI
* Protocol field browser

Helps build filters without memorizing syntax.

---

# Locating Suspicious Traffic

## Indicators of Suspicious Activity

### Port Scanning

Many SYN packets to multiple ports.

### Beaconing

Connections at regular intervals.

### DNS Tunneling

Large or unusual DNS traffic.

### Data Exfiltration

Large outbound transfers.

---

# Obtaining Files

Wireshark can reconstruct transferred files.

Examples:

* Images
* Documents
* Malware payloads
---

# Exporting Captured Objects

Menu:

```text
File > Export Objects
```

Supported protocols:

* HTTP
* SMB
* TFTP
* IMF
* DICOM
---
