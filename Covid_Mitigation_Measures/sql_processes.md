# SQL Processes for Data Cleaning and Preparation

This guide outlines the SQL processes and queries utilized for data cleaning and preparation.

## 1. Table Creation for CSV Imports

**Note:** All CSV files were backed up to allow for potential data reversion.

To streamline data imports, tables were first established in pgAdmin. The column names in the CSV files were standardized for consistency within pgAdmin. An example table creation for the `covid_stringency_index` dataset is:

```
CREATE TABLE covid_stringency_index (
    country VARCHAR(100),
    code VARCHAR(10),
    day DATE,
    stringency_index DECIMAL);
```

Each CSV file had a corresponding table created with attributes matching the file columns. For `owid_covid_data`, non-essential columns were omitted.

## 2. CSV File Imports

After defining tables, the CSV files were loaded into pgAdmin. The process entailed:
- Analyzing the file structure.
- Aligning data types and columns.
- Loading the data into its respective table.

## 3. Data Cleaning

Initial cleaning involved:

### 3.1. Duplicate Row Elimination

Both the country code and date jointly identified unique rows. The query for eliminating duplicates from `covid_stringency_index` was:

```
DELETE FROM covid_stringency_index
WHERE (code, day) IN (
    SELECT code, day
    FROM (SELECT code, day, ROW_NUMBER() OVER (partition BY code, day) AS rnum
        FROM covid_stringency_index) t
    WHERE t.rnum > 1);
```

This query was adapted for all tables to remove duplicates.

### 3.2. Country Consistency Across Tables

The 7 tables `covid_stringency_index`, `internal_movement_covid`, `international_travel_covid`, `public_gathering_rules_covid`, `school_closures_covid`, `stay_at_home_covid`, and `workplace_closures_covid` all contained identical countries. We verified this using the following queries for all pairs of aforementioned tables:

```
(SELECT DISTINCT country FROM covid_stringency_index)
EXCEPT
(SELECT DISTINCT country FROM internal_movement_covid);
```

```
(SELECT DISTINCT country FROM internal_movement_covid)
EXCEPT
(SELECT DISTINCT country FROM covid_stringency_index);
```

The first query returns countries in `covid_stringency_index` that were not in `internal_movement_covid`. The second query returns countries in `internal_movement_covid` that were not in `covid_stringency_index`. 

It suffices to compare `owid_covid_data` to any one of the previously mentioned 7 tables. All of the countries in the other tables also existed in `owid_covid_data`. However, there were 70 countries in `owid_covid_data` that were absent from the other tables. Data inspection was utilized to ensure that these 70 countries did not exist in the other tables via other names.

### 3.3. Removing Inconsistent Records

To ensure country consistency across tables, records in `owid_covid_data` not present in the other tables were deleted:

```
DELETE FROM owid_covid_data
WHERE country IN (
(SELECT DISTINCT country FROM owid_covid_data)
EXCEPT
(SELECT DISTINCT country FROM covid_stringency_index));
```

### 3.4. Addressing NULLs and Date Gaps

NULL values and date gaps were addressed using various methods depending on the nature of the data. Countries missing over 10% of their data were excluded. For instance, all records related to the country Liechtenstein were discarded due to significant data gaps. This was achieved in `covid_stringency_index` using:

```
DELETE FROM covid_stringency_index
WHERE country = 'Liechtenstein';
```

Similarly, due to sparse data in the `owid_covid_data` table, the following countries were removed from all tables: Hong Kong, Taiwan, Faeroe Islands, Greenland, Syria, South Sudan, Andorra, Bermuda, Cuba, Guam, Dominica, Kosovo, Monaco, San Marino, Somalia, United States Virgin Islands, Aruba, and Puerto Rico.

The following query searches `covid_stringency_index` for any records where `country`, `code`, or `stringency_index` is NULL between 01 February 2020 and 31 August 2020:

```
SELECT * FROM covid_stringency_index WHERE (country IS NULL OR code IS NULL OR stringency_index IS NULL) AND day between '2020-02-01' AND '2020-08-31';
```

No such NULL values existed in the tables `covid_stringency_index`, `internal_movement_covid`, `international_travel_covid`, `public_gathering_rules_covid`, `school_closures_covid`, `stay_at_home_covid`, and `workplace_closures_covid`. 

However, there were several NULL values found in the `owid_covid_data` table. We used the following two queries to verify that the column `total_cases` had NULL values whenever the column `new_cases` were 0:

```
SELECT * FROM owid_covid_data WHERE (total_cases IS NULL) AND (day between '2020-02-01' AND '2020-08-31');
```

```
SELECT * FROM owid_covid_data WHERE (new_cases <> 0) AND (total_cases IS NULL) AND (day between '2020-02-01' AND '2020-08-31');
```

