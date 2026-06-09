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
    
    <img width="515" height="132" alt="datetime conversions" src="https://github.com/user-attachments/assets/e893f971-7a35-46ee-acf9-77e6b01e72fb" />

  * **Feature Engineering:** 
    * Created `ride_length` to calculate the exact duration of each trip in minutes (`ended_at - started_at`).
      
      <img width="891" height="51" alt="ridelength column created" src="https://github.com/user-attachments/assets/63b4f7c6-d937-4392-84d1-a937e3fb31f9" />

    * Extracted `day_of_week` (e.g., Sunday, Monday) from the start date to look for weekly patterns.
      
      <img width="774" height="52" alt="datetime day extraction" src="https://github.com/user-attachments/assets/68fd2395-3094-4a8f-a5da-063db629a246" />

    * Extracted `month` and `hour` features to allow for seasonal and daily analysis.
      
      <img width="396" height="127" alt="hour month extraction" src="https://github.com/user-attachments/assets/fb6a682b-e433-401f-aa46-db2f4b4a0231" />

  * **Filtering & Data Removal:**
    * Filtered out "test" or administrative rides by checking station names.
      
      <img width="966" height="129" alt="removed test rides" src="https://github.com/user-attachments/assets/b0f9d704-765c-429e-ad46-cbf7b709bf2f" />

    * Excluded records where `ride_length` was negative or lasted less than 60 seconds (potentially false starts or docking errors).
      
      <img width="839" height="78" alt="removed outlier trips" src="https://github.com/user-attachments/assets/1256a600-fc89-48ff-b8d0-f2efed8b65e2" />

    * Removed rows with critical missing spatial data where necessary, ensuring the remaining rows provided a reliable, accurate sample for geographic mapping.
  * **Consistency Check:** Verified that the `member_casual` column only contained the two expected distinct values (`member` and `casual`).

