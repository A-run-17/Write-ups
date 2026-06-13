# Wireshark Notes and Explanation Part 2

Video : [Wireshark Video](https://www.youtube.com/watch?v=n4muxtqLhN4)

All the explanation in this file is based on the above video posted by Alpha Brains Courses Youtube channel. Part 1 explanation is in the same directory of this repository

---

# Statistics -  Endpoints

## What are Endpoint Statistics?

Endpoint statistics show all devices (endpoints) communicating in the packet capture.

Open from:

```
Statistics > Endpoints
```
---

## Available Tabs

### Ethernet

Shows:

* MAC addresses
* Physical devices on the network

Example:

* 31 MAC addresses = 31 devices

---

### IPv4 / IPv6

Shows:

* IP addresses involved in communication

Example:

* 77 IPv4 addresses
* 51 IPv6 addresses

---

### TCP / UDP

Shows:

* IP address + Port combinations

Example:

```
192.168.1.5:443
```

Different ports create separate entries.

---

## Important Difference

### IPv4/IPv6 Tabs

Show:

* Only IP addresses

### TCP/UDP Tabs

Show:

* IP + Port combinations

Use IPv4/IPv6 when:

* You only care about devices

Use TCP/UDP when:

* You care about services and ports

---

## Name Resolution

Can resolve:

* IP   → hostname
* MAC  → manufacturer
* Port → service name

Example:

```
443 -> HTTPS
```

---

# Conversations

## What are Conversations?

Endpoints show:

* Individual devices

Conversations show:

* Who is talking to whom

Open from:

```
Statistics > Conversations
```

---

## Information Shown

* Source address
* Destination address
* Packets sent
* Bytes transferred
* Duration
* Bit rate

---

## TCP Conversations

Same IP may appear multiple times because:

* Different source ports create separate sessions

Example:

```
172.30.42.8:63815
172.30.42.8:63811
```

---

## IPv4 Conversations

Combines:

* All port activity between two IPs

Useful for:

* General communication analysis

---

## Ethernet Conversations

Shows:

* MAC address pairs

---

# Graphing

## IO Graph

Open from:

```text id="3evp4m"
Statistics > IO Graph
```

Displays:

* Network traffic over time

---

## Graph Axes

### X-axis

Time

### Y-axis

Packets per second

---

## Features

### Hover

Shows packet number

### Click

Jumps to packet in capture

---

## Customization

### Interval

Controls graph granularity.

Example:

* 1 second
* 10 seconds

Larger intervals:

* Less detailed graphs

---

## Overlays

Add multiple filters as graph lines.

Examples:

* All traffic
* TCP errors
* HTTP traffic

---

## Graph Types

* Line graph
* Bar graph
* Stacked graph

---

## Flow Graph

Open from:

```
Statistics > Flow Graph
```

Shows:

* Ladder-style communication diagram

Useful for:

* Understanding message order

---

# Identifying Active Conversations

## Goal

Find:

* Most active systems
* Highest traffic generators

---

## Steps

1. Enable name resolution

```
View > Resolve Network Addresses
```

2. Open Conversations window
3. Sort by:

   * Packets
   * Bytes

---

## Packets vs Bytes

### High Packets + Low Bytes

Many small packets

### Low Packets + High Bytes

Large file transfers

---

## Why Sort by Bytes?

Bytes better represent:

* Actual bandwidth usage

---

# Using GeoIP

## What is GeoIP?

Maps IP addresses to:

* Country
* City
* Latitude
* Longitude

---

## GeoIP Databases

Uses:
MaxMind GeoLite databases.

Required databases:

* Country
* City
* ASN

---

## Setup Steps

1. Download databases
2. Extract `.gz` files
3. Add directory in Wireshark

Path:

```
Preferences > Name Resolution > GeoIP Database Directories
```

---

## Enable GeoIP

```
Preferences > Protocols > IPv4 > Enable GeoIP lookups
```

---

## GeoIP Filters

### United States Traffic

```
ip.geoip.country == "United States"
```

### Non-US Traffic

```bash id="aj56o7"
!ip.geoip.country == "United States"
```

---

# Mapping Packet Locations Using GeoIP

## Map Feature

Open:

```text id="6crs9j"
Statistics > Endpoints > Map
```

Generates:

* World map of IP locations

---

## Information Displayed

Each point shows:

* Hostname
* City
* Packet count
* Byte count

---

## Use Cases

* Threat intelligence
* Detecting suspicious foreign traffic
* Data center analysis

---

# Using Protocol Hierarchies

## Protocol Hierarchy

Open:

```
Statistics > Protocol Hierarchy
```

Shows:

* Breakdown of all protocols in capture

---

## Information Displayed

* Packet percentage
* Byte percentage
* Packet count
* Bit rate

---

## SSL/TLS Limitation

Wireshark can identify:

* TLS traffic

But cannot decrypt encrypted content without:

* Encryption keys

---

# Locating Suspicious Traffic Using Protocol Hierarchies

## Goal

Detect:

* Unusual protocols
* Suspicious applications

---

## Examples

### IRC Traffic

Unexpected on corporate networks.

May indicate:

* Unauthorized chat usage
* Malware communication

---

### Unknown Data Entries

Wireshark couldn't identify protocol.

Possible causes:

* Proprietary protocol
* Malware traffic
* Corrupted packets

---

### Tor Traffic

TLS traffic from Tor nodes may indicate:

* Anonymous browsing
* Hidden activity

---

## Workflow

1. Open Protocol Hierarchy
2. Identify suspicious protocol
3. Right-click:

```
Apply as Filter
```

4. Investigate packets

---

#  Graphing Analysis Flags

## Purpose

Visualize errors over time.

---

## TCP Error Overlay

Enable filter:

```
tcp.analysis.flags
```

Shows:

* Retransmissions
* Duplicate ACKs
* Out-of-order packets

---

## Interpretation

### Errors + High Traffic

Likely congestion

### Errors + Low Traffic

Specific issue worth investigating

---

# Voice Over IP (VoIP) / Telephony

## VoIP Analysis

Wireshark supports:

* SIP
* H.323
* RTP

---

# SIP (Session Initiation Protocol)

## Purpose

Used to:

* Start calls
* Manage calls
* End calls

---

## Characteristics

* Text-based
* Human-readable

Filter:

```
sip
```

---

## Example

```
INVITE sip:user@example.com
```

---

# H.323

## Characteristics

* Binary protocol
* Not human-readable directly

Wireshark decodes it automatically.

---

## Components

### H.225

Call setup and teardown

### H.225 RAS

Registration and status

---

## Filter

```
h225
```

---

# RTP (Real-Time Protocol)

## Purpose

Carries:

* Audio
* Video

Filter:

```
rtp
```

---

# Locating VoIP Conversations

## VoIP Calls Window

Open:

```
Telephony > VoIP Calls
```

Displays:

* Call start time
* End time
* Protocol
* Packet count

---

## RTP Streams

Open:

```
Telephony > RTP Streams
```

---

# Using VoIP Statistics

## SIP Statistics

Open:

```
Telephony > SIP Statistics
```

Shows:

* INVITE
* ACK
* 200 OK
* Ringing messages

---

## H.225 Statistics

Provides:

* Jitter
* Latency
* Packet loss

---

## Audio Quality Indicators

### High Jitter

Choppy audio

### High Latency

Delay in conversation

### Packet Loss

Missing audio pieces

---

# Ladder Diagrams / Flow Sequences

## Purpose

Visual representation of:

* Message flow between systems

---

## Structure

### X-axis

Devices

### Y-axis

Time

---

## SIP Flow Example

Sequence:

1. INVITE
2. 100 Trying
3. 180 Ringing
4. 200 OK
5. RTP begins
6. ACK

---

# Getting Audio from VoIP Captures

## Audio Playback

If RTP is unencrypted:

* Wireshark can play audio

---

## Steps

1. Open VoIP Calls
2. Select call
3. Click:

```
Play Streams
```

---

## Limitation

Encrypted SRTP cannot be played without:

* Encryption keys

---

# Advanced  -  Command Line with tshark

## What is tshark?

tshark is the command-line version of Wireshark.

Useful for:

* SSH sessions
* Servers
* Automation

---

## Basic Usage

### Live Capture

```bash
tshark
```

---

## Save Capture

```bash
tshark -w saved.pcap -s 0 -i en1
```

---

# Splitting Capture Files with editcap

## What is editcap?

editcap modifies and splits pcap files.

---

## Split by Packet Count

```bash
editcap -c 5000 saved.pcap split.pcap
```

Creates:

* Multiple smaller pcaps

---

## Split by Time

```bash
editcap -i 60 saved.pcap split.pcap
```

Creates:

* One file per 60 seconds

---

# Merging Capture Files with mergecap

## What is mergecap?

mergecap combines multiple pcap files.

---

## Merge Example

```bash
mergecap -w merged.pcap file1.pcap file2.pcap
```

---

## Merge vs Concatenate

### Merge

Sorts packets by timestamps.

### Concatenate

Appends files directly.

---

# Capture Stop Conditions with tshark

## Stop After Packet Count

```bash
tshark -c 50000
```

---

## Stop After Time

```bash
tshark -a duration:15
```

---

## Stop After File Size

```bash
tshark -a filesize:10000
```

---

# Command Line Capture Filters with tshark

## Host Filter

```bash
tshark -f "host 192.168.1.5"
```

---

## Port Filter

```bash
tshark -f "port 80"
```

---

## Combined Filter

```bash
tshark -f "host 192.168.1.5 and port 80"
```

---

# Extracting Specific Data Fields with tshark

## Extract Fields

```bash
tshark -T fields -e ip.src -e ip.dst
```

Extracts:

* Source IP
* Destination IP

---

## Save Output

```bash
tshark -T fields -e ip.src > output.txt
```

---

## Process with awk

```bash 
awk '{print $2}' ip.txt | sort | uniq
```
---

## Protocol Hierarchy Statistics

```bash 
tshark -q -z io,phs
```

---

## Active Hosts

```bash 
tshark -q -z hosts
```

---

## Other Statistics

| Command     | Purpose            |
| ----------- | ------------------ |
| `io,phs`    | Protocol hierarchy |
| `hosts`     | Active hosts       |
| `http,stat` | HTTP statistics    |
| `sip,stat`  | SIP statistics     |
| `smb,stat`  | SMB statistics     |

---
