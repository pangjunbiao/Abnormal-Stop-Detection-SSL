# Dataset Description

This document describes the processed GPS trajectory dataset used in our few-shot semi-supervised abnormal-stop detection pipeline (SAS → indicators → LTIGA → graph learning).  
The data were collected by our lab via field operation on the Liuliqiao–Zhangjiakou intercity route and contain low-frequency GPS records from multiple vehicles.

---

## Dataset Overview, Statistics, and Labeling Summary

| Item | Value |
|------|-------|
| Route | Liuliqiao (Beijing) → Zhangjiakou (Hebei) |
| Approx. route length | ~180 km |
| #Records (GPS points) | 2,410 |
| #Unique vehicles | 16 |
| Trips | Multiple vehicle-based trajectories on the same route |
| Sampling interval | 30–60 s (low-frequency GPS) |
| Avg. stay time | 102.34 s |
| Avg. speed / max speed | 13.82 km/h / 97.0 km/h |
| Min speed | 0.0 km/h (stops) |
| Avg. dist. from start | 12,913 m |
| Avg. dist. between points | 142.7 m |
| Total detected stops | Inferred from sparse GPS (Algorithm 1) |
| Abnormal-stop supervision | 10 abnormal-stop locations (GPS coordinates) |
| Labeling source | Manual field annotation |


### Abnormal-stop supervision (label source)

Supervision is provided as a set of **10 pre-identified abnormal-stop locations** (GPS coordinates) recorded during field collection, corresponding to non-designated stopping points (e.g., unauthorized pick-up/drop-off sites) or atypical dwell behavior.

- A detected stop (or its containing segment) is labeled **abnormal** if its location falls within a predefined **spatial tolerance** of any abnormal-stop coordinate.
- Otherwise, it is treated as **unlabeled/normal** depending on the experimental setting.
- The same coordinate list is used across all experiments for reproducibility.

---

## Files and Organization

> Adjust the file names below to match your repository.

- `gps_points.csv`  
  Raw/processed GPS point-level records (the schema below).
- `abnormal_stop_coordinates.csv`  
  List of the 10 abnormal-stop locations used for labeling (latitude/longitude pairs).
- (Optional) `stops_or_segments.csv`  
  Outputs after stop extraction and SAS segmentation, used for indicator construction and graph learning.

---

## Schema (column dictionary)

Each row corresponds to one GPS record (a sampled point) for a specific vehicle.

| Column | Type | Description |
|---|---|---|
| `longitude` | float | GPS longitude (WGS84 or consistent coordinate system used in collection) |
| `latitude` | float | GPS latitude |
| `stay_time` | float | Dwell/stay time at the current point (seconds) |
| `dist_from_start` | float | Distance from the trip starting point along the trajectory (meters) |
| `speed` | float | Instantaneous speed at the point (km/h) |
| `dist_from_prev` | float | Distance from the previous GPS point (meters) |
| `traffic_light_label` | int/float | Corresponding traffic light label/category (as provided in the dataset) |
| `date` | int/str | Date identifier (dataset-specific encoding; e.g., `1101` for Nov 1) |
| `time` | int/float | Time-of-day value (dataset-specific encoding, see notes below) |
| `license_plate` | str/int | Vehicle identifier (license plate number or anonymized id) |
| `arrival_clustering_time` | int/float | Arrival / clustering timestamp used for aligning or clustering events (dataset-specific encoding) |

---

## Example rows

Below are sample rows (tab-separated) to illustrate the format:

```text
longitude    latitude    stay_time   dist_from_start   speed   dist_from_prev   traffic_light_label   date   time    license_plate   arrival_clustering_time
116.331713   40.042502   20.0        26366.402078      67.0    534.228775       10.0                 1101   35640.0 5873.0         95400.0
116.332025   40.041952   24.0        25610.075754      72.0    670.466080       10.0                 1101   32583.0 5987.0         90303.0
116.323361   40.053570   210.0       27102.377309      0.0     342.283524       10.0                 1101   32703.0 5987.0         90503.0
