# ⚡ Simple End-to-End Pipeline using Python and PostgreSQL

## 📊 Project Overview

This project demonstrates an end-to-end data engineering pipeline for electric vehicle data, integrating **exploratory data analysis (EDA)** in Python with **PostgreSQL** as the backend storage engine. The dataset used is the official *Electric Vehicle Population Data* from the Washington State Department of Licensing (DOL), containing registrations of **Battery Electric Vehicles (BEVs)** and **Plug-in Hybrid Electric Vehicles (PHEVs)**.

---

## 🧱 Tech Stack

- **Python** (pandas, numpy, psycopg2)
- **PostgreSQL**
- **PostGIS** (for spatial queries)
- **SQLAlchemy** (optional ORM integration)
- **Docker** (optional, for deployment)

---

## 📁 Dataset Overview

**File:** `Electric_Vehicle_Population_Data.csv`  
**Columns include:**
- `VIN (1-10)`
- `County`, `City`, `State`, `Postal Code`
- `Model Year`, `Make`, `Model`
- `Electric Vehicle Type`, `Electric Range`, `Base MSRP`
- `CAFV Eligibility`, `Legislative District`
- `DOL Vehicle ID`, `Vehicle Location`
- `Electric Utility`, `2020 Census Tract`

---

## 🔍 Exploratory Data Analysis (EDA)

Initial analysis provides:

- Dataset structure and types
- Unique counts of:
  - Counties: `len(data_frame['County'].unique())`
  - States: `len(data_frame['State'].unique())`
  - Makes: `len(data_frame['Make'].unique())`
- Geographic and temporal distribution of vehicles

EDA can be found in [`eda/analysis.py`](./eda/analysis.py).

---

## 🗃️ PostgreSQL Schema Design

To ensure **normalization, scalability, and querying efficiency**, we design a relational schema with the following structure:

### 📌 `electric_vehicles` (Main Table)
| Column               | Type                   | Description |
|----------------------|------------------------|-------------|
| `id`                 | SERIAL PRIMARY KEY     | Surrogate key |
| `vin_prefix`         | VARCHAR(10) UNIQUE     | First 10 chars of VIN |
| `model_year`         | INTEGER                | Model year |
| `make_id`            | INTEGER (FK)           | Foreign key to `makes` |
| `model`              | TEXT                   | Vehicle model |
| `vehicle_type`       | TEXT                   | BEV / PHEV |
| `cafv_eligibility`   | TEXT                   | CAFV status |
| `electric_range`     | INTEGER                | Electric range |
| `base_msrp`          | NUMERIC(10,2)          | MSRP |
| `legislative_district`| INTEGER               | District |
| `dol_vehicle_id`     | BIGINT                 | Licensing ID |
| `location`           | GEOGRAPHY(Point, 4326) | Geo point |
| `utility_id`         | INTEGER (FK)           | Electric utility |
| `census_tract`       | BIGINT                 | Census data |
| `city_id`            | INTEGER (FK)           | FK to `cities` |

### 🔁 Lookup Tables

#### ✅ `makes`
- `id`: SERIAL PRIMARY KEY
- `name`: TEXT UNIQUE NOT NULL

#### ✅ `utilities`
- `id`: SERIAL PRIMARY KEY
- `name`: TEXT UNIQUE NOT NULL
- `raw_string`: TEXT (original string if multiple)

#### ✅ `cities`
- `id`: SERIAL PRIMARY KEY
- `city`: TEXT
- `county`: TEXT
- `state`: CHAR(2)
- `postal_code`: TEXT

#### 🔗 (Optional) `vehicle_utilities` (Many-to-Many Bridge)
- `vehicle_id`: FK to `electric_vehicles(id)`
- `utility_id`: FK to `utilities(id)`

---

## 🌍 Geospatial Queries with PostGIS

PostGIS enables powerful spatial queries such as:

```sql
SELECT * FROM electric_vehicles
WHERE ST_DWithin(location, ST_MakePoint(-122.3, 47.6)::geography, 5000);
````

This retrieves all EVs within 5 km of a given coordinate.

---

## 🚀 Usage

1. **Install Requirements**

   ```bash
   pip install -r requirements.txt
   ```

2. **Create PostgreSQL Database**

   * Enable PostGIS extension:

     ```sql
     CREATE EXTENSION postgis;
     ```

3. **Run Schema Creation**

   ```bash
   psql -U your_user -d ev_database -f schema/schema.sql
   ```

4. **Load CSV into DataFrame**

   ```python
   df = pd.read_csv("dataset/Electric_Vehicle_Population_Data.csv")
   ```

5. **Insert Data into PostgreSQL**
   Use `psycopg2` or `SQLAlchemy` script in `etl/load_data.py`

---

## 📦 Directory Structure

```
├── dataset/
│   └── Electric_Vehicle_Population_Data.csv
├── eda/
│   └── analysis.py
├── etl/
│   ├── load_data.py
│   └── transform.py
├── schema/
│   └── schema.sql
├── requirements.txt
└── README.md
```

---

## 📈 Future Improvements

* Integrate Apache Airflow or Prefect for orchestration
* Dockerize the entire stack
* Add reporting dashboards (e.g., with Metabase or Superset)
* Implement API endpoints with FastAPI

---

## 📚 References

* [Washington State DOL Electric Vehicle Data](https://data.wa.gov/)
* [PostGIS Documentation](https://postgis.net/docs/)

---

## 🧑‍💻 Author

**Harel Hernandez**
Robotics & Data Engineer | Computer Vision | ETL | PostgreSQL

Feel free to contribute, report issues, or fork this repository.