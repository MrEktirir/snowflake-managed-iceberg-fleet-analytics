# Snowflake-Managed Iceberg Fleet Analytics

An end-to-end data engineering project demonstrating how to build a fleet telemetry analytics pipeline using **Snowflake-managed Apache Iceberg tables**, Dynamic Tables, Python, DuckDB, and Apache Spark.

The project explores an open lakehouse architecture where Snowflake manages data ingestion, storage, and transformations while external query engines access Iceberg data through the Snowflake Horizon Catalog.

## Project Overview

This project simulates a vehicle fleet management scenario involving vehicle registration, sensor readings, maintenance records, and streaming telemetry events.

The primary objectives are to:

- Create and manage Apache Iceberg tables in Snowflake.
- Load structured and semi-structured fleet data.
- Simulate telemetry ingestion using Python.
- Build analytical transformations with Snowflake Dynamic Tables.
- Query Iceberg data using DuckDB and Apache Spark.
- Explore interoperability across multiple query engines.

## Technology Stack

| Technology | Purpose |
|---|---|
| Snowflake | Data platform, ingestion, and SQL transformations |
| Apache Iceberg | Open table format |
| Snowflake Horizon Catalog | Iceberg catalog integration |
| Snowflake Dynamic Tables | Declarative analytical transformations |
| Python | Telemetry simulation and external query integration |
| DuckDB | Lightweight analytical query engine |
| Apache Spark | Distributed data processing and Iceberg interoperability |
| SQL | Data modeling and analytics |
| GitHub Codespaces | Cloud-based development environment |

## Architecture

```text
Fleet Data Sources
       |
       |-- Vehicle Registry
       |-- Sensor Readings
       |-- Maintenance Logs
       |-- Simulated Telemetry Events
       |
       v
Snowflake
       |
       v
RAW Schema
       |
       |-- VEHICLE_REGISTRY
       |-- SENSOR_READINGS
       |-- MAINTENANCE_LOGS
       |-- VEHICLE_TELEMETRY_STREAM
       |
       v
Snowflake Dynamic Tables
       |
       v
ANALYTICS Schema
       |
       |-- TELEMETRY_ENRICHED
       |-- DAILY_FLEET_SUMMARY
       |
       v
Snowflake-Managed Apache Iceberg
       |
       v
Snowflake Horizon Catalog
       |
       +------------------+
       |                  |
       v                  v
     DuckDB           Apache Spark
       |                  |
       v                  v
  SQL Analytics      Iceberg Queries
```

## Repository Structure

```text
.
├── 01_create_database.sql
├── 02_create_iceberg_tables.sql
├── 03_create_dynamic_tables.sql
├── 04_load_sample_data.sql
├── 05_cleanup.sql
├── config.env.template
├── duckdb_interop.py
├── fleet_semantic_model.yaml
├── maintenance_log_001.json
├── maintenance_log_002.json
├── maintenance_log_003.json
├── setup.sh
├── spark_iceberg_interop.ipynb
├── stream_telemetry.py
└── README.md
```

## Pipeline Implementation

### 1. Snowflake Environment Setup

The project starts by provisioning the Snowflake database and schemas used for fleet data processing.

The environment includes:

- `FLEET_DB` — Main project database.
- `RAW` — Source data and telemetry tables.
- `ANALYTICS` — Transformed analytical datasets.

The SQL scripts define the database objects and Iceberg tables required by the pipeline.

### 2. Apache Iceberg Tables

Snowflake-managed Iceberg tables store fleet-related data.

The RAW layer contains four tables:

| Table | Description |
|---|---|
| `VEHICLE_REGISTRY` | Vehicle registration and fleet metadata |
| `SENSOR_READINGS` | Vehicle sensor measurements |
| `MAINTENANCE_LOGS` | Maintenance events and records |
| `VEHICLE_TELEMETRY_STREAM` | Simulated vehicle telemetry events |

### 3. Data Ingestion

Sample fleet data is loaded through the project's SQL scripts.

Maintenance records are provided as JSON files, demonstrating ingestion of semi-structured data into Snowflake.

