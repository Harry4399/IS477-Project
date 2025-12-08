Linking City Population and PM2.5 in Illinois
Contributors
Zhaojie Hua (project lead, data curation, analysis, writing)
Owen Wen (contributions to earlier milestones, feedback on design and writing)

Summary
This project has established an example of a small but complete repeatable data workflow in the field of environmental data science. We investigated the relationship between the population of cities in Illinois and fine particulate matter (PM2.5). The main purpose is not to draw authoritative conclusions on environmental research, but to document how raw public data is obtained, cleaned, associated, and analyzed in a transparent manner so that others can replicate every step.

The initial concept of this project was extremely grand. In the initial proposal, we planned to integrate multiple national datasets: historical air quality data from the U.S. Environmental Protection Agency's Air Quality System (AQS), meteorological variables, and population density data from all counties or cities in the United States. During the semester, we realized that this scope was too broad for the limited time. Therefore, in the final stage, we narrow down the scope to one state, one pollutant, and one day. This smaller scope still reflects the practical issues of data integration and reproducibility, but allows us to complete and document the entire process.

Our core research question is very simple. First of all, can we reliably link EPA monitoring data with urban population estimates using only public data and script transformation? Secondly, after the data integration is completed, what patterns of urban population and PM2.5 concentration can we observe between the selected dates? We do not attempt causal reasoning or long-term trend analysis. This project is a study on data lifecycle management from data acquisition to quality inspection. The workflow is divided into several stages. First, we filter the AQS file to contain only PM2.5 and retain the variables related to the aggregation at the city level: state code, county code, site number, date, and arithmetic mean. For each monitoring point, we append the name of the city and state where it is located to a metadata column, and then calculate the average PM2.5 value of each city on a given date. Meanwhile, we read the population spreadsheet, delete the title row, and extract the estimated value for 2024 as a numerical variable. We split the "Geographical Region" field into separate city and state strings and standardized the spelling and capitalization.

Since the name field is usually the source of implicit errors, we have put in extra effort to normalize it. The city names in the AQS exported data and census forms have removed the extra Spaces, been converted to uppercase initial letter format, and simplified when necessary to be able to match common variants. We removed suffixes such as "city" or "Village". After preprocessing, we merge the dataset based on state and normalized city names. The merged document includes 22 cities in Illinois, each with PM2.5 measurement data as of January 1, 2024, and population estimates for 2024.

Using this comprehensive dataset, we conducted a brief descriptive analysis. We calculated the pooled metrics of two variables, examined the distribution of PM2.5 values, and calculated the Pearson correlation coefficient between population and PM2.5. We also created a scatter plot, where the X-axis represents the logarithmic scale of the population and the Y-axis represents the PM2.5 value. This image was exported as the file fig_city_population_vs_pm25_20240101.png and stored as part of the repeatable artifact package in the repository.

The numerical results show that the population on the sample day is weakly negatively correlated with PM2.5. In the snapshot of this day, the PM2.5 value in big cities is not necessarily higher than that in small cities. For instance, Chicago has a large population, but compared with some medium-sized cities, its PM2.5 value is at a medium level. Due to the small sample size and the data covering only one day, we cannot interpret this pattern as a stable relationship. On the contrary, we use it to illustrate that even limited analysis can reveal surprising patterns once the data is cleaned and aligned.
Therefore, the main contribution of this project lies in its methodology. All the transformation steps from the original AQS and census files to the final merged dataset are implemented in the Jupyter Notebook code file. Described in this README file. All the intermediate and final data files used in the analysis are stored in the code repository. Anyone who has access to the same public data source and a standard Python environment should be able to reproduce our results and further expand the research to more dates, other contaminants, or different regions.


Data profile
This project uses two primary datasets plus several derived files. Both sources are produced by U.S. federal agencies and are distributed under open data policies that permit reuse for research and teaching.

EPA Air Quality System (AQS) daily data.
The first source is an extract from the AQS Data Mart. We downloaded daily PM2.5 data for all monitoring sites in Illinois on 2024-01-01 and saved them as aqs_il_pm25_20240101.csv. Each row corresponds to one site on one date. Key variables include:
  state_code, county_code, and site_number: identifiers for the monitoring site.
latitude and longitude: location coordinates in WGS84.
 parameter_code and parameter: pollutant identifier (88101 for PM2.5).
