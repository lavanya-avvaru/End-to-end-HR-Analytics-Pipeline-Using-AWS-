# 🚀 WorkforceIQ — End-to-End HR Analytics Pipeline on AWS

## 📌 Overview
WorkforceIQ is an end-to-end HR analytics pipeline built on AWS using a **Medallion Architecture (Bronze → Silver → Gold)**.

The project processes raw HR data into analytics-ready datasets and delivers business insights through an interactive dashboard.

It simulates a real-world HR use case to analyze:
- Attrition risk  
- Salary distribution  
- Workforce trends  

---

## 🎯 Objective
- Build a scalable AWS data pipeline  
- Transform raw HR data into curated datasets  
- Perform SQL-based analysis  
- Deliver business insights via dashboard  

---

## 🏗️ Architecture

WorkforceIQ implements a **Medallion Architecture** on AWS:

| Layer | Storage | Purpose |
|-------|--------|--------|
| 🥉 Bronze | S3 | Raw, unprocessed HR data (CSV) |
| 🥈 Silver | S3 | Cleaned, validated data (Parquet) |
| 🥇 Gold | S3 (via Athena CTAS) | Aggregated, business-ready data |

### Data Flow

S3 (Bronze)  
→ Glue Crawler  
→ Glue ETL (PySpark)  
→ S3 (Silver)  
→ Glue Crawler  
→ Athena (SQL + CTAS)  
→ S3 (Gold)  
→ Looker Studio Dashboard  

---

## ☁️ AWS Services Used
- **S3** — Data storage (Bronze, Silver, Gold layers)  
- **AWS Glue** — Crawlers + ETL (PySpark)  
- **AWS Athena** — Serverless SQL analytics  
- **Google Looker Studio** — Dashboard visualization  

---

## 📍 Region
All services deployed in: `ap-south-1` (Mumbai)

---

## 📊 Dataset

Synthetic HR dataset with 10,000 records and 17 columns.

| Column | Description |
|--------|------------|
| employee_id | Unique ID (EMP00001 format) |
| name | Employee name |
| age | Age (22–60) |
| gender | Male / Female / Non-binary |
| education | Bachelor / Master / PhD / Diploma |
| department | 8 departments |
| job_level | Junior → Director |
| location | 6 Indian cities |
| salary | INR-based |
| hire_date | 2010–2024 |
| years_at_company | Derived |
| performance_score | 1–5 |
| training_hours | 0–120 |
| manager_id | Reporting manager |
| attrition | Yes / No |
| last_promotion_year | 2015–2024 |
| satisfaction_score | 1.0–5.0 |

---

## 🔄 Pipeline Breakdown

### 🔹 Phase 1 — S3 Setup
Created 3 buckets:
- `workforceiq-bronze-lav-2024`
- `workforceiq-silver-lav-2024`
- `workforceiq-gold-lav-2024`

---

### 🔹 Phase 2 — Data Ingestion
- Uploaded `hr_raw_data.csv` (10,000 rows) to Bronze layer  
- Serves as raw source of truth  

---

### 🔹 Phase 3 — Glue Crawler (Bronze)

- Crawler: `workforceiq-bronze-crawler`
- Source: Bronze S3 bucket  
- Database: `workforceiq_catalog`  
- Table: `workforceiq_bronze_lav_20`  
- Schema: Auto-detected (CSV, 17 columns)  

---

### 🔹 Phase 4 — Glue ETL (Bronze → Silver)

- Job: `workforceiq-bronze-to-silver`
- Engine: PySpark (Glue 4.0)
- Runtime: ~1 min 50 sec  
- Output: Parquet (269 KB)

#### Transformations Applied

| Step | Action |
|------|-------|
| Deduplication | Removed duplicates |
| Null handling | Cleaned missing values |
| Type casting | Correct data types |
| New column | salary_band |
| New column | attrition_risk |
| New column | processed_at |
| Standardization | Cleaned text columns |
| Optimization | CSV → Parquet (~75% reduction) |

---

### 🔹 Phase 5 — Athena (SQL Analytics)

Performed serverless SQL queries on Silver data.

#### Sample Queries


SELECT department, COUNT(*) AS total_employees
FROM hr_cleaned
GROUP BY department;

SELECT attrition_risk, COUNT(*) AS count
FROM hr_cleaned
GROUP BY attrition_risk;

SELECT salary_band, AVG(salary) AS avg_salary
FROM hr_cleaned
GROUP BY salary_band;
Performance
Query runtime: < 1 second
Data scanned: minimal (Parquet optimized)
Cost per query: near zero


### 🔹 Phase 6 — Gold Layer (Athena CTAS)

Created aggregated datasets using CTAS.

Table	Description
gold_department_summary	Headcount, avg salary, attrition rate
gold_attrition_risk	Risk breakdown by dept & job level
gold_salary_band	Salary distribution analysis
Why Athena instead of Redshift?
No infrastructure setup
Zero idle cost
Fast and scalable
Stored directly in S3


### 🔹 Phase 7 — Dashboard (Looker Studio)

Built an interactive HR analytics dashboard.

Data Flow

Athena → CSV Export → Google Sheets → Looker Studio

Features
Employee distribution by department
Attrition risk segmentation
Salary band analysis
Interactive filters
Key Insights
High attrition in specific departments
Lower salary bands → higher attrition
Workforce imbalance across departments
📊 Business Impact

This pipeline enables:

Early identification of attrition risks
Data-driven HR decisions
Better workforce planning
💰 Cost Optimization
Used Parquet to reduce scan cost
Serverless architecture (Glue + Athena)
Avoided Redshift (high cost)
Used Looker Studio (free tool)
