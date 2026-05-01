# 🚀 WorkforceIQ — End-to-End HR Analytics Pipeline on AWS

## 📌 Overview
WorkforceIQ is an end-to-end HR analytics pipeline designed to transform raw employee data into business-ready insights using a scalable cloud-based architecture on AWS.

The pipeline follows a **Bronze → Silver → Gold** pattern to enable structured data processing, efficient querying, and downstream analytics. It supports key HR use cases such as **attrition analysis, salary benchmarking, and workforce distribution**.

---

## 🎯 Key Outcomes
- Built a scalable data pipeline using serverless AWS services  
- Transformed raw HR data into curated, analytics-ready datasets  
- Enabled fast, low-cost querying using columnar storage (Parquet)  
- Delivered actionable insights through an interactive dashboard  

---

## 🏗️ Architecture

**Medallion Architecture (S3-based):**

| Layer | Storage | Purpose |
|------|--------|--------|
| 🥉 Bronze | S3 | Raw HR data (CSV) |
| 🥈 Silver | S3 | Cleaned, enriched data (Parquet) |
| 🥇 Gold | S3 (Athena CTAS) | Aggregated, business-ready datasets |

### Data Flow

S3 (Bronze)  
→ AWS Glue Crawler  
→ AWS Glue ETL (PySpark)  
→ S3 (Silver)  
→ AWS Glue Crawler  
→ AWS Athena (SQL + CTAS)  
→ S3 (Gold)  
→ Looker Studio Dashboard  

---

## ☁️ Tech Stack
- AWS S3  
- AWS Glue (Crawler + PySpark ETL)  
- AWS Athena  
- Python / PySpark  
- Looker Studio  

---

## 📊 Dataset
Synthetic HR dataset with ~10,000 records covering employee demographics, compensation, performance, and attrition indicators.

---

## 🔄 Pipeline Implementation

### 🔹 Data Ingestion (Bronze)
Raw HR dataset ingested into S3 as the source-of-truth layer.

---

### 🔹 Data Processing (Silver)
ETL implemented using AWS Glue (PySpark):

**Key transformations:**
- Data cleaning and standardization  
- Schema enforcement and type casting  
- Feature engineering:
  - `salary_band` (compensation segmentation)  
  - `attrition_risk` (derived risk indicator)  
- Conversion to Parquet for optimized storage and query performance  

---

### 🔹 Analytics Layer (Athena)
Serverless SQL queries used for exploratory and analytical workloads.

**Example use cases:**
- Department-wise headcount analysis  
- Attrition segmentation  
- Salary benchmarking across job levels  

---

### 🔹 Gold Layer (Aggregations)
Business-level datasets created using Athena CTAS:

| Table | Description |
|------|------------|
| gold_department_summary | Headcount, avg salary, attrition rate |
| gold_attrition_risk | Risk segmentation by department & role |
| gold_salary_band | Salary distribution with performance metrics |

**Design choice:**  
Athena CTAS used over Redshift to maintain a **serverless, cost-efficient architecture** while still enabling fast analytical queries.

---

### 🔹 Visualization Layer
Interactive dashboard built using Looker Studio.

**Capabilities:**
- Attrition risk segmentation  
- Department-level workforce insights  
- Salary band analysis  
- Filter-driven exploration  

---

## 📈 Business Insights
- Identified departments with elevated attrition risk  
- Observed correlation between compensation bands and attrition patterns  
- Highlighted workforce distribution imbalances across teams  

---

## 💰 Cost Optimization
- Leveraged serverless services (no infrastructure overhead)  
- Used Parquet format to minimize Athena scan costs  
- Avoided persistent warehouse costs (Redshift)  
- Designed pipeline for low-cost analytical workloads  

---


## ⚠️ Challenges & Decisions
- Replaced QuickSight with Looker Studio due to access limitations  
- Adopted Athena CTAS for Gold layer to maintain cost efficiency  
- Handled schema inconsistencies during ETL stage  

---

## 🚀 Future Enhancements
- Introduce orchestration (Airflow)  
- Integrate warehouse layer (Redshift/Snowflake)  
- Add incremental data processing  
- Expand dashboard with advanced KPIs  

---

## 🎤 Summary
Designed and implemented a cloud-based HR analytics pipeline on AWS, transforming raw data into structured insights through a scalable and cost-efficient architecture.
