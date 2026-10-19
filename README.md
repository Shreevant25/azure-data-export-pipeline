# Azure Data Export Pipeline

**Author:** Shreevant Jaiswal  
**GitHub:** [Shreevant25](https://github.com/Shreevant25)  
**Repository:** [azure-data-export-pipeline](https://github.com/Shreevant25/azure-data-export-pipeline)

## Table of Contents

1. [Project Overview](#1-project-overview)
2. [Project Objectives](#2-project-objectives)
3. [Architecture](#3-architecture)
4. [Technologies Used](#4-technologies-used)
5. [Source Data](#5-source-data)
6. [Output File Formats](#6-output-file-formats)
7. [How the Pipeline Works](#7-how-the-pipeline-works)
8. [Repository Structure](#8-repository-structure)
9. [Prerequisites](#9-prerequisites)
10. [Setup and Deployment](#10-setup-and-deployment)
11. [Verified Results](#11-verified-results)
12. [Monitoring and Troubleshooting](#12-monitoring-and-troubleshooting)
13. [Security Considerations](#13-security-considerations)
14. [Limitations of the Current Implementation](#14-limitations-of-the-current-implementation)
15. [Future Enhancements](#15-future-enhancements)
16. [Learning Outcomes](#16-learning-outcomes)
17. [Screenshots and Evidence](#17-screenshots-and-evidence)
18. [Author](#18-author)
19. [Conclusion](#19-conclusion)

## 1. Project Overview

The **Azure Data Export Pipeline** is a cloud-based data integration project that demonstrates how structured data stored in an Azure SQL Database can be exported into multiple file formats using Azure Data Factory and stored in Azure Blob Storage.

The pipeline reads student records from a SQL table and creates three output files: CSV-style delimited text, Apache Parquet, and Apache Avro. This project demonstrates data movement, cloud storage, pipeline orchestration, dataset configuration, linked services, and execution monitoring.

The goal is to understand how cloud services can automate data export workflows and make the same source data available in formats suited to different downstream applications.

## 2. Project Objectives

- Store structured student data in Azure SQL Database.
- Connect Azure SQL Database to Azure Data Factory.
- Configure source and destination linked services.
- Define datasets for the SQL source and three output formats.
- Create a pipeline that exports data into text, Parquet, and Avro.
- Store the generated files in Azure Blob Storage.
- Execute the pipeline and monitor activity results.
- Verify that the expected output files were created.
- Maintain project templates, documentation, exported samples, and screenshots in GitHub.

## 3. Architecture

The project follows a simple cloud data integration architecture.

```text
             Azure SQL Database
                  StudentDB
                     |
                     | Students table
                     v
             Azure Data Factory
              ExportToAllFormats
                     |
             +-------+-------+
             |       |       |
             v       v       v
            CSV   Parquet   Avro
             |       |       |
             +-------+-------+
                     |
                     v
            Azure Blob Storage
             export-data-task-1
```

### Architecture components

| Component | Responsibility |
|---|---|
| Azure SQL Database | Stores the source student records. |
| Azure Data Factory | Orchestrates the export workflow. |
| Azure Integration Runtime | Executes the data movement activities. |
| Azure Blob Storage | Stores the generated output files. |
| Linked services | Define connectivity to the SQL database and Blob Storage. |
| Datasets | Describe the source table and destination data formats. |
| Copy activities | Move data from the source to each destination format. |

## 4. Technologies Used

- **Microsoft Azure:** Cloud platform used to deploy and operate the project.
- **Azure SQL Database:** Relational database containing the student data.
- **Azure Data Factory V2:** Cloud data integration and orchestration service.
- **Azure Blob Storage:** Object storage for the exported files.
- **Azure Integration Runtime:** Data movement execution environment.
- **JSON:** Configuration format for pipelines, datasets, and linked services.
- **Azure CLI:** Command-line management of Azure resources.
- **Git:** Version control for project files.
- **GitHub:** Repository hosting and project documentation.
- **VS Code / Terminal:** Local development and command-line workflow.

## 5. Source Data

The source database is named `StudentDB`, and the source table is named `Students`.

The table contains five sample student records and the following columns:

| Column | Data type | Description |
|---|---|---|
| `StudentID` | INT | Unique student identifier |
| `StudentName` | VARCHAR(100) | Student name |
| `Age` | INT | Student age |
| `Course` | VARCHAR(50) | Course or department |
| `Marks` | INT | Student marks |

Example source data:

| StudentID | StudentName | Age | Course | Marks |
|---:|---|---:|---|---:|
| 1 | Rahul | 20 | CSE | 85 |
| 2 | Priya | 21 | IT | 90 |
| 3 | Amit | 22 | CSE | 78 |
| 4 | Neha | 20 | ECE | 88 |
| 5 | Arjun | 21 | IT | 82 |

These records were used to test the export workflow. The project is a demonstration using sample data, not a production student information system.

## 6. Output File Formats

The pipeline exports the same source records into three formats.

### 6.1 CSV-style delimited text

**File:** `dbo.Students.txt`

Delimited text is easy to inspect and is supported by spreadsheets, scripts, and many data-processing tools.

**Common use cases:**
- Simple data exchange
- Spreadsheet reporting
- Manual inspection
- Importing data into other applications

**Implementation note:** The current pipeline configuration uses a `.txt` file extension for the delimited-text sink. The file should therefore be described as CSV-style or delimited text unless its delimiter and encoding have been independently verified. Rename the file to `.csv` only after confirming its contents and updating the pipeline configuration if necessary.

### 6.2 Apache Parquet

**File:** `dbo.Students.parquet`

Parquet is a column-oriented storage format commonly used in analytics and data engineering.

**Common use cases:**
- Analytical processing
- Large datasets
- Column-based queries
- Data lake and big-data workflows

### 6.3 Apache Avro

**File:** `dbo.Students.avro`

Avro is a row-oriented data serialization format that stores data according to a schema.

**Common use cases:**
- Data exchange between systems
- Schema-aware serialization
- Event and message processing
- Data engineering workflows

**Why use multiple formats?**

Different consumers have different requirements. Delimited text is convenient for simple inspection, Parquet is useful for analytical workloads, and Avro supports schema-based data serialization.

## 7. How the Pipeline Works

The Azure Data Factory pipeline is named `ExportToAllFormats`.

### Step 1: Establish the source connection

A SQL linked service connects Azure Data Factory to Azure SQL Database. The `SQL_Students` dataset represents the source table and its schema.

### Step 2: Establish the destination connection

A Blob Storage linked service connects Azure Data Factory to the storage account. The destination datasets identify the output container and the appropriate file format.

### Step 3: Start pipeline execution

The pipeline contains a `StartHere` Wait activity. It is followed by three copy activities that run independently.

### Step 4: Export to delimited text

`CopyToCSV` reads the source records and writes the delimited-text output to Blob Storage.

### Step 5: Export to Parquet

`CopyToParquet` reads the same source data and writes a Parquet file.

### Step 6: Export to Avro

`CopyToAvro` reads the source data and writes an Avro file.

### Step 7: Monitor execution

Azure Data Factory records the status, duration, row counts, and errors for each activity. The activity-run results can be inspected to verify whether the exports completed successfully.

### Step 8: Verify output files

The Blob Storage container is queried to confirm that all three expected files exist.

## 8. Repository Structure

The repository contains the following project assets:

```text
azure-data-export-pipeline/
├── README.md
├── Samiksha_Azure_Data_Export_Project_Report.pdf
├── Exported files/
│   ├── dbo.Students.avro
│   ├── dbo.Students.parquet
│   └── dbo.Students.txt
├── json-templates/
│   ├── dataset/
│   │   ├── SQL_Students.json
│   │   ├── CSV_Students.json
│   │   ├── Parquet_Students.json
│   │   └── Avro_Students.json
│   ├── linkedService/
│   │   ├── AzureSql_LinkedService_task_1.json
│   │   └── BlobStorage_LinkedService_task_1.json
│   ├── pipeline/
│   │   └── ExportToAllFormats.json
│   ├── diagnostic.json
│   └── info.txt
└── pipeline Screenshots/
    ├── Datasets.jpg
    ├── Linked services .jpg
    ├── Student_DB_quiry-editor.jpg
    ├── all  converted files saved in container .jpg
    ├── monitor-results.jpg
    └── pipelines published successfully.jpg
```

*The structure above reflects the files included in the project directory. Screenshot filenames are retained as they currently exist in the repository.*

## 9. Prerequisites

Before recreating the project, you need:

- An Azure account with permission to create and manage the required resources.
- An Azure subscription with sufficient available quota and budget.
- Azure SQL Database and a source table containing sample records.
- An Azure Storage account and Blob container.
- An Azure Data Factory instance.
- Azure CLI installed and authenticated.
- Git installed for repository management.
- Appropriate permissions to configure the SQL and Storage connections.

Resource availability, quotas, pricing, and supported regions may vary by subscription.

## 10. Setup and Deployment

The repository contains JSON configuration templates. These templates are intended to be adapted to your own Azure resources; they should not be assumed to work unchanged with another subscription.

### Step 1: Clone the repository

```bash
git clone https://github.com/Shreevant25/azure-data-export-pipeline.git
cd azure-data-export-pipeline
```

### Step 2: Sign in to Azure

```bash
az login
az account show --output table
```

If you have access to multiple subscriptions, select the correct subscription:

```bash
az account set --subscription "<YOUR_SUBSCRIPTION_ID>"
```

### Step 3: Inspect the project templates

```bash
find json-templates -maxdepth 3 -type f | sort
```

Review the pipeline, dataset, and linked-service JSON files before deployment.

### Step 4: Configure your Azure resources

Create or identify the following resources:

1. Resource group
2. Azure SQL logical server and database
3. Azure SQL `Students` table
4. Azure Storage account and Blob container
5. Azure Data Factory instance

Use your own resource names, region, database credentials, and storage configuration.

### Step 5: Configure linked services

Update the SQL linked service with your SQL server and database details. Update the Blob Storage linked service with your storage account configuration.

Do not commit passwords, access keys, connection strings containing secrets, or access tokens to GitHub.

Use secure parameterization, Azure Key Vault, or managed identity where supported and appropriate.

### Step 6: Configure datasets

Verify that the SQL dataset refers to the correct `Students` table and that the destination datasets point to the correct Blob container and file formats.

Check that the source schema includes:

- `StudentID`
- `StudentName`
- `Age`
- `Course`
- `Marks`

### Step 7: Deploy the pipeline

Deploy the linked services, datasets, and pipeline using the Azure Data Factory authoring workflow or supported Azure CLI/ARM REST operations.

Ensure that the resource names and dependencies match the resources in your own subscription.

### Step 8: Execute the pipeline

Run the pipeline manually from Azure Data Factory or use the supported command-line workflow configured for your environment.

### Step 9: Verify the results

Check the pipeline-run status, inspect the three copy activities, and confirm that the expected files appear in Blob Storage.

The commands below illustrate how to inspect a deployed pipeline. Replace the example resource names and run ID with values from your own deployment.

```bash
az datafactory pipeline list \
  --resource-group "<RESOURCE_GROUP>" \
  --factory-name "<DATA_FACTORY_NAME>" \
  --output table
```

Inspect a pipeline run:

```bash
az datafactory pipeline-run show \
  --resource-group "<RESOURCE_GROUP>" \
  --factory-name "<DATA_FACTORY_NAME>" \
  --run-id "<PIPELINE_RUN_ID>" \
  --output json
```

## 11. Verified Results

The implemented project was executed successfully in Azure.

| Verification | Observed result |
|---|---|
| Pipeline | `ExportToAllFormats` |
| Pipeline-run status | `Succeeded` |
| Start activity | `StartHere` — Succeeded |
| Delimited-text activity | `CopyToCSV` — Succeeded |
| Parquet activity | `CopyToParquet` — Succeeded |
| Avro activity | `CopyToAvro` — Succeeded |
| Rows read per copy activity | 5 |
| Rows copied per copy activity | 5 |
| Files written per copy activity | 1 |
| Reported activity errors | None |
| Destination container | `export-data-task-1` |

### Verified output files

| File | Reported size |
|---|---:|
| `dbo.Students.txt` | 154 bytes |
| `dbo.Students.parquet` | 1,085 bytes |
| `dbo.Students.avro` | 441 bytes |

File sizes are the values observed during the verified run. They can change when the source data, schema, serialization settings, or pipeline configuration changes.

The verification confirms that each copy activity reported five rows copied and that all three output files were present in the configured container. The activity logs reported data consistency verification as `NotVerified`, so an independent content-level comparison was not established by that setting alone.

## 12. Monitoring and Troubleshooting

### Pipeline status is failed

- Inspect the failed activity and its error message.
- Confirm that the linked service points to the correct resource.
- Check SQL credentials or managed identity permissions.
- Verify that the SQL firewall and network configuration allow the required connections.
- Confirm that the destination storage account and container are accessible.

### Output files are missing

- Verify the latest pipeline-run ID.
- Inspect each copy activity's status and output.
- Confirm the destination container and configured file paths.
- Check the storage account and authentication method.
- Ensure that the pipeline is using the expected datasets.

### SQL connection fails

- Verify the SQL server name and database name.
- Check authentication and database permissions.
- Review the SQL firewall and networking settings.
- Confirm that the source table exists and is accessible.

### File extension does not match expectations

The current delimited-text sink is configured with the `.txt` extension. If a `.csv` extension is required, update the sink's file-extension setting and rerun the pipeline after verifying the intended delimiter and text format.

### Deployment fails

- Validate the JSON syntax.
- Check resource names and API compatibility.
- Confirm that the required Azure resource providers are registered.
- Verify that the authenticated account has the necessary permissions.
- Avoid copying encrypted credentials or resource identifiers from another subscription's templates without reviewing them.

## 13. Security Considerations

This project demonstrates data movement, but a production implementation requires additional security controls.

- Never publish SQL passwords, storage account keys, SAS tokens, or access tokens.
- Prefer managed identity and least-privilege access where supported.
- Store secrets in Azure Key Vault rather than directly in configuration files.
- Restrict database network access and storage permissions.
- Avoid using real personal or confidential student data in public demonstrations.
- Review Git history and repository files for previously committed secrets before making a repository public.
- Rotate any credential that has been exposed.
- Monitor access, execution failures, and unexpected data movement.

**Important:** Removing a secret from the latest version of a file does not remove it from Git history. Exposed credentials should be rotated, and repository history should be reviewed separately.

## 14. Limitations of the Current Implementation

The current version is a small demonstration pipeline.

- It uses five sample records rather than a large production dataset.
- It exports data into three formats but does not implement downstream analytics.
- The pipeline is manually executed; recurring scheduled or event-driven triggers are potential extensions.
- The observed activity logs did not establish independent data consistency verification.
- Automated deployment, testing, alerting, and recovery have not been demonstrated as part of the verified workflow.
- The current delimited-text output uses a `.txt` extension.

These limitations define opportunities for further development rather than invalidating the successful export workflow.

## 15. Future Enhancements

Possible improvements include:

1. **Scheduled execution:** Add a recurring trigger to automate exports.
2. **Parameterization:** Make server names, database names, containers, and output paths configurable.
3. **Secure authentication:** Use managed identity and Azure Key Vault where appropriate.
4. **Validation:** Compare source and destination row counts and validate exported content.
5. **Error handling:** Add retry policies, failure notifications, and recovery procedures.
6. **Monitoring:** Integrate Azure Monitor and alerts for failed runs.
7. **Incremental exports:** Export only new or modified records for larger datasets.
8. **Partitioning:** Organize outputs by date or another useful partition key.
9. **CI/CD:** Automate validation and deployment through a version-controlled release workflow.
10. **Data analytics:** Load the exported Parquet or Avro files into a downstream analytical platform.

## 16. Learning Outcomes

Through this project, I explored:

- Cloud-based data integration using Azure.
- Relational data storage with Azure SQL Database.
- Pipeline orchestration using Azure Data Factory.
- Linked services and datasets.
- Exporting structured data into multiple formats.
- Storing generated files in Azure Blob Storage.
- Monitoring pipeline and activity execution.
- Managing project templates and documentation with Git and GitHub.
- Using command-line tools to work with Azure resources.

## 17. Screenshots and Evidence

The `pipeline Screenshots/` directory contains evidence associated with the project, including:

- Dataset configuration
- Linked-service configuration
- SQL query-editor data
- Published pipeline
- Pipeline monitoring results
- Generated files in the Blob container

These screenshots help demonstrate the configuration and execution process. They should be reviewed before publication to ensure they do not reveal credentials, access tokens, or sensitive account information.

## 18. Author

**Shreevant Jaiswal**  
GitHub: [@Shreevant25](https://github.com/Shreevant25)

This project helped me explore cloud-based data pipelines and real-world data integration using Microsoft Azure. By connecting a SQL data source to Azure Data Factory and exporting records into multiple formats, I gained practical experience with cloud storage, pipeline configuration, execution monitoring, and version control.

## 19. Conclusion

The Azure Data Export Pipeline demonstrates an end-to-end cloud data export workflow. Student records stored in Azure SQL Database were read by Azure Data Factory and exported to delimited text, Parquet, and Avro files in Azure Blob Storage.

The pipeline run and all three copy activities were verified as successful, with five rows copied by each activity and one output file written per format.

The project provides a foundation for extending a simple data export workflow into a more automated, secure, monitored, and scalable cloud data integration solution.
