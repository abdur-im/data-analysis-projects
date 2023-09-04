# Impact of COVID-19 Mitigation Measures

## Table of Contents

- [Description](#description)
- [Tools & Technologies](#tools--technologies)
- [Raw Datasets Utilized](#raw-datasets-utilized)
- [Data Cleaning & Preprocessing](#data-cleaning--preprocessing)
- [Methodology](#methodology)
- [Discussion & Results](#discussion--results)
- [Limitations](#limitations)
- [Challenges](#challenges)
- [Suggested Improvements](#suggested-improvements)
- [Conclusion](#conclusion)

## Description

This project aims to analyze the impact of various COVID-19 mitigation strategies on the trend of cases and deaths across different countries and regions. By examining specific mitigation strategies' correlation with growth rates of cases and deaths, we strive to understand the strategies' effectiveness in controlling the virus spread during the initial stages of the pandemic.

## Tools & Technologies

- **SQL**: Used for data cleaning and manipulation.
- **Python**:  Used for data processing and analysis with libraries:
  - SciPy
  - Seaborn
  - NumPy
  - Pandas
  - Matplotlib
- **Tableau**: Used for data visualization.

## Data Source

The following raw CSV files were utilized: 

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


The raw CSV files above and the cleaned CSV files, `mitigation_strategies_covid.csv` and `owid_covid_data_revised.csv`, can be found [here](https://drive.google.com/drive/folders/1mdtuJZRBXy27vSetv1_IAlE8X74G69YZ?usp=sharing). 

_For a detailed breakdown of the variables in the cleaned CSV files, consult the [codebook](https://github.com/abdur-im/data-analysis-projects/blob/main/Covid_Mitigation_Measures/codebook.md)._




## Data Cleaning & Preprocessing

### Initial Observations:
The early days of the pandemic showed limited global spread. Out of 185 countries, only 23 had confirmed cases by January 2020.

### Data Range Decision:

The scope was limited to the period between 01 February 2020 and 31 August 2020 to capture the pandemic's growth phase.

### Country Exclusion Criteria:

Countries missing data for more than 10% of the period were excluded to ensure robust analysis.

### Rationale:

The data range was chosen to provide a clear view of the pandemic's critical growth phase, emphasizing data integrity and minimizing the influence of data gaps.

### Limitations:

This approach does exclude the pandemic's complete timeline and might omit key nations due to the strict data criterion.

### Data Credibility:

Credibility of the data was ensured using the file `WHO-COVID-19-global-data.csv`, found [here](https://covid19.who.int/data).


## Methodology

### SQL Preprocessing:
Data cleaning and preprocessing were carried out using SQL. The specific details of how the data was cleaned and preprocessed can be found in the file [sql_processes.md](https://github.com/abdur-im/data-analysis-projects/blob/main/Covid_Mitigation_Measures/sql_processes.md).


### Python Analysis:
EDA was conducted using Python, followed by merging datasets and constructing time series data. Trend and correlation analyses were then performed.

### Tableau Visualization:
   - World Map: Displaying case spread and intensity.
   - Pie Charts: Distribution of various mitigation strategies across regions.
   - Time Series Analysis: Visualizing case growth over time juxtaposed with the implementation of mitigation strategies.

## Discussion/Results

### World Map Analysis:
Tableau's world map was employed to understand the geographical spread and intensity of cases.

### Pie Charts:
Pie charts created in Tableau helped visualize the distribution of different mitigation strategies across various countries and regions.

### Time Series Analysis:
Tableau's time series charts provided insights into the timeline of the pandemic, showcasing spikes in cases and deaths against the backdrop of different mitigation strategies.


## Challenges

Choosing the right correlation method, interpreting heatmaps, potential visual overload, missing countries, and balancing the number of variables were all challenges faced during the project. Since correlation does not imply causation, the findings are associations and not causative factors

## Suggested Improvements

Improvement in SQL data cleaning could be achieved in pgAdmin by scripting repetitive queries in Python. Conducting further rigorous statistical testing to draw reliable and firm conclusions.

## Conclusion

Understanding the effectiveness of different COVID-19 mitigation strategies is crucial for policymakers and public health officials, especially when preparing for future outbreaks or virus variants.
