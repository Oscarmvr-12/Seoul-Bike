---
title: Project Overview
---

# Patterns of Urban Mobility: Explainable Clustering of Seoul’s Bike-Share Stations

**CDS 303: Scientific Data Mining · Final Project · Team 8**

<p class="authors">Dae Yeong Mun &amp; Oscar Molina Romualdo</p>

<div class="stats"><div class="stat"><strong>18.5M</strong><span>Rental records</span></div><div class="stat"><strong>16</strong><span>Original columns</span></div><div class="stat"><strong>Jan–Jun</strong><span>2026 coverage</span></div></div>

<div class="actions"><a class="button" href="https://github.com/Oscarmvr-12/Seoul-Bike/blob/main/notebooks/Bike_S_.ipynb">View Project 0 notebook ↗</a><a class="button secondary" href="https://github.com/Oscarmvr-12/Seoul-Bike">GitHub repository ↗</a></div>

## Initial research question

> **What distinct usage profiles do Seoul's Ttareungi bike-share stations show in January–June 2026, and which characteristics best explain those profiles: time-of-day patterns, weekday/weekend balance, trip length, rider mix and sensitivity to weather?**


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

<div class="section-cards">
<a class="section-card" href="./"><small>01 / PROJECT OVERVIEW</small><strong>The research question ↗</strong><span>Project scope, team and primary dataset.</span></a>
<a class="section-card" href="paper-summary.html"><small>02 / PAPER SUMMARY</small><strong>Explainable cluster analysis ↗</strong><span>The framework guiding our analysis.</span></a>
<a class="section-card" href="variables.html"><small>03 / VARIABLE SELECTION</small><strong>From trips to features ↗</strong><span>Variables selected for the project.</span></a>
<a class="section-card" href="supplementary-data.html"><small>04 / SUPPLEMENTARY DATA</small><strong>Additional context ↗</strong><span>Supplementary data sources and integration.</span></a>
</div>

**Supporting code:** [CDS303_Final_project_0.ipynb](https://github.com/Oscarmvr-12/Seoul-Bike/blob/main/notebooks/CDS303_Final_project_0.ipynb) loads the full trip dataset and reports its first rows, shape, columns, data types, summary statistics and missing values.

---

