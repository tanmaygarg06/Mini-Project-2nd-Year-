# Real-Time Stock Dashboard (AWS + Power BI)

This project is a **stock analytics dashboard** that stores stock data on AWS and visualizes it in **Microsoft Power BI**.  
All backend and hosting are built on the **AWS Free Tier**, using only:

- **Amazon DynamoDB** – stock data storage  
- **AWS Lambda** – serverless data processing  
- **Amazon Cognito** – user authentication (login/sign‑up)  
- **Amazon S3** – static website hosting for the project site  
- **IAM** – permissions and security for AWS resources  

Power BI is used only for the dashboard visuals (reports and charts).

---

## 🚀 Features

- Sample stock dataset for multiple symbols (AAPL, MSFT, TSLA, AMZN, GOOGL, NFLX, META, etc.).
- Data stored in **DynamoDB** with `StockSymbol`, `Timestamp`, and `Price`.
- **Lambda** functions to:
  - Insert/seed stock data into DynamoDB.
  - Read data and export it (for Power BI import or refresh).
- **Cognito‑protected project site** hosted on **S3**:
  - Users can sign up, log in, and see project info / links.
- Power BI report showing:
  - Price vs time line charts.
  - Current price tables.
  - Filters by stock symbol and date range.
- Architecture designed so the dataset can be periodically refreshed for near real‑time updates.

---

## 🏗 Architecture (AWS + Power BI)

1. **DynamoDB – `StockData` table**

   - Partition key: `StockSymbol` (String)  
   - Sort key: `Timestamp` (Number, epoch)  
   - Attribute: `Price` (Number)

2. **AWS Lambda**

   - **Ingest function** – batch‑loads sample stock data (e.g., from `batch.json`) into DynamoDB.  
   - **Export function** – scans the table and writes the result to a CSV/JSON file in S3 that Power BI can read or that you can download manually.

3. **Amazon Cognito**

   - User Pool for authentication.  
   - Frontend site requires login before showing links to the Power BI dashboard and dataset.

4. **Amazon S3 (Static Website)**

   - Hosts the simple project website (`index.html`, `style.css`, `app.js`).  
   - Site explains the project and provides the Power BI report link.  
   - Cognito JavaScript SDK is used on the site for login/sign‑up.

5. **Power BI**

   - Imports/exported data (CSV/JSON) from S3 or local download.  
   - Builds the main dashboard with charts, KPIs, and filters.

---


## 🌐 Live Project Links

Replace these placeholders with your actual URLs:

- **Project site (S3 static hosting):**  
  https://ap-south-1l2tsdlsns.auth.ap-south-1.amazoncognito.com/login?client_id=298qto8mkdcq69laah8ig543hj&response_type=code&scope=email+openid+phone&redirect_uri=https%3A%2F%2Fstockdashboardtanmay.s3.ap-south-1.amazonaws.com%2Fdashboard.html
- **Power BI report:**  
  https://app.powerbi.com/groups/me/reports/ddad24c3-606e-452a-8289-d076fa239014/cc060d6d35cec6e80d23?experience=power-bi

---

## 🔮 Future Scope

- Schedule the **export Lambda** to run periodically (e.g., every 5 minutes) using **EventBridge** to keep the Power BI dataset fresh.  
- Extend Cognito integration on the Power BI side (row‑level security using user attributes).  
- Implement simulated buy/sell flows in Lambda and record user portfolios in DynamoDB.

---

## 📝 Notes

This project uses **only** these AWS services:

- **IAM** – secure roles for Lambda, DynamoDB, S3, Cognito  
- **DynamoDB** – stock data storage  
- **Lambda** – batch ingest + export logic  
- **Cognito** – user authentication  
- **S3** – static hosting for the project landing page  

Power BI handles all charting and dashboard visualization on top of the exported dataset.
