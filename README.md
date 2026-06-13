# AW_azure-adf-synapse-project

## Adventure Works Azure Data Engineering Project

### Project Overview

This project is an end-to-end Azure Data Engineering solution built using **Azure Data Factory**, **Azure Data Lake Storage Gen2**, and **Azure Synapse Analytics**.

The pipeline extracts Adventure Works product data from a GitHub-hosted CSV file, loads it into Azure Data Lake Storage Gen2, and uses Azure Synapse Analytics Serverless SQL to create schemas and views for analytical querying.

---

## Architecture

```text
GitHub CSV Source
        ↓
Azure Data Factory
        ↓
Azure Data Lake Storage Gen2 (Bronze Layer)
        ↓
Azure Synapse Analytics
        ↓
Serverless SQL Views (Gold Layer)
```

---

## Technologies Used

* Azure Data Factory
* Azure Synapse Analytics
* Azure Data Lake Storage Gen2
* Serverless SQL Pool
* GitHub
* SQL
* JSON-based Azure Artifacts

---

## Repository Structure

```text
AW_azure-adf-synapse-project/
│
├── adf/
│   ├── dataset/
│   │   ├── ds_http.json
│   │   └── ds_raw.json
│   │
│   ├── factory/
│   │
│   ├── linkedService/
│   │   ├── httplinkedservice.json
│   │   └── storagedatalake.json
│   │
│   ├── pipeline/
│   │   └── GitToRaw.json
│   │
│   └── publish_config.json
│
├── synapse/
│   ├── credential/
│   ├── integrationRuntime/
│   ├── linkedService/
│   ├── sqlscript/
│   │   ├── Create Schema.json
│   │   ├── Create Views Gold.json
│   │   └── SQL script 1.json
│   │
│   └── publish_config.json
```

---

## Data Ingestion

The data ingestion is handled by **Azure Data Factory**.

### ADF Pipeline

| Property      | Value         |
| ------------- | ------------- |
| Pipeline Name | GitToRaw      |
| Activity Name | CopyRawData   |
| Activity Type | Copy Activity |

The pipeline uses a Copy activity to read product data from a GitHub raw CSV source and write it into Azure Data Lake Storage Gen2.

### Source Dataset

| Property    | Value               |
| ----------- | ------------------- |
| Dataset     | ds_http             |
| Source Type | HTTP Delimited Text |

The source reads the Adventure Works product CSV file from GitHub.

### Sink Dataset

| Property              | Value                                       |
| --------------------- | ------------------------------------------- |
| Dataset               | ds_raw                                      |
| Sink Type             | Azure Data Lake Storage Gen2 Delimited Text |
| Container/File System | bronze                                      |
| Folder                | products                                    |
| File                  | products.csv                                |

---

## Data Transformation and Analytics

Azure Synapse Analytics is used to query and transform the ingested data.

### Synapse SQL Scripts

The Synapse SQL scripts include:

* Creating the **gold** schema
* Creating analytical views in the **gold** layer
* Running SQL queries on the processed data

---

## Layers Used

### Bronze Layer

The bronze layer stores raw ingested data from the source system.

**Example**

```text
bronze/products/products.csv
```

### Gold Layer

The gold layer contains SQL views created in Azure Synapse Analytics for reporting and analysis.

**Example**

```sql
CREATE SCHEMA gold;
```

---

## How to Run the Project

### 1. Create Azure Data Lake Storage Gen2 Account

Create a container/file system named:

```text
bronze
```

### 2. Import Azure Data Factory Artifacts

Create or import the Azure Data Factory artifacts from the `adf` folder.

Configure the linked services:

* HTTP linked service for GitHub source
* ADLS Gen2 linked service for storage

### 3. Run the ADF Pipeline

Run the pipeline:

```text
GitToRaw
```

Confirm that the file is copied to:

```text
bronze/products/products.csv
```

### 4. Open Azure Synapse Studio

Import or use the Synapse artifacts from the `synapse` folder.

Run the SQL scripts in this order:

1. Create Schema
2. SQL script 1
3. Create Views Gold

### 5. Query the Gold Views

Query the gold views for analytics and reporting.

---

## Security Notes

This repository should **not** contain sensitive values such as:

* Storage account keys
* SAS tokens
* Passwords
* Client secrets
* Connection strings

For production use, secrets should be stored in **Azure Key Vault** and referenced securely from Azure Data Factory or Synapse.
