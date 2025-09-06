# Crypto-Data-Analysis-Near-Realtime-Data-Pipeline

## 📌 Project Overview
This project implements a **near real-time, event-driven data pipeline** for analyzing crypto transaction data.  
The pipeline leverages **AWS (Kinesis, Lambda, Firehose, S3, Glue, Athena, QuickSight)** and **Apache Hudi** to capture **CDC-enabled DynamoDB streams**, transform them, and deliver **crypto trend dashboards refreshed every 15 minutes**.

---

## 🏗️ Architecture Diagram
<img width="1440" height="900" alt="Screenshot 2025-09-05 at 7 24 41 PM" src="https://github.com/user-attachments/assets/4e2d7978-505d-40e5-8b40-c9cd65af5ced" />


---

## 🔑 Key Features
- **Real-Time Data Ingestion** – Captures **CDC-enabled DynamoDB streams** using **Kinesis Data Streams + Firehose** into S3.  
- **Serverless Processing** – AWS **Lambda** transforms raw data in-flight before landing in S3.  
- **Incremental ETL with Apache Hudi** – AWS **Glue ETL jobs** perform **upserts** and schema evolution on S3-backed Hudi tables.  
- **Schema Management** – **Glue Crawlers & Catalog** automate table/schema detection.  
- **Analytics & Visualization** – **Athena** queries over Hudi tables power **QuickSight dashboards**, refreshed every 15 min.  
- **Monitoring** – End-to-end monitoring with **CloudWatch metrics, Glue triggers, and Kinesis throughput alerts**.  
- **Scalable & Cost-Effective** – Serverless and cloud-native, scales with crypto transaction volume.  

---

## 🛠️ Tools and Technologies Used

| Component         | Technology Used |
|-------------------|-----------------|
| Data Ingestion    | DynamoDB Streams, Kinesis, Firehose |
| Processing        | AWS Lambda, AWS Glue (PySpark) |
| Storage           | S3 (Raw, Processed Layers), Apache Hudi |
| Query Engine      | AWS Athena |
| Visualization     | AWS QuickSight |
| Monitoring        | CloudWatch, Glue Triggers |
| Security          | IAM Roles, Encryption, Least Privilege Access |

---

## 📂 Project Steps

### **Step 1: Ingesting Data**
- DynamoDB CDC Streams → **Kinesis Data Streams** → **Kinesis Firehose** → **Amazon S3 (Raw Layer)**.

---

### **Step 2: Real-Time Transformation (AWS Lambda)**

The Lambda function makes the DynamoDB stream data ready for analytics:

1. **Decode the stream record** – Converts the base64-encoded DynamoDB event into normal JSON.  
2. **Clean the data** – Changes DynamoDB’s special JSON format into standard JSON (strings, numbers, booleans, lists, maps).  
3. **Add useful info** – Attaches the event type (INSERT / MODIFY / REMOVE) and event ID for tracking.  

Finally, it sends the cleaned and enriched JSON back to Firehose in a line-by-line format so Glue, Athena, and QuickSight can process it easily.

---

### **Step 3: Schema & ETL with AWS Glue**
- **Glue Crawler** detects schema and registers tables in **Glue Catalog**.  
- **Glue ETL Jobs (PySpark + Hudi)** perform:  
  - **Incremental upserts** (avoid duplicates, only update changed records).  
  - **Schema evolution** (auto-handle new fields).  
  - Store processed data in **S3 (Hudi tables)**.  

---

### **Step 4: Query with Athena**
- Athena queries directly over Hudi tables stored in S3.  
- Supports **incremental queries** without scanning entire datasets.  

---

### **Step 5: Visualization in QuickSight**
- Athena tables are connected to **QuickSight dashboards**.  
- Dashboards **refresh every 15 minutes** to show real-time crypto market trends.  

<img width="1181" height="773" alt="Screenshot 2025-09-06 at 12 22 27 AM" src="https://github.com/user-attachments/assets/36979c50-d5bd-47d6-973f-fb2b258f71dd" />


---

## 🔄 Architecture Flow

1️⃣ **DynamoDB Streams** capture crypto CDC data.  
2️⃣ **Kinesis Data Streams + Firehose** → push to **S3 (Raw Layer)**.  
3️⃣ **Lambda** processes raw events (decode → clean → enrich).  
4️⃣ **Glue Crawler + ETL (Hudi)** → incremental upserts → **S3 Processed Layer**.  
5️⃣ **Athena** queries → real-time analysis.  
6️⃣ **QuickSight dashboards** → refreshed every 15 minutes.  
7️⃣ **CloudWatch Monitoring** → alerts on pipeline health.  

---

## 🔒 Security Measures
- **IAM Role-Based Access** – Service-specific access for Lambda, Glue, Athena.  
- **Encryption at Rest & In-Transit** – Enabled for S3, DynamoDB, and Kinesis.  
- **CloudWatch Alerts** – Error handling and pipeline reliability.  

---

## 📈 Business Impact
- **<15 min Data Latency** – From ingestion to dashboard.  
- **Near Real-Time Crypto Insights** – Enabled traders/analysts to monitor volatility and liquidity.  
- **Cost-Efficient** – Serverless, pay-per-use model scales with crypto activity.  
- **Reliable & Consistent** – Hudi-based incremental ETL ensures data consistency.  
