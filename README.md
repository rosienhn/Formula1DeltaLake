
# Building Cloud Data platform for Formula 1 data using Databricks, Delta lake and Azure Data Factory

## Table of Contents
* 1.Overview
* 2.Project Requirements
* 3.Solution Architecture
* 4.Techinal requirements & Environment Set Up
* 5.Repository Structure and Run Instructions

## Overview

The project aims to build a data pipeline for historical and up-to-date Formula1 data. Using databricks, data must be ingested into a Data lake/Delta lake in parquet/delta formats and schema, transformed into tables for reporting and analysis, and made available for machine learning and SQL workloads. The project also involves determining the most dominant drivers and teams for producing BI reports. Finally, pipelines must be scheduled, monitored, and alerts set up for pipeline failures, and the pipeline must comply with user privacy legislation.

Accessing Formula One data is possible through the Ergaste API, which provides data for all races from 1950 onwards. This database contains an API that can return data in XML or JSON formats, and the database tables can also be downloaded in CSV format. Data can be received as a full dataset, but I chose to process them incrementally in a hybrid scenario to strengthen my understanding of real-world scenarios.

Generally, Formula One races happen on Sundays, but not every Sunday. There are usually only 20 to 24 weeks in a year in which races happen, and in the other weeks, there are no races. To set up incremental loading in this project, I downloaded the file in CSV format then used Python to split each file into smaller files, each containing a specific number of race IDs based on event dates.

In this scenario, it is assumed that on day one (which is a Sunday), we will receive the first file containing data for all prior races up to race 1096 on the cutover date of 21-11-2022. On 20-03-2023, we will receive data for the next two races, which are race IDs 1098-1099. Race 1097 data is not available on Ergast API. On 2/4/2023, we will receive the data for the lastest race 1100, which is the "Australian Grand Prix" that took place on 2/4/2023. After that, pipeline will be scheduled to update every Sunday.

1. Data is stored in DataLake Azure ADLS Gen2, organized by event date.
2. Using Databricks, the data must be ingested into Data lake/Delta lake in parquet/delta formats, transformed into tables for reporting and analysis, and made available for machine learning and SQL workloads. The project also involves determining the most dominant drivers and teams for producing BI reports.
3. Finally, pipelines must be scheduled, monitored, and alerts set up for pipeline failures, and the pipeline must comply with user privacy legislation.

It's recommended to go through some documentation before proceeding with this project. The Ergast Developer API website provides extensive information about the various tables and their relationships. To get a better understanding, it's beneficial to review the ERD diagram and Ergast Database User Guide

## Project Requirements

### Data Ingestion Requirements
The task is to load historical F1 data into a data lake ADLS Gen2 in various formats and apply the appropriate schema to the data. The ingested data must have audit columns such as the ingested date and the source, and must be stored in columnar format using Parquet/Delta files. The ingested data must be made available for various workloads, including machine learning, further transformation, and analytical workloads via SQL. Additionally, the ingestion logic must be designed to handle incremental data loads.

### Data Transformation Requirements
The requirements include combining required data items into transformed table for reporting/analysis purposes, adding audit columns to the transformed tables, and storing the transformed data in columnar format using Parquet/Delta files. The transformed data must be made available for various workloads, such as machine learning, BI reporting, and SQL analytics. Finally, the transformation logic must be designed to handle incremental loads.

### Data Analytical Requirements
The tasks include determining the most dominant drivers and teams over the last decade and all-time in Formula1, ranking them by dominance, and creating visualizations to show dominant periods and level of performance. Finally, dashboards must be created in Azure Databricks.

### Data BI Reports Requirements
* Produce driver and constructor standings for current and past years.
* There is a predetermined set of data elements that should be used for BI reporting. These data items could include specific metrics, dimensions, or other relevant information that is necessary for the analysis. The list of data items could be based on the organization's needs and requirements, industry standards, or other factors. By using a defined list of data items, the BI reporting can be standardized and consistent across different reports and analyses.
### Scheduling Requirements
* Schedule pipelines to run at 11.30 p.m. every Sunday.
* Monitor pipeline status and re-run failed pipelines and set up alerts on failures.
* Delete individual records from the Data Lake to satisfy user privacy legislation (e.g. GDPR).
* Enable time travel and ability to query data based on time.
* Roll back data to previous versions in case of issues.

## Technical Requirements and Environment setup:
* Azure Data Lake Storage Gen2
* Azure Data Factory
* Azure Databricks 
  * Creating, configuring and monitoring Databricks clusters
  * Mounting Azure Storage in Databricks using secrets stored in Azure Key Vault
  * Using Delta Lake to implement a solution using Lakehouse architecture
* Azure SQL Database
* Azure Key Vault

### Languages
* Spark SQL
* PySpark

### Technologies
* Data Pipeline
* Spark (Spark streaming and Spark Machine Learning were not covered in this project)

## Repository Structure and Run Instructions

