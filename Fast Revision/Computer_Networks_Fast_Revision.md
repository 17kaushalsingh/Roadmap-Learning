# Computer Networks Fast Revision 🌐

## Why Networks is Lower Priority 🎯

**I sometimes term it as optional. You should know the very basics like difference between TCP, UDP, how does stack works, how does your browser works when you search a particular domain. But beyond that going into too much of depth is not often required in many interviews.**

### **When to Prepare in Detail**:
- **Applying for network-specific roles** (Oracle network roles)
- **Company specializes in networking products**
- **Job description emphasizes networking**

### **Basic Topics to Cover**:
- **TCP vs UDP differences**
- **HTTP vs HTTPS**
- **OSI or TCP/IP layers**
- **DNS and CDN systems**
- **How browser works when searching domain**

---

## TCP/IP Protocol Suite 📡

### **1. TCP vs UDP** 🔀

#### **Transmission Control Protocol (TCP)**:
```
Characteristics:
- Connection-oriented
- Reliable delivery
- Ordered delivery
- Flow control
- Congestion control
- Three-way handshake
- Slow but reliable
```

#### **User Datagram Protocol (UDP)**:
```
Characteristics:
- Connectionless
- Unreliable delivery
- No ordering guarantee
- No flow control
- No congestion control
- Fast but unreliable
```

#### **Comparison Table**:

| **Aspect** | **TCP** | **UDP** |
|------------|---------|---------|
| **Connection** | Connection-oriented | Connectionless |
| **Reliability** | Reliable | Unreliable |
| **Ordering** | Guaranteed | Not guaranteed |
| **Speed** | Slower | Faster |
| **Header Size** | 20 bytes minimum | 8 bytes |
| **Use Cases** | Web, email, file transfer | Streaming, gaming, DNS |
| **Flow Control** | Yes | No |
| **Error Checking** | Yes | Minimal |

#### **Interview Examples**:

**Question**: "Why use UDP over TCP in gaming?"
**Answer**:
- **Low latency** crucial for real-time gaming
- **Can tolerate packet loss** (better to show old position than wait)
- **Faster response** time
- **Less overhead** (smaller headers)

**Question**: "When to use TCP over UDP?"
**Answer**:
- **File transfers** (need all data)
- **Email** (cannot lose messages)
- **Web browsing** (need complete pages)
- **Financial transactions** (must be reliable)

#### **Code Example**:
```java
// TCP Socket Example (Reliable)
Socket tcpSocket = new Socket("example.com", 80);
OutputStream out = tcpSocket.getOutputStream();
// Data arrives reliably and in order

// UDP Socket Example (Fast)
DatagramSocket udpSocket = new DatagramSocket();
// Data may be lost or arrive out of order
```

### **2. TCP Three-Way Handshake** 🤝

#### **Connection Establishment Process**:
```
Client                                    Server
  │                                        │
  │  SYN (seq=100) ──────────────────────►│
  │                                        │  SYN received
  │◄──────────────────── SYN+ACK (seq=300, ack=101)
  │  SYN+ACK received                      │
  │  ACK (seq=101, ack=301) ──────────────►│
  │                                        │  Connection established
```

#### **Step-by-Step Explanation**:

1. **SYN (Synchronize)**:
   - Client requests connection
   - Sends initial sequence number (e.g., 100)
   - "I want to connect, my starting number is 100"

2. **SYN+ACK (Synchronize + Acknowledge)**:
   - Server acknowledges client's SYN
   - Server sends its own sequence number (e.g., 300)
   - "I acknowledge your 100, my starting number is 300"

3. **ACK (Acknowledge)**:
   - Client acknowledges server's SYN
   - "I acknowledge your 300, let's start communicating"

#### **Interview Question**: "What is a SYN flood attack?"
**Answer**:
- Attacker sends many SYN packets but never completes handshake
- Server keeps resources allocated for half-open connections
- Exhausts server resources, preventing legitimate connections
- **Defense**: SYN cookies, rate limiting, connection throttling

