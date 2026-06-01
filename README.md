# Petroineos Power Plants Data Processing Task

## Overview

This project processes raw power plant data, cleans and standardises the dataset, stores it in a central database, and produces aggregated statistical metrics at both site and country level.

The solution is implemented in Python using pandas and numpy; it is designed to be clear, reproducible, and robust to common data quality issues.

---

## Objectives

- Clean and validate raw plant data from CSV files
- Standardise data formats across datasets
- Maintain a central database of processed data
- Generate:
  - Quarterly statistics (mean, median, standard deviation) by site
  - Total volume by country and technology

---

## Data Description

Each input file follows the structure:

- Date (DD/MM/YYYY)
- Country (e.g. FR, GB)
- Technology (e.g. Gas)
- SiteName
- Volume

### Observed Data Issues

The dataset contains several realistic data quality challenges:

- Missing volume values (e.g. empty cells)
- Trailing blank rows
- Extra whitespace in column names
- Possible duplicate entries

---

## Assumptions

- Missing volume values are treated as 0
- Rows with all missing values are removed
- Country codes are mapped to full country names using a dictionary
---

## Approach

The solution is implemented through a `PowerPlants` class, with the following pipeline:

1. **Data Cleaning**
   - Standardise column names
   - Convert country codes to full names
   - Handle missing and invalid values
   - Remove duplicate and empty rows

2. **Data Preparation**
   - Convert dates to a consistent format
   - Adds columns: `updatedby`, `updatetime`
   - Ensure consistent column ordering

3. **Database Management**
   - Store processed data in a central CSV database
   - Replace previous entries from the same source file to avoid duplication

4. **Aggregation**
   - Quarterly statistics by site (mean, median, standard deviation (std))
   - Total volume by country and technology

---

## Usage

Example workflow:

```python
pp = PowerPlants()

# Step 1: Clean raw data
clean_data = pp.analyse_plant_data("gas_fr_plants.csv")

# Step 2: Prepare data
processed_data = pp.load_new_data_from_file(clean_data)

# Step 3: Save to database
pp.save_new_data(processed_data)

# Step 4: Load latest data
pp.get_data_from_database()

# Step 5: Generate outputs
quarterly = pp.aggregate_data_to_quarterly()
country = pp.aggregate_data_to_country()
