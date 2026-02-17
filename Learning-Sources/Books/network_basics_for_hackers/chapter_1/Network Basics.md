Being a successful cybersecurity engineer requires a deep and practical understanding of networks and how they operate.

**IP Addresses**  
An Internet Protocol (IP) address is what allows a device to connect to the internet and use services like email, Google Meet, and others. Every digital device connected to a network has a unique IP address, which functions much like a home address for data.

The IP system we primarily use today is IPv4 (Internet Protocol version 4). An IPv4 address is made up of four sets of numbers separated by dots. Each set represents 8 bits (ones and zeroes), meaning the total address is 32 bits long. For example, in the address `192.168.100.1`, each of the four numbers can range from 0 to 255 (which is 2<sup>8</sup> possible values).

[[1-ipv4-example.png]]

**Classes of IP Addresses**

There are three main classes and ranges for IP addresses:

**0.0.0.0 – 127.255.255.255 | Class A**

- **Purpose:** Designed for extremely large organizations (e.g., major corporations, governments, entire countries).
    
- **Capacity:** 126 possible networks, each capable of hosting approximately 16.7 million devices.
    
- **Analogy:** A giant continent with millions of addresses.
    

**128.0.0.0 – 191.255.255.255 | Class B**

- **Purpose:** Designed for medium to large organizations (e.g., large companies, universities).
    
- **Capacity:** Approximately 16,384 possible networks, each capable of hosting around 65,500 devices.
    
- **Analogy:** A large country or state.
    

**192.0.0.0 – 223.255.255.255 | Class C**

- **Purpose:** Designed for small organizations and local area networks (LANs).
    
- **Capacity:** Approximately 2.1 million possible networks, each capable of hosting 254 devices.
    
- **Analogy:** A single building or small office.
    

---

**The Other Classes (For Context)**

- **Class D:** 224.0.0.0 to 239.255.255.255
    
    - **Purpose:** Reserved for multicasting. Used to send data to a group of devices simultaneously (e.g., video streaming or online gaming), rather than assigning addresses to individual hosts.
        
- **Class E:** 240.0.0.0 to 255.255.255.255
    
    - **Purpose:** Reserved for experimental or future use. These addresses are not used on the public internet.


**Public vs. Private IP Addresses**

The IP protocol has its limitations. For example, with IPv4 we have only 4.3 billion possible addresses, yet there are more than 8 billion people in the world, each of whom may own multiple internet-connected devices.

To address this limitation, two solutions were developed. The first is IPv6, which provides an extremely large address space that can accommodate global demand for years to come. The second solution involves creating isolated environments that are separate from the public internet. Certain IP ranges were reserved for use within Local Area Networks (LANs), allowing the same addresses to be reused across countless private networks.

To access the internet, devices with private IP addresses must use a device or service called a NAT (Network Address Translation) router. The NAT device translates private addresses into a single public IP address, allowing multiple devices to share one public-facing connection.

The reserved private IP ranges are:

- **192.168.0.0 – 192.168.255.255** (Class C private block)
    
- **10.0.0.0 – 10.255.255.255** (Class A private block)
    
- **172.16.0.0 – 172.31.255.255** (Class B private block)

[[2-ifconfig-privet-ip.png]]

**DHCP (Dynamic Host Configuration Protocol)**

Dynamic Host Configuration Protocol (DHCP) automatically assigns IP addresses to devices when they connect to a network. Each time a device joins a network, it sends a request to the DHCP server asking for an IP address. The DHCP server then assigns an available IP address from its defined pool for a specific period of time. This time period is known as a **lease**.

When the lease expires, the device must either renew its current IP or request a new one. In most home or small office networks, devices typically receive a new IP address each time they connect, though these addresses usually fall within the same range—for example, `192.168.0.0` to `192.168.255.255`.

[[3-dhcp.png]]

**NAT (Network Address Translation)**

Remember how we said that IP addresses assigned by a DHCP server within a LAN are private and cannot be used directly on the internet? This is where Network Address Translation (NAT) comes in. NAT is a protocol that translates those private addresses into unique public addresses, allowing devices on a local network to access the internet.

Here's how it works: When you connect your device to the network, the DHCP server assigns it an IP address that is unique within your LAN environment. If you then want to access the internet, your device sends a request to the NAT device (usually built into your router). The NAT device records your private IP address and port number in a translation table and forwards your request to the internet using its own public IP address.

