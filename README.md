# Solution: Spotfire Dashboard for COVID-19, US Census, and Weather Data

## Problem Statement
Spotfire dashboard that integrates COVID-19 data, US Census data, and Weather data to analyze relationships between demographic, health, and environmental factors over time and across different states.

---

## Data Sources
1. **COVID-19 Data**:
   - Metrics: Daily cases, deaths, vaccination rates.
   - Source: [CDC COVID Data Tracker](https://covid.cdc.gov/).
   - Granularity: State and county level (2020–2023).

2. **US Census Data**:
   - Metrics: Population, median income, unemployment rates, age distribution.
   - Source: [US Census Bureau](https://www.census.gov/).
   - Granularity: State and county level (latest estimates).

3. **Weather Data**:
   - Metrics: Temperature, precipitation, humidity, extreme weather events.
   - Source: [National Weather Service](https://www.weather.gov/).
   - Granularity: State level, aggregated monthly (2020–2023).

---
## Data Ingestion
### Tools Used
- **Spotfire Data Connector(ODATA)**:
  - Configured to load COVID-19 data as ODATA.
- **Python Scripts via Spotfire Data Functions**:
  -Configured to load US Census and Weather data directly from API into Spotfire Data Function and convert into data table .
  - Automated data loading and initial preprocessing.
  
### Process
1. **COVID-19 Data**:
   - Imported Weekly deaths from 2020 to 2023 into Spotfire.
   
2. **US Census Data**:
   - Imported data via API containing demographic and economic data.
   - Mapped `State` and `Year` as the primary key for joining with other datasets.

3. **Weather Data**:
   - Imported single day weather data aggregated by state.
---
---

## Data Transformation
### A. Cleaning
1. **Missing Values**:
   - COVID-19: Filled missing `Vaccination Rate` with state-level averages.
   - Weather: Interpolated missing values for `Humidity` and `Precipitation` using monthly averages.

2. **Standardization**:
   - Dates converted to the format `YYYY-MM-DD`.
   - State names standardized to title case (e.g., `New York`).

3. **Filtering**:
   - Removed non-state territories (e.g., Guam, Puerto Rico) to focus on US states.

---

### B. Joining
- Merged datasets using:
  - **Primary Keys**: `State` and `Date`.
  - **Join Types**:
    - **Left Inner Join** for COVID-19 and Weather data.
    - **Left Join** for Census data with the merged COVID-19/Weather dataset.

---

### C. Feature Engineering
1. **Derived Metrics**:
   - `Covid Deaths Per Capita` = `Covid Deaths` ÷ `Population`.
   - `Deaths Per Capita` = `Deaths` ÷ `Population`.
   - `Weather-COVID Interaction`: `Humidity * Avg Temperature`.

2. **Categorical Variables**:
   - Grouped income into `Low`, `Medium`, and `High` categories using percentiles.

3. **Time-Based Aggregations**:
   - Rolled daily COVID-19 data into monthly averages for consistency with Weather data.

---

### D. Normalization
- Applied **z-score normalization** to numeric variables (e.g., population, case rates) to ensure uniform scaling across features.

---
## Approach
### Data Integration and Preprocessing
1. **Data Cleaning**:
   - Handled missing values by filling them with mean or median where applicable.
   - Standardized formats across datasets (e.g., date and state names).
2. **Data Joining**:
   - Merged datasets on common attributes like `State` and `Date`.
3. **Derived Metrics**:
   - Calculated per-capita COVID-19 deaths using census population data.
   - Derived average monthly temperature and precipitation from weather data.

---

## Dashboard Architecture/Features
### 1. **Overview Page**
   - **Visuals**:
     - Key metrics summary cards: Total COVID-19 deaths and death due to other diseases.
     - A time-series chart for COVID-19 trends (deaths) over time.
   - **Interactivity**:
     - Filters for `State`, `Year`, and `Month`.

### 2. **Demographic Analysis Page**
   - **Visuals**:
     - Bar chart comparing COVID-19 case rates across income levels and age groups.
     - Heatmap showing the correlation between demographic factors and COVID-19 deaths.
   - **Interactivity**:
     - Dropdowns to select specific demographic variables (e.g., median income, unemployment rate).

### 3. **Weather Impact Page**
   - **Visuals**:
     - Line chart overlaying average monthly temperature and COVID-19 deaths.
     - Scatter plot of precipitation vs. COVID-19 deaths, and overall death cases.
   - **Interactivity**:
     - Date slider to explore specific time periods.

### 4. **Geospatial Page**
   - **Visuals**:
     - Choropleth map displaying per-capita COVID-19 deaths and overall deaths by state.
     - Bubble overlay for weather events (e.g., hurricanes, heatwaves).
   - **Interactivity**:
     - Tooltips showing demographic and weather details for each state.

---
## Handling User Interactions

### **1. Synchronization**
- All visualizations are interlinked, so user selections in one visualization automatically update others.
- Example:
  - Selecting `Texas` in the State filters all charts and tables to show data for Texas.

### **2. Real-Time Calculations**
- Using Spotfire data functions (Python scripts) to predict key metrics, such as:
  - Actual vs Predited Economic Index.
  - K_Means Clustering used to group data into clusters.

### **3. Performance Optimization**
To handle large datasets and ensure responsiveness:
- Using Spotfire in-memory data tables for fast querying.
- Calculations are pre-aggregated where possible, reducing runtime computations.

---

## Key Insights
1. States with higher population densities and lower median incomes experienced higher COVID-19  deaths.
2. Weather conditions (e.g., low humidity, cold temperatures) correlated with higher case spikes during winter months.


---

## Challenges and Limitations
1. **Data Quality**:
   - Missing values in some weather datasets, particularly for smaller states.
2. **Temporal Misalignment**:
   - Some COVID-19 data points lacked exact weather information for corresponding dates.
3. **Granularity Discrepancies**:
   - Census and COVID-19 data was static (yearly updates), while weather data were dynamic.

---

## Future Improvements
1. **Advanced Analytics**:
   - Incorporate machine learning models to predict COVID-19 trends based on weather and demographic data.
2. **Enhanced Interactivity**:
   - Add drill-down capabilities to explore county-level data.
3. **Real-Time Updates**:
   - Automate data refresh for the dashboard to ensure up-to-date analysis.

---

## References
1. [CDC COVID-19 Data Tracker](https://covid.cdc.gov/)
2. [US Census Bureau](https://www.census.gov/)
3. [National Weather Service](https://www.weather.gov/)

---

## How to Access the Dashboard
1. Open Spotfire.
2. Navigate to the shared library: `Company Workspace > Dashboards > COVID19_Census_Weather`.
3. Use the provided filters and tools to interact with the data.

