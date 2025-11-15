## Status Report  
### *Integration and Analysis of Air Quality, Weather, and Population Data in the United States*  
### **IS 477 – Fall 2025**

---

## 1. Introduction

Our project, *“Integration and Analysis of Air Quality, Weather, and Population Data in the United States,”* aims to understand how environmental and demographic factors influence variations in air pollution across the country. By integrating two major datasets:

- **EPA Air Quality System (AQS) API data**  
- **Weather and meteorological data from Kaggle**  

we aim to build a unified dataset that will support exploratory analysis, correlation tests, and regression modeling later in the semester.

This **Status Report** documents:

- Work completed during Weeks 1–4  
- Progress relative to the original project plan  
- Contributions of each team member  
- Challenges encountered and how we addressed them  
- Planned work for Weeks 5–8  

Through this integration, we plan to analyze how weather conditions and population density jointly affect pollutant concentrations, especially PM2.5.

This progress report aims to document the progress we have made so far, point out the gap between the current progress and the original project plan, outline the contributions of each team member, and describe the subsequent steps. This report reflects the work completed from the first week to the seventh week of this quarter.

Although the project is still in its early stages, we have successfully completed a core technical milestone: establishing API access to the US Environmental Protection Agency's Air Quality Standards (EPA AQS) system and obtaining valid data sets. This step is of vital importance because without reliable AQS API access, subsequent analysis, integration and modeling work cannot be carried out. In addition, we conducted a preliminary data check and verified that the output structure met the expected requirements for cleaning and merging.

Although the data retrieved so far is limited to one date and one pollutant, this reflects our intentional decision to simplify early development and avoid unnecessary complexity. We plan to verify the entire workflow with a small amount of data before expanding the scale. This strategy can reduce the risk of subsequent errors and ensure that each link of the process is stable and reliable.

---

## 2. Work Completed So Far

### 2.1 EPA AQS Account Setup

To access the AQS API, we registered accounts with the EPA Data Mart system. This step required email verification and activation of a personal API key. We encountered multiple issues because the system struggled to authenticate `.edu` addresses. Several attempts were required before one account was successfully validated and issued an API key.

The confirmation email verified that our access credentials were active and allowed us to retrieve data through the AQS API.

---

### 2.2 Successfully Connecting to the AQS API

After obtaining valid credentials, we developed an API request script using Python’s `requests` module. Our initial focus was to retrieve **PM2.5 (parameter code 88101)** measurements for **January 1, 2024** in **Illinois (state code 17)**, following the AQS API documentation.

The request returned:

- **Status code:** 200 (success)  
- **API status:** “Success”  
- **Rows returned:** 324  

This confirmed that our API key, parameters, and request formatting were correct.

---

### 2.3 Converting API Data into a DataFrame

After receiving the JSON response, we parsed it into a pandas DataFrame. The resulting dataset contained 36 columns, including:

- `date_local`  
- `arithmetic_mean`  
- `units_of_measure`  
- `site_number`  
- `county`  
- `city`  
- `latitude`, `longitude`  
- `method_code`  
- `observation_count`  

These variables will be essential in later stages of data cleaning, geographic standardization, and statistical modeling.

---

### 2.4 Saving the Output File

We exported the retrieved air quality data into a CSV file organized under a new folder in our repository:



This file will be used as the baseline dataset for future integration with weather and population data.

---

## 3. Progress Compared to Original Project Plan

The table below summarizes the original plan and highlights completed items:

| Week | Time Period | Task | Responsible | Deliverables |
|------|-------------|-------|-------------|--------------|
| **1–2** | Sept 26 – Oct 7 | Define scope, collect sample data, finalize project plan | Both | ProjectPlan.md, API test script |
| **3–4** | Oct 8 – Oct 21 | Acquire EPA API and Kaggle data; design database schema | Hua Zhaojie | Raw data files, ER diagram |
| **5–6** | Oct 22 – Nov 4 | Data cleaning and profiling | Owen Wen | Cleaned datasets, quality report |
| **7** | Nov 5 – Nov 18 | Integrate datasets; exploratory analysis | Both | Integrated dataset, visualizations |
| **8** | Nov 19 – Nov 25 | Regression analysis | Owen Wen | Analytical graphs and statistical outputs |

### Completed as Scheduled
- Project scope finalized  
- Repository structure created  
- API account successfully registered  
- First AQS dataset collected  
- CSV output stored in `/outputs`  
- Draft of the database schema  

### Slight Delays
- Weather dataset not yet downloaded  
- Population dataset not yet downloaded  
- ER diagram still in early draft  

These delays occurred due to repeated authentication failures during API registration. However, now that the AQS system is working, the remaining data sources will be acquired at the start of Week 5.

---

## 4. Team Member Contributions

### **Hua Zhaojie**
- Resolved API account authentication issues  
- Wrote all API connection and data extraction scripts  
- Converted JSON data into structured DataFrames  
- Created initial GitHub repo layout and outputs folder  
- Drafted the full 1,500-word status report  

### **Owen Wen**
- Assisted in reviewing API request workflow  
- Prepared data cleaning plan  
- Outlined expected quality checks and profiling tasks  
- Will lead Weeks 5–6 cleaning and quality reporting  

Both members have communicated regularly and shared responsibilities based on technical strengths.

---

## 5. Challenges Encountered

### 5.1 API Authentication Failures
The EPA system repeatedly rejected `.edu` email domains, causing delays. Only after multiple re-registrations did one account finally validate correctly.

**Impact:** Delayed API connection by ~2 days  
**Resolution:** Retried with multiple email formats; stored final credentials securely  

---

### 5.2 Dataset Standardization Issues
Different datasets use inconsistent geographic units:

- AQS uses site-level data  
- NOAA weather datasets may use station-level or grid-level data  
- Census uses county-level data (FIPS codes)  

This will require additional transformations before integration.

---

### 5.3 Time Pressure During Integration Weeks
Weeks 7–8 include both dataset integration and regression modeling, which are time-intensive. Proper early cleaning will reduce pressure later.

---

## 6. Plan for the Next Four Weeks

### **Week 5–6 (Owen Wen)**
- Download and clean NOAA weather dataset  
- Download Census population dataset  
- Identify missing values and outliers  
- Produce a full Data Quality Report  

### **Week 7 (Both)**
- Integrate AQS, weather, and population datasets  
- Align geographic units using standardized FIPS codes  
- Conduct exploratory visualizations  

### **Week 8 (Owen Wen)**
- Run correlation tests  
- Build regression models  
- Generate analytical graphs for the final report  
- Prepare final summary of findings  

---

## 7. Conclusion

In summary, the project has made strong initial progress by completing one of the most challenging stages—establishing a functioning connection to the EPA AQS API and retrieving real-world environmental data. Although minor delays occurred due to authentication issues, they have been resolved, and the project remains on track for successful completion. With foundational datasets beginning to take shape, the next phase of data cleaning, integration, and exploratory analysis can proceed smoothly.

---