### **3. Four-Way Handshake (Connection Termination)** 👋

#### **Connection Termination Process**:
```
Client                                    Server
  │                                        │
  │  FIN (seq=200) ──────────────────────►│
  │                                        │  FIN received
  │◄──────────────────── ACK (ack=201)
  │  ACK received                          │
  │                                        │  Server sends FIN
  │◄──────────────────── FIN (seq=500)
  │  FIN received                          │
  │  ACK (seq=201, ack=501) ──────────────►│
  │                                        │  Connection closed
```

---

## HTTP & Web Protocols 🌍

### **1. HTTP vs HTTPS** 🔒

#### **HTTP (Hypertext Transfer Protocol)**:
```
Characteristics:
- Plain text communication
- Port 80
- Not secure
- Vulnerable to eavesdropping
- Faster due to no encryption overhead
```

#### **HTTPS (HTTP Secure)**:
```
Characteristics:
- Encrypted communication
- Port 443
- Secure with SSL/TLS
- Prevents eavesdropping
- Slower due to encryption overhead
- Browser shows padlock icon
```

#### **HTTPS Handshake Process**:
```
1. Client Hello: Client sends supported cipher suites
2. Server Hello: Server selects cipher suite, sends certificate
3. Client Key Exchange: Client generates session key, encrypts with server's public key
4. Server Key Exchange: Server decrypts session key with private key
5. Secure Communication: Both parties use symmetric encryption
```

#### **Interview Question**: "How does HTTPS work?"
**Answer**:
1. **SSL/TLS handshake** establishes secure connection
2. **Server certificate** verification
3. **Symmetric key exchange** using asymmetric encryption
4. **All subsequent communication** encrypted with symmetric key
5. **Data integrity** ensured through message authentication codes

### **2. HTTP Methods** 📝

#### **Common HTTP Methods**:

| **Method** | **Description** | **Idempotent** | **Safe** |
|------------|-----------------|----------------|----------|
| **GET** | Retrieve data | Yes | Yes |
| **POST** | Submit data | No | No |
| **PUT** | Update/replace data | Yes | No |
| **DELETE** | Remove data | Yes | No |
| **PATCH** | Partial update | No | No |
| **HEAD** | Get headers only | Yes | Yes |
| **OPTIONS** | Get allowed methods | Yes | Yes |

#### **HTTP Status Codes**:

```http
// Success Codes (2xx)
200 OK                // Request successful
201 Created           // Resource created
204 No Content        // Request successful, no content

// Redirection Codes (3xx)
301 Moved Permanently // Resource permanently moved
302 Found             // Resource temporarily moved
304 Not Modified      // Resource not modified

// Client Error Codes (4xx)
400 Bad Request       // Malformed request
401 Unauthorized     // Authentication required
403 Forbidden        // Access denied
404 Not Found        // Resource not found
409 Conflict         // Resource conflict

// Server Error Codes (5xx)
500 Internal Server Error // Server error
502 Bad Gateway          // Invalid gateway response
503 Service Unavailable  // Server temporarily unavailable
```

---

## DNS & Domain Resolution 🔍

### **1. DNS (Domain Name System)** 📚

#### **What is DNS?**
**Hierarchical distributed naming system that translates human-readable domain names to machine-readable IP addresses.**

#### **DNS Resolution Process**:
```
User types: www.example.com

1. Browser cache check
2. OS cache check
3. Router cache check
4. ISP DNS server cache check
5. Root DNS server (.com)
6. TLD DNS server (example.com)
7. Authoritative DNS server (www.example.com)
8. Return IP address: 93.184.216.34
```

#### **DNS Record Types**:

| **Record Type** | **Purpose** | **Example** |
|-----------------|-------------|--------------|
| **A** | IPv4 address | www.example.com → 93.184.216.34 |
| **AAAA** | IPv6 address | www.example.com → 2606:2800:220:1:248:1893:25c8:1946 |
| **CNAME** | Canonical name alias | blog.example.com → www.example.com |
| **MX** | Mail exchange | example.com → mail.example.com |
| **NS** | Name server | example.com → ns1.example.com |
| **TXT** | Text records | example.com → "v=spf1 include:_spf.google.com ~all" |
| **SRV** | Service location | _sip._tcp.example.com → sip.example.com:5060 |

