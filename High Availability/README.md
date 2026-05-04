# High Availability on AWS

High availability (HA) ensures that your application remains accessible even if some of its components fail. On AWS, this is typically achieved using **Elastic Load Balancing (ELB)** and **Auto Scaling Groups (ASG)**.

## Key Concepts

### Elastic Load Balancing (ELB)
Elastic Load Balancing automatically distributes incoming application traffic across multiple targets, such as Amazon EC2 instances, containers, IP addresses, and Lambda functions. It can handle the varying load of your application traffic in a single Availability Zone or across multiple Availability Zones.

- **Application Load Balancer (ALB):** Best suited for load balancing of HTTP and HTTPS traffic and provides advanced request routing targeted at the delivery of modern application architectures, including microservices and containers.
- **Network Load Balancer (NLB):** Best suited for load balancing of Transmission Control Protocol (TCP), User Datagram Protocol (UDP), and Transport Layer Security (TLS) traffic where extreme performance is required.
- **Gateway Load Balancer (GWLB):** Best suited for deploying, scaling, and managing third-party virtual appliances.

### Auto Scaling Groups (ASG)
An Auto Scaling group contains a collection of Amazon EC2 instances that are treated as a logical grouping for the purposes of automatic scalability and management.

- **Scaling Out:** Adding more instances to the group to handle increased load.
- **Scaling In:** Removing instances from the group when demand decreases.
- **Self-Healing:** If an instance becomes unhealthy, ASG automatically terminates it and launches a new one to maintain the desired capacity.

---

## 📂 Labs
1. [**Lab 1: Application Load Balancer**](./Lab%201%20-%20Application%20Load%20Balancer/README.md) - Learn how to distribute traffic between two web servers.
2. [**Lab 2: Auto Scaling Groups**](./Lab%202%20-%20Auto%20Scaling%20Groups/README.md) - Learn how to automatically scale and recover EC2 instances.
