---
title: Variable Selection
---

# Variable Selection

[← Back to project overview](./)

Our proposed unit of analysis is the **station**. The raw dataset has one row per **trip**, so most clustering inputs are **derived features**: trip-level variables aggregated per rental station. The two tables below keep **existing variables** (in the raw file today) separate from **proposed derived features** (to be built in a later session).

**Roles**
- **Clustering input:** used to form the clusters.
- **Classification input:** used as a predictor when we later classify the cluster labels.
- **Contextual:** used only to describe and interpret clusters after they are formed.
- **Exclude:** not used in modelling.

## 1. Existing variables (raw trip file, 18,511,943 rows)

Missing-value counts come from our [Project 0 notebook](https://github.com/Oscarmvr-12/Seoul-Bike/blob/main/notebooks/CDS303_Final_project_0.ipynb).

| Variable | Description & unit | Type | Proposed role | Rationale & preparation |
|---|---|---|---|---|
| `bike_id` | Bike identifier (e.g. SPB-65030) | Categorical (ID) | Exclude | Identifier with no behavioural meaning; 261 missing |
| `rental_datetime` | Rental date and time | Datetime | Source for derived features | Used for hour-of-day, weekday/weekend and date (weather join) |
| `return_datetime` | Return date and time | Datetime | Exclude | Duplicates information in `usage_duration_min` |
| `rental_station_id` | Rental station ID (e.g. ST-1461) | Categorical (ID) | **Grouping key** | Defines the unit of analysis and joins to station coordinates. Never a model input |
| `rental_station_number`, `rental_station_name` | Station number and Korean name | Categorical (ID) | Exclude | Redundant with the station ID; names can change over time, so we join on IDs, not names |
| `return_station_id` | Return station ID | Categorical (ID) | Source for derived features | Used for round-trip share and net flow; 100,206 missing (≈0.5%), and those trips are dropped from return-based features |
| `return_station_number`, `return_station_name` | Return station number and name | Categorical (ID) | Exclude | Redundant with `return_station_id` |
| `rental_dock`, `return_dock` | Dock numbers | Numeric code | Exclude | Mostly 0; no analytical meaning |
| `usage_duration_min` | Trip duration (minutes) | Numeric | Source for derived features | Strongly skewed (max 65,977 min), so we will trim implausible trips and use station **medians** |
| `usage_distance_m` | Trip distance (metres) | Numeric | Source for derived features | Strongly skewed (max ≈13.2 million m); same treatment as duration |
| `birth_year` | Rider birth year | Numeric | Source (optional), or exclude | 465,492 missing and impossible values (0, 2099). If used, keep plausible years only and aggregate to a station median rider age |
| `gender` | Rider gender (M/F) | Categorical | Exclude (contextual at most) | 4,564,501 missing (≈25%), too incomplete to rely on |
| `user_type` | Domestic / foreign / non-member | Categorical | Source for derived features | No missing values; aggregated to shares, so no one-hot encoding needed |

## 2. Proposed derived features (station level, not yet built)

| Feature | Description & unit | Type | Proposed role | Rationale & preparation |
|---|---|---|---|---|
| `total_rentals` | Rentals starting at the station, Jan–Jun 2026 (count) | Numeric | Clustering input + filter | Log-transform before scaling; exclude stations below a minimum volume so that ratios aren't noisy |
| `am_peak_share` | Share of rentals on weekdays 07:00–09:59 (0–1) | Numeric | Clustering input | Signals commuter (morning) use |
| `pm_peak_share` | Share of rentals on weekdays 17:00–19:59 (0–1) | Numeric | Clustering input | Signals commuter (evening) use; check correlation with `am_peak_share` |
| `night_share` | Share of rentals 22:00–04:59 (0–1) | Numeric | Clustering input | Signals nightlife or late-shift use |
| `weekend_share` | Share of rentals on Saturday/Sunday (0–1) | Numeric | Clustering input | Signals leisure use |
| `median_duration_min` | Median trip duration (minutes) | Numeric | Clustering input | Computed after trimming implausible trips |
| `median_distance_m` | Median trip distance (metres) | Numeric | Clustering input, or drop | Probably strongly correlated with duration; keep only one if \|r\| > 0.8 |
| `round_trip_share` | Share of trips returned to the same station (0–1) | Numeric | Clustering input | Signals leisure loops |
| `net_flow_ratio` | (returns − rentals) / (returns + rentals), −1 to 1 | Numeric | Clustering input | Shows whether a station mainly sends or receives bikes |
| `foreign_share`, `nonmember_share` | Shares of rentals by user type (0–1) | Numeric | Clustering input | Signals tourist or occasional use |
| `rain_sensitivity` | Mean daily rentals on rainy days ÷ mean on dry days (ratio) | Numeric | Contextual (interpretation) | Requires the date join to weather. Kept out of clustering, following paper 2's use of external variables |
| `cold_sensitivity` | Mean daily rentals on freezing days ÷ non-freezing days, **within Jan–Feb only** (ratio) | Numeric | Contextual (interpretation) | Only 15 freezing days, so treat as exploratory. Comparing within Jan–Feb stops seasonal growth from being mistaken for weather sensitivity |
| `dist_to_subway_m` | Distance to the nearest subway station (metres) | Numeric | Classification input / contextual | From station and subway coordinates; stations with (0, 0) coordinates are excluded |
| `district` | One of Seoul's 25 districts | Categorical | Contextual | Point-in-polygon with district boundaries |
| `cluster_label` | Cluster assigned by the clustering step | Categorical | **Classification target only** | **Never** used as an input feature |

## Preparation notes

- **Missing values:** handled at the trip level before aggregation. Trips without a return station are dropped from return-based features only; trips with implausible durations or distances are trimmed. Station-level features should then have no missing values, apart from stations removed by the volume filter or missing coordinates.
- **Scaling:** all clustering inputs are standardized (mean 0, standard deviation 1) so that no feature dominates distance-based clustering because of its units. `total_rentals` is log-transformed first.
- **Categorical encoding:** `user_type` becomes numeric shares, so no one-hot encoding is needed. `district` is used only for interpretation.
- **Identifiers:** IDs, names and dock numbers are excluded from modelling. `rental_station_id` is used only to group and join.
- **Redundancy:** we will check pairwise correlations among the derived features (for example duration vs. distance, and AM vs. PM peak share) and drop one of each highly correlated pair, or rely on dimensionality reduction to combine them.

## Clustering vs. classification inputs

The classification step uses the **cluster labels as its target**. Following the papers, its main inputs will be the same station features used for clustering, possibly plus contextual variables such as `dist_to_subway_m`. When classification reuses clustering variables, its accuracy and SHAP values **characterize** the clusters, showing which features separate them. They do **not** independently validate that the clusters exist.

