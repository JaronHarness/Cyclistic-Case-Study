# Cyclistic Bike-Share Company Case Study
## Introduction
### Project Overview
This project focuses on Cyclistic, a premier bike-share program based in Chicago. Boasting a fleet of over 5,800 geotracked, multi-type bicycles and a network of more than 690 docking stations, Cyclistic provides vital transportation flexibility across the city.

### Scenario
I am assuming to be a junior data analyst working in the marketing analyst team at Cyclistic, a bike-share company in Chicago. The director of marketing believes the company’s future success depends on maximizing the number of annual memberships. Therefore, my team wants to understand how casual riders and annual members use Cyclistic bikes differently. From these insights, my team will design a new marketing strategy to convert casual riders into annual members. But first, Cyclistic executives must approve our recommendations, so they must be backed up with compelling data insights and professional data visualizations.  

### Course
This Case Study comes from the Google Data Analytics Course and utilizes Google's 6 steps of the data analysis process: Ask, Prepare, Process, Analyze, Share, and Act. Below is a link to the case study in the course, the data sources, and the Jupyter Notebooks and Tableau dashboard used to do my analysis.

Course:  
[Google Data Analytics Capstone: Complete a Case Study](https://www.coursera.org/learn/google-data-analytics-capstone)  

Cyclistic Data Source:  
[divvy-tripdata](https://divvy-tripdata.s3.amazonaws.com/index.html)

Data Processing & Analysis (Jupyter Labs (Python) via nbviewer):  
[01. Data Ingestion](https://nbviewer.org/github/JaronHarness/Cyclistic-Case-Study/blob/main/01_data_ingestion.ipynb)  
[02. Data Exploration](https://nbviewer.org/github/JaronHarness/Cyclistic-Case-Study/blob/main/02_data_exploration.ipynb)  
[03. Data Cleaning](https://nbviewer.org/github/JaronHarness/Cyclistic-Case-Study/blob/main/03_data_cleaning.ipynb)  
[04. Data Analysis](https://nbviewer.org/github/JaronHarness/Cyclistic-Case-Study/blob/main/04_data_analysis.ipynb)

Interactive Visualization:  
[Tableau Dashboard](https://public.tableau.com/app/profile/jaron.harness/viz/CyclisticBike-ShareUserAnalysis/Dashboard1?publish=yes)

While the service caters to a broad demographic, the company’s financial sustainability heavily relies on its Annual Members, who prove to be significantly more profitable than casual users buying single-ride or full-day passes.
## Step 1: Ask
### Business Task
The core objective of this analysis is to evaluate a massive dataset of historical trip logs to isolate the behavioral differences between casual riders and annual members. By identifying exactly how, when, and why these two user cohorts utilize Cyclistic bikes, the marketing team can design highly targeted digital strategies to convert existing casual riders into long-term annual members.
### Key Questions to Answer
1. How do annual members and casual riders use Cyclistic bikes differently?  
2. Why would casual riders buy a Cyclistic annual membership?  
3. How can Cyclistic use digital media to influence casual riders to convert to members?
### Key Stakeholders
* Cyclistic Executive Leadership Team: The primary decision-makers who approve the budget and strategic direction for company-wide marketing campaigns.
* Lily Moreno: Director of Marketing: Responsible for developing and executing the digital media campaign aimed at user conversion based on these data-backed recommendations.
* Cyclistic Marketing Analytics Team: The data team responsible for collecting, analyzing, and reporting performance metrics to drive business decisions.
### Business Question
How do annual members and casual riders use Cyclistic bikes differently?

## Step 2: Prepare
### Data Source & Storage
The data used for this analysis is Cyclistic’s historical trip data, spanning the last 12 months (May 2025 to April 2026).  
* Source: The data has been made available by Motivate International Inc. [license](https://www.divvybikes.com/data-license-agreement).
* Storage: The raw data consists of 12 separate monthly CSV files. Due to the large volume of data (millions of rows), the files were downloaded locally and processed using Python inside Jupyter Labs for optimized memory management and reproducibility.

### Data Organization & Schema
Each monthly dataset contains structured columns capturing trip-level details. The schema consists of the following attributes:
* ride_id: Unique identifier for each individual trip (Primary Key)
* rideable_type: Type of bike used (Classic, Electric, or Docked)
* started_at: Timestamp indicating the start of the trip (YYYY-MM-DD hh:mm:ss)
* ended_at: Timestamp indicating the end of the trip (YYYY-MM-DD hh:mm:ss)
* start_station_name & start_station_id: Name and ID of the departure station
* end_station_name & end_station_id: Name and ID of the arrival station
* start_lat & start_lng: Latitude and longitude coordinates of the start location
* end_lat & end_lng: Latitude and longitude coordinates of the end location
* member_casual: Customer type (member for annual subscription, casual for single-ride or full-day passes)

## Step 3: Process

In the Process phase, I transitioned from raw, unorganized data to a clean, structured dataset optimized for analysis. Given the large size of the dataset (millions of rows across 12 months), I leveraged **Python (Pandas, glob, os)** within Jupyter Notebooks to programmatically handle data ingestion, initial exploration, and rigorous data cleaning.

The process was broken down into three distinct notebooks to maintain a modular and documented workflow.

### 1. Data Ingestion & Consolidation
**Notebook:** [`01_data_ingestion.ipynb`](https://github.com/JaronHarness/Cyclistic-Case-Study/blob/main/01_data_ingestion.ipynb)
* **Objective:** Combine 12 separate monthly CSV files into a unified dataset.
* **Actions Taken:**
  * Imported the standard data science stack (`pandas`, `glob`, `os`).
  * Programmatically read all 12 CSV files from the local directory using a loop.
  * Verified schema consistency (column names and data types) across all files before merging.
  * Concatenated the individual dataframes into a single master dataframe containing the full year of historical trip data.
  * Exported the raw consolidated data to the combined_raw_data CSV file to preserve the original state.

### 2. Initial Data Exploration
**Notebook:** [`02_data_exploration.ipynb`](https://github.com/JaronHarness/Cyclistic-Case-Study/blob/main/02_data_exploration.ipynb)
* **Objective:** Profile the data structure, look for anomalies, missing values, data type mismatches, and structural errors.
* **Key Discoveries:**
  * **Missing Values:** Identified substantial gaps in station names (`start_station_name`, `end_station_name`) and station IDs, while core trip data (times, coordinates, rider type) remained fully populated.
  * **Data Types:** `started_at` and `ended_at` were stored as string objects instead of datetime objects.
  * **Anomalies:** Discovered occurrences where `ended_at` was chronologically earlier than `started_at`, resulting in negative trip durations.
  * **Duplicates:** Checked for explicit duplicate rows using primary identifier keys (`ride_id`).

### 3. Data Cleaning & Transformation
**Notebook:** [`03_data_cleaning.ipynb`](https://github.com/JaronHarness/Cyclistic-Case-Study/blob/main/03_data_cleaning.ipynb)
* **Objective:** Isolate, clean, and transform the data based on exploration findings to ensure data integrity.
* **Cleaning Tasks Performed:**
  * **Data Type Conversion:** Converted `started_at` and `ended_at` columns from objects to `datetime64` format.
  * **Feature Engineering:** 
    * Created `ride_length` to calculate the exact duration of each trip in seconds/minutes (`ended_at - started_at`).
    * Extracted `day_of_week` (e.g., Sunday, Monday) from the start date to look for weekly patterns.
    * Extracted `month` and `hour` features to allow for seasonal and daily analysis.
  * **Filtering & Data Removal:**
    * Filtered out "test" or administrative rides by checking station names.
    * Excluded records where `ride_length` was negative or lasted less than 60 seconds (potentially false starts or docking errors).
    * Removed rows with critical missing spatial data where necessary, ensuring the remaining rows provided a reliable, accurate sample for geographic mapping.
  * **Consistency Check:** Verified that the `member_casual` column only contained the two expected distinct values (`member` and `casual`).

### Verification of Clean Data
Post-cleaning, the dataset was re-checked for structural integrity. The resulting clean, transformed dataset was exported to the cleaned_data CSV file, ready to be ingested into [`04_data_analysis.ipynb`](https://github.com/JaronHarness/Cyclistic-Case-Study/blob/main/04_data_analysis.ipynb) and imported into **Tableau** for dashboard creation.