#### **Interview Question**: "What happens when you type google.com in browser?"
**Answer** (Complete process below in Browser section)

---

## OSI vs TCP/IP Models 🏗️

### **OSI 7-Layer Model**:

```
┌─────────────────────────────────────┐
│ 7. Application Layer    (HTTP, FTP, SMTP) │
├─────────────────────────────────────┤
│ 6. Presentation Layer (SSL/TLS, JPEG)  │
├─────────────────────────────────────┤
│ 5. Session Layer         (NetBIOS)   │
├─────────────────────────────────────┤
│ 4. Transport Layer       (TCP, UDP)  │
├─────────────────────────────────────┤
│ 3. Network Layer          (IP, ICMP) │
├─────────────────────────────────────┤
│ 2. Data Link Layer        (Ethernet) │
├─────────────────────────────────────┤
│ 1. Physical Layer         (Cables)   │
└─────────────────────────────────────┘
```

### **TCP/IP 4-Layer Model**:

```
┌─────────────────────────────────────┐
│ Application Layer      (HTTP, FTP, DNS) │
├─────────────────────────────────────┤
│ Transport Layer         (TCP, UDP)   │
├─────────────────────────────────────┤
│ Internet Layer          (IP, ICMP)   │
├─────────────────────────────────────┤
│ Link Layer              (Ethernet)   │
└─────────────────────────────────────┘
```

#### **Layer Mapping**:

| **OSI Layer** | **TCP/IP Layer** | **Protocols** |
|---------------|------------------|---------------|
| **Application** | Application | HTTP, FTP, SMTP, DNS |
| **Presentation** | Application | SSL/TLS, JPEG, MPEG |
| **Session** | Application | NetBIOS, RPC |
| **Transport** | Transport | TCP, UDP |
| **Network** | Internet | IP, ICMP, ARP |
| **Data Link** | Link | Ethernet, Wi-Fi |
| **Physical** | Link | Cables, Radio waves |

---

## Complete Browser Request Process 🌐

### **What Happens When You Type google.com** 🤔

#### **Step-by-Step Process**:

1. **URL Parsing**:
   ```
   Browser parses: https://www.google.com
   Protocol: https
   Domain: www.google.com
   Port: 443 (default for HTTPS)
   ```

2. **DNS Resolution**:
   ```
   Browser cache → OS cache → Router cache → ISP DNS → Root DNS → TLD DNS → Authoritative DNS
   Result: www.google.com → 142.250.181.78
   ```

3. **TCP Connection Setup**:
   ```
   Client → Server: SYN (seq=100)
   Server → Client: SYN+ACK (seq=300, ack=101)
   Client → Server: ACK (seq=101, ack=301)
   Connection established
   ```

4. **TLS Handshake** (for HTTPS):
   ```
   Client Hello: Supported cipher suites
   Server Hello: Selected cipher + certificate
   Client Key Exchange: Generate session key
   Server Key Exchange: Confirm session key
   Secure channel established
   ```

5. **HTTP Request**:
   ```
   GET / HTTP/1.1
   Host: www.google.com
   User-Agent: Mozilla/5.0...
   Accept: text/html...
   Connection: keep-alive
   ```

6. **Server Response**:
   ```
   HTTP/1.1 200 OK
   Content-Type: text/html; charset=UTF-8
   Content-Length: 14567
   Date: Mon, 06 Nov 2025 12:00:00 GMT

   <!DOCTYPE html><html>... (HTML content)
   ```

7. **HTML Parsing & Rendering**:
   ```
   Browser parses HTML
   Downloads CSS, JavaScript, images
   Constructs DOM tree
   Applies CSS styles
   Executes JavaScript
   Renders page
   ```

8. **Page Load Complete**:
   ```
   All resources loaded
   Page displayed to user
   Connection may be kept alive for future requests
   ```

