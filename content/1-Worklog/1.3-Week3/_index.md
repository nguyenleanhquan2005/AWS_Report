---
title: "Week 3 Worklog"
date: "2025-09-23"
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

### Week 3 Objectives:

* Master AWS backup and storage services for data protection and hybrid cloud integration.
* Learn multi-account management and governance with AWS Organizations and Control Tower.
* Understand operations automation and infrastructure as code with Systems Manager and CloudFormation.
* Master core AWS services: EC2, IAM, RDS, Route 53, Auto Scaling, ELB, CloudFront, and S3.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
|:---:|------|:----------:|:---------------:|:-------------------|
| 2 | - The team project topic has been selected: a chatbot using RAG and Bedrock. We are now planning the division of tasks for each project member <br> - Learn AWS Backup for centralized backup management<br>- Master AWS Storage Gateway for hybrid cloud storage<br>- Understand Amazon S3 Glacier for archival storage<br>- Learn AWS DataSync for automated data transfer<br>- **Practice:**<br>&emsp; + Configure backup plans and vaults<br>&emsp; + Deploy File/Volume/Tape Gateway<br>&emsp; + Implement Glacier lifecycle policies<br>&emsp; + Set up DataSync tasks | 09/29/2025 | 09/29/2025 | [AWS Backup](https://000102.awsstudygroup.com/)<br>[Storage Gateway](https://000032.awsstudygroup.com/)<br>[S3 Glacier](https://000074.awsstudygroup.com/)<br>[DataSync](https://000075.awsstudygroup.com/) |
| 3 | - Master AWS Control Tower for multi-account environment<br>- Understand AWS Organizations for centralized management<br>- Learn AWS Service Catalog for standardized provisioning<br>- **Practice:**<br>&emsp; + Set up Control Tower landing zone<br>&emsp; + Configure OUs and guardrails<br>&emsp; + Create Service Catalog portfolios | 09/30/2025 | 09/30/2025 | [Control Tower](https://000063.awsstudygroup.com/)<br>[Organizations](https://000064.awsstudygroup.com/)<br>[Service Catalog](https://000088.awsstudygroup.com/) |
| 4 | - Master AWS Systems Manager for operations management<br>- Understand AWS CloudFormation for infrastructure as code<br>- **Practice:**<br>&emsp; + Configure Session Manager and Run Command<br>&emsp; + Set up Patch Manager and Parameter Store<br>&emsp; + Create CloudFormation stacks and StackSets | 10/01/2025 | 10/01/2025 | [Systems Manager](https://000089.awsstudygroup.com/)<br>[CloudFormation](https://000087.awsstudygroup.com/) |
| 5 | - Learn Amazon EC2 fundamentals and compute services<br>- Master AWS IAM for identity and access management<br>- Understand Amazon RDS for managed databases<br>- Learn Amazon Route 53 for DNS management<br>- **Practice:**<br>&emsp; + Launch EC2 instances with AMIs<br>&emsp; + Configure IAM users, roles, and policies<br>&emsp; + Deploy RDS Multi-AZ instances<br>&emsp; + Set up Route 53 hosted zones | 10/02/2025 | 10/02/2025 | [EC2](https://000012.awsstudygroup.com/)<br>[IAM](https://000030.awsstudygroup.com/)<br>[RDS](https://000044.awsstudygroup.com/)<br>[Route 53](https://000018.awsstudygroup.com/) |
| 6 | - Master AWS Auto Scaling for capacity management<br>- Understand Elastic Load Balancing for traffic distribution<br>- Learn Amazon CloudFront for content delivery<br>- Master Amazon S3 for object storage<br>- **Practice:**<br>&emsp; + Create Auto Scaling groups with policies<br>&emsp; + Configure ALB and NLB<br>&emsp; + Set up CloudFront distributions<br>&emsp; + Implement S3 versioning and replication | 10/03/2025 | 10/03/2025 | [Auto Scaling](https://000111.awsstudygroup.com/)<br>[ELB](https://000026.awsstudygroup.com/)<br>[CloudFront](https://000033.awsstudygroup.com/)<br>[S3](https://000090.awsstudygroup.com/) |

### Week 3 Achievements:
* First step of my project with team, knowing what are we going to do.
* Successfully mastered 17 AWS services covering backup/storage, multi-account governance, operations automation, and core infrastructure.
* Configured AWS Backup with automated schedules, cross-region backup, and disaster recovery strategies.
* Deployed Storage Gateway for hybrid cloud integration with on-premises infrastructure.
* Implemented S3 Glacier lifecycle policies and Vault Lock for regulatory compliance.
* Established Control Tower landing zone with guardrails and Account Factory for multi-account management.
* Configured AWS Organizations with SCPs and consolidated billing for centralized governance.
* Mastered CloudFormation templates, nested stacks, and StackSets for infrastructure as code.
* Implemented Systems Manager for unified operations management including Session Manager, Run Command, and Patch Manager.
* Gained hands-on experience with EC2, IAM, RDS Multi-AZ, Route 53, Auto Scaling, ELB, CloudFront, and S3.

### Notes and Knowledge Gained in Week 3:

#### Core AWS Services
* **EC2:** Compute service with various instance types, AMIs, security groups, EBS volumes, and CloudWatch monitoring.
* **IAM:** Identity and access management with users, groups, roles, policies, MFA, and least privilege principle.
* **RDS:** Managed relational databases with Multi-AZ for high availability, read replicas for scaling, automated backups, and encryption.
* **Route 53:** DNS service with hosted zones, routing policies (simple, weighted, latency, failover), health checks, and alias records.
* **Auto Scaling:** Automatic capacity management with scaling policies (target tracking, step, scheduled), lifecycle hooks, and CloudWatch integration.
* **ELB:** Traffic distribution with ALB (HTTP/HTTPS), NLB (TCP/UDP), target groups, health checks, and SSL/TLS termination.
* **CloudFront:** Global content delivery network with edge locations, cache behaviors, Lambda@Edge, origin failover, and signed URLs.
* **S3:** Object storage with storage classes, versioning, lifecycle policies, replication (CRR/SRR), encryption, and event notifications.
