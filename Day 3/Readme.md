# Day 3 — Networking Fundamentals

🌐 Networking Fundamentals for DevOps & DevSecOps Engineers

If you’re entering the world of DevOps, Cloud, Cybersecurity, or DevSecOps, there’s one thing you simply cannot escape:

👉 **Networking.**

You can automate Kubernetes deployments, build CI/CD pipelines, scan containers, or secure APIs all day long…
But if you don’t understand how systems communicate over a network, eventually things will break — and debugging becomes pure pain.

And trust me…

Every DevOps engineer has faced moments like:

- “Why is the service unreachable?”
- “Why is DNS failing?”
- “Why is port 443 blocked?”
- “Why is the pod timing out?”
- “Why does curl work but browser doesn’t?”
- “Why is UDP packet loss happening?”

At that moment, networking fundamentals stop being “theory” and become survival skills.

So in this blog, we’ll break networking down in a professional but chill chit-chat way — the way engineers actually learn in real life.

---

## 🚀 Why Networking Matters in Modern Tech

Today everything is connected:

- Cloud servers
- Kubernetes clusters
- APIs
- Microservices
- Databases
- CI/CD runners
- Containers
- Security tools
- VPNs
- CDNs

Even your Git push travels through multiple networking layers before reaching GitHub.

Understanding networking helps you:

✅ Debug faster
✅ Secure systems properly
✅ Understand cloud architecture
✅ Configure firewalls
✅ Work with Kubernetes confidently
✅ Handle load balancers & reverse proxies
✅ Understand attacks like DDoS, MITM, spoofing, scanning, etc.

---

## 🧠 What is Networking?

In simple words:

> Networking is the communication between devices.

When two systems exchange data, they follow a set of rules called **protocols**.

Example:

- Your browser requests a website
- DNS converts domain → IP
- TCP establishes connection
- HTTPS encrypts communication
- Server sends response

All this happens in milliseconds.

Crazy, right?

---

## 🏢 OSI Model — The Foundation of Networking

The **OSI Model (Open Systems Interconnection)** is a conceptual framework used to understand how data travels across a network.

It has **7 layers**.

Think of it like delivering a package through multiple departments.

---

## 📚 The 7 Layers of OSI Model

![OSI Model](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/ri4fagmoj6kuhoycsqbr.png)

---

## 🔍 Understanding Each Layer

---

### 7️⃣ Application Layer

This is where users interact.

Protocols:

- HTTP
- HTTPS
- DNS
- FTP
- SMTP

Example:
When you open YouTube in browser.

---

### 6️⃣ Presentation Layer

Handles:

- Encryption
- Compression
- Data formatting

Examples:

- SSL/TLS encryption
- JPEG/PNG formatting

This layer makes HTTPS secure.

---

### 5️⃣ Session Layer

Responsible for:

- Opening sessions
- Maintaining sessions
- Closing sessions

Example:
Keeping your login session active.

---

### 4️⃣ Transport Layer

This is where **TCP and UDP** live.

Responsibilities:

- Data delivery
- Error checking
- Packet sequencing

Protocols:

- TCP
- UDP

This layer is extremely important in DevOps and Security.

---

### 3️⃣ Network Layer

This layer handles:

- IP addressing
- Routing

Protocol:

- IP (Internet Protocol)

Routers operate here.

---

### 2️⃣ Data Link Layer

Handles:

- MAC addresses
- Local network communication

Switches operate here.

---

### 1️⃣ Physical Layer

The actual hardware:

- Cables
- Fiber optics
- Wi-Fi signals

This is the physical transmission layer.

---

## ⚡ TCP/IP Model — The Real Internet Model

Now here’s the interesting part:

The internet doesn’t actually use the full OSI model directly.

It mainly follows the **TCP/IP Model**.

![TCP IP Model](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/klh550qcyc8sghrmr37u.png)

---

### 📚 TCP/IP Layers

| TCP/IP Layer   | OSI Equivalent |
| -------------- | -------------- |
| Application    | OSI 5,6,7      |
| Transport      | OSI 4          |
| Internet       | OSI 3          |
| Network Access | OSI 1,2        |

---

## 🤔 OSI vs TCP/IP

| OSI                    | TCP/IP                      |
| ---------------------- | --------------------------- |
| Theoretical model      | Practical model             |
| 7 layers               | 4 layers                    |
| Used for understanding | Used in real internet       |
| More detailed          | More implementation-focused |

---

## 🌍 What is an IP Address?

Every device connected to a network needs an identity.

That identity is called an **IP Address**.

Example:

```bash
192.168.1.1
```

Think of IP like a house address for devices.

Without IP addresses:
❌ Internet communication is impossible.

---

### 🧩 Types of IP Addresses

### IPv4

Example:

```bash
192.168.0.1
```

32-bit addressing.

Limited addresses.

---

### IPv6

Example:

```bash
2001:0db8:85a3::8a2e:0370:7334
```

128-bit addressing.

Created because IPv4 addresses were running out.

---

## 🏠 Public vs Private IP

| Type       | Usage             |
| ---------- | ----------------- |
| Public IP  | Internet-facing   |
| Private IP | Internal networks |

Private ranges:

```bash
10.0.0.0/8
172.16.0.0/12
192.168.0.0/16
```

---

## 🌐 What is DNS?

DNS = **Domain Name System**

DNS converts human-friendly names into IP addresses.

Example:

```bash
google.com → 142.250.x.x
```

Because humans remember names better than numbers.

---

## 🔥 DNS Flow

![DNS Flow](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/gqzeabgmvx8pjhphpooj.png)

---

## 🛠 Common DNS Record Types