The Python telemetry simulator generates vehicle events and sends them to the Snowflake environment.

During development, a telemetry simulation was executed for approximately five minutes, generating roughly 600 events.

### 4. Dynamic Table Transformations

Snowflake Dynamic Tables implement the analytical transformation layer.

Two analytical datasets are created:

**TELEMETRY_ENRICHED**

Enriches telemetry events for downstream analysis.

**DAILY_FLEET_SUMMARY**

Provides daily aggregated fleet analytics.

The project also demonstrates manually refreshing Dynamic Tables to update analytical results.

### 5. DuckDB Interoperability

The `duckdb_interop.py` script demonstrates querying Snowflake-managed Iceberg data through DuckDB.

The integration uses Snowflake Horizon Catalog and programmatic authentication.

The interoperability exercise includes querying vehicle registration data and performing analytical queries on sensor measurements.

### 6. Apache Spark Interoperability

The `spark_iceberg_interop.ipynb` notebook demonstrates Apache Spark integration with Snowflake-managed Iceberg tables.

The tested environment includes:

- Apache Spark 4.0
- Apache Iceberg 1.10.1
- Scala 2.13
- Java 21

The notebook demonstrates:

- Configuring Spark with Iceberg catalog integration.
- Discovering available Iceberg tables.
- Reading vehicle registry data.
- Querying telemetry datasets.
- Accessing semi-structured telemetry fields with Spark SQL.

The integration was tested in GitHub Codespaces.

## Getting Started

### Prerequisites

- Snowflake account with appropriate permissions.
- Python environment.
- Snowflake CLI.
- DuckDB for external analytical queries.
- Java 21 and Apache Spark for Spark interoperability.
- Snowflake authentication credentials.

### 1. Clone the Repository

```bash
git clone https://github.com/<your-username>/snowflake-managed-iceberg-fleet-analytics.git
cd snowflake-managed-iceberg-fleet-analytics
```

### 2. Configure Credentials

Use the provided configuration template:

```bash
cp config.env.template config.env
```

Populate the configuration file with the required Snowflake connection settings.

Do not commit credentials or access tokens to Git.

### 3. Review the Setup Scripts

The SQL files are organized in execution order:

```text
01_create_database.sql
02_create_iceberg_tables.sql
03_create_dynamic_tables.sql
04_load_sample_data.sql
```

The repository also provides `setup.sh` to automate the setup workflow.

### 4. Simulate Telemetry

Use the Python telemetry simulator:

```bash
python stream_telemetry.py
```

Review the script's configuration before running the simulation.

### 5. Explore DuckDB Interoperability

```bash
python duckdb_interop.py
```

Configure the required catalog authentication before running the script.

### 6. Explore Spark Interoperability

Open:

```text
spark_iceberg_interop.ipynb
```

Configure the Spark environment and Snowflake Horizon Catalog credentials, then execute the notebook cells.

## Security and Resource Management

The project follows basic development security practices:

- Local environment configuration is excluded from Git using `.gitignore`.
- Authentication tokens are not intended to be stored in the repository.
- Temporary programmatic access tokens can be disabled after testing.
- Snowflake compute resources can be removed after the exercises are complete.

The development environment was cleaned up after the interoperability demonstrations, including disabling the test access token and dropping the temporary Snowflake warehouse.

## Key Learnings

This project provided hands-on experience with:

- Snowflake-managed Apache Iceberg tables.
- Open lakehouse architecture.
- Structured and semi-structured data ingestion.
- Python-based telemetry simulation.
- Declarative data transformations using Dynamic Tables.
- Cross-engine interoperability with DuckDB and Apache Spark.
- Snowflake Horizon Catalog integration.
- Cloud-based development using GitHub Codespaces.
- Authentication and compute resource lifecycle management.

## Reference

Based on Snowflake's official developer quickstart materials covering Snowflake-managed Apache Iceberg tables and open lakehouse interoperability.

## Project Status

**Completed — Educational / Portfolio Project**

The Snowflake ingestion and transformation workflow, DuckDB interoperability, and Apache Spark interoperability exercises were completed and tested.