When the response returns from the internet, the NAT device checks its table to see which private device originally made the request. It then translates the public destination back to your private IP address and delivers the information to your device.

In short, NAT acts as an intermediary between your private network and the public internet, allowing many devices to share a single public IP address.

[[4-nat.png]]

**Ports**

Ports are like sub-addresses that work alongside IP addresses. Think of it this way: if an IP address is the address of a building in a city, then a port is the specific person or apartment within that building that should receive the information.

There are 65,535 available ports (ranging from 0 to 65,535). The first 1,024 ports (0–1023) are known as **well-known ports** or common ports, and they are reserved for specific services. While memorizing all of them is impractical, there are certain ports that every hacker and network professional should know by heart.

---

### Common Ports I Should Know

| Port Number | Protocol | Service |
|-------------|----------|---------|
| 20/21 | TCP | FTP (File Transfer Protocol) |
| 22 | TCP | SSH (Secure Shell) |
| 23 | TCP | Telnet (insecure remote access) |
| 25 | TCP | SMTP (Email sending) |
| 53 | TCP/UDP | DNS (Domain Name System) |
| 67/68 | UDP | DHCP (Dynamic Host Configuration Protocol) |
| 80 | TCP | HTTP (Standard web traffic) |
| 110 | TCP | POP3 (Email retrieval) |
| 123 | UDP | NTP (Network Time Protocol) |
| 139/445 | TCP | NetBIOS / SMB (Windows file sharing) |
| 143 | TCP | IMAP (Email retrieval) |
| 161/162 | UDP | SNMP (Simple Network Management Protocol) |
| 443 | TCP | HTTPS (Secure web traffic) |
| 514 | UDP | Syslog |
| 636 | TCP | LDAPS (Secure LDAP) |
| 989/990 | TCP | FTPS (FTP over SSL) |
| 3306 | TCP | MySQL (Database) |
| 3389 | TCP | RDP (Remote Desktop Protocol) |
| 5432 | TCP | PostgreSQL (Database) |
| 5900 | TCP | VNC (Remote access) |
| 6379 | TCP | Redis (Database) |
| 8080 | TCP | HTTP Alternate (Web proxies, Tomcat) |

---

For example, you can scan a target system using a tool like **Nmap** to discover which ports are open and which are closed. This is a fundamental step in reconnaissance during a penetration test.

[[5-nmap-st-scan.png]]


**TCP/IP**

The most common protocols used for communication on the internet are TCP (Transmission Control Protocol) and IP (Internet Protocol). Together, they form the backbone of modern networking and are often referred to as the **TCP/IP suite**.

To become a skilled network engineer or cybersecurity professional, you need a solid understanding of how these protocols work at a fundamental level. When working in this field, building tools or analyzing traffic without understanding the underlying protocols and standards is simply wasting time. TCP/IP knowledge is essential for tasks like packet analysis, firewall configuration, and exploit development.

**What Are Protocols?**

Protocols are like languages. They establish a set of rules for communication between devices. If every device used whatever language it preferred, no one would understand each other—just like two people speaking different languages.

In networking, these rules are typically defined in documents called **RFCs (Requests for Comments)** . RFCs are published by the IETF (Internet Engineering Task Force) and serve as the official standards for how internet protocols should work.

There are many protocols used on the internet, each serving a different purpose. Some handle web browsing, others manage email, and some take care of file transfers or remote access.

[[6-common-protocols.png]]

### Why This Matters for Hackers

Understanding protocols is critical because every attack exploits how a protocol works—or how it was implemented poorly. For example:

- **HTTP** can be manipulated for web application attacks (SQL injection, XSS).
    
- **DNS** can be abused for tunneling or poisoning attacks.
    
- **SMB** is often targeted in Windows environments (EternalBlue, PSExec).
    
- **SSH** can be brute-forced or misconfigured.
    

When you know the rules, you can spot the exceptions—and that's where vulnerabilities live.


**IP (Internet Protocol)**

The Internet Protocol (IP) is responsible for addressing and routing packets between devices. In simple terms, the IP header tells us where a packet is coming from (source) and where it is going (destination), along with other important information needed to deliver it correctly.

Let's take a closer look at the IP header structure.

[[7-ipv4-header.png]]

**Row 1**

