# AWS Observability Infrastructure: CloudWatch Metric Alarms & Dynamic Scaling Policies

> **Performance Monitoring Notice:** The complete system architecture maps, load simulation metrics, and live CloudWatch dashboard charts are hosted natively on my personal portfolio website.
> 
> 👉 **[Read the Full Step-by-Step Lab Write-up Here](https://tahaasim.com)**

---

## 🏗️ Observability Architecture Flow
This infrastructure lab injects telemetry and automated logic parameters into our high-availability cluster. By linking live CloudWatch monitoring alerts directly to automated Target Tracking Scaling Policies, the computing environment dynamically scales itself up or down to handle heavy workloads before any application performance drops happen.

---

## 🔬 Practical Execution Phases

### Phase 1: Alarm Threshold Automation (CloudWatch Setup)
*   **Metric Isolation:** Configured an explicit infrastructure monitor tracking the `Application Load Balancer Request Count Per Target` parameter.
*   **Threshold Boundary:** Built a custom data condition rule that automatically moves the system state into an alert phase if any single compute node processes more than 50 requests within a tight validation evaluation window.

### Phase 2: Elastic Target Tracking Policies
*   **Scale-Out Automation:** Tied the Auto Scaling engine directly to the CloudWatch tracking stream.
*   **Resource Guardrails:** Modified capacity limits to provide a safe burst workspace under heavy stress:
    *   *Minimum Capacity:* 2 Nodes (Baseline protection).
    *   *Desired Capacity:* 2 Nodes (Normal operations).
    *   *Maximum Capacity:* 4 Nodes (Maximum scale-out roof buffer).

### Phase 3: Hardware Stress Simulation (AWS CloudShell CLI)
*   **Load Injection Loop:** Launched an active **AWS CloudShell CLI** container terminal.
*   **Strain Automation Script:** Executed continuous background processing bash strings designed to forcefully hammer the load balancer DNS endpoint and mimic a sudden massive spike in concurrent user traffic.
*   **Observability Tracking:** Monitored the console interface closely to observe the exact moment the metric breached the boundary, automatically moving the alert state from `OK` to `ALARM` in under 3 minutes.

### Phase 4: Dynamic Scale-Out & Cool-Down Validation
*   **Horizontal Expansion:** Successfully verified that the high-availability cluster caught the alert signal and automatically provisioned 2 additional, identical EC2 nodes (scaling up from 2 to 4) to distribute the traffic load.
*   **Resource Scale-In Stabilization:** Terminated the manual stress simulation scripts and observed CloudWatch track the traffic drop, safely triggering the scale-in policy to terminate the extra instances back to the baseline profile without causing downtime.

---

## 🛠️ Underlying Systems Stack
*   **Cloud Platform:** Amazon Web Services (AWS Management Console Core)
*   **Observability & Telemetry:** Amazon CloudWatch Metrics, CloudWatch Metric Alarms
*   **Traffic Routing & Fleet:** Application Load Balancer (ALB), Auto Scaling Groups (ASG), Amazon EC2
*   **Automation Platforms:** AWS CloudShell Interface, Bash Shell Loop Automation Scripting