#### **Interview Follow-up Questions**:

**Q: "What is the difference between HTTP/1.1 and HTTP/2?"**
**A**: HTTP/2 supports multiplexing (multiple requests over single connection), header compression, server push, and binary protocol instead of text.

**Q: "What is CDN and how does it help?"**
**A**: Content Delivery Network caches content at edge locations closer to users, reducing latency and improving load times.

---

## Network Security 🔒

### **1. Firewall Concepts** 🛡️

#### **Types of Firewalls**:

| **Type** | **Operation Layer** | **Example** |
|----------|-------------------|-------------|
| **Packet Filtering** | Network Layer | IP, port filtering |
| **Stateful Inspection** | Transport Layer | Tracks connection state |
| **Proxy Firewall** | Application Layer | Application-level filtering |
| **Next-Gen Firewall** | All Layers | Deep packet inspection |

#### **Common Firewall Rules**:
```bash
# Allow HTTP/HTTPS traffic
iptables -A INPUT -p tcp --dport 80 -j ACCEPT
iptables -A INPUT -p tcp --dport 443 -j ACCEPT

# Block specific IP
iptables -A INPUT -s 192.168.1.100 -j DROP

# Allow SSH from specific network
iptables -A INPUT -p tcp -s 192.168.1.0/24 --dport 22 -j ACCEPT
```

### **2. VPN (Virtual Private Network)** 🔐

#### **How VPN Works**:
```
User Device → VPN Server → Internet
     ↓              ↓
   Encrypted    Encrypted
   Tunnel       Tunnel
```

#### **VPN Benefits**:
- **Privacy**: Hide real IP address
- **Security**: Encrypt all traffic
- **Access**: Bypass geo-restrictions
- **Business**: Secure remote access

---

## Network Performance Metrics 📊

### **1. Bandwidth vs Throughput** 📈

#### **Bandwidth**:
```
Definition: Maximum data transfer capacity
Units: Mbps, Gbps
Example: 100 Mbps connection
```

#### **Throughput**:
```
Definition: Actual data transfer rate
Units: Mbps, Gbps
Example: 80 Mbps actual speed (due to overhead)
```

#### **Latency**:
```
Definition: Time for data to travel from source to destination
Units: milliseconds (ms)
Example: 50ms round-trip time
```

### **2. Network Optimization Tips** ⚡

#### **Reduce HTTP Requests**:
- **Combine CSS/JS files**
- **Use image sprites**
- **Enable HTTP/2 multiplexing**

#### **Caching Strategies**:
- **Browser caching** with proper headers
- **CDN caching** for static content
- **Database query caching**

#### **Compression**:
- **Gzip compression** for text content
- **Image optimization** (WebP, lazy loading)
- **Minification** of CSS/JS

---

## Common Interview Questions & Solutions 💼

### **Question 1: TCP vs UDP - When to use which?** 🔀

**Answer Structure**:
- **TCP**: When reliability is critical (file transfer, email, web)
- **UDP**: When speed is critical (gaming, streaming, DNS)
- **Trade-offs**: Reliability vs speed
- **Real-world examples**: HTTP uses TCP, DNS uses UDP

### **Question 2: Explain the complete process when you type a URL** 🌐

**Answer**: Provide step-by-step process from URL parsing to page rendering (as shown above)

### **Question 3: What is the difference between HTTP and HTTPS?** 🔒

**Answer**:
- **HTTP**: Plain text, port 80, not secure
- **HTTPS**: Encrypted, port 443, secure
- **SSL/TLS handshake** process
- **Certificate validation**
- **Performance impact** of encryption

### **Question 4: How does DNS work?** 🔍

**Answer**:
- **Hierarchical system** (root → TLD → authoritative)
- **Caching** at multiple levels
- **Record types** (A, AAAA, CNAME, MX, NS)
- **DNS resolution** process with examples

### **Question 5: What are the main differences between OSI and TCP/IP models?** 🏗️

