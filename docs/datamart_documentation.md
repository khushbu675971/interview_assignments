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

Only events satisfying:
- event_type = 'view_item'
- parameter_name = 'item_id'

were included in the aggregation.

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

### 6. Item Enrichment

The item master table (silver_item) is joined to provide additional business context such as:
- item name
- category

This improves usability of the final analytical dataset.

## Datamart Attributes

| Column Name | Description |
|---|---|
| year | Year of the event |
| item_id | Unique identifier of the item |
| item_name | Item name |
| category | Item category |
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
(data cleansing, payload parsing)
        ↓
Gold Layer
(top_item datamart)

---

## Output Example

| year | item_id | item_name | category | total_views | item_rank | most_used_platform |
|--------|--------|--------|--------|--------|--------|--------|
| 2025 | 3526 | Wireless Headphones | Electronics | 1200 | 1 | android |
| 2025 | 1452 | Running Shoes | Sportswear | 980 | 2 | ios |
| 2025 | 2086 | Coffee Maker | Home Appliances | 875 | 3 | web |

### Output Description

The `gold_top_item` datamart provides yearly item performance metrics and contains:

- **year** – Calendar year extracted from the event timestamp.
- **item_id** – Unique identifier of the item.
- **item_name** – Business-friendly item name sourced from the item master dataset.
- **category** – Item category sourced from the item master dataset.
- **total_views** – Total number of item view events recorded for the item in the given year.
- **item_rank** – Ranking of the item within the year based on descending total views.
- **most_used_platform** – Platform with the highest number of view events for the item during the year.

### Business Value

The datamart enables business users to:

- Identify the most popular items for each year.
- Compare item performance across different years.
- Analyze customer platform preferences.
- Support merchandising and marketing decisions using item popularity trends.
- Create dashboards and reports for item performance analysis.

---

## Assumptions

The following assumptions were made:
- `view_item` events represent item views
- `parameter_value` in payload represents item_id
- event timestamps are valid and parsable
- parameter_name = 'item_id' identifies valid item events
- parameter_value contains the actual item identifier

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