The first query showed the records where the column `total_cases` had NULL values but the second query yielded no results. Thus, the NULL values in the column `total_cases` were replaced with 0 in the `owid_covid_data` table:

```
UPDATE owid_covid_data
SET total_cases = 0
WHERE (total_cases IS NULL) AND (day between '2020-02-01' AND '2020-08-31'); 
```

Careful data inspection revealed that the column `new_cases` had NULL values whenever there was an overcount of previous cases that resulted in a decrease in the `total_cases` column. Thus, these NULL values were replaced with 0. Similarly, the NULL values in the columns `total_deaths`, `new_deaths`, `total_cases_per_million`, `new_cases_per_million`, `total_deaths_per_million`, and `new_deaths_per_million` were all replaced with 0.

Data imputation was applied appropriately to ensure minimal error was introduced. For instance, the following query on `stay_at_home_covid` revealed that the country Bulgaria had a missing data row for the day 03 June 2020:

```
WITH DateSeries AS (
    SELECT country, generate_series('2020-02-01'::date, '2020-08-31'::date, interval '1 day')::date AS series_date
    FROM (SELECT DISTINCT country FROM stay_at_home_covid) AS sub
)

SELECT d.country, d.series_date AS missing_date
FROM DateSeries d
LEFT JOIN stay_at_home_covid s ON d.series_date = s.day AND d.country = s.country
WHERE s.day IS NULL
ORDER BY d.country, d.series_date;
```

Similarly, it was found that `public_gathering_rules_covid` had a missing data row for the country Lebanon on the day 21 August 2020. Data was manually compared with the dataset `WHO-COVID-19-global-data.csv`, found [here](https://covid19.who.int/data). This allowed us to insert these missing data points:

```
INSERT INTO stay_at_home_covid (country, code, day, stay_home_requirements)
VALUES ('Bulgaria', 'BGR', '2020-06-03', 0);
```

```
INSERT INTO public_gathering_rules_covid (country, code, day, restriction_gatherings)
VALUES ('Lebanon', 'LBN', '2020-08-21', 0);
```

Similarly, we found that the country Macao had no records in `owid_covid_data` between 01 February 2020 and 31 August 2020. Hence, all records associated with Macao were dropped from all tables.

## 4. Joining Tables

With multiple CSV files having shared columns like `day`, `country`, and `country_code`, data from various tables were joined into a single comprehensive, robust table called `mitigation_strategies_covid`. This allowed all relevant data to be stored in one place, enhancing table operations. We used the following query:

```
CREATE TABLE mitigation_strategies_covid AS
SELECT 
    a.country, 
    a.code, 
    a.day, 
    a.stringency_index, 
    b.restrictions_internal_movements,
    c.international_travel_controls,
    d.restriction_gatherings,
	e.school_closures,
	f.stay_home_requirements,
	g.workplace_closures
FROM covid_stringency_index a
LEFT JOIN internal_movement_covid b ON a.code = b.code AND a.day = b.day 
LEFT JOIN international_travel_covid c ON a.code = c.code AND a.day = c.day
LEFT JOIN public_gathering_rules_covid d ON a.code = d.code AND a.day = d.day
LEFT JOIN school_closures_covid e ON a.code = e.code AND a.day = e.day
LEFT JOIN stay_at_home_covid f ON a.code = f.code AND a.day = f.day
LEFT JOIN workplace_closures_covid g ON a.code = g.code AND a.day = g.day;
```

In particular, the above query combined data from the following CSV files: 
`covid_stringency_index.csv`;
`internal_movement_covid.csv`;
`international_travel_covid.csv`;
`public_gathering_rules_covid.csv`;
`school_closures_covid.csv`;
`stay_at_home_covid.csv`;
`workplace_closures_covid.csv`

## 5. Exporting the Final Datasets

After all cleaning and transformations, the combined data table `mitigation_strategies_covid` and the table `owid_covid_data` were extracted as CSV files. The latter table was renamed as `owid_covid_data_revised` for distinction. The following queries were used to ensure an appropriate ordering of the columns before being exported for further analysis:

```
SELECT * FROM mitigation_strategies_covid WHERE day BETWEEN '2020-02-01' AND '2020-08-31' ORDER BY country ASC, day ASC;
```

```
SELECT * FROM owid_covid_data WHERE day BETWEEN '2020-02-01' AND '2020-08-31' ORDER BY country ASC, day ASC;
```

The raw and cleaned CSV files can be found in the folder [Covid_Mitigation_Measures Datasets](https://drive.google.com/drive/folders/1Vy46-p5e1UwxxGeiAEJUmGANAziTDBZU?usp=sharing).

---

Note: This document provides a high-level overview of the SQL processes. Some specific queries, especially those related to detailed cleaning and transformations, have been abbreviated for clarity.
