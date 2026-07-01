# OpenAQ Data Lakehouse Pipeline 🌍💧

An end-to-end Data Engineering pipeline that extracts air quality telemetry from the OpenAQ API and builds a Modern Data Lakehouse using **Python, Delta Lake, and MinIO (S3-compatible storage)**.

## 🏗️ Architecture (Medallion Approach)

1. **🥉 Bronze Layer (Ingestion):** - **Locations:** Full Load (Overwrite) of static metadata regarding physical monitoring stations.
   - **Measurements:** Stateful Incremental Load of dynamic time-series pollution readings (e.g., PM10). Built-in rate limiting and data quality gates to drop inactive sensors.
2. **🥈 Silver Layer (Processing):** - Data cleaning (deduplication, null handling).
   - Dimensional enrichment (LEFT JOIN between facts and dimensions).
   - Strict type casting and string normalization.
   - **Partitioning:** Stored in Parquet/Delta format, physically partitioned by `year` to optimize Big Data OLAP queries and prevent the "Small File Problem".
3. **🥇 Gold Layer (Business Aggregations):** - Aggregated metrics grouped by Date and Location.
   - Pre-calculated KPIs: Daily Averages, Min/Max peaks, Anomaly Counts, and Data Quality percentages.
   - Derivation of a business-ready `air_quality_index` (GOOD, MODERATE, POOR).
   - Optimized for direct connection to BI tools like Power BI.

## 🛠️ Tech Stack
* **Language:** Python 3.10
* **Storage:** MinIO (Local S3 Cloud)
* **Table Format:** Delta Lake (ACID transactions, Time Travel, UPSERT/MERGE capabilities)
* **Data Processing:** Pandas & PyArrow

## 🚀 How to Run Locally
1. Clone this repository.
2. Create a `.env` file based on the provided `.env.example`.
3. Insert your MinIO credentials and OpenAQ API Key.
4. Execute the Jupyter Notebook `FacundoArnaudo_Full_TP.ipynb` sequentially.