### Verification of Clean Data
Post-cleaning, the dataset was re-checked for structural integrity. The resulting clean, transformed dataset was exported to the cleaned_data CSV file, ready to be ingested into [`04_data_analysis.ipynb`](https://github.com/JaronHarness/Cyclistic-Case-Study/blob/main/04_data_analysis.ipynb) and imported into **Tableau** for dashboard creation.

## Step 4: Analyze

With a clean, unified dataset prepared, I transitioned to the Analysis phase to uncover distinct behavioral patterns between casual riders and annual members. The analysis was conducted programmatically using Python to compute key descriptive statistics and aggregate metrics.

**Notebook:** [`04_data_analysis.ipynb`](https://github.com/JaronHarness/Cyclistic-Case-Study/blob/main/04_data_analysis.ipynb)

### Key Questions Addressed:
1. What is the total volume of rides for each user type, and how do they compare?
2. How does average trip duration (`ride_length`) differ between annual members and casual riders?
3. How do bike preferences and usage patterns fluctuate across days of the week, hours of the day, and seasons?

---

### Summary | Core Insights

#### 1. Ride Volume vs. Trip Duration
* **Members** account for the majority of total trips, indicating stable, recurring usage.
  
  <img width="479" height="253" alt="number of riders by type" src="https://github.com/user-attachments/assets/cdd463b6-27cb-4f72-b220-ca413d24e06d" />

* **Casual riders**, while taking fewer total trips, exhibit significantly longer average trip durations (`ride_length`) compared to members—often more than double the average ride time.
  
  <img width="947" height="302" alt="rideduration by rider type" src="https://github.com/user-attachments/assets/82342137-7dcb-4ca7-8ed4-bd4db0fdaa46" />

* *Insight:* Members use the service frequently for short, efficient trips, whereas casual riders use it for prolonged, leisure-oriented outings.

#### 2. Weekly & Hourly Usage Patterns (Commuters vs. Leisure)
* **Weekly Variation:** Member ridership remains consistently high Monday through Friday and dips slightly on weekends. Casual ridership spikes dramatically on Saturdays and Sundays.
  
 <img width="1021" height="403" alt="rides per day by rider type" src="https://github.com/user-attachments/assets/a8a8ee1a-1b97-4958-a51d-331560083a54" />

* **Hourly Variation:** On weekdays, member usage peaks sharply during traditional commuting windows (7:00 AM – 9:00 AM and 4:00 PM – 6:00 PM). Casual usage increases gradually throughout the day, peaking in the afternoon without sharp commuting spikes.
  
  <img width="685" height="53" alt="riders by hour 1 of 2" src="https://github.com/user-attachments/assets/bb2945da-af14-4b87-9773-8ec011d2e901" />  \
  <img width="237" height="560" alt="riders by hour 2 of 2" src="https://github.com/user-attachments/assets/aec7cd29-039a-4dbc-a7bb-53ef0bf653fc" />
  
* *Insight:* Annual members primarily use Cyclistic as a utility for daily commuting. Casual riders utilize it heavily for weekend recreation and midday leisure.

#### 3. Seasonal & Monthly Trends
* Both user types show heavy seasonal dependence, with ridership peaking during the summer months (June–August) and dropping significantly during the winter (December–February).
* Casual ridership is highly volatile and drops much more drastically in cold weather compared to the relatively resilient baseline of commuting members.
  
  <img width="708" height="50" alt="riders by month 1 of 2" src="https://github.com/user-attachments/assets/fe59982c-a005-4e66-abb7-09d416a8fee5" />  \
  <img width="241" height="322" alt="riders by month 2 of 2" src="https://github.com/user-attachments/assets/f5c17aa4-6d7d-48c0-aada-cd7dced17795" />

#### 4. Bike Type Preferences
* Both groups favor classic and electric bikes.
* Interestingly, **docked bikes** are used exclusively by casual riders, whereas members utilize classic and electric models almost entirely.

---

### Sample Aggregations Executed in Python
To support these conclusions, the following data aggregations were performed within the notebook:
* Computed the mean, max, and mode of `ride_length` globally and grouped by `member_casual`.
* Aggregated total ride counts and average durations pivoted by `day_of_week` and user type.
* Isolated peak usage hours by extracting and grouping timestamps to identify traffic spikes.

## Step 5: Share

For the Share phase, I translated the statistical insights discovered in Python into an interactive visual narrative using **Tableau**. The goal of this dashboard is to provide stakeholders with a clear, data-driven comparison of how annual members and casual riders utilize the Cyclistic network differently.

### Interactive Tableau Dashboard
The complete interactive visualization is published and available on Tableau Public. 
**[View the Live Cyclistic User Analysis Dashboard](https://public.tableau.com/app/profile/jaron.harness/viz/CyclisticBike-ShareUserAnalysis/Dashboard1)**

---

### Visual Storytelling & Key Dashboards

#### 1. The Big Picture: Volume vs. Duration
* **What it shows:** A side-by-side comparison of total trip counts versus the average duration of those trips, segmented by user type.
  
  <img width="573" height="165" alt="total rides visual" src="https://github.com/user-attachments/assets/638dad9e-2560-41c1-a9e3-7b9bcd48c756" />  \
  <img width="1106" height="222" alt="average ride length visual" src="https://github.com/user-attachments/assets/0f72015f-e298-4385-bce7-94c1c2478d3a" />

* **Key Takeaway:** While annual members dominate total trip volume, casual riders hold onto the bikes significantly longer per trip. This immediately visualizes the "commuter vs. leisure" user profile divide.

#### 2. Temporal Analysis: Weekly & Hourly Trends
* **What it shows:** Line charts mapping ride volume over the course of the week and throughout a 24-hour day.
  
  <img width="841" height="372" alt="hourly rush hours visual" src="https://github.com/user-attachments/assets/d76d9843-9292-4017-9404-2e846af2441c" />  \
  <img width="831" height="375" alt="weekly trends visual" src="https://github.com/user-attachments/assets/be002deb-d7c5-4678-9ce8-df4e03308334" />


* **Key Takeaway:** The visual charts clearly show dual spikes for members at 8:00 AM and 5:00 PM on weekdays, illustrating consistent business-hour commuting. Conversely, casual riders exhibit a smooth, bell-shaped curve that peaks on Saturday and Sunday afternoons.

#### 3. Seasonal Volatility
* **What it shows:** A month-over-month ridership tracker.
  
  <img width="1772" height="298" alt="seasonal trends visual" src="https://github.com/user-attachments/assets/cc02206e-2be6-45bb-9b0f-ec2dffaf0a99" />

* **Key Takeaway:** The visual timeline highlights that summer is the peak acquisition window for casual riders, making it the prime target for marketing campaigns.

## Step 6: Act

Based on the data-driven insights uncovered during this analysis, there is a clear distinction between how the two user groups interact with Cyclistic. **Annual Members use the service as a functional daily utility (commuting)**, while **Casual Riders use it as a recreational and seasonal luxury**.

To convert casual riders into annual members, Cyclistic should avoid generic messaging and instead target the specific pain points and behaviors of weekend and seasonal leisure users.

---

### Recommendations

#### 1. Introduce a Seasonal Membership Pass
* **The Data Justification:** Casual ridership spikes dramatically on Saturdays and Sundays, and peaks intensely between June and August. A full, upfront 1-year commitment might feel too restrictive or expensive for a user who only intends to ride for weekend leisure.
* **The Action:** Create a specialized, lower-tier annual membership specifically tailored for weekend-only access, or a "Summer Pass" valid from May through September. This lowers the barrier to entry, hooks the heavy weekend users, and creates a predictable recurring revenue stream from a segment that is currently highly volatile.

#### 2. Launch Targeted Digital Campaigns at High-Duration Leisure Spots
* **The Data Justification:** Casual riders exhibit an average trip duration more than double that of members, and are the exclusive users of "docked bikes," indicating prolonged recreational trips.
* **The Action:** Deploy geo-targeted digital marketing (social media ads, push notifications) and physical QR-code ad placements directly at top docking stations near parks, waterfronts, and tourist hotspots during peak weekend hours (12:00 PM – 4:00 PM). The campaign should focus on the financial savings of switching from single-trip/day-pass rates to a membership for long-duration rides.

#### 3. Gamify Commuting & Leverage Weekday Off-Peak Incentives
* **The Data Justification:** Casual ridership is highly active during the middle of the day on weekdays when member commuting dips. 
* **The Action:** Introduce a "Commuter Challenge" trial for casual users who frequently ride during peak hours, showing them how much time and money they would save by using Cyclistic for their daily work commute. Additionally, offer current casual riders a dynamic weekday discount or a "midday member trial" to incentivize them to use the bikes for remote-work lunch breaks, shifting their behavior toward routine, daily utilization.

---

### Conclusion & Next Steps
By shifting the marketing focus from a broad "buy a membership" message to a highly targeted strategy emphasizing **weekend value, long-ride savings, and seasonal flexibility**, Cyclistic can successfully capture the high-value casual segments that already love the service but need a more tailored incentive to lock into an annual plan.
