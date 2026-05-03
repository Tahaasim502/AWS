# 🖥️ Lab 01 — Apache vs Nginx Setup from Scratch

 **Environment:** AWS EC2 (Amazon Linux 2023, t3.micro) **Status:** ✅ Completed

---

## 📌 Tags

`#AWS` `#EC2` `#Linux` `#Apache` `#Nginx` `#WebServer` `#DevOps` `#Lab`

---

## 1. 🌍 Real-World Motivation

Every application on the internet needs a **web server** to receive requests and serve responses. Apache and Nginx are the two most dominant web servers in production environments globally.

|Use Case|Where You'll See This|
|---|---|
|Hosting company websites|Apache/Nginx on EC2 or bare metal|
|Reverse proxy for microservices|Nginx in front of Node.js, Flask apps|
|Load balancing|Nginx distributing traffic across instances|
|DevOps pipelines|Nginx serving built React/Angular apps|

> As a Security/DevOps Engineer, you will configure, troubleshoot, and secure these servers constantly.

---

## 2. 🏗️ Architecture Overview

```
User Browser
     |
     | HTTP Request (Port 80)
     ▼
[EC2 Instance - Public IP]
     |
     ▼
[Security Group - Inbound Port 80 Open]
     |
     ▼
[Web Server - Apache or Nginx]
     |
     ▼
[/var/www/html/index.html]  ← Apache
[/usr/share/nginx/html/index.html]  ← Nginx
     |
     ▼
Response sent back to browser
```

**Key Concept:** Only one web server can use Port 80 at a time. Running both requires stopping one before starting the other.

---

## 3. 🔧 Step-by-Step Setup

### Prerequisites

- EC2 instance running (Amazon Linux 2023)
- SSH access via key pair
- Port 80 open in Security Group (Inbound)

---
### 3.1 — Apache Setup

**Install Apache:**

```bash
yum update -y
yum install -y httpd
```

**Start and enable Apache:**

```bash
systemctl start httpd
systemctl enable httpd
```

**Deploy a webpage:**

```bash
echo "<h1>First Apache Server</h1>" > /var/www/html/index.html
```

**Verify:** Open browser → `http://<your-public-ip>`

---

### 3.2 — Switch to Nginx

**Stop Apache first (port conflict):**

```bash
systemctl stop httpd
```

**Install Nginx:**

```bash
yum install -y nginx
```

**Start and enable Nginx:**

```bash
systemctl start nginx
systemctl enable nginx
```

**Deploy a webpage:**

```bash
echo "<h1>First Nginx Server</h1>" > /usr/share/nginx/html/index.html
```

**Verify:** Open browser → `http://<your-public-ip>`

---

## 4. ✅ Testing & Verification

|Test|Command|Expected Result|
|---|---|---|
|Check service status|`systemctl status httpd` or `systemctl status nginx`|`active (running)`|
|Check port 80 in use|`ss -tlnp \| grep :80`|Shows httpd or nginx|
|Test from terminal|`curl http://localhost`|Returns your HTML|
|Test from browser|`http://<public-ip>`|Page loads|

---

## 5. ❌ Common Errors & Fixes

|Error|Cause|Fix|
|---|---|---|
|Port 80 conflict|Apache and Nginx both trying to use port 80|`systemctl stop httpd` before starting nginx|
|Page not loading in browser|Port 80 not open in Security Group|Add inbound HTTP rule in AWS console|
|`systemctl start nginx` fails|Port already in use|Check with `ss -tlnp \| grep :80`|
|Nginx shows default page after echo|Wrong file path used|Apache uses `/var/www/html/`, Nginx uses `/usr/share/nginx/html/`|
|IMDSv2 curl returns nothing|Old metadata endpoint used|Use token-based IMDSv2 method (see script above)|

---

## 6. 🧠 Key Takeaways

- `systemctl` manages services — start, stop, restart, enable, disable
- Only **one service can bind to port 80** at a time
- Apache and Nginx serve files from **different directories**
- **Port 80 = HTTP, Port 443 = HTTPS** — always open correct ports in Security Group
---

## 7. ⚖️ Apache vs Nginx — When to Use Which

|Feature|Apache|Nginx|
|---|---|---|
|Architecture|Process/thread per request|Event-driven, async|
|Performance|Good for low-medium traffic|Better for high concurrency|
|Config style|`.htaccess` per directory|Centralized config|
|Best for|Dynamic content, PHP apps|Static files, reverse proxy, load balancing|
|Memory usage|Higher|Lower|
|Industry use|Traditional hosting, WordPress|Modern microservices, CDN, proxy|

> **Rule of thumb:** Nginx for performance and proxying, Apache for legacy or PHP-heavy apps.

---
## 8. Lets look how to setup an Apache Server

First click on All-Services and then use EC2(the server in the cloud )

![[Pasted Image 20260503121305_962.png]]   

After clicking on EC2, click on launch instance then name the VM and OS image, and make sure 
to allow both HTTP traffic, if not you will have to configure it in the security group.

![[Pasted image 20260503121625.png]]

Once the instance has launched you can connect to the CLI amazon Linux

![[Pasted image 20260503121900.png]]

Switch to root user and then update all the packages using yum that is used for amazon linux just like apt in Ubuntu, after that we install the apache server using httpd(apche server).

![[Pasted image 20260503122254.png]]

Started and enabled the server and wrote few commands to be displayed on the website.

![[Pasted image 20260503122438.png]]

Apache Server has been created,
In order to access it you to access it using the public IP address, that can be seen on your instance.

![[Pasted image 20260503122527.png]]

