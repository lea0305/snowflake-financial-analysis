# 🧠 Snowflake Financial Transactions Analysis

This project analyzes **U.S. state-level financial transactions** from the SSBCI dataset using **Snowflake SQL**.

The goal is to transform raw financial data into meaningful insights about **funding distribution**, **job creation**, and **efficiency** across states.

---

## 📊 Project Overview

Using Snowflake as a cloud data warehouse, I performed:

- Data cleaning and transformation  
- Aggregation of metrics per U.S. state  
- Ranking and performance scoring using SQL window functions  
- Funding efficiency and KPI analysis  

The result is a state-level summary showing which states use their funding most efficiently.

---

## ⚙️ Tools & Technologies

- **Snowflake** — data storage and querying  
- **SQL** — data analysis, aggregation, and window functions  
- **CTEs (Common Table Expressions)** — multi-step transformations  
- **Excel / Power BI** — visualization-ready output  

---

## 🧩 Key SQL Concepts Used

```sql
WITH BASE_AGG AS (
  SELECT 
    STATE_NAME,
    ROUND(SUM(LOAN_INVESTMENT_AMOUNT), 2) AS TOTAL_FUNDING,
    SUM(JOBS_CREATED) AS JOBS_CREATED,
    RANK() OVER (ORDER BY SUM(LOAN_INVESTMENT_AMOUNT) DESC) AS FUNDING_RANK
  FROM FINANCIAL_TRANSACTIONS
  GROUP BY STATE_NAME
)
SELECT 
  STATE_NAME,
  TOTAL_FUNDING,
  JOBS_CREATED,
  FUNDING_RANK
FROM BASE_AGG
ORDER BY TOTAL_FUNDING DESC;