date_local: local calendar date of the measurement.
 arithmetic_mean: daily average PM2.5 concentration in micrograms per cubic meter.
Additional fields describing sample duration, units, method, events, and quality indicators.
The AQS data are sensitive because they describe environmental exposures that may relate to health outcomes, but they are not individually identifiable and do not contain personal information. The main ethical issue concerns correct interpretation. Single-day values should not be over-interpreted as chronic exposure or regulatory violations. We therefore treat this file as environmental monitoring data rather than clinical or personal data.

Illinois population estimates for incorporated places.
The second main source is an Excel file from the U.S. Census Bureau's Population Estimates Program. The original file, SUB-IP-EST2024-POP-17.xlsx, provides annual resident population estimates for all incorporated places in Illinois, from April 1, 2020, through July 1, 2024. For each place, the table includes:
Geographic Area: text string combining place name and state (for example, "Abingdon city, Illinois").
2020, 2021, 2022, 2023, 2024: population estimates as of July 1 in each year.
Some header rows and notes that describe methodology and limitations.
We convert the Excel file to a cleaned CSV called IL_population_2024.csv. In our cleaned version, we keep only the columns needed for integration: normalized state, simplified city name, and population_2024 (the 2024 estimate). These data represent aggregate counts at the place level and therefore do not involve personal identifiers. However, population data are often used in funding formulas and policy discussions, so we take care to cite the source and year correctly.
Derived integration and analysis files.
Several additional files are produced during the workflow:
 il_population_2024_clean.csv: cleaned population table with standardized city and state names and numeric population values.
pm25_population_merged_20240101_v3.csv: merged dataset linking AQS daily PM2.5 to 2024 population for cities that appear in both sources after normalization.
 city_pm25_population_summary_20240101.csv: city-level summary table with one row per city reporting the mean PM2.5, population, and number of monitoring sites contributing to the mean.
fig_city_population_vs_pm25_20240101.png: scatterplot of population versus PM2.5 on a log scale.
All data files are stored under the repository's data/ directory. The Jupyter notebook code.ipynb documents the full path from raw to derived data. No proprietary data or confidential information is included. The only potential ethical concern is that readers might combine these results with other datasets to make claims about individual risk. We address this by clearly stating that our analysis operates at the city level and covers only a single day, and by avoiding any statements about specific individuals or neighborhoods.


Data quality
We evaluated the data quality at multiple stages: initial collection, analysis of the raw files, data cleaning and standardization, and post-merging checks. Since both main data sources are official government products, they have already met many basic standards. However, the data integration task introduces additional risks, especially in terms of identifiers and coverage.

For the AQS daily data, we first confirmed the completeness of the data extraction for the data from Illinois on January 1, 2024, by checking the number of unique monitoring sites and the range of the date_local values. All records had the same date, and each site appeared only once, so there were no duplicate data in the time dimension. Then, we checked for missing values or obviously unreasonable values in the arithmetic_mean column. On this day, the PM2.5 average value fell within a narrow range of approximately 1 to 5 µg/m³, which was reasonable for winter conditions. There were no negative values or extreme outliers. Therefore, we retained all observations, but noted that single-day data may not represent typical situations. However, for simplicity, we grouped some very similar means together.

The population file required more extensive cleaning before the quality assessment. The original Excel table contained multiple header rows, footnotes, and combined text fields for geographical areas. We deleted rows that did not contain numerical population estimates and then converted the year column to numbers. Minor inconsistencies existed between the base year and subsequent years in 2020, but these were likely rounding issues or minor updates that did not affect our use of the 2024 data. We considered this data valid because it was extracted from the government's population census database, but we knew t

One of the main quality risks of this project was the matching of city names between the AQS and census data sources. The recording methods of city names are different: some include "city" or "village", some include punctuation marks, and the capitalization is also different. We applied a series of transformations to reduce the possibility of mismatching or incorrect matching. We converted the data to have capitalized initial letters and removed leading and trailing spaces. We also simplified suffixes like "city" and "village". We also sampled the data to confirm the form was correct. However, the names of the monitoring sites could not fully correspond to the division area names of the population table, so there would still be unmatched locations.

