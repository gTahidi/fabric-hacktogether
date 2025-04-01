# fabric-hacktogether
A submission for the Microsoft Hack together Hackathon, demonstranting the application of Microsoft Fabric to real world data problems
The architectural components of the project are detailed below:

## Introduction and Overview

This project demonstrates the application of Microsoft Fabric to address a real-world data problem: sales prediction for a local retailer. By leveraging the capabilities of Fabric, we aim to build an end-to-end solution that ingests raw sales data, processes it through a Medallion architecture, and ultimately provides accurate sales forecasts. This solution helps the retailer optimize inventory, staffing, and marketing efforts based on data-driven insights.

## Problem Statement

Local retailers often face challenges in accurately predicting future sales due to various factors like seasonality, promotions, and changing customer behavior. Inaccurate forecasts can lead to stockouts or overstocking, impacting revenue and operational efficiency. This project aims to develop a robust sales prediction model using historical sales data provided by the retailer. The goal is to predict future sales trends for different goods, enabling better business planning and decision-making.

## Architecture (Medallion)

We utilize the Medallion architecture within Microsoft Fabric to structure our data processing pipeline, ensuring data quality and scalability.

![Medallion Architecture](image.png)

### Bronze Layer

*   **Purpose:** Ingests raw, unprocessed data from source systems.
*   **Data:** Raw sales transaction data from the local retailer (e.g., CSV files, database dumps). Data is kept in its original format with minimal changes, primarily for archival and lineage tracking.
*   **Fabric Components:** Data Factory pipelines or Dataflows Gen2 for ingestion, OneLake/Lakehouse for storage.

### Silver Layer

*   **Purpose:** Cleansed, conformed, and enriched data ready for analysis.
*   **Data:** Raw data is transformed, validated, and standardized. This includes handling missing values, correcting data types, joining related datasets (e.g., product details, store information), and potentially creating basic features. Data is often stored in optimized formats like Delta tables.
*   **Fabric Components:** Notebooks (PySpark/Spark SQL) or Dataflows Gen2 for transformations, Lakehouse for storage.

### Gold Layer

*   **Purpose:** Curated, aggregated data optimized for specific business use cases and analytics.
*   **Data:** Data is aggregated to the required level for sales prediction (e.g., daily/weekly sales per product/store). Features relevant for forecasting models are engineered. This layer serves as the source for reporting dashboards and machine learning models.
*   **Fabric Components:** Notebooks (PySpark/Spark SQL) for aggregation and feature engineering, Lakehouse/Warehouse for storage, Power BI datasets for reporting.

# Implementation Details

## Data Preparation Flow

1.  **Ingestion:** Raw sales data files (e.g., daily transaction logs) are ingested into the Bronze layer of the Lakehouse using Fabric Data Factory pipelines or Dataflows.
2.  **Initial Storage:** Data is stored in its original format in the 'Files' section or as basic Delta tables within the Bronze zone of OneLake.

## Data Processing Flow

1.  **Bronze to Silver:** Notebooks (like `Spice_Test.ipynb`) read raw data (e.g., `SalesData.csv`) from the Bronze layer. Cleaning involves:
    *   Handling null values in key columns (`Quantity Sold`, `Revenue`).
    *   Converting date columns to the correct `DateType`.
    *   Extracting date components (`Year`, `Month`, `Day`, `Weekday`) for easier analysis and feature engineering.
    The cleaned, validated data is stored as a Delta table (e.g., `CleanedSalesData`) in the Silver layer.
2.  **Silver to Gold:** Notebooks process the cleaned Silver layer data. This involves:
    *   Aggregating data to the desired granularity (e.g., calculating total monthly quantity and revenue per product).
    *   Transforming data into a structure suitable for modeling, such as pivoting the table to have time periods (Year, Month) as rows and products as columns, with sales quantity as values (e.g., `PivotedSalesQuantity` table).
    *   Further feature engineering (lag features, rolling averages) might be applied here depending on the modeling approach.
    The final, curated, and aggregated data is stored in the Gold layer, ready for reporting and ML model training.

## Fabric Features & Azure AI

*   **Microsoft Fabric:** Leverages OneLake, Lakehouse, Warehouse, Data Factory, Dataflows Gen2, Notebooks (Spark), and Power BI for an integrated data platform experience.
*   **Azure AI (Potential):** While the core focus is Fabric, Azure Machine Learning could be integrated for advanced model training, deployment (MLflow tracking within Fabric), and monitoring if needed for more complex forecasting scenarios. (Details to be added based on actual implementation).



