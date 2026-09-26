# EDA-NYC-Taxi-Rides-Dataset / NYC Taxi Operations Analysis – 2023

## Project Overview

This project performs an Exploratory Data Analysis (EDA) of NYC Taxi trip records for 2023 to identify travel patterns, passenger behaviour, revenue trends, geographic demand, vendor pricing patterns, and operational inefficiencies.

The analysis focuses on using historical taxi trip data to generate insights that can support:

- Taxi routing and dispatch optimisation
- Cab positioning across NYC zones
- Demand-based operational planning
- Revenue analysis
- Pricing analysis
- Passenger and trip behaviour analysis
- Surcharge and payment analysis

---

## Objectives

The main objectives of this project are:

1. Prepare and sample the NYC Taxi 2023 dataset.
2. Clean missing, invalid and negative values.
3. Identify and handle data-quality issues and outliers.
4. Analyse taxi pickup patterns by:
   - Hour
   - Day of week
   - Month
5. Analyse revenue trends.
6. Analyse the relationship between distance and fare.
7. Analyse fare, tips, passengers and trip duration.
8. Analyse payment-type distribution.
9. Perform geographic analysis using NYC Taxi Zone shapefiles.
10. Identify high-demand pickup and drop-off zones.
11. Identify slow routes using average speed.
12. Compare weekday and weekend demand.
13. Analyse nighttime taxi demand.
14. Analyse fare per mile and fare per passenger.
15. Compare Vendor 1 and Vendor 2.
16. Analyse tip percentages.
17. Analyse passenger-count trends.
18. Analyse surcharge and extra-charge prevalence.
19. Develop operational and pricing recommendations.

---

## Dataset

The project uses NYC Taxi 2023 trip-record data along with the NYC Taxi Zone shapefile.

### Main Trip Data

The trip dataset contains fields related to:

- Vendor information
- Pickup and drop-off timestamps
- Passenger count
- Trip distance
- Rate code
- Pickup and drop-off locations
- Payment type
- Fare
- Tips
- Tolls
- Taxes
- Surcharges
- Total trip amount

### Taxi Zone Data

The taxi zone shapefile is used to map:

- Location ID
- Zone
- Borough
- Geographic boundaries

The trip data is joined with the taxi zone data using:

`PULocationID → LocationID`

---

## Data Sampling

The original monthly NYC Taxi files were sampled at approximately **5% within each pickup date and pickup hour group**.

The sampled monthly files were then combined.

The combined sampled dataset contained approximately:

**1.9 million records**

To meet the project requirement of keeping the working dataset around **250,000–300,000 records**, a reproducible sample of approximately **300,000 records** was retained for further analysis.

A fixed random seed was used to make the sampling reproducible.

---

## Data Cleaning

The following data-cleaning activities were performed:

### Missing Values

Missing values were analysed for each column.

The following fields received specific treatment:

- `passenger_count`
- `RatecodeID`
- `congestion_surcharge`

### Monetary Values

Negative monetary values were identified and removed where appropriate.

The monetary fields analysed include:

- `fare_amount`
- `extra`
- `mta_tax`
- `tip_amount`
- `tolls_amount`
- `improvement_surcharge`
- `total_amount`
- `congestion_surcharge`
- `airport_fee`

### Outlier Handling

The analysis included checks for:

- Very large trip distances
- Invalid payment types
- Unusually high fare values
- Very low/zero trip distances
- Passenger-count anomalies
- Unusual trip durations
- Other data-quality issues specified in the assignment

Trips with distances above 250 miles were treated as outliers according to the assignment requirement.

---

# Exploratory Data Analysis

## 1. Pickup Demand Analysis

Taxi pickup demand was analysed by:

- Hour of day
- Day of week
- Month
- Weekday vs weekend
- Nighttime hours

These analyses help identify recurring demand patterns and support operational planning.

### Hourly Pickup Trends

The analysis shows that taxi demand is relatively low during the early morning hours and increases substantially during the daytime, reaching its highest levels during the late afternoon/evening period.

---

### Daily Pickup Trends

Pickup activity varies across the days of the week.

