
# Building Cloud Data platform for Formula 1 data using Azure Data Factory, Databricks and Lakehouse architecture.

## Overview

The project aims to build a data pipeline for historical and up-to-date Formula1 data using Azure Data Factory. Databricks and  Delta Lake were used to implement a solution using Lakehouse architecture.
Using databricks, data must be ingested into a Delta lake in delta formats, transformed into tables for reporting and analysis, and made available for machine learning/reporting and copy to Azure SQL database. Data pipelines must be scheduled, monitored, and alerts set up for pipeline failures.

Accessing Formula One data is possible through the Ergaste API, which provides data for all races from 1950 onwards. This database contains an API that can return data in XML or JSON formats, and the database tables can also be downloaded in CSV format. Data can be received as a full dataset, but I chose to process them incrementally in a hybrid scenario to strengthen my understanding of real-world scenarios.

Generally, Formula One races happen on Sundays, but not every Sunday. There are usually only 20 to 24 weeks in a year in which races happen, and in the other weeks, there are no races. To set up incremental loading in this project, I downloaded the zip file from Ergast API, extract all files into CSV format. Each CSV file contains a single database table. Python was used later to split files into smaller files, each containing a specific number of race IDs based on event dates.

In this scenario, it is assumed that on day one (which is a Sunday), we will receive the first file containing data for all prior races up to race 1096 on the cutover date of 21-11-2022. On 20-03-2023, we will receive data for the next two races, which are race IDs 1098-1099. Race 1097 data is not available on Ergast API. On 2/4/2023, we will receive the data for the lastest race 1100, which is the "Australian Grand Prix" that took place on 2/4/2023. After that, pipeline will be scheduled to update every Sunday 11:30pm.
| File name| Desciption | 
| -------- | -------- | 
| Circuits.csv, Drivers.csv, Constructors.csv, Races.csv| contains all the information related to the circuits, the drivers, the teams and the races. It was assumed that those information remains consistent throughout the race years. | 
| Lap_times.csv, Pitstops.csv, Results.csv| The CSV files has been divided into three smaller files. The first two contain race information from race_id 1 to 1096 and from 1098 to 1099, respectively. The third file includes information solely on the 1100th race. | 
| Qualifying.csv | The CSV file was converted into three smaller json files. The first two contain race information from race_id 1 to 1096 and from 1098 to 1099, respectively. The third file includes information solely on the 1100th race. |  |

The new dataset after splitting was uploaded into DataLake Azure ADLS Gen2, organized by 3 different folder 2022-12-24, 2023-03-19 and 2023-04-02.


1. Data is stored in DataLake Azure ADLS Gen2, organized by event date.
2. Using Databricks, the data must be ingested into Data lake in delta formats, transformed into tables for reporting and analysis, made available for machine learning and copied to Azure SQL database. 
3. Finally, pipelines must be scheduled, monitored, and alerts set up for pipeline failures.

It's recommended to go through some documentation before proceeding with this project. The Ergast Developer API website provides extensive information about the various tables and their relationships. To get a better understanding, it's beneficial to review the ERD diagram and Ergast Database User Guide

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
