# Project Plan: Integration and analysis of air quality, weather and population data in the United States

## 1. Overview
This project aims to study the interaction between environmental and demographic factors that affect air pollution levels in the United States. Specifically, we will integrate air quality and meteorological data from the EPA Air Quality System (AQS) API, as well as population data from the 2024 U.S. All Cities Population (Kaggle) dataset. Our goal is to analyze how weather conditions (such as temperature, humidity and wind speed) and population density jointly affect pollutant concentrations, especially PM2.5 and ozone.

Air pollution remains one of the most persistent environmental challenges in the United States, posing serious public health risks, including respiratory diseases, cardiovascular diseases and premature death. Although federal and local monitoring systems provide a large amount of data on pollutant concentrations, most of this information is scattered across different formats, sources and temporal resolutions. To address this issue, our project will focus on data integration, standardization and reproducibility.

We will utilize the EPA AQS API to obtain dynamic and high-resolution air quality data, which provides real-time and historical measurement data from thousands of monitoring stations across the country. This dataset will be supplemented with static demographic information from the CSV dataset, which covers population estimates for all U.S. cities in 2024. In conclusion, these data sources will provide a multi-faceted perspective on how human activities and environmental conditions interact to form air quality patterns.

By integrating these datasets, our goal is to establish a repeatable analytical pipeline that can answer research questions related to urbanization, weather impacts, and pollutant behavior. This project will also demonstrate how transparent and well-documented workflows can transform open data into actionable insights for policymakers and researchers.

---

## 2. Research the problem

1. **How do meteorological variables such as temperature, humidity and wind speed affect the concentrations of air pollutants (PM2.5 and O3)?**  
   We expect that due to the stagnation effect and limited diffusion, high humidity and low wind speed may lead to an increase in PM2.5 levels.

2. **Compared with less populated areas, are more densely populated urban areas always associated with higher levels of air pollution?**  
   Using population data, after controlling for meteorological factors, test whether the prediction of pollutant levels by population density is significant.

3. **To what extent does population density amplify or mitigate the impact of weather variables on air quality?**  
   This issue explores the interaction between the impact of population and climate on air pollution, which helps to determine regional vulnerability.

These research questions are in line with the objectives of the IS477 course, integrating multiple reliable data sources, applying repeatable workflows, and generating socially relevant insights into sustainability and public health.

---

## 3. Team

| Name | Role | Main Responsibilities |
|------|------|-----------------------|
| **Hua Zhaojie** | Data Acquisition and Workflow Automation | Lead data collection via EPA AQS API, manage repository setup, and implement reproducible workflows using Python scripts and Snakemake. Design database schema and manage GitHub version control. |
| **Owen Wen** | Data Cleaning, Visualization, and Analysis | Conduct data profiling, cleaning, and exploratory data analysis (EDA). Create statistical and visual analyses illustrating the relationships between population density, weather, and air quality. |

---

## 4. Datasets

This project will integrate two datasets of different formats and access methods to ensure diversity, reliability, and reproducibility.

| Data Source | Description | Access Method | Project Use |
|--------------|-------------|----------------|--------------|
| **Air Quality System (AQS) API – U.S. Environmental Protection Agency (EPA)** | Provides real-time and historical measurements of pollutants such as PM2.5, O3, NO2, CO, and SO2, as well as meteorological variables including temperature, humidity, and wind speed from nationwide monitoring stations. | REST API (JSON) | Serves as the primary data source for environmental and weather metrics. Enables retrieval of daily and hourly pollutant readings for multiple U.S. regions. |
| **Population of All U.S. Cities 2024 (Kaggle Dataset by dataanalyst001)** | Contains 2024 population estimates for U.S. cities, with city names, state identifiers, and growth rates. Data is presented in a standardized CSV format, suitable for integration and analysis. | Direct CSV download | Provides demographic and population indicators to analyze how urban density correlates with air quality and weather data. |

**Integration strategy**  
These datasets will be linked by city and state identifiers, or, if necessary, mapped to county-level FIPS codes using standardized crosswalk files. EPA AQS provides high-frequency, time series environmental data, while the population dataset offers static demographic attributes. The integration will be implemented using Python's Pandas and SQLAlchemy libraries. The analysis program will include correlation analysis, multiple linear regression, and spatial visualization using Plotly and Matplotlib.

