# Storage and Archiving Labs

This section explores AWS storage options and how they handle **data persistence, durability, performance, and cost**.  
Each lab demonstrates a different storage service, from **block** to **object** to **shared file** systems.

> ⚠️ **Note:** Running these labs may generate AWS charges if you go beyond the Free Tier.  
> Always delete EC2 instances, EBS volumes, snapshots, and S3 objects after finishing to avoid unnecessary costs.

---

## Labs Included

| Service | Description | Link |
|----------|--------------|------|
| **EBS (Elastic Block Store)** | Persistent block storage for EC2, including volume creation, attachment, snapshots, and restoration. | [Go to EBS Lab](./EBS/README.md) |
| **Instance Store** | High-performance ephemeral storage directly attached to EC2 instances. | [Go to Instance Store Lab](./Instance%20Store/README.md) |
| **EFS (Elastic File System)** | Scalable, shared file system accessible by multiple EC2 instances. | [Go to EFS Lab](./EFS/README.md) |
| **S3 (Simple Storage Service)** | Object storage for backups, versioning, and archival with lifecycle management. | [Go to S3 Lab](./S3/README.md) |

---

## Learning Objectives

- Compare AWS storage types: **block**, **file**, and **object**
- Understand **when to use** each storage option
- Practice **mounting**, **snapshotting**, and **restoring** data across services

---

