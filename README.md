# Sparkify Airflow Data Pipeline

## Overview

This project implements a data pipeline for Sparkify using Apache Airflow to implement and orchestrate the running of an ETL pipeline.

The source, JSON logs,  and destination data, warehouse, are hosted on AWS,
in S3 and Redshift respectively. The data warehouse leverages a star schema
to allow the Sparkify Analytics team to readily run queries to surface data 
about user activity on their app, such as what are the users favorite songs.

## Structure

The project contains the following components:

```bash
├── README.md - This file.
├── dags # Python script containing the tasks and depencdencies of the DAG
	├── udac_example_dag.py # Python script containing the tasks and depencdencies of the DAG
	├── create_tables.py # contains DDL SQL queries defining the data warehouse schema.
├── plugins
	├── helpers
		├── __init__.py
		├── sql_queries.py # Defining prepared and reusable SQL queries
    ├── operators
		├── __init__.py
		├── data_quality.py # with `DataQualityOperator`, running data quality check by passing an SQL query and expected result as arguments, 									failing if the results don't match.
		├── load_dimension.py # with `LoadDimensionOperator`, loading a dimension table from data in the staging table(s).
		├── load_fact.py # with `LoadFactOperator`, loading a fact table from data in the staging table(s).
		└── stage_redshift.py # with `StageToRedshiftOperator`, copying JSON data from S3 to staging tables in the Redshift data warehouse
```

## Configuration

The following connection are expected 

* `aws_credentials`: AWS IAM user with read access to S3 bucket storing input data
* `redshift`: connection object with all attributes necessary to connect to Redshift cluster

* Make sure to add the following Airflow connections:
    * AWS credentials
    * Connection to Postgres database