After standardization, we performed the merge and checked several quality indicators. First, we counted the number of cities that only existed in the AQS dataset, only in the population data table, and simultaneously in both datasets. We found that many small towns in the census list did not have AQS monitoring stations, and many small towns in the census list did not have AQS monitoring stations either. Finally, we decided that the merged dataset contained 22 cities that had both population and PM2.5 data. However, we knew there would be a problem that the sample size would be very limited and the representativeness would be greatly reduced.

In addition, we also checked the internal consistency of the merged file. For each city, we verified that the PM2.5 average value and the population number were correct. And the number of monitoring sites involved in the calculation was at least one. We briefly checked extreme cases, such as the largest and smallest cities in terms of population and the cities with the highest and lowest PM2.5 values, to ensure they corresponded to real cities and not outliers from the data cleaning process. Finally, we believe that quality issues may also arise from the limitations of the records. For instance, the PM2.5 data represents the value for a specific day in winter, and thus may differ from other seasons. The population estimation values are constant estimates for the entire year. Therefore, the merged data set combines an instantaneous snapshot of air quality with the annual population estimation values. However, from an evaluation perspective, we think this mismatch is acceptable for the purpose of data integration.

Overall, we believe that the data quality is sufficient for the exploration of our project, but there are obvious limitations in terms of coverage and temporal representativeness. These limitations will be elaborated on in the "Research Findings" and "Future Work" sections.


Findings
The integrated dataset we have includes 22 cities in Illinois. These cities have both PM2.5 and population data for the year 2024. For each city, we have the average daily PM2.5 concentration on January 1, 2024, and the estimated population for 2024. We believe these data are sufficient to support our study on the relationship between urban size and particulate matter pollution.

Our data shows that the PM2.5 values are relatively low. There are no extreme outliers, and the average values of each city are slightly higher than 3 µg/m³, with most cities ranging between 2 to 4 µg/m³. However, the population figures in another dataset vary greatly, from small towns with only a few thousand residents to Chicago with over 2.6 million people, spanning several orders of magnitude.

We calculated the Pearson correlation coefficient between population and average PM2.5. The correlation coefficient was slightly negative (approximately -0.27), indicating that in this sample, cities with larger populations tended to have the same or slightly lower PM2.5 concentrations as cities with smaller populations. If people expect higher emissions in big cities, this result seems contrary to expectations. We believe there is another factor that affects PM2.5 changes, which is that monitoring stations in small towns may be located near specific pollution sources or flight routes.

The scatter plot with a logarithmic scale for the population axis can clearly show this pattern. The population gaps between cities are large, but the PM2.5 values have small differences. Chicago is located on the right side of the chart, with moderate PM2.5 values. Some medium-sized cities show similar or slightly higher concentrations. We did not see any obvious monotonic trend or threshold. Instead, the scatter plot composed of these points indicates that there is almost no correlation between urban size and PM2.5 on a specific date at the observation station.

Due to the small sample size and the focus on data from a single date, we cannot fully trust this data. They do not guarantee that the population is unrelated to air pollution overall. Instead, they emphasize the danger of drawing strong conclusions based on a small cross-sectional dataset without considering time averages, emission inventories, or atmospheric transport. The main value of these results is to show that even simple exploratory analysis can be contrary to people's intuitive understanding and encourage us to think and explore more. From a methodological perspective, the key outcome is that we successfully linked climate monitoring data with population estimation data through data analysis. Now, the combined dataset and charts can serve as the initial form for more complex models, such as multi-day averages, regression analysis, or weighted exposure indicators based on the population size of PM2.5.

Future work

We believe that this project can provide a very good platform for subsequent research. Currently, our analysis only covers one pollutant, one state, and one day of data. The next step is naturally to expand these. Using the same AQS interface, we can download daily PM2.5 data for an entire year or several years. Using these data, we can calculate the seasonal or annual average values for each city and study whether the non-relevance we observed in 2024 persists over a longer period, and also study the data trends within quarters.

The second direction is to expand the geographical scope. The population integration method used in this project can be extended to other states. In such cases, we can repeat this data processing procedure for neighboring states or the entire United States. However, this will increase the workload, such as refining each place name, and may require the use of external reference tables based on Federal Information Processing Standards (FIPS) or GNIS identifiers. Moreover, new geographical distinctions can be used to compare urban and rural areas and test whether there are differences in the relationship between population and PM2.5 in different lifestyle regions.

