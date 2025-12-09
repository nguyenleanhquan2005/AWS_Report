---
title: "Module 3: Developing Serverless Backend"
date: "2025-09-09"
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---

## CONNECT DATA

#### Step 1: New Dataset

1. In the QuickSight console, choose **Datasets** from the left navigation pane.
2. Choose **New dataset**.
3. Under **FROM NEW DATA SOURCES**, choose **Upload a file**.
4. Select the `sales_data.csv` file you created in the previous step.

#### Step 2: Preview and Edit

1. After the file uploads, QuickSight will show a preview.
2. Choose **Edit settings and prepare data**.
3. Here you can see the columns: `Date`, `Region`, `Product`, `Sales`, `Quantity`.
4. Ensure `Sales` and `Quantity` are detected as numbers (Integers or Decimals).
5. Ensure `Date` is detected as a Date type.
6. Choose **Save & Publish** at the top right.
7. Choose **Cancel** to go back to the main screen, or directly click **Visualize** if prompted.

---

### Next Steps
Proceed to [Module 4: API & Security](../5.4-s3-onprem/) to expose these functions via API Gateway with authentication.