The weekday/weekend comparison was also performed to identify differences in hourly demand patterns.

---

### Monthly Pickup Trends

Monthly pickup volumes were compared across all twelve months of 2023 to identify seasonal variations in taxi demand.

---

## 2. Revenue Analysis

Monthly revenue was calculated using:

`total_amount`

grouped by pickup month.

Quarterly revenue proportions were also calculated to understand the contribution of each quarter to annual observed revenue.

Because the working dataset is sampled, revenue values represent the analysed sample unless explicitly scaled.

---

## 3. Distance and Fare Analysis

The relationship between:

`trip_distance`

and

`fare_amount`

was analysed using scatter plots and correlation analysis.

The analysis helps identify the relationship between journey distance and fare while also highlighting unusual observations.

---

## 4. Trip Duration Analysis

Trip duration was derived from:

`tpep_dropoff_datetime - tpep_pickup_datetime`

Trip duration was analysed against fare amount to understand how journey duration relates to revenue.

Extremely long durations were also reviewed as potential data-quality observations.

---

## 5. Payment Type Analysis

Payment types were analysed using the following mapping:

| Payment Type | Description |
|---|---|
| 1 | Credit card |
| 2 | Cash |
| 3 | No charge |
| 4 | Dispute |
| 5 | Unknown |
| 6 | Voided trip |

Credit-card transactions represent the largest payment category in the analysed sample.

---

## 6. Geographic Analysis

The NYC Taxi Zone shapefile was loaded using GeoPandas.

Trip records were joined with taxi zones using:

`PULocationID = LocationID`

The analysis includes:

- Number of trips by location
- Top pickup zones
- Top drop-off zones
- Pickup/drop-off ratios
- Geographic demand distribution
- Average passenger count by pickup zone

A choropleth map was created to visualise pickup demand across NYC taxi zones.

---

## 7. Route Analysis

Average route speed was calculated using:

`Average Speed = Average Trip Distance / Average Trip Duration`

Routes with sufficient trip volume were analysed to identify relatively slow routes.

These routes can indicate:

- Congestion
- Operational inefficiencies
- Potential routing opportunities
- Areas requiring improved dispatch planning

---

## 8. Weekday vs Weekend Analysis

Hourly pickup patterns were compared between:

- Weekdays
- Weekends

The analysis shows that the shape of demand varies significantly between weekday and weekend periods.

This supports using different dispatch strategies for different day types.

---

## 9. Nighttime Analysis

Nighttime demand was defined as:

**23:00 to 04:59**

Pickup and drop-off activity during these hours was analysed separately.

Nighttime revenue was also compared with daytime revenue.

This provides a basis for planning late-night cab positioning.

---

## 10. Fare per Mile Analysis

Fare per mile was calculated using:

`fare_amount / trip_distance`

Fare per mile was analysed by:

- Hour
- Day of week
- Passenger count
- Vendor
- Distance tier

For passenger-level analysis:

`Fare per Mile per Passenger = fare_amount / trip_distance / passenger_count`

---

## 11. Vendor Analysis

The dataset contains two vendor IDs used in this analysis:

- **Vendor 1 – VendorID 1**
- **Vendor 2 – VendorID 2**

The analysis compares the vendors based on observed fare per mile.

Vendor pricing was also compared across distance tiers:

- Up to 2 miles
- 2–5 miles
- More than 5 miles

The comparison is performed on a like-for-like basis without assigning an overall winner.

---

## 12. Tip Analysis

Tip percentage was calculated as:

`Tip Percentage = tip_amount / fare_amount × 100`

The analysis examined tip behaviour by:

- Pickup hour
- Distance
- Passenger count

Zero-tip trips were retained because a zero tip can represent a legitimate transaction.

---

## 13. Passenger Analysis

Passenger-count trends were analysed by:

- Hour of day
- Day of week
- Pickup zone

The geographic analysis was used to identify areas with relatively higher average passenger counts.

---

## 14. Surcharge and Extra Charge Analysis

The following charges were analysed:

- `extra`
- `mta_tax`
- `tolls_amount`
- `improvement_surcharge`
- `congestion_surcharge`
- `airport_fee`

