# AWS Auto Scaling Infrastructure: Multi-AZ Self-Healing Deployment

> **System Telemetry Notice:** The comprehensive deployment validations, user-data automation logs, and architecture teardowns are hosted natively on my website.
> 
> **[Read the Full Step-by-Step Lab Write-up Here](https://tahaasim.com)**

---

## 🏗️ Architecture Design Matrix
This infrastructure lab focuses on building a highly resilient, automated server layer. By deploying an Auto Scaling Group (ASG) across separate geographical Availability Zones, the system eliminates any single point of failure and manages its own infrastructure health automatically.

---

## 🔬 Practical Execution Phases

### Phase 1: Infrastructure Blueprinting (Launch Template)
*   **Operating System Image:** Standardized compute nodes using an immutable **Ubuntu Server LTS (64-bit)** platform.
*   **Hardware Profiling:** Selected the cost-efficient `t2.micro` hardware execution layer to optimize testing resource allocation.
*   **Bootstrap Automation:** Embedded an explicit user-data shell script module to automatically install base system dependencies, spin up an **Apache/Nginx web server**, and inject custom environment tags on machine launch.
*   **Access Credentials:** Tied an Amazon EC2 Key Pair array to enforce secure, cryptographic SSH access controls.

### Phase 2: High-Availability Network Mapping
*   **Fault Isolation:** Mapped the infrastructure layout across independent AWS **Availability Zones (AZs)**.
*   **Subnet Distribution:** Configured the underlying networking rules so the system automatically spreads new server nodes across different physical data centers to prevent localized downtime.

### Phase 3: Scaling Boundary Enforcement
*   **Capacity Constraints:** Configured the automated scaling parameters to lock down resource guardrails:
    *   *Minimum Capacity:* 2 Nodes (Guarantees redundant system operations at all times).
    *   *Desired Capacity:* 2 Nodes (Maintains normal baseline workload performance).
    *   *Maximum Capacity:* 4 Nodes (Establishes a burst roof boundary during computational strain).

### Phase 4: Self-Healing Architecture Validation
*   **Integrity Monitoring:** Tracked active node states natively via EC2 system parameters.
*   **Destructive Simulation:** Manually targeted and terminated a healthy, active EC2 server instance from the console layer.
*   **Automated Remediation:** Observed the scaling engine immediately flag the missing capacity, step in to provision a completely new, identical clone, and bring it online to restore the desired state automatically.

---

## 🛠️ Underlying Systems Stack
*   **Cloud Platform:** Amazon Web Services (AWS Management Console Core)
*   **Compute Engine:** Amazon EC2 (Elastic Compute Cloud), AWS Launch Templates
*   **Automation Mechanics:** Auto Scaling Groups (ASG), Linux Bash Shell Scripting
*   **Source Management:** Git Tracking Matrix, Markdown Records