| Record | Purpose               |
| ------ | --------------------- |
| A      | Maps domain → IPv4    |
| AAAA   | Maps domain → IPv6    |
| CNAME  | Alias                 |
| MX     | Mail server           |
| TXT    | Verification/security |

---

## 🌍 What is HTTP?

HTTP = **HyperText Transfer Protocol**

Used for communication between:

- Browser
- Server

HTTP is stateless.

---

## 📦 Example HTTP Request

```http
GET /index.html HTTP/1.1
Host: example.com
```

---

## 🔒 What is HTTPS?

HTTPS = HTTP + SSL/TLS encryption.

This secures:
✅ Passwords
✅ Payments
✅ Tokens
✅ Sensitive data

Without HTTPS:
Attackers can sniff traffic.

---

## 🔥 HTTP vs HTTPS

| HTTP        | HTTPS     |
| ----------- | --------- |
| Unencrypted | Encrypted |
| Port 80     | Port 443  |
| Insecure    | Secure    |

---

## 🚪 What are Ports?

Ports are logical communication endpoints.

Think of IP as:
🏢 Building Address

And ports as:
🚪 Room Numbers

---

## 📚 Common Ports

| Port  | Service    |
| ----- | ---------- |
| 22    | SSH        |
| 53    | DNS        |
| 80    | HTTP       |
| 443   | HTTPS      |
| 3306  | MySQL      |
| 5432  | PostgreSQL |
| 6379  | Redis      |
| 27017 | MongoDB    |

---

## ⚔️ TCP vs UDP

This is one of the most important networking concepts.

---

### 📦 TCP (Transmission Control Protocol)

TCP is:
✅ Reliable
✅ Connection-oriented
✅ Ordered matters
✅ Error-checked

Used when data integrity matters.

Examples:

- HTTPS
- SSH
- FTP
- Database communication

---

### 🚀 UDP (User Datagram Protocol)

UDP is:
✅ Fast
✅ Lightweight
❌ No guarantee of delivery

Used when speed matters more than perfection.

Examples:

- Gaming
- Live streaming
- VoIP
- DNS queries

---

### 🔥 TCP vs UDP Comparison

| Feature        | TCP | UDP |
| -------------- | --- | --- |
| Reliable       | ✅  | ❌  |
| Fast           | ❌  | ✅  |
| Ordered        | ✅  | ❌  |
| Connection     | Yes | No  |
| Error Recovery | Yes | No  |

---

## 🔥 3-Way Handshake

Before TCP communication begins, client and server establish connection using the famous:

This ensures both systems are ready.

---

### 📡 Step 1 — SYN

Client sends:

```bash
SYN
```

Meaning:

> “Hey server, can we communicate?”

---

### 📡 Step 2 — SYN-ACK

Server replies:

```bash
SYN-ACK
```

Meaning:

> “Yes, I’m ready.”

---

### 📡 Step 3 — ACK

Client sends:

```bash
ACK
```

Meaning:

> “Perfect, let’s start.”

Connection established ✅

---

![Three Way Handshake](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/risahr8mmm5f1jwhzhei.png)

After this:
Actual data transfer begins.

---

## 🔥 Why 3-Way Handshake Matters in Security

Understanding handshake helps detect:

- SYN Flood attacks
- Connection hijacking
- Network scanning
- Reconnaissance

This is heavily used in:

- SOC operations
- Threat detection
- DevSecOps monitoring

---

## ☁️ Networking in Cloud & Kubernetes

Now comes the modern world.

In Kubernetes and Cloud:

Networking becomes even more important.

You deal with:

- Pod networking
- Service discovery
- Ingress controllers
- Load balancers
- DNS resolution
- Service mesh
- Internal routing

One small DNS issue can break entire production systems.

---

## 🔐 Networking + DevSecOps

DevSecOps engineers constantly work with:

- WAFs
- Firewalls
- Reverse proxies
- TLS certificates
- Network policies
- VPNs
- Zero Trust networking

Without networking knowledge:
Security becomes guesswork.

---

## 🧪 Essential Networking Commands Every Engineer Should Know

---

## ping

Checks connectivity.

```bash
ping google.com
```

---

## nslookup

Checks DNS resolution.

```bash
nslookup google.com
```

---

## curl

Tests HTTP requests.

```bash
curl https://example.com
```

---

## traceroute

Shows network path.

```bash
traceroute google.com
```

---

## netstat

Shows active connections.

```bash
netstat -tulnp
```

---

## ss

Modern replacement for netstat.

```bash
ss -tulnp
```

---

## 🧠 Real Industry Truth

A lot of engineers jump directly into:

- Kubernetes
- Docker
- Cloud
- Terraform
- Security tools

But skip networking fundamentals.

Then later:
everything becomes confusing.

The best DevOps and Security engineers usually have:
✅ Strong Linux basics
✅ Strong networking understanding
✅ Strong debugging mindset

Because infrastructure is ultimately just:

> Systems communicating with systems.

---

## 🎯 Final Thoughts

Networking is not optional anymore.

Whether you're:

- DevOps Engineer
- Cloud Engineer
- Backend Developer
- DevSecOps Engineer
- Security Researcher
- SRE

You must understand:

- IP
- DNS
- HTTP/HTTPS
- TCP/UDP
- Ports
- OSI Model
- TCP/IP Model
- 3-Way Handshake

These concepts are the backbone of modern infrastructure.

Once networking clicks in your brain…

Cloud starts making sense.
Kubernetes starts making sense.
Security starts making sense.
Even debugging becomes easier.

And honestly?

Most “complex production issues” eventually come down to:

> Networking somewhere broke.
