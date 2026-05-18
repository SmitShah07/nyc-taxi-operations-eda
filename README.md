# Optimising NYC Taxi Operations: EDA on 2023 Yellow Taxi Trips

This project performs an end-to-end Exploratory Data Analysis (EDA) on NYC Yellow Taxi trip data for 2023.  
The goal of this analysis is to understand taxi demand patterns, revenue behaviour, fare trends, passenger behaviour, pickup/dropoff zone activity, and operational inefficiencies that can help improve taxi dispatching, cab positioning, and pricing strategy.

---

## Project Objective

New York City taxi operations depend heavily on time, location, demand, trip distance, fare structure, and customer behaviour.  
In this project, I analysed 2023 yellow taxi trip data to identify useful patterns that can help taxi companies make better decisions around:

- Routing and dispatching
- Cab positioning across zones
- Peak-hour supply planning
- Revenue and pricing strategy
- Passenger and tip behaviour
- Extra charge and surcharge patterns

---

## Dataset

The analysis uses NYC Yellow Taxi trip records for the year 2023.

The dataset contains monthly parquet files with information such as:

- Vendor ID
- Pickup and dropoff datetime
- Passenger count
- Trip distance
- Pickup and dropoff location IDs
- Payment type
- Fare amount
- Tip amount
- Tolls and surcharges
- Total amount

A taxi zone shapefile was also used to connect location IDs with zone names and borough-level geographical information.

> Note: The raw dataset is not included in this repository due to size.  
> The notebook expects the 2023 monthly Yellow Taxi parquet files and taxi zone shapefile to be available locally.

---

## Tools and Libraries Used

- Python
- NumPy
- Pandas
- Matplotlib
- Seaborn
- GeoPandas
- Plotly

---

## Project Workflow

### 1. Data Preparation

The twelve monthly 2023 yellow taxi parquet files were sampled using `frac = 0.007` and combined into one dataframe.

Sampling was done from each monthly file first, and then the sampled parts were concatenated. This helped keep the dataset size manageable while still covering all months of 2023.

For count-based results, sampled values can be scaled using the sampling fraction to estimate approximate full-dataset values.

---

### 2. Data Cleaning

The cleaning process included:

- Resetting the dataframe index after combining monthly files
- Combining duplicate `airport_fee` columns caused by naming inconsistency
- Dropping columns not required for the main analysis
- Handling missing values in important columns
- Clipping invalid negative monetary values
- Checking valid value ranges for code columns such as `VendorID`, `RatecodeID`, and `payment_type`
- Identifying outliers using boxplots and IQR
- Removing only clearly invalid or unrealistic records
- Creating `trip_duration` from pickup and dropoff timestamps

Boxplots and the IQR method were used to identify possible outliers, but not all IQR-based outliers were removed directly because some high values can still represent valid taxi trips.

---

### 3. Exploratory Data Analysis

The EDA was divided into two parts:

---

## General EDA

### Pickup Demand Trends

Taxi pickup demand was analysed by:

- Hour of the day
- Day of the week
- Month of the year

This helped identify when taxi demand was higher and how demand changed across the year.

### Revenue Trends

Monthly and quarterly revenue trends were analysed using `total_amount`.  
This showed how revenue was distributed across different months and quarters.

### Distance and Fare Relationship

The relationship between `trip_distance` and `fare_amount` was analysed using valid positive records.  
The analysis showed that fare amount generally increases as trip distance increases.

### Correlation Analysis

Pairwise correlations were calculated and visualised using heatmaps for:

- `fare_amount` and `trip_duration`
- `fare_amount` and `passenger_count`
- `tip_amount` and `trip_distance`

This helped understand how fare, trip duration, passenger count, tips, and distance were related.

### Payment Type Distribution

Payment type distribution was analysed after removing invalid payment codes.  
Credit card was the major payment method, followed by cash, while other payment types had very small proportions.

### Taxi Zone Analysis

