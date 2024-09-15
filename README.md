# Arrest Data Analysis

## Overview

This Jupyter Notebook performs an exploratory data analysis on arrest data from the Los Angeles Police Department. The dataset spans from 2020 to the present and includes various attributes related to arrests, such as date, time, location, and demographic information.

## Analysis Goals

The primary goals of this analysis are to explore and visualize:

- **Arrest Trends Over Time**: Changes in the number of arrests month-by-month and year-by-year.
- **Seasonal Variations**: How arrest rates vary with seasons.
- **Days of the Week**: Patterns of arrests across different days of the week.
- **Time of Day**: Frequency of arrests at different hours of the day.
- **Geographic Distribution of Arrests**: Areas with the highest and lowest arrest rates.
- **Common Charges**: The most frequent charges leading to arrests.
- **Demographics of Arrestees**: Analysis based on gender, age, and descent.

## Dataset

The dataset used in this analysis is `Arrest_Data_from_2020_to_Present_20240915.csv` and includes the following columns:

- `Report ID`
- `Report Type`
- `Arrest Date`
- `Time`
- `Area ID`
- `Area Name`
- `Reporting District`
- `Age`
- `Sex Code`
- `Descent Code`
- `Charge Group Code`
- `Charge Group Description`
- `Arrest Type Code`
- `Charge`
- `Charge Description`
- `Disposition Description`
- `Address`
- `Cross Street`
- `LAT`
- `LON`
- `Location`
- `Booking Date`
- `Booking Time`
- `Booking Location`
- `Booking Location Code`

## Installation

To run this notebook, ensure you have the following Python packages installed:

- `pandas`
- `matplotlib`
- `seaborn`

You can install the required packages using `pip`:

```bash
pip install pandas matplotlib seaborn
```

## Usage

1. **Load the Data**: The data is loaded from the CSV file.
2. **Data Preprocessing**: The notebook includes steps for cleaning and transforming the data, including handling missing values and converting date and time formats.
3. **Analysis and Visualization**:
   - Monthly and yearly trends in arrests
   - Seasonal variations
   - Arrests by day of the week and time of day
   - Geographic distribution of arrests
   - Common charges and demographic analysis

## Code Snippets

Below are some key code snippets from the notebook:

### Load Data

```python
import pandas as pd
df = pd.read_csv("Arrest_Data_from_2020_to_Present_20240915.csv")
```

### Monthly Arrest Trends

```python
df['Arrest Date'] = pd.to_datetime(df['Arrest Date'], errors='coerce')
df['YearMonth'] = df['Arrest Date'].dt.to_period('M')
monthly_arrests = df.groupby('YearMonth').size()
monthly_arrests.plot(kind='bar')
```

### Seasonal Arrests

```python
def get_season(date):
    month = date.month
    if month in [12, 1, 2]:
        return 'Winter'
    elif month in [3, 4, 5]:
        return 'Spring'
    elif month in [6, 7, 8]:
        return 'Summer'
    else:
        return 'Fall'

df['Season'] = df['Arrest Date'].apply(get_season)
seasonal_arrests = df.groupby('Season').size()
seasonal_arrests.plot(kind='bar')
```

### Arrests by Day of the Week

```python
df['Day of Week'] = df['Arrest Date'].dt.day_name()
day_of_week_arrests = df.groupby('Day of Week').size()
day_of_week_arrests.plot(kind='bar', color='purple')
```

### Arrests by Hour of the Day

```python
def transform_time_column(df, column_name):
    def transform_time(num):
        num_int = int(num)
        num_str = str(num_int).zfill(4)
        return f"{num_str[:2]}:{num_str[2:]}"
    df[column_name] = df[column_name].apply(transform_time)
    return df

df = df.dropna(subset=["Time"])
df = transform_time_column(df, 'Time')
df['Hour'] = df['Time'].str[:2].astype(int)
hourly_arrests = df.groupby('Hour').size()
sns.lineplot(x=hourly_arrests.index, y=hourly_arrests.values, marker='o', palette='viridis')
```

## Results

The notebook generates various plots and visualizations to help understand arrest trends, seasonal patterns, and demographic distributions. Results include bar charts, pie charts, and line plots to illustrate the findings.

## Contributing

Feel free to contribute by submitting issues or pull requests if you have suggestions or improvements.



