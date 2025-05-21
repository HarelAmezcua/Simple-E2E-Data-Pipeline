This is the recommended Dtypes for the pandas data frame.

| Column                    | Pandas Type            | Resulting SQL Type                     |
| ------------------------- | ---------------------- | -------------------------------------- |
| `VIN (1-10)`              | `string`               | `VARCHAR`                              |
| `County`, `City`, `State` | `category` or `string` | `VARCHAR`                              |
| `Postal Code`             | `string`               | `VARCHAR`                              |
| `Model Year`              | `int16` or `int32`     | `INTEGER`                              |
| `Make`, `Model`           | `category`             | `VARCHAR`                              |
| `Electric Vehicle Type`   | `category`             | `VARCHAR`                              |
| `CAFV Eligibility`        | `category`             | `VARCHAR`                              |
| `Electric Range`          | `float32`              | `FLOAT`                                |
| `Base MSRP`               | `float32`              | `FLOAT`                                |
| `Legislative District`    | `category`             | `VARCHAR`                              |
| `DOL Vehicle ID`          | `int64`                | `BIGINT`                               |
| `Vehicle Location`        | `string`               | `VARCHAR`                              |
| `Electric Utility`        | `category`             | `VARCHAR`                              |
| `2020 Census Tract`       | `float64`              | `FLOAT` or `BIGINT` (if whole numbers) |


Here's the resulting pandas inplementation:

import pandas as pd

# Load CSV
df = pd.read_csv("vehicles.csv")

# Convert dtypes
df = df.astype({
    "VIN (1-10)": "string",
    "County": "string",
    "City": "string",
    "State": "string",
    "Postal Code": "string",
    "Model Year": "int16",
    "Make": "string",
    "Model": "string",
    "Electric Vehicle Type": "string",
    "Clean Alternative Fuel Vehicle (CAFV) Eligibility": "string",
    "Electric Range": "float32",
    "Base MSRP": "float32",
    "Legislative District": "string",
    "DOL Vehicle ID": "int64",
    "Vehicle Location": "string",
    "Electric Utility": "string",
    "2020 Census Tract": "float64",
})