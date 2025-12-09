---
title: "Module 2: Building Knowledge Base"
date: "2025-09-09"
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---

### Building the Knowledge Base (Data Layer)

#### Step 1: Sign up for Amazon QuickSight
1. Log in to the AWS Management Console.
2. Search for **QuickSight**.
3. If you have not signed up yet, you will be prompted to do so.
4. Choose **Sign up for QuickSight**.
5. Select the **Enterprise** edition (it offers a free trial).
6. Follow the on-screen instructions:
   - **Authentication method**: Use IAM federated identity & QuickSight-managed users (default).
   - **Region**: Select **US East (N. Virginia)**.
   - **QuickSight account name**: Enter a unique name.
   - **Notification email**: Enter your email.
   - **Allow access**: You can leave the defaults or uncheck them if we are only uploading a file. For this workshop, we don't strictly need S3 access, but it's good practice to leave S3 checked if you plan to use it later.
7. Click **Finish**.

#### Step 2: Prepare Sample Data

We will use a simple CSV file for this workshop.

1. Copy the following data and save it as a file named `sales_data.csv` on your computer:

```csv
Date,Region,Product,Sales,Quantity
2023-01-01,North,Laptop,1200,2
2023-01-02,South,Phone,800,5
2023-01-03,East,Tablet,400,3
2023-01-04,West,Laptop,1200,1
2023-01-05,North,Phone,800,2
2023-01-06,South,Tablet,400,4
2023-01-07,East,Laptop,1200,3
2023-01-08,West,Phone,800,6
2023-01-09,North,Tablet,400,2
2023-01-10,South,Laptop,1200,4
```

Alternatively, you can use any CSV file you have, but the instructions will follow this structure.

---

### Next Steps
Proceed to [Module 3: Developing Serverless Backend](../5.3-s3-vpc/) to build Lambda functions that automate ingestion and handle chat queries.
