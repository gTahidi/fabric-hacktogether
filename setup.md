## Set Up Process

1.  **Provision Microsoft Fabric Workspace:** Ensure you have access to a Microsoft Fabric enabled workspace (Trial or Paid Capacity).
2.  **Create Lakehouse:** Within the workspace, create a new Lakehouse (e.g., `SalesPredictionLH`). This will serve as the central storage for Bronze, Silver, and Gold data layers.
3.  **Upload Raw Data:** Upload the initial raw sales data (e.g., `SalesData.csv`) to the `Files/Bronze/RawInput` directory within the Lakehouse using the Fabric UI or OneLake File Explorer. Alternatively, configure Data Factory pipelines or Dataflows Gen2 to ingest data from source systems into this location.
4.  **Import Notebooks:** Upload or create the necessary Fabric Notebooks, such as `Spice_Test.ipynb` (for cleaning, transformation, and pivoting) and potentially separate notebooks for model training, within the workspace.
5.  **Configure Pipelines/Dataflows (Optional):** If using pipelines or dataflows for ingestion or orchestration, configure their connections, parameters, and schedules.
6.  **Set up Power BI (Optional):** Connect Power BI Desktop to the Fabric Lakehouse/Warehouse (Gold layer) or create a Power BI Dataset directly in the Fabric service to build reports.

## Configuration Details

*   **Lakehouse Structure:** The Lakehouse (`SalesPredictionLH`) is structured according to the Medallion architecture:
    *   `Files/Bronze/`: Stores raw, ingested data.
    *   `Tables/Silver/`: Stores cleaned, conformed Delta tables.
    *   `Tables/Gold/`: Stores aggregated, analysis-ready Delta tables or views for modeling and reporting. (Alternatively, a Fabric Warehouse could be used for the Gold layer).
*   **Notebook Environments:** Notebooks utilize the default Fabric Spark runtime. Ensure necessary libraries (e.g., `pandas`, `scikit-learn`, `prophet`, `mlflow`) are installed using `%pip install` if not included in the standard runtime.
*   **Data Connections:** Configure connections within Data Factory/Dataflows if ingesting data from external sources. Secure credential management should be used.
*   **MLflow Tracking:** Fabric's integrated MLflow is used automatically when running experiments within Notebooks. No separate MLflow server setup is required.

### Stage Set up & Separation of Concerns

The Medallion architecture provides clear separation:
*   **Bronze:** Handled by initial data loading into the `Files/Bronze/RawInput` path within the Lakehouse. Access might be restricted to ingestion processes.
*   **Silver:** Created by processing Bronze data using specific Notebooks (e.g., the initial cleaning/transformation parts of `Spice_Test.ipynb`) or Dataflows, outputting tables (e.g., `CleanedSalesData`) to the `Tables/Silver/` location.
*   **Gold:** Created by processing Silver data using dedicated Notebooks (e.g., the aggregation and pivoting parts of `Spice_Test.ipynb`, or separate notebooks like `Sales_Forecasting_Model.ipynb`), outputting tables/views (e.g., `PivotedSalesQuantity`) to `Tables/Gold/` or a Warehouse. This layer is consumed by Power BI and modeling notebooks.
*   **Artifact Naming:** Use clear naming conventions for Lakehouses, Notebooks, Pipelines, Dataflows, and Tables to indicate their purpose and layer (e.g., `df_ingest_raw_sales_bronze`, `nb_clean_sales_silver`, `tbl_gold_pivoted_sales`).

### Azure AI set up

*   **Current Scope:** This project primarily utilizes the built-in capabilities of Microsoft Fabric, including its integrated MLflow for experiment tracking within Notebooks.
*   **Potential Integration (If Extending):**
    1.  **Azure Machine Learning Workspace:** Provision an Azure Machine Learning workspace if advanced capabilities (e.g., managed endpoints, custom model deployments beyond Fabric's scope) are needed.
    2.  **Linking:** Connect the Fabric workspace to the Azure ML workspace if required for specific cross-platform scenarios (though often MLflow within Fabric is sufficient).
    3.  **Compute:** Configure compute resources within Azure ML if training or deploying models there.

## Technologies Used

*   **Microsoft Fabric:**
    *   **Workspace:** Central environment for development and execution.
    *   **OneLake:** Unified storage foundation.
    *   **Lakehouse:** Primary storage and processing artifact for Bronze, Silver, and potentially Gold layers (using Delta tables).
    *   **Warehouse (Optional):** Alternative for the Gold layer, offering SQL endpoint and performance optimizations.
    *   **Data Factory / Dataflows Gen2:** For data ingestion and potentially ETL/ELT orchestration.
    *   **Notebooks:** For data transformation, feature engineering, model training, and experimentation using Spark (PySpark/Spark SQL) and Python.
    *   **MLflow (Integrated):** For tracking machine learning experiments, parameters, metrics, and models within Fabric.
    *   **Power BI:** For data visualization and reporting, connected to the Gold layer.
*   **Languages/Libraries:**
    *   Python
    *   PySpark / Spark SQL
    *   Pandas
    *   Scikit-learn (or other ML libraries like Prophet, Statsmodels)
    *   Delta Lake (underlying format for Lakehouse tables)



