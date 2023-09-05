# COVID-19 Mitigation Measures Codebook

This codebook provides detailed information about the datasets used in the project `Impact of COVID-19 Mitigation Measures`.

## Dataset 1: Mitigation Strategies Covid

### Details:

**Dataset Name:** mitigation_strategies_covid.csv 

### **Dataset Description:** 
This dataset stores the impact of various mitigation strategies during the initial stages of COVID-19, specifically between 01 February 2020 and 31 August 2020.

### **Source:** 
Data originated from the 'Coronavirus (COVID-19) In-depth Dataset' from Kaggle. [Link to Source](https://www.kaggle.com/datasets/pranjalverma08/coronavirus-covid19-indepth-dataset?select=owid-covid-data+%281%29.csv).

### **General Information:** 
- **Date of Last Update:** 2021 (2 years ago from the current year)
- **Number of Observations:** 35145 entries (excluding the header row)
- **Related Documents:** Refer to the [sql_processes.md](https://github.com/abdur-im/data-analysis-projects/blob/main/Covid_Mitigation_Measures/sql_processes.md) for more details on the data cleaning process.

## Variable Information

### `country` 
- **Type:** String
- **Description:** Name of the country.

### `code`
- **Type:** String (ISO Alpha-3)
- **Description:** ISO Alpha-3 country code.

### `day` 
- **Type:** Date (`yyyy-mm-dd`)
- **Description:** Specific day of data recording.

### `stringency_index`
- **Type:** Float (0 to 100)
- **Description:** Index calculated as the mean score of nine mitigation metrics, with a higher score indicating a stricter response.

### `restrictions_internal_movements` 
- **Type:** Integer (0 to 2)
- **Description:** Represents government policies in restricting internal movements, with 2 being the strictest.

### `international_travel_controls` 
- **Type:** Integer (0 to 2)
- **Description:** Represents government policies on international travel, with 2 being the strictest.

### `restriction_gatherings`
- **Type:** Integer (0 to 4)
- **Description:** Categorization based on the size of public gatherings.

### `school_closures`
- **Type:** Integer (0 to 3)
- **Description:** Indication of school closure policies, with 3 being fully closed.

### `stay_home_requirements`
- **Type:** Integer (0 to 3)
- **Description:** Grouping of countries based on stay-at-home regulations.

### `workplace_closures`
- **Type:** Integer (0 to 3)
- **Description:** Indication of workplace closure policies, with 3 being fully closed.


## Dataset 2: OWID COVID Data (Revised)

### Details:

**Dataset Name:** owid_covid_data_revised.csv 

### **Dataset Description:** 
This dataset tracks total and new COVID-19 cases and deaths by country, as well as other demographic and economic metrics. The data spans from 01 February 2020 to 31 August 2020.

### **Source:** 
Data is based on the `owid_covid_data.csv` from [Our World in Data](https://ourworldindata.org/covid-cases). 

### **General Information:** 
- **Date of Last Update:** Daily updates.
- **Number of Observations:** 35145 entries (excluding the header row).
- **Related Documents:** Refer to the [sql_processes.md](https://github.com/abdur-im/data-analysis-projects/blob/main/Covid_Mitigation_Measures/sql_processes.md) for more details on the data cleaning process.

## Variable Information

### `code`
- **Type:** String (ISO Alpha-3)
- **Description:** ISO Alpha-3 country code.

### `continent` 
- **Type:** String
- **Description:** The continent to which the country belongs.

### `country` 
- **Type:** String
- **Description:** Name of the country.

### `day` 
- **Type:** Date (`yyyy-mm-dd`)
- **Description:** Specific day of data recording.

### `total_cases`, `new_cases`, `total_deaths`, `new_deaths`
- **Type:** Integer
- **Description:** Cumulative and new counts for COVID-19 cases and deaths.

### `total_cases_per_million`, `new_cases_per_million`, `total_deaths_per_million`, `new_deaths_per_million`
- **Type:** Float
- **Description:** Adjusted counts for COVID-19 cases and deaths per million people.

### `population_density`
- **Type:** Float
- **Description:** Population density of the country (people per sq. km).

### `median_age`
- **Type:** Float
- **Description:** Median age of the country's population.

### `gdp_per_capita`
- **Type:** Float
- **Description:** GDP per capita of the country.

### `human_development_index`
- **Type:** Float (0 to 1)
- **Description:** Human Development Index (HDI) score of the country.

### `population`
- **Type:** Integer
- **Description:** Total population of the country.

