# Incremental Load Pipeline (Azure Data Engineering)

This repository contains a complete implementation of an incremental data load pipeline built using Azure Data Factory, Azure SQL Database, and Azure Data Lake Storage. The project demonstrates how to ingest large operational datasets efficiently by loading only new or changed records instead of full refreshes.

## Project Overview

The goal of this project is to simulate a real-world enterprise data-engineering pattern:

- Source system delivers large daily extracts.
- Only a subset of rows change each day.
- Pipeline detects new and updated records.
- Historical data is preserved.
- Run time remains stable as data grows.



## Technology Stack

- Azure Data Factory – pipeline orchestration and incremental logic
- Azure Data Lake Storage Gen2 – raw and staged file storage
- Azure SQL Database – staging, comparison, merge operations
- T-SQL MERGE – apply inserts and updates
- Parameterized ADF Pipelines – reusable dataset loading pattern

## How the Incremental Load Works

1. **Data Landing**  
   ADF ingests the daily CSV batch (e.g. `customers_data.csv) from the `/raw` folder.

2. **Load to Staging**  
   File is copied into partitioned data lake staging folder, then load everything in staging folder to SQL staging table without transformations.

3. **Delta Comparison (SQL)**  
   Stored procedure compares staging vs target using the business key  
   (e.g., `CustomerID`, `CustomerNumber') to detect:
   - New rows  
   - Updated rows (via `ModifiedOn` or checksum)

4. **Merge Operation**  
   `MERGE` inserts and updates rows in the target table.

5. **Watermark Update**  
   ADF updates the watermark table with the latest processed timestamp.

6. **Cleanup**  
   Staging is truncated to keep storage light


## Pipelines execution sequence:
1. initialLoadCustomers (one time big event)
2. incrementalLoadCustomers (runs daily once)
3. loadToSqlCustomers (dependent on successful execution of incrementalLoadCustomers)


