# netflix-data-analytics-eda-python
Advanced exploratory data analysis (EDA) on Netflix dataset using Python, featuring data cleaning, feature engineering, time-series analysis, and business insight extraction.
Netflix Data Analytics
Advanced Exploratory Data Analysis & Data Transformation Using Python

1. Project Summary
This project performs an end-to-end exploratory data analysis (EDA) on Netflix’s content dataset with a focus on data cleaning, feature engineering, trend analysis, and business-oriented insight extraction.
The objective was not only descriptive analytics but to simulate a production-style analytical workflow including:
•	Raw data ingestion
•	Data validation & quality checks
•	Missing value treatment
•	Feature engineering
•	Dimensional breakdown analysis
•	Time-series exploration
•	Insight documentation
This project demonstrates practical data wrangling and analytical reasoning aligned with industry expectations for Data Analyst roles.

2. Analytical Objectives
The analysis was structured around the following technical objectives:
1.	Quantify distribution of content types (Movies vs TV Shows)
2.	Identify geographic production concentration
3.	Evaluate temporal growth patterns of content additions
4.	Analyze genre distribution across content types
5.	Investigate lag between release year and Netflix onboarding
6.	Examine rating and duration distributions

3. Dataset Overview
Source File: Netflix Dataset.csv
Environment: Jupyter Notebook
Dataset Shape: (Rows × Columns determined dynamically during analysis)
Core Attributes
Column	Description
show_id	Unique identifier
type	Movie / TV Show
title	Content title
director	Director(s)
cast	Cast members
country	Country of production
date_added	Date added to Netflix
release_year	Original release year
rating	Maturity classification
duration	Runtime or seasons
listed_in	Genre(s)
description	Content summary
The dataset contains high-cardinality categorical variables and multi-value fields (e.g., genres, countries), requiring structured normalization.

4. Data Engineering & Cleaning Pipeline
4.1 Data Quality Assessment
•	Checked for null distributions using .isnull().sum()
•	Identified high-null columns (director, cast, country)
•	Inspected duplicates
•	Evaluated inconsistent categorical formatting
4.2 Missing Value Treatment
•	Imputed categorical nulls with "Unknown" where appropriate
•	Dropped records only when analytically necessary
•	Standardized country and genre formatting
4.3 Date Handling & Feature Engineering
•	Converted date_added to datetime format
•	Extracted:
o	year_added
o	month_added
•	Computed:
o	content_age_at_addition = year_added - release_year
4.4 Multi-Value Column Normalization
•	Split country and listed_in columns
•	Used .explode() for row-level normalization
•	Enabled granular country-level and genre-level aggregation
This step mimics dimensional modeling practices for analytical querying.

5. Exploratory Data Analysis (EDA)
5.1 Content Type Distribution
•	Aggregated content counts by type
•	Computed proportional distribution
•	Visualized imbalance between Movies and TV Shows
Observation: Movies significantly dominate content inventory.

5.2 Country-Level Production Analysis
•	Grouped normalized country-level dataset
•	Ranked countries by content volume
•	Evaluated geographic production concentration
Finding: United States leads content contribution, followed by India and the UK.

5.3 Time-Series Growth Analysis
•	Grouped by year_added
•	Analyzed content onboarding trend
•	Visualized acceleration in post-2015 period
Finding: Sharp growth observed between 2016–2019, indicating expansion strategy.

5.4 Genre Distribution Analysis
•	Exploded listed_in
•	Computed frequency of genres
•	Identified dominant content themes
Top recurring categories:
•	Drama
•	International Movies
•	Comedy

5.5 Release Year vs Addition Year Gap
•	Computed lag metric
•	Analyzed distribution of content age at onboarding
•	Observed mixed strategy: new releases + older acquisitions
This provides insight into Netflix’s acquisition lifecycle model.

5.6 Rating Distribution
•	Aggregated maturity ratings
•	Identified target demographic concentration
•	Observed majority clustering in TV-MA and TV-14 segments

6. Visualization Stack
•	Matplotlib for foundational plotting
•	Seaborn for statistical visualization
•	Count plots
•	Bar charts
•	Time-series line graphs
•	Distribution plots
Visualization decisions were made to prioritize interpretability and executive-level readability.

7. Key Analytical Insights
1.	Content library is movie-heavy (~ dominant share).
2.	Geographic production is concentrated in North America.
3.	Significant growth phase post-2015 indicates scaling.
4.	Genre diversity supports international audience targeting.
5.	Lag analysis suggests strategic back-catalog acquisition.
6.	Rating skew reflects adult and teen audience prioritization.

8. Business Interpretation
This analysis simulates how streaming platforms can leverage descriptive analytics to:
•	Optimize content acquisition strategy
•	Identify underrepresented markets
•	Forecast genre demand
•	Support recommendation systems
•	Analyze lifecycle of content inventory
•	Assist regional expansion decisions

9. Technical Skills Demonstrated
•	Advanced Pandas operations
•	Multi-column normalization
•	Feature engineering
•	Time-series grouping
•	High-cardinality categorical analysis
•	Data transformation pipeline design
•	Business-aligned EDA
•	Analytical storytelling

10. Scalability & Future Extensions
Potential production-level extensions:
•	Build dimensional star schema for BI modeling
•	Deploy interactive dashboard using Power BI or Tableau
•	Perform clustering on genre-country combinations
•	Apply NLP sentiment analysis on descriptions
•	Develop predictive model for content onboarding trends
•	Deploy Streamlit app for live exploration
BY : Ashok Modh.
