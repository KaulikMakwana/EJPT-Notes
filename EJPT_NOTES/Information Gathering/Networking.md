# Networking Fundamentals

## 1. Introduction

- **Active Information Gathering**: Interacting directly with target systems/networks to gather data (e.g., scanning, probing)[cite: 17, 18]. Contrasts with passive gathering (OSINT)[cite: 18, 20].
- **Penetration Testing Phases**: Information Gathering -> Enumeration -> Exploitation -> Post-Exploitation -> Reporting[cite: 15]. Active info gathering includes Network Mapping, Host Discovery, Port Scanning, Service & OS Detection[cite: 15, 20].

## 2. Core Concepts

### 2.1. Network Protocols & Packets

- **Protocols**: Rules enabling communication between diverse computer systems[cite: 22, 23]. Facilitated via packets[cite: 25].
- **Packets**: Streams of bits (electrical signals) used for data transmission[cite: 27, 28]. Structure: Header + Payload[cite: 29].
    - **Header**: Protocol-specific structure for interpretation[cite: 30].
    - **Payload**: Actual data being sent[cite: 31].

### 2.2. OSI Model

- **OSI (Open Systems Interconnection)**: 7-layer conceptual framework standardizing network functions[cite: 32, 33]. Aids in understanding and designing network architectures[cite: 46].
    - **Layer 7 (Application)**: User-facing services (HTTP, FTP, DNS, SSH)[cite: 35, 36].
    - **Layer 6 (Presentation)**: Data translation, encryption, compression (SSL/TLS, JPEG)[cite: 36, 37, 38].
    - **Layer 5 (Session)**: Manages connections/sessions (APIs, NetBIOS)[cite: 38, 39].
    - **Layer 4 (Transport)**: End-to-end communication, flow control (TCP, UDP)[cite: 39, 40].
    - **Layer 3 (Network)**: Logical addressing, routing (IP, ICMP, IPSec)[cite: 40].
    - **Layer 2 (Data Link)**: Physical addressing, error detection (Ethernet, PPP, Switches)[cite: 40, 41, 42].
    - **Layer 1 (Physical)**: Physical connection (Cables, Hubs, USB)[cite: 42, 43].

📌 **Remember**: OSI is a *conceptual* model, not a strict implementation blueprint[cite: 46].

## 3. Network Layer (Layer 3)

- **Function**: Logical addressing, routing, forwarding packets across networks[cite: 48]. Determines the optimal path[cite: 49].
- **Key Protocols**:
    - **IP (Internet Protocol)**: Foundation for internet communication[cite: 52].
        - **IPv4**: 32-bit addresses (e.g., `192.168.0.1`)[cite: 52, 60]. Facing address exhaustion[cite: 61].
        - **IPv6**: 128-bit addresses (hexadecimal, e.g., `2001:0db8::8a2e:0370:7334`)[cite: 53, 63]. Larger address space[cite: 62].
    - **ICMP (Internet Control Message Protocol)**: Error reporting and diagnostics (e.g., ping, traceroute)[cite: 54, 55, 73].

### 3.1. Internet Protocol (IP)

- **Functionality**:
    - **Logical Addressing**: Assigns unique IP addresses to network interfaces[cite: 64, 65].
    - **Packet Structure**: Organizes data into packets (Header + Payload)[cite: 66]. Header includes Source/Destination IP, TTL, Protocol type[cite: 67].
    - **Fragmentation/Reassembly**: Breaks large packets for networks with smaller MTU, reassembles at destination[cite: 68, 69].
    - **Address Types**: Unicast (1-to-1), Broadcast (1-to-all in subnet), Multicast (1-to-many)[cite: 70].
    - **Subnetting**: Divides large networks into smaller subnets for efficiency/security[cite: 71, 72].
    - **DHCP**: Often used with IP to dynamically assign addresses[cite: 75].

### 3.2. IPv4 Header

