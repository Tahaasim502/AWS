# AWS Cloud & Networking — Corvit Notes

> **Course:** Corvit DevOps | **Topic:** Cloud Types, AWS Global Infrastructure, Protocols **Tags:** #aws #cloud #networking #corvit #devops

---

## 1. Types of Cloud

### 1.1 Public Cloud

- Example: **PTCL** → connects to Public Internet → Public Cloud
- Data is **not secure** (sits on shared infrastructure)
- **Not a good option** for sensitive workloads

### 1.2 Private Cloud

- Infrastructure owned and managed by your **own company** (on-prem data center)
- Data stays inside your organization → **Secure**
- Example: Building your own data center

### 1.3 Hybrid Cloud

- **Public + Private** combined
- Public side handles → **Application Services**
- Private side handles → **Storing Data**
- **Seamless Connection** between both sides
    - Users cannot tell which cloud the data is coming from (transparent from user view)

> 📝 **Why keep data in the middle (between public & private)?** → **Less latency (delay)** — data is closer to both the app and the user

> ✅ **Missing — 4th Cloud Type: Community Cloud** Shared infrastructure between organizations with common concerns (e.g., government agencies, hospitals). Worth knowing for completeness.

---

## 2. AWS Global Infrastructure

### 2.1 Why Does AWS Give Regions?

- All **data-centers** are connected together → called a **Zone (Availability Zone)**
- Multiple **Zones** are connected together → form a **Region**

```
Data-Centers → Zone → Region
```

- **Region** = Collection of Zones (more than 1)
- **Zone (AZ)** = Collection of Data-Centers

> ✅ **Added:** AWS currently has **30+ Regions** and **90+ Availability Zones** worldwide. Region names follow the format: `us-east-1`, `ap-southeast-1` etc. Zone names are like `us-east-1a`, `us-east-1b` — but AWS **does not display zone names publicly** (your notes correctly capture this).

### 2.2 Key Properties of Regions & Zones

- Every region has a **different cost** (pricing varies by region)
- Zone names are **not displayed** to users
- Zones are connected with **ultra-low bandwidth**:
    - **Bandwidth** = rate per data transfer
    - **Throughput** = amount of data that can flow (e.g., 2GB, 3GB, etc.)
- **If one Zone fails → traffic connects to another Zone** (fault tolerance)

### 2.3 Edge Locations

- Data is stored **temporarily** at the nearest data-center to the user
- Purpose: User can **access it easily and quickly**
- AWS service that uses Edge Locations: **CloudFront (CDN)**

> ✅ **Added:** Edge Locations are part of **Amazon CloudFront**. There are **400+ Edge Locations** globally — far more than Regions. They cache content (images, videos, static files) so users get fast response times regardless of where the origin server is.

### 2.4 Summary — 4 Key Concepts (from your "4:-" heading)

1. **Zone** (Availability Zone)
2. **Data-Center**
3. **Region**
4. _(Edge Location — this was the missing 3rd/4th point in your notes)_

---

## 3. Protocols

> **Definition:** A protocol is a **set of rules and regulations for communication** **Goal:** Ensure two systems can talk to each other correctly

Common protocols: `TCP`, `IP`, `SSH`, `HTTPS`, `FTP` …

---

### 3.1 Ports (Logical — used in AWS Security Groups)

|Protocol|Port|Purpose|
|---|---|---|
|HTTP|80|Web traffic (unsecured) — images, photos, etc.|
|HTTPS|443|Secure web traffic|
|SSH|22|CLI access to Linux servers|
|RDP|3389|Remote Desktop (GUI-based, Windows)|
|Telnet|21*|Unsecured remote access _(actually port 23)_|
|FTP|21|File transfer|
|DNS|53|Domain name resolution|
|ICMP|—|No port number (used for Ping)|


#### Port 80 — HTTP

- Handles: images, photos, web pages
- Flow: **Request → Response → Receive**
- Example: Browser → HTTP → Google

#### Port 443 — HTTPS

- Secured version of HTTP (uses TLS/SSL encryption)

#### Port 22 — SSH

- Only gives access to the **CLI** (no GUI)
- Used to remotely manage Linux EC2 instances in AWS

#### Port 3389 — RDP (Remote Desktop Protocol)

- **GUI-based** remote access
- Easier for non-technical users (visual interface)
- Used for **Windows** servers
- Allows full desktop control

#### Port 23 — Telnet

- **Not secure** (data sent in plain text)
- Replaced by SSH

---

### 3.2 ICMP & Ping

- **Ping** → tests connectivity of a system
- Uses **ICMP protocol**
    - ICMP has **no port number**
    - Only checks connectivity — does not transfer data

---

### 3.3 TCP vs UDP

|Feature|TCP|UDP|
|---|---|---|
|Type|Connection-Oriented|Connectionless|
|Handshake|Yes (3-way handshake)|No handshake|
|Reliability|Reliable — guarantees delivery|Unreliable — no guarantee|
|Speed|Slower (overhead)|Faster|
|Use Case|Web, SSH, HTTPS, Email|Video streaming, DNS, VoIP, Gaming|

>  TCP 3-Way Handshake:**
> 
> 1. **SYN** — Client sends connection request
> 2. **SYN-ACK** — Server acknowledges
> 3. **ACK** — Client confirms → connection established

---

## 4. EC2 — User Data & Automation

### 4.1 User Data Script

- Runs **once** at EC2 instance launch (bootstrap)
- Used for: **Bootstrap Info** → server creation details
    - Automatically creates the server and runs the product
    - You need to provide a **script**

```bash
#!/bin/bash        ← Shebang line (tells OS: a script is coming)
hostname -f        ← FQDN = Fully Qualified Domain Name
```

- `$(hostname)` → **environment variable** that holds the machine's hostname

### 4.2 Automation

- **Launch Template** → saves EC2 configuration so you can reuse/automate instance creation

#### Template vs Image (AMI)

- A **Launch Template cannot be deleted first** if an AMI depends on it
- Images (AMIs) are snapshots of your instance state

> Key distinction:**
> 
> - **AMI (Amazon Machine Image)** = snapshot of OS + installed software (what's on the disk)
> - **Launch Template** = configuration blueprint (instance type, key pair, security group, user data)
> - You can use a Launch Template to launch instances consistently without re-entering settings each time

---

## 5. Quick Reference — AWS Port Rules (Security Groups)

> When configuring **Inbound Rules** in AWS Security Groups, remember:

- Allow **Port 22** → SSH into Linux EC2
- Allow **Port 3389** → RDP into Windows EC2
- Allow **Port 80** → HTTP web traffic
- Allow **Port 443** → HTTPS web traffic
- Allow **ICMP** → Enable ping (no port needed)

---
