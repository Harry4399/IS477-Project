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
| **3–4** | Oct 8 – Oct 21 | Acquire EPA API and Kaggle data; design database schema | Zhaojie Hua| Raw data files, ER diagram |
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

Looking back on the progress made in the first four weeks, we clearly see that our work has laid a relatively solid foundation for the remaining time of this semester. Although data collection and integration are still in the early stages, we have still successfully obtained and verified data from the EPA Air Quality Standards Application Programming Interface (EPA AQS API) of the United States Environmental Protection Agency. This achievement not only demonstrates our ability to handle real-world environmental datasets, but also confirms that our planned workflow, including API data collection, data cleaning, integration, and final analysis, is feasible within the project's time limit.

In addition, resolving issues such as failed email authentication, API key verification, Python environment configuration, and file system permissions has helped us enhance our debugging experience and technical adaptability. These early challenges provided valuable learning opportunities. We believe that these initial obstacles strengthened our workflow and enhanced our confidence in successfully completing the project.

Looking ahead to Weeks 5 to 8, our main goal is to expand the dataset so that it is no longer confined to a single date and parameter. This includes obtaining data on multiple pollutants (PM2.5, O₃, NO₂), incorporating meteorological variables such as temperature, humidity and wind speed, and preparing the Kaggle population dataset for merging. After the standardization and cleaning of the dataset are completed, we will commence exploratory data analysis (EDA) to identify the changing trends of pollutant levels and their relationship with weather conditions and population density. These analyses will lay the foundation for subsequent regression modeling and more advanced statistical comparisons.

We also look forward to deeper cooperation among team members in the next stage. Zhaojie Hua will continue to lead the data collection and workflow automation efforts, especially the expansion of API retrieval scripts and the construction of a unified directory structure for the project. Owen Wen will focus on cleaning and visualizing the expanded dataset, conducting quality checks, and preparing charts and descriptive statistics, which will be included in our interim report. By coordinating our work more closely, our goal is to significantly accelerate the progress by weeks 5 to 8 and ensure that the integrated dataset is completed by Week 9 for correlation and regression analysis.

In conclusion, although our project is still under development, the progress recorded in this report indicates that our technical foundation is solid, the division of responsibilities is effective, and the subsequent steps are also clear. Through continuous collaboration, constant testing and iterative improvement, we are confident that we can provide a comprehensive analysis report on how weather and population patterns across the United States affect air quality.

---