| Field | Description |
|-------|-------------|
| **Version** | Indicates the IP version used (e.g., IPv4 or IPv6). This is always 4 for IPv4 packets. |
| **IHL (Internet Header Length)** | Defines the length of the IP header in 32-bit words. The minimum is 5 (20 bytes) and the maximum is 15 (60 bytes) when options are present. |
| **Type of Service (ToS)** | Specifies how the packet should be handled. Can request: minimize delay, maximize throughput, maximize reliability, or minimize monetary cost. In modern networking, this field is often re-purposed for Differentiated Services (DiffServ). |
| **Total Length** | Defines the complete length of the IP datagram (header + data) in bytes. The maximum value is 65,535 bytes. |

---

**Row 2**

| Field | Description |
|-------|-------------|
| **Identification** | A unique ID number assigned to each packet. If a packet is fragmented, all fragments share the same identification number so they can be reassembled correctly at the destination. |
| **IP Flags** | Controls fragmentation. The flags include: |
| | • **Reserved bit:** Always set to 0. |
| | • **DF (Don't Fragment):** If set to 1, the packet should not be fragmented. |
| | • **MF (More Fragments):** If set to 1, more fragments follow; if 0, this is the last fragment. |
| | Manipulating these flags can sometimes help evade firewalls or intrusion detection systems (IDS). |
| **Fragment Offset** | Used when a packet is fragmented. This field indicates where this fragment belongs in the original packet, allowing the fragments to be reassembled in the correct order. |

---

**Row 3**

| Field | Description |
|-------|-------------|
| **TTL (Time to Live)** | Prevents packets from looping endlessly on the network. Each router that handles the packet decreases this value by 1. When TTL reaches 0, the packet is discarded. The initial TTL value can help identify the target's operating system (e.g., Windows often uses 128, Linux uses 64, Cisco uses 255). |
| **Protocol** | Identifies the transport layer protocol used inside the IP packet. Common values include: |
| | • **1:** ICMP (Internet Control Message Protocol) |
| | • **6:** TCP (Transmission Control Protocol) |
| | • **17:** UDP (User Datagram Protocol) |
| **Header Checksum** | Provides error checking for the IP header only (not the data). Each router recalculates this because the TTL field changes. If the checksum doesn't match, the packet is discarded. |

---

**Rows 4 & 5**

| Field | Description |
|-------|-------------|
| **Source Address** | The 32-bit IP address of the sender. |
| **Destination Address** | The 32-bit IP address of the intended receiver. |

---

**Row 6**

| Field | Description |
|-------|-------------|
| **Options** | An optional field used for additional features like timestamps, routing records, or security restrictions. Rarely used in modern networks, but can be exploited for attacks like IP spoofing or covert channels. |
| **Padding** | Ensures the header ends exactly at a 32-bit boundary. If the options don't fill a complete 32-bit word, padding zeros are added. |

---

### Hacker's Perspective

Understanding each field opens up possibilities for both attack and defense:

- **TTL:** Use initial TTL values for OS fingerprinting (nmap does this automatically).
- **Flags & Fragment Offset:** Crafting fragmented packets can sometimes bypass weak firewall rules or IDS signatures.
- **Protocol Field:** Tells you what service is running—scanning for protocol 6 (TCP) vs. 17 (UDP) reveals different attack surfaces.
- **Options:** Can be abused for source routing attacks (though modern networks often block this).
- **Identification:** Some tools use this field for covert data storage (packet crafting).


**TCP (Transmission Control Protocol)**

While IP handles addressing and routing, TCP is responsible for reliable, connection-based communication. Before data is sent, TCP establishes a connection between devices using the famous **three-way handshake** (SYN, SYN-ACK, ACK).

The TCP header contains many fields, but some are especially important to understand for packet analysis, scanning techniques, and exploitation.

[[8-tcp-header.png]]

**Row 1**

| Field | Description |
|-------|-------------|
| **Source Port** | The port number from which the packet is sent (random high port for clients, e.g., 54322). |
| **Destination Port** | The port number to which the packet is going (e.g., 80 for HTTP, 22 for SSH). |

---

**Row 2**

| Field | Description |
|-------|-------------|
| **Sequence Number** | A randomly generated number that tracks the order of data bytes sent. It ensures packets are reassembled in the correct order and helps maintain data integrity. This number is also important for defending against certain attacks, like session hijacking or replay attacks, because an attacker must guess or know the correct sequence number to inject malicious packets. |

---

**Row 3**

| Field | Description |
|-------|-------------|
| **Acknowledgment Number** | Confirms receipt of data. It tells the sender: "I have received up to this sequence number, and I'm expecting the next byte to have this sequence number." If the sender doesn't receive an acknowledgment for a packet within a certain time, it will retransmit that packet. This ensures reliable delivery. |

---

**Row 4 (Flags)**

This row contains several critical fields, including the **Data Offset**, **Reserved** bits, and the **TCP Flags**. The flags are 9 bits that control the state of the connection and are essential for understanding the three-way handshake and other TCP operations.

| Flag | Name | Description |
|------|------|-------------|
| **NS** | ECN-nonce Conceal Protection | Part of Explicit Congestion Notification (rarely used in basic scenarios). |
| **CWR** | Congestion Window Reduced | Indicates that the sender received a congestion notification and reduced its transmission rate. |
| **ECE** | ECN-Echo | Indicates that the sender supports ECN and received a congestion notification. |
| **URG** | Urgent | Marks data as urgent. The Urgent Pointer field (Row 5) becomes active when this flag is set. |
| **ACK** | Acknowledgment | Confirms receipt of data. After the three-way handshake, **all** packets should have this flag set. |
| **PSH** | Push | Instructs the receiver to push the data directly to the application immediately, rather than buffering it. |
| **RST** | Reset | Abruptly terminates a connection. This is sent when a packet arrives for a port that is closed or when something is wrong with the connection. |
| **SYN** | Synchronize | Initiates a new connection. This is the first packet sent in the three-way handshake. |
| **FIN** | Finish | Gracefully closes a connection. Indicates that the sender has no more data to send. |

**Hacker Note:** Tools like **Nmap** and **hping3** allow you to craft packets with custom flag combinations. By manipulating these flags, you can sometimes:
- Evade firewalls or intrusion detection systems (IDS)
- Perform stealthy reconnaissance
- Get responses from secure systems that might ignore standard probes
- Conduct OS fingerprinting (different OSes handle unusual flag combinations differently)

---

**Window Size**

| Field | Description |
|-------|-------------|
| **Window Size** | Used for flow control. It tells the sender how much data (in bytes) the receiver is willing to accept before needing an acknowledgment. This prevents the sender from overwhelming a slow receiver. The initial window size can sometimes help identify the operating system of the sender. While not 100% accurate, it can give you a reliable guess about 80% of the time when combined with other factors like TTL and TCP options. |

---

**Row 5**

| Field | Description |
|-------|-------------|
| **Checksum** | Similar to the IP header checksum, this field verifies the integrity of the entire TCP segment (header + data). If the checksum doesn't match, the packet is discarded. |
| **Urgent Pointer** | Points to the last byte of urgent data in the sequence. This field is only valid if the **URG** flag is set in Row 4. It tells the receiver where the urgent data ends. |

---

**Row 6**

| Field | Description |
|-------|-------------|
| **Options** | Similar to the IP options field, this allows for additional TCP features like Maximum Segment Size (MSS), window scaling, timestamps, and selective acknowledgments (SACK). The size varies depending on which options are used. |
| **Padding** | Ensures the TCP header ends exactly on a 32-bit boundary. Zeros are added if necessary to fill any remaining space after the Options field. |

---

### Summary: Why TCP Headers Matter for Hackers

| Field | Hacker Use Case |
|-------|-----------------|
| **Source/Destination Port** | Identifying services, port scanning |
| **Sequence Number** | Session hijacking, packet injection |
| **Acknowledgment Number** | Understanding connection state |
| **Flags (SYN, ACK, RST, etc.)** | Port scanning, OS fingerprinting, firewall evasion |
| **Window Size** | OS fingerprinting |
| **Checksum** | Packet crafting, evasion (sometimes checksums can be intentionally corrupted) |
| **Options** | OS fingerprinting, advanced evasion |

**TCP Three-Way Handshake**

Every TCP connection begins with a three-step process known as the **three-way handshake**. This establishes a reliable connection between two devices before data transfer begins.

Here's how it works:

1. **SYN** (Client → Server)
    
    - The client sends a packet with the **SYN** (Synchronize) flag set.
        
    - This essentially says: _"Hey, I want to establish a connection. Here's my initial sequence number."_
        
2. **SYN-ACK** (Server → Client)
    
    - If the server is willing to accept the connection, it responds with a packet containing both the **SYN** and **ACK** (Acknowledgment) flags set.
        
    - This says: _"I received your request, and I'm also ready to talk. Here's my initial sequence number, and I acknowledge yours."_
        
3. **ACK** (Client → Server)
    
    - The client sends back a packet with the **ACK** flag set.
        
    - This says: _"Got it. Let's start communicating."_
        

At this point, the connection is established, and data transfer can begin.

### Why This Matters for Hackers

Understanding the three-way handshake is essential for several reasons:

|Concept|Hacker Application|
|---|---|
|**SYN Scan (-sS)**|Nmap's stealth scan sends a SYN packet and analyzes the response. If SYN-ACK comes back, the port is open; if RST comes back, it's closed. This never completes the full handshake, making it harder to log.|
|**SYN Flood**|A DoS attack where the attacker sends many SYN packets but never responds to the SYN-ACKs, exhausting the server's resources as it waits for half-open connections.|
|**Firewall Evasion**|Some firewalls only inspect fully established connections. Crafting packets with just SYN or other flag combinations can sometimes slip through.|
|**Session Hijacking**|Understanding sequence numbers is critical for predicting or guessing them to inject packets into an existing connection.|
|**Wireshark Analysis**|Following the handshake in a packet capture helps you understand the state of a connection and spot anomalies.|
[[9-three-way-handhsake.png]]

**UDP (User Datagram Protocol)**

User Datagram Protocol (UDP) is a connectionless protocol that prioritizes speed over reliability. Unlike TCP, UDP does not establish a connection before sending data, nor does it verify that packets arrived successfully. It simply sends the packets and forgets about them—hence its nickname, the "fire-and-forget" protocol.

This makes UDP ideal for applications where speed is more important than perfect data delivery. For example:

- **Video calls** – A few lost frames are preferable to buffering and delays.
    
- **Online gaming** – Fast updates matter more than every single packet arriving correctly.
    
- **DNS queries** – Quick lookups with minimal overhead.
    
- **Streaming media** – Continuous flow matters more than retransmitting lost packets.

[[10-udp.png]]

However, for applications where every byte counts, TCP is the better choice. Banking transactions, file transfers, and web browsing all rely on TCP because missing or corrupted data is unacceptable.

### UDP and Port Scanning

Scanning for open UDP ports on a target system can be slow and tricky. Here's why:

- **TCP scanning** gives you immediate feedback: open ports respond with SYN-ACK, closed ports respond with RST.
    
- **UDP scanning** is different. When you send a UDP packet to a closed port, the target often responds with an ICMP "Port Unreachable" message. But if the port is open, there may be **no response at all**—the application may simply ignore empty probes.
    

This means Nmap and other scanners must wait for a timeout period before assuming a port is closed. If no response is received within the default timeout, Nmap marks the port as `open|filtered` because it can't distinguish between:

- An open port that ignored the probe
    
- A port that is filtered by a firewall
    
- A lost packet

Because of this, UDP scans can take much longer than TCP scans. To speed things up, you can send protocol-specific payloads that might trigger a response from the listening service (e.g., DNS queries to port 53, SNMP requests to port 161).

[[11-nmap-udp-scan.png]]

**Network Topologies**

When devices are connected together on a network, the way they are physically or logically arranged matters. The physical layout affects factors like **distance**, **latency**, **congestion**, and **availability**. Each device on the network is called a **node**, and the configuration of how these nodes are connected is known as the **network topology**.

### Key Factors Influenced by Topology

|Factor|Description|Why It Matters|
|---|---|---|
|**Distance**|The physical length of cables or range of wireless signals between nodes.|Longer distances can cause signal degradation and require repeaters or switches.|
|**Latency**|The delay before data begins to transfer after a request is sent.|High latency slows down communication—critical for real-time applications like VoIP or gaming.|
|**Congestion**|When network traffic exceeds capacity, causing delays and packet loss.|Poor topology design can create bottlenecks where too much traffic competes for the same path.|
|**Availability**|The ability of the network to remain operational even if part of it fails.|Redundant topologies keep systems online; single points of failure can take down entire networks.|
### Why This Matters for Hackers

Understanding network topologies helps you:

- **Identify single points of failure** – Take out one device and the whole network collapses.
    
- **Plan physical access** – In bus or star topologies, tapping into the right spot gives you maximum visibility.
    
- **Map the network mentally** – Knowing whether you're in a star, mesh, or tree topology tells you how traffic flows and where to position yourself for sniffing or attacks.
    
- **Understand redundancy** – In mesh networks, you may need to compromise multiple nodes to disrupt communications.
    
- **Choose attack vectors** – ARP spoofing works well in star topologies; ring topologies might require different approaches.



**Bus Topology**

In a bus topology, all devices are connected to a single central cable, often called the "bus" or backbone.

- **Advantages:** Cheap and simple to set up for small networks.
    
- **Disadvantages:** All nodes can see all traffic passing through the bus (which is a security risk). On busy networks, the single cable can become overwhelmed, causing slowdowns and collisions. If the main cable breaks, the entire network goes down.

[[12-bus-topology.png]]

---

**Star Topology**

This is the most common way of connecting devices in a Local Area Network (LAN) today. All devices connect to a central device—usually a switch or router—using their own separate cable.

- **Advantages:** If one cable or device fails, the rest of the network continues to function normally. Easy to add or remove devices without disrupting others. Centralized management makes troubleshooting easier.
    
- **Disadvantages:** The central switch or router is a single point of failure. If it goes down, the entire network goes down.

[[13-star-topology.png]]

---

**Ring Topology**

In a ring topology, each device connects to two other devices, forming a circular path. Data travels around the ring in one direction (or both directions in a dual-ring setup). Each node examines the data and forwards it to the next node until it reaches its destination.

- **Advantages:** Simple and relatively cheap. Handles traffic well because data flows in an organized manner.
    
- **Disadvantages:** If a single cable or device breaks, the entire ring can fail (unless it's a dual-ring design). Adding or removing devices disrupts the network.

[[14-ring-topology.png]]


**Key Difference Between Bus and Ring Topology**

A bus topology has two physical ends that must be terminated to prevent signal reflection. A ring topology has no ends—it forms a continuous loop, so terminators are not needed.

---

**Mesh Topology**

In a mesh topology, devices are interconnected with multiple redundant paths. In a **full mesh**, every device connects directly to every other device. In a **partial mesh**, most devices connect to each other, but not all.

- **Advantages:** Extremely fault-tolerant and reliable. If one link fails, data can take an alternate path. Excellent for networks where uptime is critical.
    
- **Disadvantages:** Expensive and complex to set up because of the large number of cables and ports required. Difficult to manage and reconfigure.
    

This topology is commonly used in wide area networks (WANs), backbone infrastructure, and wireless mesh networks—not typically in small LANs. While it's a modern approach for certain applications, the star topology remains the dominant choice for most office and home networks.

[[15-mesh-topology.png]]

### Hacker's Perspective on Topologies

|Topology|Security Implications|
|---|---|
|**Bus**|Easy to tap into the cable and sniff all traffic. No isolation between devices.|
|**Star**|The central switch is the prime target. Compromise it, and you can monitor or manipulate all traffic. ARP spoofing and man-in-the-middle attacks are common here.|
|**Ring**|Compromising one node gives you access to all traffic passing through it. In a single-ring design, taking out one node disrupts the entire network.|
|**Mesh**|Harder to take down due to redundancy, but also harder to fully monitor. Compromising a central or highly connected node gives you multiple attack paths.|
### Topology Comparison Table

| Topology   | Cost        | Fault Tolerance                  | Scalability | Security Considerations             |
| ---------- | ----------- | -------------------------------- | ----------- | ----------------------------------- |
| **Bus**    | Low         | Poor (single point of failure)   | Poor        | Easy to tap and sniff               |
| **Star**   | Medium      | Good (node failures isolated)    | Good        | Central switch is critical target   |
| **Ring**   | Medium      | Moderate (dual ring improves it) | Moderate    | One compromised node affects others |
| **Mesh**   | High        | Excellent                        | Good        | Hard to fully monitor               |

### What Is the OSI Model?

The **Open Systems Interconnection (OSI) model** is a conceptual framework that standardizes the functions of a network into seven distinct layers. Each layer has a specific role and communicates with the layers directly above and below it. This approach helps engineers troubleshoot problems, design networks, and understand how data flows from one application to another across the internet.

Think of it like sending a letter through the postal system: you write the message (Application), put it in an envelope (Presentation), add addressing (Session), and so on, until it's physically transported (Physical) to the destination, where the process reverses.

---

### The Seven Layers of the OSI Model

[[16-osi-model.png]]

| Layer | Name | Function | Common Protocols | Hacker's Perspective |
|-------|------|----------|------------------|---------------------|
| **7** | **Application** | The closest layer to the end user. Provides network services directly to applications (e.g., email, web browsing, file transfers). | HTTP, HTTPS, FTP, SMTP, DNS, SSH | Attack surface for web exploits, credential theft, and application-level vulnerabilities (SQL injection, XSS). |
| **6** | **Presentation** | Translates data between the application layer and the network. Handles encryption, compression, and formatting (e.g., converting EBCDIC to ASCII). | SSL/TLS, JPEG, GIF, ASCII, MPEG | Where encryption/decryption happens. Weak encryption or misconfigurations can be exploited here. |
| **5** | **Session** | Manages sessions or connections between applications. Establishes, maintains, and terminates conversations. | NetBIOS, RPC, PPTP, SIP | Session hijacking, token theft, and man-in-the-middle attacks target this layer. |
| **4** | **Transport** | Ensures reliable or unreliable delivery of data. Handles segmentation, flow control, and error checking. | **TCP**, **UDP** | Port scanning, TCP flag manipulation, SYN floods, and session hijacking occur here. |
| **3** | **Network** | Responsible for logical addressing and routing. Determines the best path for data to travel from source to destination. | **IP**, ICMP, ARP, RIP, OSPF | IP spoofing, routing attacks, ICMP tunneling, and packet fragmentation evasion. |
| **2** | **Data Link** | Provides node-to-node data transfer across the physical layer. Handles MAC addressing and error detection. | Ethernet, PPP, Switch, ARP, MAC | MAC flooding, ARP spoofing, switch attacks (CAM table overflow), and VLAN hopping. |
| **1** | **Physical** | Transmits raw bits over a physical medium. Defines cables, voltages, frequencies, and hardware specifications. | Ethernet cables, fiber optics, hubs, repeaters, radio frequencies | Physical security breaches: tapping cables, rogue access points, hardware implants. |

---

### How Data Moves Through the OSI Model (Encapsulation)

When one device sends data to another, it travels **down** the layers on the sender side and **up** the layers on the receiver side. At each layer, information is added (encapsulation) and then stripped away (de-encapsulation).

```
Sender                                    Receiver
  |                                          |
7: Application Data                         7: Application Data
  |                                          |
6: Presentation (adds formatting)           6: Presentation (removes formatting)
  |                                          |
5: Session (adds session info)              5: Session (removes session info)
  |                                          |
4: Transport (adds TCP/UDP header)          4: Transport (removes TCP/UDP header)
  |                                          |
3: Network (adds IP header)                 3: Network (removes IP header)
  |                                          |
2: Data Link (adds MAC header + trailer)    2: Data Link (removes MAC header + trailer)
  |                                          |
1: Physical (bits on the wire) ------------> 1: Physical (receives bits)
```

This process is often remembered by the phrase: **"Please Do Not Throw Sausage Pizza Away"** (Physical, Data Link, Network, Transport, Session, Presentation, Application) from bottom to top.

---

### Why the OSI Model Matters for Hackers

| Layer | What Hackers Look For |
|-------|------------------------|
| **7 - Application** | Web app vulnerabilities, weak authentication, business logic flaws |
| **6 - Presentation** | Weak encryption, SSL stripping, compression side-channel attacks |
| **5 - Session** | Session fixation, cookie theft, token replay |
| **4 - Transport** | Open ports, vulnerable services, TCP sequence prediction |
| **3 - Network** | IP spoofing, man-in-the-middle, routing protocol attacks |
| **2 - Data Link** | ARP poisoning, MAC flooding, VLAN hopping |
| **1 - Physical** | Physical access, rogue devices, cable tapping |

Understanding the OSI model helps you:
- **Isolate problems** – Know exactly which layer an issue or attack is targeting.
- **Choose the right tool** – Different tools work at different layers (e.g., Wireshark at Layers 2-7, Nmap at Layer 4, Metasploit at Layer 7).
- **Evade detection** – Some security controls only monitor specific layers; you can craft attacks that fly under the radar by targeting a different layer.
- **Understand exploits** – Many CVEs (Common Vulnerabilities and Exposures) are described in terms of which OSI layer they affect.
