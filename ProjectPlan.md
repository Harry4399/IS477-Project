## Project Plan: Climate and Environment Data Integration and Analysis

## 1. Overview

Our project aims to understand the relationship between air pollution levels in major American cities and weather conditions as well as population density.
Our goal is to explore the interaction between meteorological variables such as temperature, humidity, wind speed, and air quality indicators (PM2.5, ozone, AQI).

By integrating data from multiple public sources, our goal is:
Identify weather factors related to high pollution levels.
Compare the differences in air quality between urban and rural populations.

---

## 2. Research Questions

1. How do temperature and humidity affect the concentrations of PM2.5 and ozone in American cities?
2. Have the pollution levels in urban areas with high population density always been high?

---

## 3. Team

**Team Members**
- **Zhaojie Hua** — Data acquisition, integration, and workflow automation.  
- **Owen Wen** — Data cleaning, exploratory analysis, and visualization.

---

## 4. Datasets

We will use **two primary datasets** (and one optional enrichment source) from open, trustworthy U.S. government APIs.

| Dataset | Description | Source / Access Method | Format |
|----------|--------------|------------------------|---------|
| **EPA Air Quality System (AQS)** | Daily air quality data (PM2.5, ozone, AQI) by monitoring station. | [EPA AQS API](https://aqs.epa.gov/aqsweb/documents/data_api.html) | JSON via REST API (Key:khakikit96)|
| **US Air Quality (1980–Present)** | Long-term air pollution data (PM2.5, ozone, etc.) | [US Air Quality](https://aqs.epa.gov/aqsweb/documents/data_api.html#signup) | CSV / tabular |

---

## 5. Timeline

| Week | Task | Responsible | Deliverable |
|------|-------|--------------|--------------|
| **Week 1** | Obtain API keys: initial data exploration scripts for EPA and US Air Quality. | Zhaojie | `get_epa_data.py`, `get_UAQ_data.py` |
| **Week 2** | Clean and normalize datasets: find and replace missing values and unit. | Owen | Cleaned CSVs + VC code notebook |
| **Week 3** | Integrate datasets by location and date; store in SQLite. | Both | Integrated dataset + SQL schema |
| **Week 4** | Understand the data and develop analysis and find correlations. | Owen | Analysis notebook + plots |
| **Week 4** | Finalize report and prepare for GitHub release. | Both | Final `README.md`, metadata, Box link |

---

## 6. Constraints

- **Missing data:** Some weather stations have missing daily data, so imputation and handling is needed.  
- **Data alignment:** Weather and pollution stations may have minor differences, for example: same location may have little difference in latitude.  

---

## 7. Gaps
- **Data alignment:** The Kaggle air quality dataset and the EPA climate data may differ in data formate and ways to represent the location of the station.  We may need to design a strategy for matching same and air quality stations.
- **Scope Definition:** The EPA API and UAQ csv can return large volumes of data. We need to make sure whether using only a fraction of the total dataset for our project.
