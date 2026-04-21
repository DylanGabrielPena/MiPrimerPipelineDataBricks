# 🍷 Wine Data Pipeline – Databricks (Delta Lake)

## 📌 Overview

This project implements a data pipeline in Databricks using PySpark and Delta Lake. It ingests wine datasets, applies transformations, and stores the results in a structured Silver layer.

The pipeline is **externally configurable via JSON**, enabling reuse without modifying the core code.

---

## 🧱 Architecture

```
Config (JSON) → Ingestion → Transformation → Delta Table (Silver)
```

* **Source**: Public Databricks datasets
* **Processing**: PySpark
* **Storage**: Delta Lake (managed table)
* **Configuration**: External JSON file

---

## ⚙️ Configuration

The pipeline reads parameters from a JSON config file:

```json
{
  "catalog": "workspace",
  "schema": "silver",
  "table": "vinos_limpios"
}
```

📍 Recommended location:

```
/Volumes/workspace/config/files/config.json
```

### Why configuration?

Decoupling configuration from code allows:

* Reusability across environments
* Flexibility without code changes
* Cleaner pipeline design

---

## ▶️ How to Run

1. Upload the `config.json` file to:

   ```
   /Volumes/workspace/config/files/config.json
   ```

2. Execute the notebook in Databricks

3. The output table will be available at:

   ```
   <catalog>.<schema>.<table>
   ```

Example:

```
workspace.silver.vinos_limpios
```

---

## 🔄 Data Processing

### Ingestion

* `winequality-white.csv`
* `winequality-red.csv`

### Transformations

* Add `type` column (white / red)
* Union both datasets
* Round `alcohol` to 2 decimals
* Generate `id_vino` identifier
* Normalize column names (remove spaces)
* Null value analysis

---

## 💾 Data Storage

```python
df.write \
  .format("delta") \
  .mode("overwrite") \
  .option("overwriteSchema", "true") \
  .saveAsTable(target_path)
```

* Output is stored as a **Delta Table**
* Table is dynamically defined via config

---

## 🧪 Exploratory Analysis

The notebook includes basic visualizations:

* Wine quality distribution by type
* Alcohol vs quality relationship

---

## ⚠️ Considerations

* The pipeline uses `overwrite` mode → table is fully replaced on each run
* No incremental processing is implemented
* `display()` is used for interactive exploration (not production-ready)
* `row_number()` without partitioning is not scalable for large datasets

---

## 📈 Future Improvements

* Implement incremental loads (append + partitioning)
* Support multiple tables via config
* Add data quality validations
* Parameterize input sources
* Integrate with orchestration tools (Airflow / Databricks Jobs)

---

## 🛠️ Tech Stack

* Databricks
* PySpark
* Delta Lake
* Unity Catalog

---

## 📂 Project Structure

```
.
├── notebook/
│   └── pipeline_vinos.py
├── config/
│   └── config.json
└── README.md
```

---

## 🎯 Purpose

This project demonstrates:

* Parameterized data pipelines
* Data processing with PySpark
* Delta Lake usage in a Silver layer
* Separation of configuration and logic
* Awareness of production considerations and trade-offs

---