The percentage of trips where each charge was greater than zero was calculated.

Additional analysis can be used to identify the hours and zones where charges occur more frequently.

---

# Key Insights

The analysis indicates several important operational patterns:

### Demand

- Taxi demand is substantially lower during the early morning hours.
- Demand increases through the daytime.
- Higher pickup volumes occur during afternoon/evening periods.
- Weekday and weekend demand patterns differ.

### Geographic Demand

- Pickup demand is concentrated in specific NYC taxi zones.
- Zone-level trip counts can be used for demand-based vehicle positioning.
- Pickup/drop-off imbalance can indicate areas requiring vehicle repositioning.

### Pricing

- Fare per mile varies considerably by hour.
- Fare per mile differs between Vendor 1 and Vendor 2.
- Distance-tier analysis provides a more meaningful comparison of vendor pricing.

### Passenger Behaviour

- Average passenger count varies by pickup hour.
- Passenger occupancy also varies across geographic zones.

### Operations

- Slow routes can indicate congestion or routing inefficiencies.
- Historical demand patterns can be used to improve dispatch and repositioning.

---

# Recommendations

## Routing and Dispatch

1. Allocate available cabs based on hourly and zone-level demand.
2. Identify recurring slow routes and review alternative routing options.
3. Use separate weekday and weekend dispatch strategies.
4. Use pickup/drop-off ratios to identify vehicle imbalance.
5. Use nighttime demand patterns for late-night positioning.
6. Monitor trip duration and route speed to improve vehicle utilisation.

## Cab Positioning

1. Position more cabs near high-demand pickup zones before peak periods.
2. Use hourly zone trends for proactive repositioning.
3. Use pickup/drop-off ratios to identify areas requiring additional vehicles.
4. Apply different positioning strategies for weekdays, weekends and nighttime.
5. Reassess zone demand periodically to capture seasonal changes.

## Pricing

1. Use observed fare-per-mile values as pricing benchmarks.
2. Analyse pricing by distance tier rather than comparing all trips together.
3. Compare Vendor 1 and Vendor 2 using the same distance categories.
4. Consider demand, trip duration and congestion when evaluating pricing.
5. Monitor the impact of pricing changes on trip volume and revenue.
6. Maintain transparent customer-facing pricing.

---

# Technologies Used

- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn
- GeoPandas
- Jupyter Notebook
- Parquet
- Git / GitHub

---

# Project Structure

```text
NYC-Taxi-Operations/
│
├── README.md
│
├── NYC_Taxi_Operations_EDA.ipynb
│
├── final_nyc_taxi_2023_file.parquet
│
├── sampled_nyc_taxi_2023_file.parquet
│
├── plots/
│   ├── hourly_pickups.png
│   ├── daily_pickups.png
│   ├── monthly_pickups.png
│   ├── monthly_revenue.png
│   ├── payment_types.png
│   ├── fare_per_mile.png
│   ├── vendor_analysis.png
│   ├── passenger_analysis.png
│   └── ...
│
├── NYC_Taxi_Operations_Report.docx
├── NYC_Taxi_Operations_Report.pdf
│
└── requirements.txt


How to Run the Project
1. Clone the repository
git clone <YOUR-GITHUB-REPOSITORY-URL>
2. Navigate to the project
cd NYC-Taxi-Operations
3. Install dependencies
pip install pandas numpy matplotlib seaborn geopandas pyarrow jupyter
4. Start Jupyter Notebook
jupyter notebook

Open the project notebook and execute the cells sequentially.

Outputs

The project produces:

Cleaned/sample NYC Taxi dataset
Exploratory data analysis tables
Demand analysis
Revenue analysis
Geographic maps
Vendor comparisons
Passenger analysis
Tip analysis
Surcharge analysis
Operational recommendations
Final project report
Conclusion

This project demonstrates how NYC Taxi trip data can be analysed to understand demand, revenue, passenger behaviour, geographic patterns, pricing differences and operational inefficiencies.

The insights from the analysis can support data-driven decisions related to taxi dispatching, cab positioning, routing and pricing.

Author

Prabhakar Gedela
NYC Taxi Operations Analysis – 2023
