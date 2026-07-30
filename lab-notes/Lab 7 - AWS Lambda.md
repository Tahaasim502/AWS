# AWS Event-Driven Microservices: Asynchronous Message Processing via Lambda & SQS

> **Engineering Logs Notice:** The complete JavaScript handler functions, queue parameter schemas, and live execution runtime log traces are hosted natively on my personal portfolio website.
> 
> 👉 **[Read the Full Step-by-Step Lab Write-up Here](https://tahaasim.com)**

---

## 🏗️ Serverless Microservices Architecture
This infrastructure lab implements a fundamental cloud production standard: an asynchronous, completely decoupled event-driven data pipeline. By isolating heavy or unpredictable application data traffic behind an elastic queue layer, the architecture guarantees zero data loss and completely eliminates component bottlenecks between backend layers.

---

## 🔬 Practical Execution Phases

### Phase 1: Message Pipeline Instantiation (Amazon SQS Setup)
*   **Queue Framework Matrix:** Created a highly resilient, standard **Simple Queue Service (SQS)** communications lane.
*   **Buffer Parameter Optimization:** Tailored message retention boundaries, visibility timeouts, and delivery delays to safely shield downstream compute processing components.

### Phase 2: Serverless Compute Configuration (AWS Lambda Setup)
*   **Blueprint Infrastructure:** Provisioned an isolated, serverless **AWS Lambda function** running a performant Node.js runtime environment.
*   **Least-Privilege Authorization:** Attached strict AWS IAM execution rules, granting the compute node secure permissions to poll, read, and delete items from the queue stream while blocking all unrelated cloud tables.

### Phase 3: Event Source Trigger Mapping
*   **Batch Data Polling:** Configured an explicit Event Source Mapping matrix connecting our queue directly to the serverless function.
*   **On-Demand Compute Automation:** The architecture automatically initializes the Lambda worker only when an active message lands inside the queue, ensuring absolute zero computing costs while idle.

### Phase 4: End-to-End Pipeline Transmission Validation
*   **Payload Injection:** Dispatched structural string data packages (`"Test message 1"` and `"Test message 2"`) into your active SQS queue console lane.
*   **Observability Logging Analysis:** Inspected the **CloudWatch Log Stream** lines to verify that the serverless Node.js handler woke up instantly, parsed the batch string array payload accurately, logged the execution outputs, and spun down cleanly.

---

## 🛠️ Underlying Systems Stack
*   **Cloud Platform:** Amazon Web Services (AWS Management Console Core)
*   **Serverless Compute Engine:** AWS Lambda (Node.js Container Runtime)
*   **Message Orchestration Arrays:** Amazon Simple Queue Service (SQS) Standard
*   **System Observability Tracking:** Amazon CloudWatch Logs Engine