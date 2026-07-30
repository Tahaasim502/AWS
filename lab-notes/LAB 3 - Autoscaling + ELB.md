# AWS Resilient Routing: Application Load Balancer (ALB) with Auto Scaling Integration

> **Network Routing Notice:** The complete system architecture maps, connection distribution logs, and step-by-step health check validations are hosted natively on my personal portfolio website.
> 
> **[Read the Full Step-by-Step Lab Write-up Here](https://tahaasim.com)**

---

## 🏗️ Architecture Design Matrix
This infrastructure lab integrates an intelligent, application-layer routing node with our existing elastic compute fleet. By sitting an Application Load Balancer (ALB) in front of the Auto Scaling Group (ASG), the system securely masks individual instance IPs, manages inbound connection paths, and enforces seamless high-availability balancing.

---

## 🔬 Practical Execution Phases

### Phase 1: Target Fleet Aggregation (Target Group Configuration)
*   **Logical Grouping:** Created a dedicated target framework (labeled `TG1`) to logically index instances provisioned dynamically by our scaling group.
*   **Protocol Hardening:** Initialized tracking rules exclusively matching standard `HTTP:80` traffic routes.
*   **Proactive Health Check Parameters:** Built automated scanning parameters mapping straight to the root directory path (`/`) to evaluate node integrity using tight boundaries:
    *   *Healthy Threshold:* 3 consecutive successful ping matches before a node is routed live.
    *   *Unhealthy Threshold:* 2 consecutive failures to instantly isolate and quarantine a broken instance.

### Phase 2: Router Layer Provisioning (Application Load Balancer)
*   **Boundary Enforcement:** Deployed an internet-facing **Application Load Balancer (ALB)** acting as the single, public-facing gateway for the system architecture.
*   **Network Mapping:** Anchored the routing nodes securely across multiple geographical Availability Zones (AZs) to prevent routing failure points.
*   **Listener Logic:** Attached an active tracking rule listening for inbound `HTTP` traffic, configured to instantly forward requests directly down to our active Target Group (`TG1`).

### Phase 3: Elastic Attachment & Mapping Integration
*   **Infrastructure Binding:** Re-configured the downstream **Auto Scaling Group (ASG)** parameters to safely hook directly into the new load-balanced Target Group.
*   **Health Parameter Handshake:** Swapped default EC2 basic instance checks for **ALB Health Checks**. This forces the scaling group to terminate and replace a machine not only if the hardware breaks, but also if the web server software (Apache/Nginx) crashes.

### Phase 4: Round-Robin Distribution Verification
*   **DNS Access Routing:** Grabbed the public, fully-qualified DNS entry URL automatically assigned to the ALB.
*   **Traffic Balancing Check:** Ran continuous browser refresh sequences hitting the load balancer endpoint.
*   **System Validation:** Successfully verified real-time round-robin traffic distribution as the page content dynamically toggled back and forth, showcasing connection handshakes splitting perfectly between separate compute nodes.

---

## 🛠️ Underlying Systems Stack
*   **Cloud Platform:** Amazon Web Services (AWS Management Console Core)
*   **Traffic Routing Elements:** Application Load Balancer (ALB), Target Groups, HTTP Listeners
*   **Elastic Compute Fleet:** Amazon EC2, Multi-AZ Auto Scaling Groups (ASG)
*   **Foundations & Tooling:** Linux Systems Administration, Bash CLI Scripting, Markdown Docs

