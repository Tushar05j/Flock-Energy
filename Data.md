# Data Documentation

## 1. Dataset Overview

This project uses the REFIT Electrical Load Measurements dataset, which contains household electricity consumption readings and individual appliance measurements.

**Dataset source:** [REFIT Electrical Load Measurements](https://pureportal.strath.ac.uk/en/datasets/refit-electrical-load-measurements-cleaned)

The project uses the official cleaned REFIT files for washing-machine disaggregation. Twenty official cleaned household files were identified in the dataset directory. Six households were selected for the modeling stage: Houses 1, 2, 3, 5, 6, and 9.

## 2. Household Selection Rationale
Six households (Houses 1, 2, 3, 5, 6, and 9) were selected from the official cleaned REFIT dataset after checking the availability of the washing-machine appliance channel (Appliance3). The selection process involved identifying households with this channel, preparing their readings at one-minute resolution, and examining their data coverage, missing intervals, and zero/off readings. These six households were chosen to enable comparison of washing-machine usage patterns across homes with different levels of appliance activity and detected operating events. Using multiple households also allowed us to compare cycle duration, peak power, energy consumption, and usage patterns by hour of day and day of week. The analysis was restricted to these six households to provide a manageable cross-household comparison while meeting the assignment requirement of analysing at least four households.

## 3. Data Coverage

The selected households cover different periods between September 2013 and July 2015.

| Household | Start date | End date |
|---|---|---|
| House 1 | 2013-10-09 | 2015-07-10 |
| House 2 | 2013-09-17 | 2015-05-28 |
| House 3 | 2013-09-25 | 2015-06-02 |
| House 5 | 2013-09-26 | 2015-07-06 |
| House 6 | 2013-11-28 | 2015-06-28 |
| House 9 | 2013-12-17 | 2015-07-08 |

The data were resampled to one-minute intervals for the analysis and modeling stages.

## 4. Data Cleaning and Preprocessing

The preprocessing pipeline included the following steps:

- **Timestamp sorting:** Readings were sorted chronologically before further processing.
- **Duplicate handling:** Duplicate timestamps were consolidated using the documented rule in the cleaning notebook.
- **One-minute resampling:** Readings were aggregated into one-minute intervals using mean power.
- **Short missing gaps:** Short gaps were handled according to the cleaning pipeline's documented rule.
- **Long outages:** Longer outages were flagged rather than automatically filled without justification.
- **Missing-value handling:** Missing aggregate and appliance readings were inspected. Missing targets and required features were excluded from model-ready data rather than automatically treated as zero.

The cleaning notebook also compares aggregate household power and appliance-level channels before and after cleaning to inspect the effects of preprocessing.

## 5. Data Quality Findings

Missing readings were a significant concern in the selected households.

| Household | Aggregate missing (%) | Washing-machine missing (%) |
|---|---:|---:|
| House 1 | 12.64 | 12.64 |
| House 2 | 24.62 | 24.62 |
| House 3 | 11.38 | 11.38 |
| House 5 | 9.34 | 9.34 |
| House 6 | 15.25 | 15.25 |
| House 9 | 15.38 | 15.38 |

House 2 had the highest missing-data proportion among the selected households, while House 5 had the lowest. Missing readings and long outages reduce the amount of usable data and may affect the representativeness of the model's evaluation.

The appliance-power distributions were also highly skewed: most washing-machine readings were zero or near zero, with occasional high-power events. This imbalance makes it important to evaluate model performance during appliance-active periods as well as across all observations.

## 6. Washing-Machine Target Mapping

The following appliance channels were used as targets in the modeling notebook.

| Household | Target channel |
|---|---|
| House 1 | Appliance5 |
| House 2 | Appliance2 |
| House 3 | Appliance6 |
| House 5 | Appliance3 |
| House 6 | Appliance2 |
| House 9 | Appliance3 |

These mappings should be checked against the official REFIT appliance metadata to confirm that each channel corresponds to the washing machine.

## 7. Model-Ready Data

After feature engineering and exclusion of incomplete required features, the following observation counts were available for modeling.

| Household | Model-ready rows |
|---|---:|
| House 1 | 767,808 |
| House 2 | 646,315 |
| House 3 | 763,609 |
| House 5 | 808,018 |
| House 6 | 678,927 |
| House 9 | 676,781 |
| **Total** | **4,340,468** |

The difference between one-minute coverage and model-ready observations results from missing readings and the availability requirements of the engineered features.

## 8. Limitations and Assumptions

- Missing readings may not occur randomly and could affect the observed usage patterns.
- Resampling can smooth short power changes within a minute.
- Long outages cannot be assumed to represent zero consumption.
- Appliance channel mappings must be verified using official metadata.
- The selected six households may not represent all REFIT households or households in other countries.
- Results depend on the documented preprocessing and missing-data handling rules.

## 9. Reproducibility

The data-cleaning notebook documents the preprocessing workflow, while the subsequent notebooks use the cleaned data for exploratory analysis and appliance disaggregation.

The raw dataset is obtained separately from the official source. Dataset paths must be configured for the local environment before running the notebooks. The precise cleaning rules and generated outputs should be checked against the notebook implementation before submission.