- **Key Fields**:
    - **Version (4 bits)**: IP version (Value 4 for IPv4)[cite: 81, 82].
    - **Header Length (4 bits)**: Header size in 32-bit words (Min 5 = 20 bytes, Max 15 = 60 bytes)[cite: 82, 83].
    - **Type of Service (ToS) (8 bits)**: Packet priority (DSCP, ECN)[cite: 79, 84].
    - **Total Length (16 bits)**: Total packet size (Header + Payload), max 65,535 bytes[cite: 85, 86].
    - **Identification (16 bits)**: Reassembles fragmented packets[cite: 86, 87].
    - **Flags (3 bits)**: Fragmentation control (DF - Don't Fragment, MF - More Fragments)[cite: 88, 89].
    - **Time-to-Live (TTL) (8 bits)**: Max hop count, decremented by each router[cite: 78, 90, 91]. Prevents infinite loops.
    - **Protocol (8 bits)**: Identifies next-layer protocol (TCP=6, UDP=17, ICMP=1)[cite: 80, 91, 92].
    - **Source IP Address (32 bits)**: Sender's IP[cite: 78, 93, 98].
    - **Destination IP Address (32 bits)**: Recipient's IP[cite: 78, 94, 99].

### 3.3. IPv4 Addresses

- **Format**: 4 octets (bytes), dot-delimited (e.g., `73.5.12.132`)[cite: 101, 102].
- **Reserved Ranges**:
    - `0.0.0.0/8`: "This" network[cite: 103].
    - `127.0.0.0/8`: Loopback/localhost[cite: 104].
    - `192.168.0.0/16`: Private Network[cite: 104]. (Also `10.0.0.0/8`, `172.16.0.0/12`).
    - See RFC5735 for details[cite: 105].

### 3.4. ICMP

- Used by tools like `ping` and `traceroute`.
- **Ping (Echo Request/Reply)**:
    - **Echo Request**: Type 8, Code 0[cite: 228, 231, 232]. Sent to check reachability.
    - **Echo Reply**: Type 0, Code 0[cite: 229, 232, 235]. Sent by a reachable host.
- Absence of reply can mean host offline, network issues, or ICMP blocked by firewall[cite: 236, 237, 238].

💡 **Tip**: TTL values can sometimes hint at the OS type (different OS start with different default TTLs).

## 4. Transport Layer (Layer 4)

- **Function**: End-to-end communication, reliability, flow control, data segmentation[cite: 108, 109, 110].
- **Key Protocols**:
    - **TCP (Transmission Control Protocol)**: Connection-oriented, reliable, ordered delivery[cite: 111, 114].
    - **UDP (User Datagram Protocol)**: Connectionless, faster, unreliable, unordered delivery[cite: 112, 154, 155].

### 4.1. TCP (Transmission Control Protocol)

- **Characteristics**:
    - **Connection-Oriented**: Establishes a connection (virtual circuit) before data transfer using the 3-way handshake[cite: 116, 117].
    - **Reliable**: Uses ACKs and retransmissions for guaranteed delivery[cite: 118, 119].
    - **Ordered**: Reorders out-of-sequence segments[cite: 120, 121].
- **TCP 3-Way Handshake**:
    1.  **Client -> Server**: SYN (Synchronize sequence numbers)[cite: 124, 125, 126].
    2.  **Server -> Client**: SYN-ACK (Acknowledge client SYN, send server SYN)[cite: 124, 127, 128, 129].
    3.  **Client -> Server**: ACK (Acknowledge server SYN)[cite: 124, 130, 131].
    - Connection established after step 3[cite: 132].
- **TCP Header**: Includes Source/Destination Ports (16 bits each)[cite: 136].
- **Control Flags (6 bits)**: Manage connection state.
    - `SYN`: Initiate connection[cite: 139].
    - `ACK`: Acknowledge received data[cite: 141].
    - `FIN`: Terminate connection[cite: 143].
    - `RST`: Reset connection (abort).
    - `PSH`: Push data immediately.
    - `URG`: Urgent data.

### 4.2. UDP (User Datagram Protocol)

- **Characteristics**:
    - **Connectionless**: No handshake needed; sends datagrams independently[cite: 157, 158].
    - **Unreliable**: No guarantee of delivery, order, or error correction[cite: 159, 160]. No retransmissions[cite: 160].
    - **Fast & Lightweight**: Lower overhead due to simplicity[cite: 161].
    - **Stateless**: Each datagram is independent[cite: 163, 164].
- **Use Cases**: Real-time applications (VoIP, streaming, gaming), DNS, DHCP[cite: 162, 165].

### 4.3. TCP vs UDP

| Feature      | TCP                                             | UDP                                      |
| :----------- | :---------------------------------------------- | :--------------------------------------- |
| Connection   | Connection-Oriented (Handshake) [cite: 165]     | Connectionless [cite: 165]               |
| Reliability  | Reliable (ACKs, Retransmission) [cite: 165]     | Unreliable (Best Effort) [cite: 165]     |
| Ordering     | Ordered Delivery [cite: 165]                    | Unordered Delivery                       |
| Speed        | Slower                                          | Faster                                   |
| Header Size  | Larger [cite: 165]                              | Smaller [cite: 165]                      |
| Use Cases    | HTTP, HTTPS, FTP, SMTP, SSH [cite: 165, 166]    | DNS, DHCP, SNMP, VoIP, Gaming [cite: 165] |

### 4.4. Ports

- **Function**: Distinguish between different services/applications on a device[cite: 144].
- **Range**: 0 - 65535 (16-bit unsigned integers)[cite: 145, 146].
- **Categories**:
    - **Well-Known Ports (0-1023)**: Reserved for standard services (IANA assigned)[cite: 147, 148].
        - `21`: FTP [cite: 149]
        - `22`: SSH [cite: 149]
        - `25`: SMTP [cite: 149]
        - `80`: HTTP [cite: 149]
        - `110`: POP3 [cite: 149]
        - `443`: HTTPS [cite: 149]
    - **Registered Ports (1024-49151)**: Assigned by IANA for specific applications[cite: 150, 151].
        - `3306`: MySQL [cite: 153]
        - `3389`: RDP [cite: 153]
        - `8080`: HTTP Alternate [cite: 153]
    - **Dynamic/Private/Ephemeral Ports (49152-65535)**: Used for temporary client-side connections.

❗ **Important**: Knowing common ports helps identify running services during scans.

## 5. Network Mapping & Scanning

- **Goal**: Discover live hosts, open ports, running services, OS types, and network topology[cite: 174, 175, 182, 184, 188]. Crucial for understanding the attack surface[cite: 185].
- **Why Map?**: To identify active hosts within a large IP range (e.g., 200.200.0.0/16 has 65536 possible IPs)[cite: 176, 177, 178].

### 5.1. Host Discovery Techniques

- **Goal**: Identify which IP addresses belong to active/online hosts[cite: 203].
- **Common Methods**:
    - **ICMP Echo Ping Sweeps**: Send ICMP Echo Requests (Type 8) to a range of IPs; look for Echo Replies (Type 0)[cite: 205, 224, 225, 228, 229].
        - **Pros**: Quick, widely supported[cite: 219].
        - **Cons**: Often blocked by firewalls, easily detectable[cite: 220, 221].
        ```bash
        # Example using standard ping (less efficient for ranges)
        ping <target_ip>
        ```
    - **ARP Scans**: Use ARP requests on the *local* network. Very effective within the same broadcast domain[cite: 206, 207].
        ```bash
        # Nmap ARP Scan
        nmap -PR -sn <target_network/CIDR>
        ```
        ❗ **Important**: ARP scans only work on the local subnet.
    - **TCP SYN Ping**: Sends SYN packet (like start of handshake) to a port (e.g., 80). SYN-ACK response indicates host is up[cite: 208, 209]. Stealthier than ICMP[cite: 209].
        - **Pros**: Stealthier, might bypass some firewalls[cite: 221].
        - **Cons**: Can be blocked, not all hosts respond[cite: 222].
        ```bash
        # Nmap TCP SYN Ping (requires root/administrator)
        nmap -PS<port_list> -sn <target_ip_or_range>
        # Example: Nmap TCP SYN Ping to port 80
        sudo nmap -PS80 -sn 192.168.1.0/24
        ```
    - **TCP ACK Ping**: Sends ACK packet. RST response indicates host is up[cite: 212, 213]. Used to map out firewall rulesets.
        ```bash
        # Nmap TCP ACK Ping (requires root/administrator)
        nmap -PA<port_list> -sn <target_ip_or_range>
        # Example: Nmap TCP ACK Ping to port 80
        sudo nmap -PA80 -sn 192.168.1.0/24
        ```
    - **UDP Ping**: Sends UDP packet to a port. ICMP "Port Unreachable" means host is up (but port closed). No response could mean host up (port open/filtered) or host down[cite: 210, 211]. Less reliable but can find hosts missed by TCP/ICMP.
        ```bash
        # Nmap UDP Ping (requires root/administrator)
        nmap -PU<port_list> -sn <target_ip_or_range>
        # Example: Nmap UDP Ping to common UDP port 161 (SNMP)
        sudo nmap -PU161 -sn 192.168.1.0/24
        ```

### 5.2. Nmap (Network Mapper)

- **Overview**: Open-source tool for network discovery, security auditing, port scanning, OS/Service detection[cite: 194, 195].
- **Core Functionality**:
    - Host Discovery [cite: 197]
    - Port Scanning [cite: 198]
    - Service Version Detection [cite: 199]
    - OS Fingerprinting [cite: 201]
    - Scripting Engine (NSE) for advanced tasks [cite: 248]

💡 **Tip**: Nmap combines multiple discovery techniques by default when run as root/administrator for better accuracy. Use `-sn` (formerly `-sP`) to perform *only* host discovery (no port scan).

```bash
# Nmap Default Host Discovery (as root/admin: ICMP Echo, TCP SYN to 443, TCP ACK to 80, ICMP Timestamp)
sudo nmap -sn <target_network/CIDR>

# Nmap Default Host Discovery (as non-root: TCP SYN Handshake to 80 & 443)
nmap -sn <target_network/CIDR>