# AirBnB CDC Ingestion Pipeline

## Overview

This project implements a **Change Data Capture (CDC) ingestion pipeline** for AirBnB booking analytics using Azure cloud services. It demonstrates a real-world data engineering solution for processing customer and booking data in near real-time.

## Problem Statement

AirBnB generates massive volumes of booking and customer data that needs to be:
- **Captured in real-time** as bookings occur
- **Transformed and cleansed** for analytical use
- **Stored efficiently** in a data warehouse for business intelligence
- **Kept in sync** with source systems without full data reloads

Traditional batch ETL approaches fail to meet these requirements due to:
- High latency between data generation and availability
- Resource-intensive full table scans
- Inability to track incremental changes efficiently

## Solution

This pipeline solves these challenges using:

| Challenge | Solution |
|-----------|----------|
| Real-time booking capture | CosmosDB Change Feed for CDC events |
| Incremental customer updates | SCD Type-1 merge from ADLS |
| Efficient data sync | Upsert operations (no full reloads) |
| Automated processing | ADF pipelines with scheduled/event triggers |



## Tech Stack

- **Python** - Mock data generation
- **Azure Data Lake Storage (ADLS)** - Customer data source
- **Azure CosmosDB** - Booking transactions with Change Feed
- **Azure Data Factory** - ETL orchestration and pipelines
- **Azure Synapse Analytics** - Data warehouse
- **SQL** - Data transformations and table definitions

## Project Components

### 1. Data Generation
- `mock_data_in_cosmosdb.py` - Generates realistic booking data and streams to CosmosDB

### 2. Source Data
- `CustomerData/` - Sample customer CSV files simulating ADLS source

### 3. Data Warehouse Schema
- `synapse_table_creation.sql` - DDL for dimension, fact, and aggregation tables

## Data Model

### Dimension Table: `customer_dim`
Stores customer master data with fields like name, contact info, signup date, and account status.

### Fact Table: `bookings_fact`
Stores booking transactions with derived fields:
- `stay_duration` - Calculated nights
- `booking_year`, `booking_month` - Time dimensions
- `full_address` - Concatenated location

### Aggregation Table: `BookingCustomerAggregation`
Pre-computed metrics by country for reporting dashboards.

## Pipeline Features

1. **Hourly Customer Sync**
   - Reads new/updated customer CSVs from ADLS
   - Performs SCD Type-1 (overwrite) on `customer_dim`

2. **Real-time Booking Ingestion**
   - Captures CDC events via CosmosDB Change Feed
   - Transforms and enriches booking data
   - Upserts to `bookings_fact` table

3. **Automated Orchestration**
   - Scheduled triggers for batch processing
   - Event-based triggers for streaming data
   - Dependency management between pipelines

## Setup Instructions

1. **Azure Resources Required:**
   - Azure Data Lake Storage Gen2
   - Azure CosmosDB account
   - Azure Data Factory
   - Azure Synapse Analytics workspace

2. **Configure CosmosDB:**
   - Update `mock_data_in_cosmosdb.py` with your CosmosDB URL and key
   - Run the script to generate sample booking data

3. **Create Synapse Tables:**
   - Execute `synapse_table_creation.sql` in your Synapse workspace

4. **Deploy ADF Pipelines:**
   - Create linked services for ADLS, CosmosDB, and Synapse
   - Configure pipeline triggers

## Author

Varun Pruthviraj Patil

## License

This project is for educational and demonstration purposes.
