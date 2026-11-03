---
title: Supplementary Data
---

# Supplementary Data

[← Back to project overview](./)

## Primary supplementary dataset: Seoul daily weather (KMA ASOS)

| | |
|---|---|
| **Title** | Synoptic Weather Observation (종관기상관측, ASOS), daily data |
| **Provider** | Korea Meteorological Administration (KMA), Open MET Data Portal (기상자료개방포털) |
| **Source link** | <https://data.kma.go.kr/data/grnd/selectAsosRltmList.do?pgmNo=36> (free account login required to download) |
| **Station** | 108 (Seoul) |
| **Coverage** | January 1 – June 30, 2026 (181 days), the same period as our bike rental data |
| **File in our project** | `seoul_weather_2026_01-06.csv` (181 rows, one per day) |
| **Accessed** | November 3, 2026 |

### Relevant variables

| Original name | English name | Unit | Notes |
|---|---|---|---|
| 일시 | `date` | date | Join key |
| 평균기온(°C) | `avg_temp_c` | °C | Daily mean temperature (range in our data: −9.7 to 27.2) |
| 최저기온(°C) | `min_temp_c` | °C | Daily minimum |
| 최고기온(°C) | `max_temp_c` | °C | Daily maximum |
| 일강수량(mm) | `precip_mm` | mm | Blank on 120 days, meaning no measurable precipitation; we fill these with 0 |
| 평균 풍속(m/s) | `avg_wind_mps` | m/s | Daily mean wind speed |
| 평균 상대습도(%) | `avg_humidity_pct` | % | Daily mean relative humidity |
| 합계 일조시간(hr) | `sunshine_hr` | hours | Total daily sunshine |
| 지점, 지점명 | `station_id`, `station_name_kr` | — | Constant (108, 서울); excluded from analysis |

### How it contributes to our research question

Weather is one of the most obvious external influences on bike-share demand. For cluster interpretation, it lets us ask whether different usage profiles respond differently to weather. For example, do stations with strong weekday rush-hour patterns hold up better on rainy or cold days than leisure-oriented stations?

### How it connects to the bike data

- **Join key: date.** Each trip's `rental_datetime` is reduced to a calendar date and matched to that day's weather row.
- **Coverage check:** the weather file has exactly one row for each of the 181 days in the rental data, with no missing dates.

### Use if direct merging is not appropriate

The weather data is **city-wide and daily**: one station, one row per day. If our unit of analysis is the **station**, weather cannot be merged as a column directly, because every station would get identical values on a given day. Instead, we plan to:

1. Join trips to daily weather by date.
2. Aggregate per station into a **weather-sensitivity feature**, such as a station's average daily rentals on rainy days divided by its average on dry days.
3. Use that feature mainly as an **external (interpretation) variable**: described per cluster after clustering, rather than used to form the clusters.

Known limitations:

- One observation point for the whole city, so local differences in weather are not captured.
- Only 15 days had a mean temperature below −5 °C, all in January–February. Any cold-sensitivity measure must therefore compare freezing and non-freezing days *within* January–February, so that seasonal growth in ridership isn't mistaken for weather sensitivity.
- Rain gives more contrast: 61 days have a recorded precipitation value.

## Planned context datasets (later sessions)

These have been collected but are **not** part of the Session 1 analysis. They may be used later to interpret clusters.

| Dataset | Provider / source | How it would connect | Possible use |
|---|---|---|---|
| Ttareungi station information (`station_locations.csv`) | Seoul Open Data Plaza, [OA-13252](https://data.seoul.go.kr/dataList/OA-13252/F/1/datasetView.do) | **Station ID** (`rental_station_id` ↔ `대여소_ID`) | Coordinates for all spatial features; 77 stations with (0, 0) coordinates must be excluded |
| Seoul subway station master (`subway_station_locations.csv`) | Seoul Open Data Plaza, [OA-21232](https://data.seoul.go.kr/dataList/OA-21232/S/1/datasetView.do) | Distance from each bike station to the nearest subway station | First/last-mile interpretation |
| Seoul district boundaries (`seoul_district_boundaries.geojson`) | [datainworld/administrative_district](https://github.com/datainworld/administrative_district) (GitHub) | Point-in-polygon: which of the 25 districts a station is in | District labels for interpretation |
| District-level daily living population (`seoul_floating_population_2026_01-06.csv`) | Seoul Open Data Plaza, [OA-23031](https://data.seoul.go.kr/dataList/OA-23031/S/1/datasetView.do) | District + date | Coarse context only (all stations in a district share one value); the citywide total row (code 11000) must be removed |
| Seoul cycleway network (`seoul_bike_path_network.geojson`) | OpenStreetMap contributors, via Overpass Turbo ([licence: ODbL](https://www.openstreetmap.org/copyright)) | Distance to the nearest cycleway | Infrastructure context; OSM is volunteer-mapped, so coverage may be uneven |

## Data sources

| Dataset | Provider | Link | Licence |
|---|---|---|---|
| Public bike rental history (main dataset) | Seoul Metropolitan Government | [OA-15182](https://data.seoul.go.kr/dataList/OA-15182/F/1/datasetView.do) | KOGL Type 1 (attribution) |
| Public bike daily rental counts | Seoul Metropolitan Government | [OA-14994](https://data.seoul.go.kr/dataList/OA-14994/F/1/datasetView.do) | KOGL Type 1 (attribution) |
| Daily weather, ASOS station 108 | Korea Meteorological Administration | [data.kma.go.kr](https://data.kma.go.kr/data/grnd/selectAsosRltmList.do?pgmNo=36) | See KMA portal terms |
| Station information | Seoul Metropolitan Government | [OA-13252](https://data.seoul.go.kr/dataList/OA-13252/F/1/datasetView.do) | KOGL Type 1 (attribution) |
| Subway station master | Seoul Metropolitan Government | [OA-21232](https://data.seoul.go.kr/dataList/OA-21232/S/1/datasetView.do) | KOGL Type 1 (attribution) |
| District-level living population | Seoul Metropolitan Government | [OA-23031](https://data.seoul.go.kr/dataList/OA-23031/S/1/datasetView.do) | KOGL Type 1 (attribution) |
| District boundaries | datainworld (GitHub) | [repository](https://github.com/datainworld/administrative_district) | See repository |
| Cycleway network | © OpenStreetMap contributors | [openstreetmap.org](https://www.openstreetmap.org/copyright) | ODbL |

All sources accessed November 3, 2026. Raw data files are not stored in this repository because the main trip file (~3.1 GB) exceeds GitHub's file-size limit; download them from the links above.

<!-- To confirm later: the 2026 trip-history and daily-count files are listed on OA-15182 / OA-14994; floating population columns match OA-23031 rather than OA-23060. -->

