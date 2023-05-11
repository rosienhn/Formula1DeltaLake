
# Building Cloud Data platform for Formula 1 data using Azure Data Factory, Databricks and Lakehouse architecture.

## Overview

This project aims to build a data pipeline which is scheduled and triggered by Azure Data Factory pipelines for historical and up-to-date Formula1 data. Databricks and Delta Lake were used to implement a solution using Lakehouse architecture.

To access Formula One data, the Ergast API was used to retrieve data for all races dating back to 1950. The API offers data in XML or JSON formats, and CSV-formatted database tables can also be downloaded. Although the entire dataset can be obtained at once, I opted to process it incrementally to better understand real-world scenarios.

Formula One races usually take place on Sundays, but not every Sunday is reserved for a race. Races are typically scheduled for only 20 to 24 weeks per year, with no races scheduled during the remaining weeks. To implement incremental loading, I downloaded a zip file from the Ergast API and extracted its contents into CSV format. Each CSV file contained a single database table, which I split into smaller segments based on race IDs using Python.

For this project, I assumed that the first data containing data for all races up to race 1096 on the cutover date of December 24, 2022, will be received on that day, which is a Sunday. Data for the next two races, raceIDs 1098-1099, will be received on March 19, 2023 (data for race 1097 is not available on the Ergast API). On April 2, 2023, data for the latest race, race ID 1100, which is the "Australian Grand Prix" held on that day, will be received. After that, data will be updated every Sunday.
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

## Environment setup:
* Azure subcription
* Azure Data Lake Storage Gen2
* Azure Data Factory
* Azure SQL Database
* Azure Key Vault
* Azure Databricks
** Create, configure and monitor Databricks clusters
** Configure Databricks to use the ABFS driver to read and write data stored on Azure Data Lake Storage Gen2 using secrets stored in Azure Key Vault. A Pyspark function was used to mount the AZure ADLS Gen 2 to Databricks.
** Use Delta Lake to implement a solution using Lakehouse architecture

### Languages
* Spark SQL
* PySpark

### Technologies
* Data Pipeline
* Spark (Spark streaming and Spark Machine Learning were not covered in this project)

This project takes its inspiration from 2 Udemy courses: https://www.udemy.com/course/learn-azure-data-factory-from-scratch/ and https://www.udemy.com/course/azure-databricks-spark-core-for-data-engineers/
