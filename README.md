# Top Item Datamart – Data Engineering Assignment

## Overview

This project implements a Medallion Architecture (Bronze → Silver → Gold) using Databricks, PySpark, and Delta Lake.
The solution processes raw item and event datasets and creates a business-ready `top_item` datamart.

---

## Business Requirement

Build a top_item datamart that provides:

- Total number of item views in a particular year
- Rank of an item based on yearly views
- Most used platform for a particular item in a particular year

## Architecture:
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

## Gold Datamart Output

| Column | Description |
|----------|----------|
| year | Event year |
| item_id | Unique item identifier |
| item_name | Item name |
| category | Item category |
| total_views | Total number of views |
| item_rank | Rank based on yearly views |
| most_used_platform | Most frequently used platform |

## Assumptions

- view_item events represent item views
- parameter_name = 'item_id' identifies valid item events
- parameter_value contains the actual item identifier
- Event timestamps are valid and parsable

## Technologies Used

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
```

## Input Datasets

Input files:
- item.csv
- event.csv

Source:
- Provided S3 URLs in assignment

Note:
Raw CSV files are not stored in GitHub due to size limitations.

## Execution Order

Run notebooks in the following sequence:

1. 01_bronze_ingestion.py
2. 02_silver_transformation.py
3. 03_gold_datamart.py