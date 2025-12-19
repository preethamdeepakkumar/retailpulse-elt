# retailpulse-elt
# RetailPulse: Automated E-Commerce ELT Pipeline 

![CI/CD Status](https://github.com/preethamdeepakkumar/retailpulse-elt/actions/workflows/dbt_ci.yml/badge.svg)
**RetailPulse** is an end-to-end analytics engineering pipeline that ingests raw, messy e-commerce data, transforms it into a clean Star Schema, and ensures data quality through automated CI/CD testing.

---

## Architecture

**Raw Data (Python)** -> **Ingestion (BigQuery)** -> **Transformation (dbt)** -> **Quality Assurance (GitHub Actions)** -> **BI (Looker Studio)**

* **Ingestion:** Python script generates mock e-commerce data (including "dirty" records like negative prices) and loads it into Google BigQuery.
* **Warehousing:** Google BigQuery stores raw and transformed data.
* **Transformation:** dbt (Data Build Tool) cleans data, handles duplicates, and models it into a Star Schema (`fact_orders`, `dim_customers`).
* **Orchestration & CI/CD:** GitHub Actions automatically runs `dbt test` on every code push to block bad data from production.

---

## Tech Stack

* **Language:** Python 3.9 (Pandas, Faker)
* **Warehouse:** Google BigQuery
* **Transformation:** dbt (Data Build Tool)
* **Automation:** GitHub Actions (CI/CD)
* **Version Control:** Git & GitHub

---

## Key Features

### 1. Automated Data Quality Tests ("The Guardrails")
The pipeline proactively stops "bad data" from corrupting dashboards.
* **Test:** `assert_revenue_positive.sql` checks for negative revenue.
* **Logic:** If `unit_price < 0`, the pipeline **fails** and alerts the engineer.
* **Result:** 100% trust in the final reporting numbers.

### 2. CI/CD for Data
Implemented a Continuous Integration pipeline using **GitHub Actions**.
* **Trigger:** Runs on every `git push` to the `main` branch.
* **Steps:** Installs dependencies ➔ Authenticates to GCP ➔ Runs `dbt run` ➔ Runs `dbt test`.
* **Benefit:** Prevents broken SQL logic from ever reaching the production warehouse.

---

## Dashboard
*(Placeholder: You can upload your Looker Studio screenshot here)*
The final `fact_orders` table feeds into a Looker Studio dashboard tracking **Total Revenue** and **Daily Sales Trends**.

---

## How to Run This Project

### Prerequisites
* Google Cloud Platform (GCP) Account with BigQuery enabled.
* Python 3.9+
* dbt CLI installed (`pip install dbt-bigquery`).

### 1. Setup Environment
```bash
git clone [https://github.com/preethamdeepakkumar/retailpulse-elt.git](https://github.com/preethamdeepakkumar/retailpulse-elt.git)
cd retailpulse-elt
pip install -r requirements.txt

2. Authenticate
Place your GCP Service Account Key in the root folder and name it gcp-key.json. (Note: This file is git-ignored for security reasons. You must generate your own key from the Google Cloud Console).

3. Generate & Ingest Data
Bash
python ingest_data.py
# This script generates 200 mock records and uploads them to BigQuery 'raw_data'.

4. Run Transformation & Tests
Bash
dbt run
dbt test

## Project Impact
Every retail company struggles with "dirty data" from multiple sources. This project demonstrates how to build a Self-Healing Data Pipeline that:

Centralizes data into a Single Source of Truth.

Automates quality checks (saving hours of manual debugging).

Delivers reliable metrics for executive decision-making.


Author: Preetham Deepak Kumar