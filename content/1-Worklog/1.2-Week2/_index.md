---
title: "Week 2 Worklog"
date: "2025-09-15"
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---

### Week 2 Objectives:

* Master VPCs, hybrid connectivity, and edge networking.
* Understand network monitoring, security, and content delivery.
* Learn advanced networking scenarios including VPC Peering, Transit Gateway, and Site-to-Site VPN.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
|:---:|------|:----------:|:---------------:|:-------------------|
|  2  | - VPC Essentials: <br>&emsp; + Create VPC, Subnets (Public/Private) <br>&emsp; + Configure Route Tables and Internet Gateways <br>&emsp; + Workshop: Networking on AWS (Part 1) | 15/09/2025 | 15/09/2025 | [AWS VPC Workshop](https://000003.awsstudygroup.com/) |
|  3  | - Network Monitoring & Security: <br>&emsp; + Enable VPC Flow Logs <br>&emsp; + Configure AWS WAF (Web Application Firewall) <br>&emsp; + Set up VPC Endpoints (Gateway & Interface) | 16/09/2025 | 16/09/2025 | [VPC Flow Logs](https://000092.awsstudygroup.com/)<br>[AWS WAF](https://000074.awsstudygroup.com/)<br>[VPC Endpoints](https://000026.awsstudygroup.com/) |
|  4  | - Connectivity: <br>&emsp; + Configure VPC Peering <br>&emsp; + Explore AWS Transit Gateway <br>&emsp; + Manage DNS with Amazon Route 53 | 17/09/2025 | 17/09/2025 | [VPC Peering](https://000019.awsstudygroup.com/)<br>[Transit Gateway](https://000020.awsstudygroup.com/)<br>[Route 53 Hybrid DNS](https://000010.awsstudygroup.com/) |
|  5  | - Content Delivery: <br>&emsp; + Create CloudFront Distribution <br>&emsp; + Implement Lambda@Edge for dynamic routing <br>&emsp; + Configure Route 53 Geolocation Routing | 18/09/2025 | 18/09/2025 | <https://000130.awsstudygroup.com/> |
|  6  | - Practice: Advanced VPC Scenarios <br>&emsp; + Simulate Site-to-Site VPN connection | 19/09/2025 | 19/09/2025 | [Networking Workshop](https://000003.awsstudygroup.com/) |

### Week 2 Achievements:

* Successfully created VPCs with public and private subnets, route tables, and internet gateways.
* Configured VPC Flow Logs for network traffic monitoring and troubleshooting.
* Implemented AWS WAF to protect web applications from common exploits.
* Set up VPC Endpoints (Gateway and Interface) for private connectivity to AWS services.
* Configured VPC Peering for secure communication between VPCs.
* Explored AWS Transit Gateway for centralized network connectivity across multiple VPCs.
* Mastered Route 53 DNS management including geolocation routing policies.
* Created CloudFront distributions for global content delivery with low latency.
* Implemented Lambda@Edge for dynamic content routing at edge locations.
* Practiced advanced VPC scenarios and simulated Site-to-Site VPN connections.

### Notes and Knowledge Gained in Week 2:

#### 1. VPC Essentials
* **VPC Components:**
    * **Subnets:** Public subnets have routes to Internet Gateway, private subnets do not.
    * **Route Tables:** Control traffic routing within VPC and to external networks.
    * **Internet Gateway:** Enables communication between VPC and the internet.
    * **NAT Gateway:** Allows private subnet instances to access internet without exposing them.
* **CIDR Planning:**
    * Choose appropriate CIDR blocks to avoid IP conflicts with on-premises networks.
    * Plan for future growth when allocating subnet ranges.
* **Best Practices:**
    * Use private subnets for databases and backend servers.
    * Place load balancers and bastion hosts in public subnets.
    * Implement multiple availability zones for high availability.