This dual-source design also meets the course requirements, utilizing different data acquisition methods: one based on API and the other based on CSV, to ensure a balanced demonstration of the concept of data lifecycle management.

---

## 5. Considerations in terms of morality, law and policy

All datasets are publicly available and comply with the principles of open data.

- **Data integrity:** The EPA AQS API is a trusted federal source, while Kaggle datasets are derived from publicly available census data.  
- **Privacy protection:** There are no personal identifiers or sensitive attributes.  
- **Transparency and repeatability:** All code, workflow scripts, and documentation will be published in the GitHub repository, including a requirements.txt file and an optional Docker configuration to ensure repeatability.  
- **Licensing compliance:** Datasets and software licenses will be correctly referenced in accordance with the course guidelines.  
- **Ethical Data Report:** The results will be presented objectively, with close attention paid to uncertainties, limitations, and non-causal explanations.

By ensuring these practices, our project will become a model for ethical data management and responsible discovery communication.

---

## 6. Timeline

| Week | Time Period | Task | Responsible | Deliverables |
|------|--------------|------|--------------|---------------|
| **Week 1–2** | Sept 26 – Oct 7 | Define scope, collect sample data, finalize project plan | Both | ProjectPlan.md, API test script |
| **Week 3–4** | Oct 8 – Oct 21 | Acquire EPA API and Kaggle data; design database schema | Hua Zhaojie | Raw data files, ER diagram |
| **Week 5–6** | Oct 22 – Nov 4 | Perform data cleaning and profiling | Owen Wen | Cleaned datasets, quality report |
| **Week 7** | Nov 5 – Nov 18 | Integrate datasets and perform exploratory analysis | Both | Integrated dataset, descriptive visuals, Intermediate report |
| **Week 8** | Nov 19 – Nov 25 | Conduct correlation and regression analyses | Owen Wen | Analytical graphs and statistical outputs |
| **Week 9** | Nov 26 – Dec 3 | Automate workflow using Snakemake and finalize scripts | Hua Zhaojie | Automated workflow pipeline |
| **Week 10** | Dec 4 – Dec 10 | Complete final report and GitHub release | Both | Final README.md, visualizations, Box data link |

This timeline ensures the interim report (Nov 11) and final project (Dec 10) are submitted on time with ample room for refinement.

---

## 7. Constraints

- **API Rate limit:** The EPA AQS API limits requests to approximately 5 per second, requiring batch processing and local caching.  
- **Geographical consistency:** EPA data is county-level, while population data is municipal-level; pedestrian crossing mapping and aggregation are needed.  
- **Data size:** The dataset will be very large; compression and efficient storage (Parquet format) will be used.  
- **Time management:** Tasks will be clearly divided among team members to meet all milestones.  
- **Visualization complexity:** High-dimensional data may require aggregation and feature selection to enhance interpretability.

---

## 8. Gaps and Next Steps

Although the current plan has laid a solid foundation, the following steps still need to be taken:

1. Verify the accuracy of geographical pedestrian crossings between urban and county boundaries.  
2. Add optional land area data from the census bulletin to calculate the population density per square mile.  
3. If time permits, add an additional climate variable (for example, precipitation).  
4. Complete the metadata documentation for all variables and create a concise data dictionary.  
5. Before the final submission, prepare for the repeatability test by re-running the simulated complete workflow.

These steps will enhance data quality assurance, reproducibility and policy relevance.

---

## 9. Expected outcomes

This project will provide a comprehensive analytical framework to demonstrate how weather patterns and demographic factors jointly affect air quality in the United States. The specific deliverables include:

- A unified dataset integrating air quality, meteorological and population data.  
- Statistical evidence and visualization show how temperature, humidity and urban density interact to form pollutant concentrations.  
- Automated, repeatable workflow scripts that can perform end-to-end operations from data collection to analysis.  
- A visual dashboard that summarizes the relationship between pollution and population in various regions of the United States.  
- Policy-related findings may inform future air quality regulations or urban planning initiatives.

After this semester, the project structure can serve as a template for future environmental analysis research, demonstrating how to integrate and automate publicly available datasets to generate transparent, data-driven insights.

