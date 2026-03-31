# Formula 1 Analysis (1950–2020)

## Dataset
The dataset is sourced from Kaggle: [Formula 1 World Championship 1950–2020](https://www.kaggle.com/datasets/rohanrao/formula-1-world-championship-1950-2020)

# Formula 1 Analytics: Driver Performance, Team Trends, Clustering and Forecasting

## Overview

This project analyzes historical Formula 1 race data to study driver performance, constructor performance, race-level outcomes, and long-term team trends.

The project combines data integration, exploratory data analysis, clustering, predictive modeling, and time-series forecasting to generate insights from Formula 1 results data.

The main goals are to:
- identify the top-performing drivers and teams
- segment drivers into performance tiers
- predict race points using historical features
- forecast team performance trends over time

---

## Business Problem

Although Formula 1 is primarily a sport, it is also a highly data-driven competitive environment. Teams, analysts, and fans are interested in questions such as:

- Which drivers have performed best historically?
- Which constructors have been most successful over time?
- Can drivers be grouped into meaningful performance categories?
- How strongly does race position influence points scored?
- Can team-level performance trends be forecasted from historical data?

This project addresses these questions through structured analytics and machine learning.

---

## Dataset

This project uses the Formula 1 World Championship dataset from Kaggle.

Source:
`rohanrao/formula-1-world-championship-1950-2020`

Files used:
- `drivers.csv`
- `constructors.csv`
- `races.csv`
- `results.csv`
- `circuits.csv`

The dataset includes historical information on:
- drivers
- constructors
- races
- race results
- circuits
- standings and lap-level details

---

## Dataset Structure

### Main Tables Used

#### drivers.csv
Contains driver metadata such as:
- driverId
- forename
- surname
- nationality
- date of birth

#### constructors.csv
Contains constructor or team information such as:
- constructorId
- constructorRef
- name
- nationality

#### races.csv
Contains race-level metadata such as:
- raceId
- year
- round
- circuitId

#### results.csv
Contains race results such as:
- driverId
- constructorId
- grid position
- finishing position
- points
- laps
- time
- fastest lap
- status

#### circuits.csv
Contains circuit metadata such as:
- circuitId
- circuit name
- location

---

## Data Preparation

The project integrates multiple tables using shared keys:

- `driverId` to attach driver details
- `constructorId` to attach team details
- `raceId` to attach season and race information
- `circuitId` to attach circuit details

### Key Cleaning Steps

- Filled missing `points` values with 0 where needed
- Converted `position` values from text to numeric
- Removed rows with invalid or missing finishing positions
- Converted `year` to integer
- Removed duplicates
- Built a corrected merged table containing:
  - `year`
  - `team`
  - `driver_name`
  - `points`

---

## Exploratory Data Analysis

The project includes several exploratory analyses to understand historical Formula 1 performance.

### Analyses Performed

- Top 10 drivers by total points
- Top 10 teams by total points
- Distribution of race points
- Yearly team points aggregation

### Key Insights

- A small number of elite drivers account for a large share of total points
- Certain constructors dominate across long historical periods
- Points distribution is highly uneven because many drivers score low or zero points in individual races
- Team-level performance varies significantly across eras

---

## Driver Segmentation

Driver segmentation was performed to identify performance tiers.

### Features Used

Driver-level aggregates were created from race results:

- average points
- total points
- number of races

### Method

- Features were standardized using `StandardScaler`
- KMeans clustering was applied with `n_clusters = 4`

### Output

Drivers were grouped into 4 performance clusters representing broad tiers such as:

- elite high-performing drivers
- strong consistent scorers
- mid-tier drivers
- low-scoring or infrequent participants

### Visualization

A scatter plot was used to visualize:
- number of races
- average points
- cluster membership

This helps distinguish long-career elite drivers from occasional or lower-performing drivers.

---

## Predictive Modeling

The project uses a Random Forest Regressor to estimate race points.

### Target Variable

- `points`

### Features Used

- `position`
- `year`

### Model

- Random Forest Regressor
- `n_estimators = 100`
- `random_state = 42`

### Result

- Mean Squared Error (MSE): `0.2127`

### Interpretation

The low MSE suggests that finishing position is highly informative for predicting awarded points, which is expected because Formula 1 points systems are directly tied to finishing order.

However, this also means the model is more descriptive than deeply predictive, since `position` is itself very close to the final outcome being modeled.

---

## Time-Series Forecasting

The project also forecasts team performance trends over time using Prophet.

### Forecasting Setup

- Team-level yearly points were aggregated
- Top-performing teams were selected
- Prophet was used to forecast the next 5 years of points for each team

### Teams Forecasted

Top historical teams included:
- Ferrari
- Mercedes
- Red Bull
- McLaren
- Williams

### Forecast Horizon

- 5 future yearly periods

### Notes

- Prophet was configured with yearly seasonality
- Forecasts were generated separately for each top team

---

## Key Results

### Performance Analysis
- Top drivers and top teams were identified by total points
- Driver careers were segmented into 4 performance clusters

### Machine Learning
- Random Forest regression achieved:
  - MSE = `0.2127`

### Forecasting
- Future performance trends were projected for top constructors
- Team-level historical points were modeled as yearly time series

---

## Business and Analytical Impact

This project demonstrates how sports data can be transformed into meaningful analytical insights.

It can support:

- historical performance benchmarking
- team comparison across eras
- driver performance segmentation
- race outcome modeling
- long-term trend forecasting

For a sports organization, media platform, or analytics portfolio, this project shows the ability to move from raw relational datasets to interpretable analytical outputs.

---

## Recommendations

Based on the current project, the following improvements would make the analysis stronger:

### 1. Add Better Predictive Features
The regression model currently uses only:
- position
- year

To improve modeling quality, add:
- grid position
- constructor/team
- circuit
- fastest lap rank
- laps completed
- driver historical average points

### 2. Use Time-Based Validation
Because the dataset is sequential over time, model evaluation should use:
- chronological train/test splits
instead of random splitting when forecasting future outcomes.

### 3. Forecast Modern Era Separately
The dataset spans many decades, but Formula 1 changed significantly across eras:
- points systems
- number of races
- regulations
- team competitiveness

Consider forecasting only modern seasons to improve realism.

### 4. Add More Driver-Level Features
Possible additions:
- podium counts
- win counts
- average finishing position
- DNF rate
- constructor changes across career

### 5. Build an Interactive Dashboard
A Power BI, Tableau, or Streamlit dashboard would make this project even stronger for recruiters.

---

## Technologies Used

- Python
- pandas
- numpy
- matplotlib
- seaborn
- scikit-learn
- Prophet

---

## Project Workflow

1. Download Formula 1 data from Kaggle
2. Load multiple CSV files
3. Merge driver, constructor, race, result, and circuit data
4. Clean and standardize important fields
5. Perform exploratory analysis
6. Aggregate driver-level statistics
7. Cluster drivers into performance tiers
8. Train regression model to estimate race points
9. Aggregate yearly team points
10. Forecast future team trends with Prophet

---

## Limitations

- The initial merge had column-name collisions and required correction
- The regression model is heavily influenced by finishing position, which is closely tied to the target points
- Prophet forecasts on long historical sports data may not capture regulation changes or era shifts
- No external features such as weather, pit stops, or qualifying performance were included
- The Kaggle dataset version in the notebook references 1950–2020, while some outputs appear to include newer seasons; this should be clarified in the final repo

---

## Future Improvements

- Add classification models for podium prediction or points finish prediction
- Build driver and constructor dashboards
- Use more advanced time-based sports forecasting methods
- Compare model performance across eras
- Include qualifying and lap time analysis
- Add circuit-specific performance modeling

---

## Conclusion

This project shows how historical Formula 1 data can be used for performance analysis, clustering, predictive modeling, and forecasting.

By combining data cleaning, table joins, exploratory analysis, machine learning, and time-series methods, the project provides a complete sports analytics workflow suitable for a portfolio in data science, analytics, or machine learning.

---

## Requirements

Install dependencies with:

```bash
pip install -r requirements.txt