Thirdly, we can improve the population measurement indicators themselves. Our current analysis uses the total urban population as a single scalar. In fact, the climate situation depends on people's daily travel patterns. Combining monitoring data with more refined spatial units (such as census tracts) or grid-based population surface data will generate more realistic indicators. However, from an ethical perspective, this will also introduce new privacy issues and require more complex spatial connections.

Fourth, we can expand the pollutant dimension. AQS provides data on ozone, nitrogen dioxide, sulfur dioxide, and other pollutants. The spatial and temporal patterns of some of these pollutants are different from PM2.5. Studying multiple pollutants allows us to investigate their correlations and understand the trends in cities that face multiple pollutants. Because regulatory standards usually consider multiple pollutants simultaneously, we can integrate regulatory standards more smoothly when doing so.

We can also improve the assessment of data quality and uncertainty. For example, we can collect and analyze information on monitor types, sampling duration, and daily record completeness. We can also use hierarchical models to explicitly model measurement errors in the data, which distinguish site-level noise and city-level averages. We can use census files to quantify the uncertainty related to small-area estimates to handle population data.

Another important research direction is automation and data sources. Currently, the workflow is implemented in a Jupyter Notebook file. Although this is sufficient for small projects, it mixes code, narrative, and results in one file. We believe a better approach is to use workflow managers such as Snakemake or Make to coordinate and separate data acquisition, cleaning, and analysis into modularized modules. Each script can record its input and output, and the code library can include machine-readable descriptions. This will make it easier for others to re-run the analysis in new environments or with updated data.

Finally, we can delve into the ethical and policy aspects of the project. We believe that air quality and population data used for environmental assessment and the location decision-making of monitoring stations will have an impact. Our small dataset has already shown that the monitoring coverage is uneven. We found that many small towns lack monitoring stations, but they are listed in the population table. Some monitoring stations are not in residential areas but in industrial or traffic corridors. Future projects can clearly analyze these patterns, integrate socio-economic indicators and mapping tools to show which communities are being well-monitored.

In summary, we have now established a solid data model foundation. Future work can be expanded in multiple dimensions (time, space, pollutants, methods, and ethics) without having to repeat the initial data integration efforts.

Reproducing the analysis
The repository is organized to allow another user to reproduce the workflow with minimal manual effort.
Software environment
Install Python 3.10 or later.
Install required packages: pandas, numpy, and matplotlib.
Optional: Use a virtual environment or conda environment to isolate dependencies.
Repository structure
data/aqs_il_pm25_20240101.csv – raw AQS daily PM2.5 data for Illinois on 2024-01-01.
data/SUB-IP-EST2024-POP-17.xlsx – raw Illinois population estimates for incorporated places.
data/IL_population_2024.csv – intermediate CSV converted from the Excel file.
data/il_population_2024_clean.csv – cleaned population table.
data/pm25_population_merged_20240101_v3.csv – merged dataset with PM2.5 and population.
data/city_pm25_population_summary_20240101.csv – city-level summary table.
data/fig_city_population_vs_pm25_20240101.png – final figure.
code.ipynb – Jupyter notebook implementing the full workflow.
ProjectPlan.md, StatusReport.md – earlier milestone documents.
Steps
Clone the repository from GitHub.
Open code.ipynb in Jupyter Lab, Jupyter Notebook, or VS Code.
Run all cells in order. The notebook will:
Read the raw AQS CSV and population Excel file.
Create cleaned CSVs for population and for the merged dataset.
Compute descriptive statistics and the correlation between population and PM2.5.
Produce the scatterplot and save it as fig_city_population_vs_pm25_20240101.png.
Check that the generated files in data/ match those already tracked in the repository.
External data sources
If you wish to re-download the raw data rather than using the copies in this repository, follow the links in the References section to the AQS Data Mart and the Census population estimates site. Use the same filters (Illinois, PM2.5, date 2024-01-01) to match our data.

References
U.S. Environmental Protection Agency. Air Quality System (AQS) API Key
U.S. Census Bureau.City and Town Population Totals: 2020-2024 (https://www.census.gov/data/tables/time-series/demo/popest/2020s-total-cities-and-towns.html)
OpenRefine
Jupyter Notebook 

