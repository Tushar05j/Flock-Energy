# REFIT Energy Analysis and Washing-Machine Disaggregation

## Project Overview

This project uses the REFIT household electricity-consumption dataset to
explore energy-use patterns and develop a baseline
appliance-disaggregation model for washing-machine power consumption.

The workflow includes data cleaning, exploratory data analysis (EDA),
feature engineering, chronological model evaluation, and comparison of
baseline approaches with a Random Forest regressor.

## Objectives

-   Clean and validate household power time-series data.
-   Examine washing-machine usage patterns across selected households.
-   Analyze usage by hour, day of week, and cycle-level characteristics.
-   Estimate washing-machine power consumption from aggregate household
    power readings.
-   Compare model performance using overall and active-period error
    metrics.

## Dataset and Household Selection

The analysis uses the REFIT dataset. Six households were selected for
the washing-machine analysis: **Houses 1, 2, 3, 5, 6, and 9**.

The final cleaned dataset used in the reported analysis contains
**920,031 rows and 11 columns**, covering **9 October 2013 to 10 July
2015**.

The raw dataset is not included in this repository. To reproduce the analysis, obtain the REFIT dataset from its official source and place the required files in the appropriate data directory, as described in the project instructions.

## Methodology

### 1. Data Cleaning and Preprocessing

The preprocessing workflow includes:

-   Sorting readings chronologically.
-   Removing duplicate timestamps.
-   Interpolating short missing gaps where appropriate.
-   Flagging extended aggregate-power outages rather than treating
    missing readings as zero consumption.
-   Checking timestamp order, missing-value counts, power statistics,
    and final dataset dimensions.

In the final reported dataset, aggregate power has **114,544 missing
readings (12.45%)**. Appliance-level missingness varies, with Appliance
9 having the highest reported missing-value percentage at **22.54%**.

### 2. Exploratory Data Analysis

The EDA examines:

-   Detected washing-machine events and energy across households.
-   Washing-machine usage by hour of day.
-   Washing-machine usage by day of week.
-   Hourly usage patterns across households.
-   Median cycle duration, peak power, and energy per cycle.
-   Aggregate and appliance power readings over time.

See [`RESULTS.md`](RESULTS.md) for the figures and detailed findings.

### 3. Appliance Disaggregation

The goal is to estimate washing-machine power from aggregate household
power and time-based features.

The model features include:

-   Current aggregate power.
-   Lagged aggregate power readings.
-   Rolling aggregate-power statistics.
-   Cyclical hour-of-day features.
-   Cyclical day-of-week features.

A separate Random Forest model is evaluated for each selected household.

### 4. Models Compared

Three approaches are compared:

-   **Always-off baseline:** Predicts zero washing-machine power.
-   **Usage-profile baseline:** Uses an average washing-machine power
    profile based on day of week and hour, calculated from training
    data.
-   **Random Forest:** Uses aggregate-power history and time-based
    features to estimate appliance power.

The reported Random Forest configuration uses 100 trees, a maximum depth
of 15, a minimum leaf size of 5, `max_features=0.8`, and
`random_state=42`.

### 5. Evaluation

Data is split chronologically into **70% training, 15% validation, and
15% testing** for each household. This preserves time order and avoids
randomly mixing future observations into earlier parts of the dataset.

The reported metrics are:

-   **MAE (W):** Mean absolute error in predicted power.
-   **RMSE (W):** Root mean squared error, which penalizes larger errors
    more strongly.
-   **R²:** Coefficient of determination.
-   **Active MAE (W):** Mean absolute error during
    washing-machine-active minutes.

## Key Results

On the combined test set, the reported results are:

  Model             MAE (W)   RMSE (W)       R²   Active MAE (W)
  --------------- --------- ---------- -------- ----------------
  Always-off         19.704    169.455   -0.014          404.114
  Usage-profile      33.361    166.790    0.018          371.022
  Random Forest      25.160    141.299    0.295          297.896

The Random Forest model achieved the lowest RMSE and active-period MAE
among the three approaches. The Always-off baseline had the lowest
overall MAE, reflecting the large proportion of time when the washing
machine was inactive. Performance varied across households, and
high-power washing-machine peaks remained difficult to estimate
accurately.

For the full household-level results, plots, and failure analysis, see
[`RESULTS.md`](RESULTS.md).

## Repository Contents

The exact file layout may vary depending on how the project is
organized. The main deliverables are expected to include:

-   `README.md` --- project overview, methodology, and key results.
-   `RESULTS.md` --- data-cleaning results, EDA figures, model
    comparisons, prediction plots, and limitations.
-   `notebooks/` --- notebooks used for preprocessing, EDA, and
    modeling.
-   Data files or instructions for obtaining the REFIT dataset, subject
    to its terms and conditions.

Update this section if your repository uses different folder or file
names.

## How to Reproduce

1.  Clone or download this repository.
2.  Obtain the REFIT dataset from its official source and place it in
    the location expected by the notebooks.
3.  Open the notebooks in Jupyter Notebook or VS Code.
4.  Run the preprocessing notebook before running EDA and modeling
    notebooks.
5.  Run the analysis and modeling cells in order to regenerate the
    figures and evaluation metrics.

Install the Python libraries imported by your notebooks before running
them. A definitive dependency list should be maintained in
`requirements.txt` once the project's imports and environment have been
checked.

## Limitations

-   Missing readings and extended outages can affect analysis and model
    predictions.
-   Aggregate household power can contain overlapping signatures from
    multiple appliances.
-   Washing-machine usage differs across households.
-   The model can underestimate high-power events and generate false
    positives during low-power periods.
-   Results depend on the selected households, preprocessing choices,
    features, and evaluation period.

## Future Improvements

-   Improve washing-machine event detection and cycle segmentation.
-   Investigate features that better represent appliance operating
    states.
-   Evaluate additional models and compare them using the same
    chronological test periods.
-   Analyze uncertainty and performance across different households.
-   Add a verified dependency file and reproducible configuration for
    the complete pipeline.

## Acknowledgements

This project uses the REFIT household electricity dataset for research
and analysis. Please consult the dataset's official documentation and
terms for citation and permitted use.
