# 🔍 Fake Job Posting Analysis — Fraud Pattern Detection

![MySQL](https://img.shields.io/badge/MySQL-Data%20Cleaning-4479A1?style=flat-square&logo=mysql&logoColor=white)
![Power BI](https://img.shields.io/badge/Power%20BI-Dashboard-F2C811?style=flat-square&logo=powerbi&logoColor=black)
![Excel](https://img.shields.io/badge/Excel-Validation-217346?style=flat-square&logo=microsoftexcel&logoColor=white)
![Status](https://img.shields.io/badge/Status-Complete-brightgreen?style=flat-square)

An independent data analytics project identifying reliable fraud indicators in online job postings, using a real-world dataset of 17,079 listings.

🔗 Live write-up: https://github.com/Lavanya-DA

👤 Author: Lavanya Shanmugaraj — [LinkedIn](https://linkedin.com/in/lavanya-shanmugaraj)

---

## 📌 Table of Contents

- [Overview](#overview)
- [Dataset](#dataset)
- [Methodology](#methodology)
- [Key Findings](#key-findings)
- [Tools](#tools)
- [Repository Contents](#repository-contents)
- [Notes on Rigor](#notes-on-rigor)

---

## Overview

Roughly 1 in 21 job postings in this dataset (**4.78%**) were confirmed fraudulent. This project set out to answer a practical question: **which posting attributes reliably predict fraud, and which ones just look suspicious by coincidence?**

The analysis was done end-to-end — cleaning raw data in Excel and MySQL, running aggregation queries to test hypotheses, and building an interactive Power BI dashboard to communicate findings to a non-technical audience.

## Dataset

| | |
|---|---|
| **Source** | [Real or Fake Job Posting Prediction](https://www.kaggle.com/datasets/shivamb/real-or-fake-fake-jobposting-prediction) (Kaggle) |
| **Rows** | 17,079 postings |
| **Fields used** | company logo presence, employment type, telecommuting status, salary range, industry, location, confirmed `fraudulent` label |

## Methodology
1. Deduplicate → removed duplicate rows in Excel before loading into MySQL
2. Load & verify → loaded cleaned data into MySQL Workbench, re-verified zero duplicates via a partition-based row-count query
3. Audit nulls & blanks → checked every column (e.g. department blank in 64% of rows)
4. Parse compound fields → split salary_range into salary_min / salary_max
5. Standardize → trimmed whitespace in title, employment_type
6. Query & aggregate → SQL hypothesis testing (see Fake_Job_Analysis_Project.sql)

Full SQL is in [`Fake_Job_Analysis_Project.sql`](./Fake_Job_Analysis_Project.sql).

## Key Findings

| Risk Indicator | Fraud Rate | Sample Size |
|---|---|---|
| 🚩 No company logo | **16.21%** | n = 3,503 (vs. 1.83% with a logo) |
| 🚩 Part-time role | **9.83%** | n = 743 (vs. 4.09% full-time) |
| 🚩 Remote / telecommuting | **8.66%** | n = 739 (vs. 4.60% on-site) |

> **Counter-intuitive finding:** the original hypothesis assumed fraudulent postings would mention salary *less* often, to stay vague. The data showed the opposite — **25.12%** of fraudulent postings listed a salary range, versus only **14.87%** of genuine ones. Scammers appear to use a concrete number as bait, not avoid it.

> **Highest-risk industry:** Accounting, at **36.84%** fraud rate — among industries with at least 50 postings, to avoid small-sample noise (e.g., a single-posting industry hitting a misleading "100% fraud rate").

## Tools

- **Excel** — initial deduplication and salary parsing validation
- **MySQL Workbench** — data loading, verification, cleaning, and all aggregation queries
- **Power BI** — interactive dashboard with KPI cards, fraud-rate breakdowns by employment type/logo/remote status, an industry leaderboard (filtered for sample-size validity), and a geographic view of fraud distribution by country

## Repository Contents
├── Fake_Job_Analysis_Project.sql 

├── fake_job_postings_CLEANED.csv 

├── FraudLens_Dashboard.pbix

├── index.html (case-file page)

└── README.md 


## Notes on Rigor

A couple of deliberate corrections made during this project, documented here for transparency:

- Small-sample industries (e.g., a single posting in "Ranching") were excluded from the industry leaderboard after they produced misleading 100% fraud rates — a minimum threshold of **50 postings per industry** was applied instead.
- Rows with missing/unlabeled industry data are excluded from the "top industries" ranking rather than reported as a finding, since "Unknown" is a data-quality gap, not a real industry signal.

---

*This project was built independently as part of a data analytics portfolio. Dataset sourced from Kaggle; all analysis, cleaning, and dashboard design is original work.*
