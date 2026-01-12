# H-1B Visa Dataset Cleaning Project (3M+ Records)

![Python](https://img.shields.io/badge/Python-3.x-blue?style=for-the-badge&logo=python)
![Pandas](https://img.shields.io/badge/Pandas-Data%20Cleaning-150458?style=for-the-badge&logo=pandas)
![Status](https://img.shields.io/badge/Status-Completed-success?style=for-the-badge)
![Data Size](https://img.shields.io/badge/Data-3M%2B%20Rows-orange?style=for-the-badge)


## Project Overview
This project demonstrates a robust data cleaning pipeline applied to the **H-1B Visa Applications dataset**. The dataset contains over **3 million records** of visa applications from 2011-2016.

The primary goal was to transform raw, messy data containing null values, outliers, and inconsistencies into a clean, analysis-ready dataset. The process utilized advanced Pandas techniques for context-aware imputation and statistical outlier removal.

## Dataset
The raw H-1B visa dataset used in this project is publicly available on Kaggle and contains over 3 million records.

Due to GitHub file size limitations, the raw CSV file is not included in this repository. The notebook demonstrates the complete data cleaning pipeline applied to the full dataset.

**Dataset Link:** [H-1B Visa Kaggle Dataset](https://www.kaggle.com/datasets/nsharan/h-1b-visa)

## Data Transformation Summary

| Metric | Initial Raw State | Final Cleaned State |
| :--- | :--- | :--- |
| **Row Count** | 3,002,458 | 2,895,123 |
| **Missing Values** | ~107,000+ (in location data) | **0** (Fully Imputed) |
| **Columns** | 11 (Inconsistent naming) | 10 (Standardized) |
| **Data Quality** | Contains outliers & skew | Normalized & Capped |

## Pipeline Execution Screenshots

**Initial Data Set Overview**
*(The raw dataset showing missing values in longitude, latitude, and job codes)*

<img width="277" height="202" alt="initial" src="https://github.com/user-attachments/assets/fb947f7c-6056-4f4b-b0bd-767a4a502d80" />

<br>

**Final Cleaned Data Set Overview**
*(The processed dataset with zero null values and standardized formatting)*

  <img width="299" height="200" alt="final" src="https://github.com/user-attachments/assets/1a66f7cf-a4b7-4005-9d0f-6f041a0e898c" />

## The Cleaning Pipeline

The following steps were executed to clean the data:

### 1. Standardization & Formatting
* **Column Sanitization:** Renamed columns to remove whitespace and enforce Title Case format (e.g., CASE_STATUS -> Case_Status) for better readability.
* **ID Removal:** Dropped the `Unnamed: 0` column as it was a redundant index artifact.

### 2. Handling Missing Values (Imputation Strategy)
Instead of dropping data, I applied logic-based imputation to preserve dataset volume:
* **Geospatial Imputation:** Missing Lat and Lon coordinates were filled by grouping the data by `Worksite` (City). If a city had coordinates in another row, those were mapped to the missing entries.
* **Wage Imputation:** Missing `Prevailing_Wage` values were filled using the **median wage** of the specific `Soc_Name` (Job Category). This ensures that a missing Engineer's wage isn't filled with a CEO's salary.
* **Categorical Handling:** Missing text fields (`Employer_Name`, `Job_Title`) were explicitly labeled as "Unknown" to prevent analysis errors.

### 3. Outlier Management (Statistical Approach)
Wage data is often heavily skewed. I used the **IQR (Interquartile Range)** method to handle anomalies:
* **Lower Bound:** Dropped unrealistic entries with wages < $15,000/year.
* **Upper Bound:** Capped wages exceeding the upper outlier threshold (~$162k) to reduce skew without deleting high-earning valid data points.

## Detailed Overview

### Initial State
The raw dataset contained significant gaps in geolocation data and inconsistent text formatting.
* **Initial Shape:** (3,002,458, 11)
 
* **Major Issues:** 107k missing lat/lon values, extreme wage outliers.

### Final State
The resulting dataset is strictly typed, fully populated, and statistically balanced.
* **Final Shape:** (2,895,123, 10)

* **Outcome:** The cleaning process revealed that after standardizing text and removing unique IDs, the dataset contains ~924k duplicate entries (representing identical applications), which can now be easily aggregated or analyzed.

## Technologies Used
* **Python:** Core programming language.
* **Pandas:** For high-performance dataframe manipulation.
* **NumPy:** For numerical operations.

## How to Run
1. Clone the repository.
2. Download the `h1b_kaggle.csv` file from the link in the **Dataset** section above.
3. Place the CSV file in the root directory.
4. Run the Jupyter Notebook:
   ```bash
   jupyter notebook H1-B_dataset_cleaning_pandas.ipynb
