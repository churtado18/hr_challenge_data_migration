# HR Challenge – Data Migration

This repository contains the implementation of data pipelines developed with **Azure Data Factory (ADF)** to data movement, backups and restore.

---
## Challenge
### Data movement
1. Move historic data from files in CSV format to the new database.
2. Create a feature to backup for each table and save it in the file system in AVRO format.
3. Create a feature to restore a certain table with its backup.

---
## High-level architecture

![HR Challenge Architecture](https://github.com/churtado18/hr_challenge_data_migration/blob/develop/Architecture.png?raw=true)

### Data movement
1. Pipelines in Azure Data Factory obtain information on jobs, departments, and hired employees in CSV format from the Azure Blob storage repository.
2. Pipelines in Azure Data Factory store the information obtained in the Azure SQL database.
3. Pipelines in Azure Data Factory perform the following operations:
3.1 Full backups of the jobs, departments, and hired employees tables and store them in Azure Blob storage in AVRO file format.
3.2 Full restore AVRO files for jobs, departments, and hired employees to the jobs, departments, and hired employees tables.
4. The following REST APIs were developed using FastAPI and deployed to Azure App Service:
    - **Human Resources:** Provides read and write access to the jobs, departments, and hired_employees tables.
    - **Metrics:** Retrieves quarterly metrics on hired employees by department.
5. APIs can be accessed by internal and external systems through Azure API Management.
---

---

## Repository Contents

- `/pipeline/` — JSON definitions of the pipelines.
- `/dataset/` — source and destination datasets.
- `/linkedService/` — linked services (e.g., databases, blob storage).
- `/trigger/` — pipeline triggers.
- `/dataflow/` — transformation logic.

---

## Implemented Pipelines

### 1. **Data Migration Pipelines**
- Copies data of jobs, departments and hired employees from Azure Blob Storage in **csv** format to tables in Azure SQL database.
- If data exists in the destination tables, it overwrites them.

### 2. **Data Backups Pipelines**
- Copies data of jobs, departments and hired employees tables in Azure SQL database to Azure Blob Storage in **AVRO** format.
- Dynamically creates folders based on the current execution date (`yyyy/mm/dd`).

### 3. **Data Restore Pipelines**
- Copies data of jobs, departments and hired employees from Azure Blob Storage in **AVRO** format to tables in Azure SQL database.
- If data exists in the destination tables, it overwrites them.
- Dynamically reads folders based on the current execution date (`yyyy/mm/dd`).

---

## Configuration & Setup

### Connect ADF to GitHub

1. Go to Azure Data Factory Studio →  *Manage* → *Git configuration*.
2. Set:
   - Repository type: **GitHub**
   - Collaboration branch: `develop`
   - Publish branch: `adf_publish`
3. Click **Apply** and import resources using *Import existing resource*.

### Publishing to Production

1. Save your changes in the `develop` branch using **Save All**.
2. Use **Publish** to generate ARM templates into the `adf_publish` branch.

---

