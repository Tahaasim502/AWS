# AWS Storage — EBS, EFS & S3 (Object Storage)

> **Course:** Corvit DevOps | **Topic:** AWS Storage Types | **Tags:** #aws #storage #ebs #efs #s3 #devops

---

## 1. AWS Storage Overview

| Storage Type | Service | Use Case |
|---|---|---|
| Block Storage | EBS (Elastic Block Store) | OS disks, databases, single EC2 |
| File Storage | EFS (Elastic File System) | Shared storage across multiple EC2 |
| Object Storage | S3 (Simple Storage Service) | Files, images, backups, static websites |

---

## 2. EBS — Elastic Block Store (Block Storage)

### 2.1 What is Block Storage?
- **Fixed storage size** — you define the size upfront (e.g. 8 GiB)
- Acts like a **hard drive attached to your EC2 instance**
- One EBS volume → can only attach to **one EC2 instance** at a time
- Locked to **one Availability Zone** — cannot move across AZs directly

### 2.2 Key EBS Metrics

#### IOPS (Input/Output Operations Per Second)
- How fast the disk can **read/write data**
- Higher IOPS = faster performance
- Critical for **databases** and high-read/write apps

#### Bandwidth
- Default: **125 MB/s** (gp3 volume type)
- How much data can flow through the disk per second
- Think of it as the **width of a pipe**

### 2.3 MiB vs MB

| Unit | Full Name | Base | Value |
|---|---|---|---|
| MiB | Mebibyte | Base 2 (2²⁰) | 1,048,576 bytes |
| MB | Megabyte | Base 10 (10⁶) | 1,000,000 bytes |

- **1 MiB = 1.04878 MB**
- **AWS uses MiB** → accurate representation of how RAM and hardware work at binary level
- **MB** = used in networking (internet standard, base 10)

> **MiB** = Accurate representation of how data is stored on RAM/hardware  
> **MB** = Networking/internet standard

### 2.4 EBS Storage Capacity
- Maximum EBS volume size: **64 TiB**

### 2.5 EBS Snapshots
- **Point-in-time backup** of an EBS volume
- Stored in **S3** (region-wide, not AZ-locked)
- Use cases:
  - Transfer data between AZs or Regions
  - Disaster recovery
  - Clone data to a new instance

```
EBS Volume (AZ-locked) → Snapshot (Region-wide) → New Volume (any AZ)
```

> **Why use snapshots instead of sharing the same volume?**  
> EBS can only attach to one instance at a time. Snapshots let you copy data to a new volume and attach it to another instance independently.

### 2.6 EBS Volume Types

| Type | Full Name | Use Case |
|---|---|---|
| gp3 | General Purpose SSD | Default — balanced performance |
| io2 | Provisioned IOPS SSD | High-performance databases |
| st1 | Throughput Optimized HDD | Big data, log processing |
| sc1 | Cold HDD | Infrequent access, lowest cost |

---

## 3. EFS — Elastic File System (File Storage)

### 3.1 What is EFS?
- **Shared file system** — multiple EC2 instances can access it simultaneously
- Works across **multiple Availability Zones**
- Only for **Linux** instances (NFS protocol)
- For Windows → use **FSx** (File System Extensible)

### 3.2 EFS vs EBS

| Feature | EBS | EFS |
|---|---|---|
| Instances | 1 EC2 at a time | Multiple EC2 simultaneously |
| AZ Support | Single AZ | Multi-AZ |
| OS Support | Linux + Windows | Linux only |
| Size | Fixed (you set it) | Elastic (auto-scales) |
| Use Case | OS disk, database | Shared files, content management |

### 3.3 How EFS Works
```
EC2 (AZ-1) ──┐
EC2 (AZ-2) ──┼──→ EFS (filesystem-1) ← All instances see the same files
EC2 (AZ-3) ──┘
```

- Create a file on one instance → instantly visible on all others
- Delete two instances → files still exist on EFS (data persists independently)

### 3.4 EFS Mount Point
- Default mount path: `/mnt/efs/fs1`
- AWS auto-configures this when you attach EFS during EC2 launch

### 3.5 EFS Pricing
- **No minimum fee** — pay only for storage used
- More expensive than EBS per GB — but only pay for what you use
- **Delete EFS after lab** to avoid charges

---

## 4. S3 — Simple Storage Service (Object Storage)

### 4.1 What is Object Storage?
- Stores data as **objects** (files) in **buckets** (containers)
- Each object = file + metadata + unique key
- No folder structure (flat) — folders are just key prefixes
- Accessed via **HTTP/HTTPS URL** — not mounted like a disk

### 4.2 S3 Key Concepts

| Term | Meaning |
|---|---|
| Bucket | Container for objects (files) |
| Object | Any file stored in S3 |
| Key | Unique name/path of the object |
| Region | Bucket is created in one region |
| ARN | Unique identifier e.g. `arn:aws:s3:::my-bucket` |

> **Bucket names are globally unique** — no two buckets across all AWS accounts can share the same name.

### 4.3 S3 Access Control

#### Block Public Access
- By default, all S3 buckets are **private**
- Must explicitly unblock public access for static websites

#### Bucket Policy (JSON)
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowGetObjectAccess",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::YOUR_BUCKET_NAME/*"
    }
  ]
}
```
- `Principal: "*"` → anyone on the internet
- `Action: s3:GetObject` → read/download files
- `Resource: /*` → applies to all objects in the bucket

### 4.4 S3 Static Website Hosting
- Host HTML, CSS, JS files directly from S3 — **no EC2 server needed**
- Enable under: **Properties → Static website hosting → Enable**
- Set **Index document** to `index.html`
- Website URL format:
```
http://BUCKET-NAME.s3-website.REGION.amazonaws.com
```

> **Important:** Use just the filename in `<img src>` — not your local path  
> ✅ `<img src="image.jpg">` → correct  
> ❌ `<img src="C:\Users\tahaa\Desktop\image.jpg">` → only works locally

### 4.5 S3 Versioning
- Keeps **multiple versions** of the same object
- If one version is corrupted/deleted → restore previous version
- Enable under: **Properties → Bucket Versioning → Enable**
- Use case: Rollback a bad website deployment, accidental file deletion

### 4.6 S3 Storage Classes

| Class | Use Case | Cost |
|---|---|---|
| Standard | Frequently accessed data | Highest |
| Standard-IA | Infrequently accessed | Lower |
| Glacier | Archival, rarely accessed | Lowest |
| Intelligent-Tiering | Unknown access patterns | Auto |

### 4.7 S3 vs EBS vs EFS Summary

| Feature | S3 | EBS | EFS |
|---|---|---|---|
| Type | Object | Block | File |
| Access | HTTP URL | Mounted disk | Mounted filesystem |
| Multi-instance | ✅ Yes | ❌ No (1 at a time) | ✅ Yes |
| Multi-AZ | ✅ Yes | ❌ No | ✅ Yes |
| OS Required | No | Yes | Yes (Linux) |
| Use Case | Files, websites, backups | EC2 disk, DB | Shared storage |
| Pricing | Per GB stored | Per GB provisioned | Per GB used |

---

## 5. Quick Reference — When to Use What

```
Need a disk for your EC2?           → EBS
Need to share files across servers? → EFS
Need to store files/images/backups? → S3
Need to host a static website?      → S3 Static Hosting
Need to run a dynamic website?      → EC2 + EBS
```

---