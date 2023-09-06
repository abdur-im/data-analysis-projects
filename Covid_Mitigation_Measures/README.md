# Impact of COVID-19 Mitigation Measures

## Table of Contents

- [Description](#description)
- [Tools & Technologies](#tools--technologies)
- [Data Source](#data-source)
- [Data Cleaning & Preprocessing](#data-cleaning--preprocessing)
- [Methodology](#methodology)
- [Discussion & Results](#discussion--results)
- [Challenges](#challenges)
- [Suggested Improvements](#suggested-improvements)
- [Conclusion](#conclusion)

## Description

This project aims to analyze the impact of various COVID-19 mitigation strategies on the trend of cases and deaths across different countries and regions during the initial stages of the pandemic. By examining specific mitigation strategies' correlation with growth rates of cases and deaths, we strive to understand the strategies' effectiveness in controlling the virus spread.

## Tools & Technologies

- **SQL**: Used for data cleaning and manipulation.
- **Python**:  Used for data processing and analysis with libraries:
  - SciPy
  - Seaborn
  - NumPy
  - Pandas
  - Matplotlib
- **Tableau**: Built a dashboard for data visualization.

## Data Source

### [Kaggle](https://www.kaggle.com/datasets/pranjalverma08/coronavirus-covid19-indepth-dataset?select=owid-covid-data+%281%29.csv)
   - `covid_stringency_index.csv`
   - `internal_movement_covid.csv`
   - `international_travel_covid.csv`
   - `public_gathering_rules_covid.csv`
   - `school_closures_covid.csv`
   - `stay_at_home_covid.csv`
   - `workplace_closures_covid.csv`

### [Our World in Data](https://ourworldindata.org/covid-cases)
   - `owid_covid_data.csv`


The raw CSV files above and the cleaned CSV files, `mitigation_strategies_covid.csv` and `owid_covid_data_revised.csv`, can be found [here](https://drive.google.com/drive/folders/1Vy46-p5e1UwxxGeiAEJUmGANAziTDBZU?usp=sharing). 

_For a detailed breakdown of the variables in the cleaned CSV files, consult the [codebook](https://github.com/abdur-im/data-analysis-projects/blob/main/Covid_Mitigation_Measures/codebook.md)._




## Data Cleaning & Preprocessing

### Initial Observations:
The early days of the pandemic showed limited global spread. Out of 185 countries, only 23 had confirmed cases by January 2020.

### Data Range:

The scope was limited to the period between 01 February 2020 and 31 August 2020 to capture the pandemic's growth phase. This data range was chosen to provide a clear view of the pandemic's critical growth phase, emphasizing data integrity and minimizing the influence of data gaps.


### Country Exclusion Criteria:

Countries missing data for more than 10% of the period were excluded to ensure robust analysis.


### Limitations:

This approach does exclude the pandemic's complete timeline and might omit key nations due to the strict data criterion which may impact the findings.

### Data Credibility:

The credibility of the data was ensured using the World Health Organization file `WHO-COVID-19-global-data.csv`, found [here](https://covid19.who.int/data).


## Methodology

### SQL Preprocessing:
Data cleaning and preprocessing were carried out using SQL. The specific details of how the data was cleaned and preprocessed can be found in the file [sql_processes.md](https://github.com/abdur-im/data-analysis-projects/blob/main/Covid_Mitigation_Measures/sql_processes.md).


### Python Analysis:
EDA was conducted using Python, followed by merging datasets and constructing time series data. Trend and correlation analyses were then performed using statistical tools and heatmaps. The detailed code and the visual outputs are displayed in the file [COVID_Mitigation_Measures.ipynb](https://github.com/abdur-im/data-analysis-projects/blob/main/Covid_Mitigation_Measures/COVID_Mitigation_Measures.ipynb).

### Tableau Visualization:
Data visualization using the Tableau dashboard, [The Global COVID-19 Landscape: Metrics and Measures](https://public.tableau.com/views/TheGlobalCOVID-19LandscapeMetricsandMeasures/Dashboard1?:language=en-US&:display_count=n&:origin=viz_share_link), highlights the following: 
   - World Map: Displaying country policy stringency and several key demographics.
   - Pie Charts: Distribution of various mitigation strategies across regions.
   - Time Series Analysis: Visualizing and comparing the growth of cases and deaths over time among countries.

## Discussion & Results


### Tableau Visualization

- **Growth of Cases and Deaths:** Over time, there was a clear increase in the average total cases and deaths per million people. This trend highlights the pandemic's escalating severity as it spread across different regions and countries.

- **Impact of Stringency Index:** A noticeable trend emerged correlating the average stringency index with the average total cases and deaths per million people. Specifically, lenient measures seemed to lead to surges in cases and deaths, while stricter measures often resulted in a slower growth rate of these metrics.

- **Geographical Trends:** It became evident that breaking down the data by continent offered clearer insights. This is attributed to the geographical, socio-political, and cultural differences affecting the pandemic's spread and management in various regions. For instance, Europe experienced larger spikes early on compared to other continents, while Oceanian countries, with the exception of Australia, managed to keep the initial stages of the pandemic relatively under control.

### Python Analysis

#### Spearman Rank Correlation Analysis:
In order to assess potential non-linear relationships between the various mitigation strategies and the growth rate of COVID-19 cases and deaths, we used Spearman rank correlations. The measures investigated include the stringency index, restrictions on internal movements, controls on international travel, restrictions on gatherings, school closures, stay-home requirements, and workplace closures. Key findings are:

- `Restriction on Gatherings` showed a strong positive correlation with new deaths per million (0.84). Similarly, it had a significant correlation with new cases per million (0.77).
  
- `Stay Home Requirements` demonstrated a notable correlation with both new deaths per million (0.77) and new cases per million (0.70).

- `Restrictions on Internal Movements` and `Workplace Closures` also depicted notable positive correlations with the trends of new cases and deaths per million.

#### Relationship between Stringency Index and Mortality:
Analysis of the relationship between the Stringency Index and New Deaths per Million across different continents yielded varied insights:

- Some continents exhibited a significant negative correlation between stringency measures and deaths, suggesting that where stricter measures were in place, fewer deaths were reported.

- Contrarily, other regions showed no clear correlation, suggesting potential external influences or policy enforcement disparities.

- Europe the was only continent to present a weak positive correlation. This could hint at possible delayed effects of stringent measures, or that these measures were reactionary to escalating situations rather than preventive.


#### Aggregated Analysis by Continent:
An aggregated analysis by continent was conducted to understand the average measures and COVID trends on a broader geographical scale. Highlights are:

- `South America` observed the highest stringency index (64.94) and recorded the highest new cases and deaths per million, with 43.23 and 1.98 respectively.

- `Oceania`, apart from Australia, recorded the lowest average stringency index (42.61) and also had the least new cases and deaths per million, supporting our earlier observation that these countries managed the initial stages of the pandemic efficiently.

- `Europe` and `North America`, despite having varying stringency indexes, both observed significant new cases per million with Europe at 18.86 and North America at 26.73. Their deaths per million were also considerable with values of 0.85 and 0.83 respectively.



## Challenges

1. **Choosing the Appropriate Correlation Method:** 
    - Python offers multiple statistical methods, making selection challenging.

2. **Potential Visual Overload:** 
    - Tableau's dynamic visual capabilities risked over-complication, requiring informed design decisions.

3. **Missing Countries:** 
    - Data gaps during SQL preprocessing affected the project's overall scope.

4. **Balancing the Number of Variables:** 
    - Python provided extensive data processing options, but selecting the most relevant variables was crucial.
    - Tableau visualizations required selective inclusion of variables to avoid visual clutter.

5. **Correlation Does Not Imply Causation:** 
    - All findings should be interpreted with caution. The correlations observed might be influenced by external factors or unaccounted variables. For instance, testing and reporting strategies vary significantly from country to country and could significantly impact the analysis.


## Suggested Improvements

1. **Enhancing SQL Data Cleaning in pgAdmin:** 
    - Python scripting can be integrated for automating repetitive SQL queries, enhancing the data cleaning process.

2. **Conducting Rigorous Statistical Testing:** 
    - While Python provided robust tools for analysis, the next steps involve deeper statistical testing.
    - Leveraging more advanced features in libraries like `scipy.stats` can further validate the conclusions.


## Conclusion

Analyzing and understanding the effectiveness of different COVID-19 mitigation strategies is crucial for policymakers and public health officials, especially when preparing for future outbreaks or virus variants. 