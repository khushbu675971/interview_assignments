# interview_assignments#

## Overview

This project implements a Medallion Architecture (Bronze → Silver → Gold) using Databricks, PySpark, and Delta Lake.

The solution processes raw item and event datasets and creates a business-ready `top_item` datamart.

---

# Architecture

## Bronze Layer

Purpose:
- Raw data ingestion
- Minimal transformations
- Store raw historical datasets

Tables:
- bronze_item
- bronze_event

---

## Silver Layer

Purpose:
- Data cleansing
- Standardization
- JSON payload parsing
- Payload flattening

Tables:
- silver_item
- silver_event

Key transformations:
- event payload parsing
- extraction of:
  - event_type
  - platform
  - parameter_name
  - parameter_value
  - item_id
- timestamp conversion
- year derivation

---

## Gold Layer

Purpose:
- Business aggregations
- Analytical datamart creation

Datamart:
- gold_top_item

The datamart provides:
- total yearly item views
- item ranking by yearly views
- most used platform per item/year

---

# Technologies Used

- Databricks
- PySpark
- Delta Lake
- Spark SQL
- Window Functions
- GitHub

---

# Project Structure

```text
de-case-study/
│
├── notebooks/
│   ├── 01_bronze_ingestion.py
│   ├── 02_silver_transformation.py
│   └── 03_gold_datamart.py
│
├── docs/
│   ├── datamart_documentation.md
│   └── open_questions.md
│
└── README.md