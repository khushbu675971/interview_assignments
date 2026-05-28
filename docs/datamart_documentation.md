# top_item Datamart Documentation

## Overview

The `top_item` datamart provides yearly analytical insights for item view activity across different platforms.

The datamart was created using a Bronze-Silver-Gold medallion architecture implemented in Databricks using PySpark and Delta Lake.

The purpose of the datamart is to identify:
- the most viewed items
- yearly item popularity
- platform usage trends

---

## Source Tables

The datamart uses the following Silver layer tables:

| Table Name | Description |
|---|---|
| silver_event | Cleaned and flattened event data |
| silver_item | Cleaned item master data |

---

## Business Logic

The following business logic was applied during Gold layer processing.

### 1. Filter View Events

Only events with:

event_type = 'view_item'

were included in the aggregation because the datamart focuses on item view analytics.

### 2. Year Extraction

The year was extracted from the event timestamp to support yearly aggregations.

### 3. Total Item Views

The total number of views was calculated for each:
- item
- year

using Spark aggregations.

### 4. Item Ranking

Items were ranked within each year based on:
- descending total views

Spark window functions were used to generate rankings.

### 5. Most Used Platform

The most frequently used platform was identified for every:
- item
- year

using platform usage counts and row_number window functions.

---

## Datamart Attributes

| Column Name | Description |
|---|---|
| item_id | Unique identifier of the item |
| year | Year of the event |
| total_views | Total number of item views in the year |
| item_rank | Rank of the item based on yearly views |
| most_used_platform | Most frequently used platform for the item in the year |

---

## Technical Implementation

The datamart was implemented using:
- PySpark DataFrame API
- Delta Lake tables
- Spark SQL window functions
- Medallion architecture principles

### Spark Functions Used

The following Spark concepts were used:
- groupBy
- aggregations
- count
- rank
- row_number
- window functions
- joins

### Partitioning Strategy

The Gold table was partitioned by:
- year

This improves query performance for yearly analytical workloads.

---

## Data Flow Architecture

Raw CSV Files
      ↓
Bronze Layer
(raw ingestion)
      ↓
Silver Layer
(cleaned & flattened)
      ↓
Gold Layer
(top_item datamart)

---

## Output Example

| item_id | year | total_views | item_rank | most_used_platform |
|---|---|---|---|---|
| 3526 | 2025 | 1200 | 1 | android |
| 1452 | 2025 | 980 | 2 | ios |

---

## Assumptions

The following assumptions were made:
- `view_item` events represent item views
- `parameter_value` in payload represents item_id
- event timestamps are valid and parsable
- platform values are available in the event payload

---

## Conclusion

The implemented `top_item` datamart provides a scalable and analytics-ready business layer for item performance reporting.

The solution demonstrates:
- Bronze-Silver-Gold architecture
- semi-structured data handling
- JSON payload flattening
- Spark transformations
- window function usage
- analytical datamart creation
