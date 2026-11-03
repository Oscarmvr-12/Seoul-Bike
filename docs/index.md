---
title: Project Overview
---

# Explainable Cluster Analysis of Seoul Bike-Share (Ttareungi) Usage

**CDS 303: Scientific Data Mining · Final Project · Team 8**

| Team member |
|---|
| Dae Yeong Mun |
| Oscar Molina Romualdo |

## Initial research question

> **What distinct usage profiles do Seoul's Ttareungi bike-share stations show in January–June 2026, and which characteristics best explain those profiles: time-of-day patterns, weekday/weekend balance, trip length, rider mix and sensitivity to weather?**

*Proposed unit of analysis: the station (about 2,700–3,400 stations), with features aggregated from individual trips. The team is still confirming this choice. Alternatives considered: days (only 181 observations, so clusters would mostly reflect season and weather) and individual trips (18.5 million records with few descriptive features each, so clusters would be hard to explain).*

## Current dataset

**Seoul Public Bicycle Rental History (서울특별시 공공자전거 대여이력 정보)**

- **Provider:** Seoul Metropolitan Government, via Seoul Open Data Plaza
- **Source:** <https://data.seoul.go.kr/dataList/OA-15182/F/1/datasetView.do>
- **Licence:** Korea Open Government License (KOGL) Type 1, attribution required
- **Coverage:** January 1 – June 30, 2026
- **Size:** 18,511,943 trip records × 16 columns
- **Contents:** one row per rental. Each row records the bike ID; rental and return date-time; rental and return station (ID, number and name); dock numbers; trip duration (minutes); distance (metres); rider birth year; gender; and user type (domestic, foreign or non-member).

The original column names are in Korean and are translated to English in our notebook. Missing values, data types and summary statistics are in the [Project 0 notebook](https://github.com/Oscarmvr-12/Seoul-Bike/blob/main/notebooks/CDS303_Final_project_0.ipynb).

---

## Session 1

| Section | Page |
|---|---|
| 1. Project overview | *this page* |
| 2. Paper summary: explainable cluster analysis framework | [Paper Summary](paper-summary.html) |
| 3. Variable selection | [Variable Selection](variables.html) |
| 4. Supplementary dataset | [Supplementary Data](supplementary-data.html) |

**Supporting code:** [CDS303_Final_project_0.ipynb](https://github.com/Oscarmvr-12/Seoul-Bike/blob/main/notebooks/CDS303_Final_project_0.ipynb) loads the full trip dataset and reports its first rows, shape, columns, data types, summary statistics and missing values.

---

## Future updates

*Space reserved for later sessions (preprocessing, feature engineering, clustering, classification and interpretation).*