Taxi zone shapefile data was loaded and merged with trip records to analyse zone-level trip activity.  
A choropleth map was used to show how trip demand varies across NYC taxi zones.

---

## Detailed EDA

### Slow Route Analysis

Slow routes were identified by calculating trip duration and average speed for pickup-dropoff route pairs.  
Routes with low average speed were treated as inefficient because drivers may complete fewer trips on those routes.

### Hourly Demand

Hourly trip counts were calculated to identify busy pickup periods.  
The highest pickup demand was observed around 18:00.

### Weekday vs Weekend Traffic

Hourly traffic patterns were compared between weekdays and weekends to understand how demand changes across different types of days.

### Top Pickup and Dropoff Zones

The analysis identified zones with high pickup and dropoff activity.  
This helped understand where taxi demand is most active.

### Pickup/Dropoff Ratio

Pickup/dropoff ratios were calculated for each zone to identify pickup-heavy and dropoff-heavy locations.  
This can help in cab repositioning decisions.

### Night-Time Demand

Night hours were defined between 11 PM and 5 AM.  
East Village showed high night pickup and dropoff traffic, making it an important late-night demand zone.

### Revenue Share: Day vs Night

Revenue share was compared between daytime and nighttime trips.  
Daytime trips contributed the majority of revenue.

### Fare Per Mile Analysis

Fare per mile was analysed by:

- Passenger count
- Pickup hour
- Day of the week
- Vendor
- Distance tier

This helped understand how fare efficiency changes across different trip conditions.

### Tip Percentage Analysis

Tip percentages were analysed based on:

- Trip distance
- Passenger count
- Pickup time

This helped identify factors associated with lower or higher tip percentages.

### Passenger Count Trends

Passenger count was analysed by pickup hour, pickup day, and pickup zone to understand how passenger behaviour changes by time and location.

### Extra Charges and Surcharge Analysis

Surcharge and extra charge columns were analysed to understand how often different charges are applied.  
Zone and time-based extra charge patterns were also checked.

---

## Key Findings

Some of the main findings from the analysis were:

- Taxi demand changes clearly by pickup hour, day, and month.
- Pickup demand is not evenly distributed across all zones.
- Some zones act more like pickup-heavy zones, while others are more dropoff-heavy.
- Night-time demand is strong in selected zones such as East Village.
- Fare amount generally increases with trip distance.
- Passenger count has weak relation with fare amount.
- Daytime trips contribute the majority of revenue.
- Fare per mile changes by distance tier, pickup hour, day, and vendor.
- Tip percentage varies by distance, passenger count, and pickup time.
- Extra charges are more frequent in selected time and zone patterns.

---

## Final Recommendations

### 1. Routing and Dispatching

Taxi dispatching should be planned based on pickup hour, high-demand zones, night traffic zones, and slow route patterns.

More taxis should be available around high-demand pickup periods instead of reacting after demand has already increased.  
Slow routes should also be monitored because they reduce the number of trips a driver can complete.

### 2. Cab Positioning

Taxi positioning should not be random.  
Cabs should be placed near zones that show high pickup demand, especially around peak hours.

Airport zones, Midtown areas, and other high-demand pickup zones should have enough taxi supply.  
For late-night hours, zones with strong night traffic should receive more attention.

### 3. Pricing Strategy

Pricing strategy should not be changed in one common way for all trips.  
Fare patterns change by distance, pickup hour, day, passenger count, and vendor.

Short, medium, and long-distance trips should be analysed separately before making pricing changes.  
Customer experience should also be considered, especially where tip percentages are already low.

---

## Repository Structure

```text
nyc-taxi-operations-eda/
│
├── notebooks/
│   └── EDA_Optimising_NYC_Taxis_Smit_Shah.ipynb
│
├── reports/
│   └── EDA_Optimising_NYC_Taxis_Smit_Shah.pdf
│
├── README.md
│
└── .gitignore