**Answer**:
- **OSI**: 7 layers, theoretical model
- **TCP/IP**: 4 layers, practical implementation
- **Layer mapping** between models
- **Protocols** at each layer

### **Question 6: How does TCP ensure reliable data transfer?** 🤝

**Answer**:
- **Three-way handshake** for connection establishment
- **Sequence numbers** for ordering
- **Acknowledgments** for reliability
- **Flow control** (sliding window)
- **Congestion control** (slow start, congestion avoidance)
- **Error detection** (checksums)

### **Question 7: What is CDN and why is it used?** 🌍

**Answer**:
- **Content Delivery Network** - distributed servers
- **Benefits**: Reduced latency, load distribution, high availability
- **How it works**: Caches content at edge locations
- **Use cases**: Static content, video streaming, software downloads

### **Question 8: Difference between 2G, 3G, 4G networks** 📱

**Answer**:
- **2G**: Digital voice, SMS (GPRS, EDGE)
- **3G**: Mobile broadband, video calling (UMTS, HSPA)
- **4G**: High-speed internet (LTE)
- **Key differences**: Speed, technology, capabilities

---

## Network Cheat Sheet 📝

### **Quick Reference**:

#### **Port Numbers**:
```bash
HTTP: 80
HTTPS: 443
FTP: 21, 20
SSH: 22
Telnet: 23
SMTP: 25
DNS: 53
POP3: 110
IMAP: 143
MySQL: 3306
PostgreSQL: 5432
```

#### **Common Commands**:
```bash
# Network testing
ping google.com          # Test connectivity
traceroute google.com     # Trace route
nslookup google.com       # DNS lookup
netstat -an               # Show network connections
ipconfig /all            # Windows network config
ifconfig -a              # Linux network config

# Troubleshooting
telnet host port          # Test port connectivity
curl -I https://example.com # HTTP headers
wget https://example.com   # Download file
```

#### **Subnetting Quick Math**:
```bash
/24 = 255.255.255.0 = 256 addresses = 254 hosts
/16 = 255.255.0.0 = 65,536 addresses = 65,534 hosts
/8 = 255.0.0.0 = 16,777,216 addresses = 16,777,214 hosts
```

---

## Interview Preparation Checklist ✅

### **Must-Know Concepts**:
- [ ] **TCP vs UDP differences** and use cases
- [ ] **HTTP vs HTTPS** and security implications
- [ ] **DNS resolution** process
- [ ] **TCP handshake** (connection establishment)
- [ ] **OSI vs TCP/IP** models
- [ ] **Browser request** complete process
- [ ] **Network layers** and protocols
- [ ] **Basic network security** concepts

### **Common Scenarios to Understand**:
- [ ] **What happens when you type google.com**
- [ ] **How does HTTPS work**
- [ ] **Why use UDP for gaming**
- [ ] **How does CDN improve performance**
- [ ] **Network troubleshooting** steps

### **Optional Advanced Topics** (if applying for network roles):
- [ ] **Advanced routing protocols** (OSPF, BGP)
- [ ] **Network virtualization** (VLANs, VXLAN)
- [ ] **Load balancing algorithms**
- [ ] **Network monitoring tools**
- [ ] **Cloud networking** (VPC, load balancers)

---

## Key Takeaways 💡

1. **Focus on Basics**: TCP/UDP, HTTP/HTTPS, DNS are most important
2. **Understand Process**: Complete browser request process is commonly asked
3. **Practical Applications**: Connect theory to real-world scenarios
4. **Security Awareness**: HTTPS and basic security concepts
5. **Layer Understanding**: Know OSI/TCP/IP layer functions
6. **Performance Factors**: Bandwidth, latency, throughput differences
7. **Troubleshooting Skills**: Basic network diagnostic commands
8. **Company-Specific**: Adjust depth based on target companies

**Remember**: Networks is often optional unless applying for network-specific roles. Focus on the basics and most commonly asked concepts! 🌐

---

**Next Steps**: Review all Fast Revision materials and you're ready for core CS subject interviews! 🎯