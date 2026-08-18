# The Art of Extract, Transform & Load with Matillion

![Matillion](https://img.shields.io/badge/Matillion-ETL%20%2F%20DPC-19A0D2)
![Snowflake](https://img.shields.io/badge/Snowflake-Cloud%20DW-29B5E8)
![Amazon S3](https://img.shields.io/badge/Amazon%20S3-Staging-569A31)
![Pipelines](https://img.shields.io/badge/pipelines-93-blue)

A complete Matillion learning path — **93 importable pipelines** that run from a first S3 load all the way to a Data Vault and star schema, with the datasets to drive them.

Every file here is a real pipeline definition (`.orch.yaml` = orchestration, `.tran.yaml` = transformation). Import them into Matillion and run them.

---

## The path through this repo

| Stage | Folder | What you build |
|---|---|---|
| 1. Foundations | `Test/Practice/` | Numbered 1–10: S3 load, create/truncate/delete, external tables, SQL script, variables, then three iterator styles (fixed, file, loop) |
| 2. Daily drills | `Test/Day2/` – `Test/Day4/` | Table creation on load, environment and job variables, variable-driven S3 loads, date filtering |
| 3. Core patterns | `Test/` | Banking transformation, dynamic table creation from a CSV header, sales calculator, airport dimension, job timeout handling |
| 4. Calendar | `Test/Calendar/` | Generating a full date-dimension table |
| 5. Set operations | `Test/joins_practice/`, `Test_2/` | `UNION` vs `UNION ALL`, filtered unions, output tables |
| 6. Case study | `Project_AWA/Bike_Store_Project/` | Join case studies over a 9-table bike-store dataset |
| 7. End to end | `Test/ENd2EndProject/` | Load-type-driven extraction, active-table checks, iterative vs full reload |
| 8. Warehouse modelling | `Test/ENd2EndProject/matillion-examples/` | The full modelling progression — see below |

---

## The modelling progression

`matillion-examples/datamodel/` walks one dataset through every layer of a warehouse, one folder per layer:

```
01 Raw  →  02 Staging  →  03 ODS  →  04 3NF  →  05 Data Vault  →  06 Star  →  07 Aggregate
```

Two routes to the star schema are built out in full: **3NF → Star** and **Data Vault → Star**, three transformation parts each.

Alongside it, the patterns that separate a working warehouse from a toy one:

| Pattern | Folder | Why it matters |
|---|---|---|
| Slowly changing dimensions | `slowly changing dimensions/` | Type 1 and Type 2 updates, plus a virtualized variant — how history is kept when a dimension changes |
| Late-arriving dimensions | `late arriving dimension/` | Facts that show up before the dimension row they point at |
| Densification | `densification/` | Filling gaps so every period has a row, even an empty one |
| Transposing | `transposing/` | Wide-to-narrow and narrow-to-wide reshaping |
| Feature engineering | `feature engineering/` | Contingency tables and surrogate-key patterns |
| Spatial | `spatial/` | Loading and transforming PB2002 tectonic-plate geometry |

---

## Datasets included

| Dataset | Location | Shape |
|---|---|---|
| Bike Store | `Project_AWA/Bike_Store_Data/` | 9 related CSVs — brands, categories, customers, orders, order_items, products, staffs, stocks, stores |
| Transactions | `Test/Practice/` | `trnx_16` / `trnx_17` / `trnx_18` — the files the iterator lessons loop over |

---

## Component coverage

What the 93 pipelines actually exercise, by frequency:

| Area | Components |
|---|---|
| Read | `table-input` (79), `s3-load` (28), `database-query` |
| Transform | `calculator` (43), `filter` (24), `join` (23), `aggregate` (14), `rank`, `distinct`, `unite`, `lead-lag`, `map-values`, `generate-sequence`, `convert-type` |
| Write | `create-table-v2` (28), `rewrite-table` (14), `table-update` (12), `create-view` (10), `table-output`, `truncate-table`, `delete-tables` |
| Script | `sql-script` (36), `python-script` (12), `bash-script` (8) |
| Orchestrate | `run-transformation` (24), `run-orchestration`, `data-transfer-object` |

---

## Getting these running

1. **Import** the `.yaml` files into your Matillion project (orchestration and transformation import separately).
2. **Repoint the environment** — Snowflake account, warehouse, database and schema are from the original setup.
3. **Re-stage the data** — upload the CSVs to your own S3 bucket and update each S3 Load component.
4. Start at `Test/Practice/1_S3Load.orch.yaml` if you are learning; start at `matillion-examples/datamodel/` if you are modelling.

Credentials and bucket names in these files are placeholders. Nothing runs unchanged.

---

## Three things this repo is really teaching

- **Iterators over copy-paste.** Once `7_S3load_fixed_iterator` clicks, you stop building one pipeline per file.
- **Variables are the seam between dev and prod.** `Day3` and `Day4` exist entirely so that a pipeline stops caring which environment it is in.
- **The layer you skip is the one that hurts.** Raw → Staging → ODS looks like ceremony until a source system changes shape and only the staging layer breaks.
