# Project README: Medical Data Processing and Analysis

## Overview

This project contains a collection of Python scripts aimed at processing and analyzing medical data, specifically focusing on patient diagnosis codes. The scripts are designed to filter, transform, and analyze large datasets, making them suitable for tasks such as identifying patterns in medical diagnoses, tracking patient visit history, and performing demographic analyses.

## Table of Contents

1. [Prerequisites](#prerequisites)
2. [Installation](#installation)
3. [Scripts Overview](#scripts-overview)
   - [Data Filtering and Transformation](#data-filtering-and-transformation)
   - [Patient Visit History Analysis](#patient-visit-history-analysis)
   - [Demographic Analysis](#demographic-analysis)
4. [Usage](#usage)
5. [File Descriptions](#file-descriptions)
6. [Output](#output)
7. [License](#license)

## Prerequisites

Before running the scripts, ensure you have the following installed on your system:

- Python 3.7 or later
- Pandas library

## Installation

1. **Clone the repository**:
   ```bash
   git clone https://github.com/yourusername/medical-data-analysis.git
   ```
   
2. **Navigate to the project directory**:
   ```bash
   cd medical-data-analysis
   ```
   
3. **Install required Python packages**:
   ```bash
   pip install pandas
   ```

## Scripts Overview

### Data Filtering and Transformation

This script reads in a dataset of patient records, filters the data based on specific diagnosis codes, and transforms the data for further analysis.

- **Key operations**:
  - Filters rows based on a set of diagnosis codes (e.g., `E11.2`, `E11.3`, `L97`).
  - Replaces non-matching codes with `NaN`.
  - Converts admission dates to a datetime format.
  - Sorts the data by patient ID and admission date.

- **Example usage**:
  ```python
  import pandas as pd

  df = pd.read_excel("data/feuille11.xlsx")
  code_pattern = '|'.join(["E11.2", "E11.3", "E11.4", "E11.5", "L97"])
  df_filtered = df[
      df["Code Dx Principal"].str.contains(code_pattern) |
      df["Ecle Me Cim  Diagnostic (Autres)"].str.contains(code_pattern)
  ]
  ```

### Patient Visit History Analysis

This script processes patient records to create a history of visits, including diagnosis codes, and adds a visit number to track sequential visits.

- **Key operations**:
  - Groups patient records by patient ID and admission date.
  - Adds a visit number to each unique visit.
  - Pivots the data to create a wide-format DataFrame with diagnosis codes by visit.

- **Example usage**:
  ```python
  df_sorted = df_filtered.sort_values(['PatienId', 'Dt  Admission'])
  df_sorted['Visit_Number'] = df_sorted.groupby(['PatienId', 'Dt  Admission']).cumcount() + 1
  df_pivot_principal = df_sorted.pivot(index='PatienId', columns=['Dt  Admission', 'Visit_Number'], values='Code Dx Principal')
  ```

### Demographic Analysis

This script analyzes the filtered dataset to provide insights into diagnosis codes based on demographic factors such as sex and age.

- **Key operations**:
  - Filters the dataset based on diagnosis codes and demographic factors (e.g., age, sex).
  - Counts the occurrences of specific diagnosis codes within defined demographic groups.

- **Example usage**:
  ```python
  df10 = df1[(df1['Dx Principal_v0']=='E11.2')&(df1['Sexe']=='F')&(df1['Age  Du Pt\n(au séjour)_v0']<=50)]
  len(df10)
  ```

## Usage

To use the scripts, follow these steps:

1. **Run the data filtering script** to filter and clean the dataset:
   ```bash
   python data_filtering.py
   ```

2. **Run the patient visit history analysis script** to generate a wide-format DataFrame with patient visit data:
   ```bash
   python patient_visit_history.py
   ```

3. **Run the demographic analysis script** to analyze the dataset based on sex, age, and diagnosis codes:
   ```bash
   python demographic_analysis.py
   ```

## File Descriptions

- **data_filtering.py**: Script for filtering and cleaning the dataset based on specific diagnosis codes.
- **patient_visit_history.py**: Script for processing and analyzing patient visit history.
- **demographic_analysis.py**: Script for performing demographic analysis based on the filtered dataset.

## Output

The scripts generate various outputs, including:

- A filtered and cleaned dataset with relevant diagnosis codes.
- A wide-format DataFrame that tracks patient visit history with diagnosis codes.
- Statistical summaries and counts of diagnosis codes across different demographic groups.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

If you have any questions or need further assistance, please feel free to contact me or open an issue in the repository.
