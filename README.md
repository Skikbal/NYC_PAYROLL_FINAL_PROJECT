# NYC Payroll Data Analytics – Data Integration Pipelines

## Project Overview

This project delivers an **end-to-end data integration and analytics solution** for the City of New York using **Azure Synapse Analytics**.

The objectives are:

1. Analyze how financial resources are allocated across agencies, salaries, and overtime.
2. Provide transparent, public-facing data access for payroll spending trends.

Raw payroll data (current + historical) and master datasets are ingested from **Azure Data Lake Gen2**, processed through **Azure Data Factory (ADF)**, stored in **Azure SQL Database**, and aggregated for analytics in **Azure Synapse**.

---

## High-Level Architecture

![Pipeline Overview](https://github.com/eloisjr/Data_Pipeline_Azure/assets/81710422/0147f556-daf1-47c4-81d1-62e1eca34289)

---

## ⚙️ Azure Components

* **Azure Data Lake Gen2** → Raw + historical payroll files, staging outputs
* **Azure Data Factory** → Orchestration of ingestion, transformation, and aggregation
* **Azure SQL Database** → Master data, transactional tables, and summary tables
* **Azure Synapse Analytics** → Query engine with external tables for reporting

---

## Implementation Steps

### 1. Data Infrastructure

* Created Data Lake with folders: `dirpayrollfiles`, `dirhistoryfiles`, `dirstaging`.
* Uploaded CSVs: Employee, Agency, Title master data + Payroll 2020 & 2021.
* Provisioned **SQL DB (`db_nycpayroll`)** and **Synapse Workspace**.
* Defined SQL tables for master, transactional, and summary datasets.

### 2. Linked Services & Datasets

* Configured ADF linked services for **Data Lake, SQL DB**.
* Defined datasets for input CSVs, SQL target tables, and staging outputs.

### 3. Dataflows

* Built dataflows to ingest **master data** and **payroll data** into SQL DB.
* Created **Summary Dataflow**:

  * Union of Payroll 2020 & 2021.
  * Parameterized fiscal year filter.
  * Derived column `TotalPaid = Regular + OT + Other`.
  * Aggregated by **Agency + FiscalYear**.

### 4. Pipeline Orchestration

* Designed ADF pipeline:

  * Master dataflows → run in parallel.
  * Payroll dataflows → sequential.
  * Summary aggregation → final stage.

### 5. Execution & Validation

* Triggered pipeline runs manually and monitored in ADF.
* Validated outputs in:

  * **SQL DB summary table**.
  * **Data Lake staging folder** (CSV/Parquet).
  * **Synapse external table** (end-to-end reporting).

---

## Business Outcomes

* **Integrated pipeline** to process payroll data across years.
* **Dynamic parameterization** enables flexible year-based filtering.
* **Aggregated reporting** at **Agency + FiscalYear** level.
* **Dual storage** of results:

  * SQL DB → structured, relational queries.
  * Data Lake staging → Synapse external queries.


---

**Lisence:** [MIT](./LICENSE)