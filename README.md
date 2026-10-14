# Hanoi PM2.5 Forecasting — Midterm Project

**Fundamentals of Data Science** | Data Collection → Data Cleaning → Exploratory Data Analysis

This repository contains the midterm-stage pipeline for hourly air quality data in Hanoi and preparation for a 6-hour-ahead PM2.5 forecasting task. Pollution measurements are accessed using [OpenAQ](https://openaq.org/); historical weather data are accessed using [Open-Meteo](https://open-meteo.com/).

## Notebooks

1. [`01_data_collection.ipynb`](notebooks/01_data_collection.ipynb): evaluate station completeness, collect PM2.5, PM10, and NO2 measurements alongside hourly weather, integrate on UTC timestamps using full outer joins, and export the integrated dataset.
2. [`02_data_cleaning.ipynb`](notebooks/02_data_cleaning.ipynb): analyze missingness and gaps, retain missing pollutant measurements, engineer PM2.5 lags, cyclic features, and a 6-hour-ahead target; export feature sets.
3. [`03_eda.ipynb`](notebooks/03_eda.ipynb): analyze PM2.5 distributions, temporal variation, and correlation with meteorological and predictive variables. Outputs and figures are retained for review on GitHub.

## How to run

Install Python 3.10+ and packages:

```bash
python -m pip install -r requirements.txt
cd notebooks
jupyter lab
```

Launch Jupyter with `notebooks/` as the current working directory because the notebooks use paths such as `../data/interim/` and `../data/processed/`.

Run notebooks **01 → 02 → 03**. Notebook 1 prompts for your own OpenAQ API key via `getpass()`; it requires an internet connection and creates the local data files. Notebook 2 then regenerates the intermediate and processed datasets used in Notebook 3. Historical API data may change over time, so re-running collection later may yield slightly different results.

**Data files are not committed to this public repository.** The raw OpenAQ provider's redistribution permissions for the selected Hanoi monitoring network were not explicitly identified in the retrieved station metadata. The project author retains the local processed data for validation. This does not imply that redistributing the data is prohibited; it is a precaution pending verification of the data provider's terms.

## Methodology

- Period: 2025-08-01 to 2026-08-01 (end exclusive); hourly UTC timeline.
- Bach Khoa is selected based on full-year PM2.5 completeness.
- No pollutant interpolation; missing values remain `NaN`.
- PM2.5 lags of 1, 2, 3 and 6 hours; wind-direction and local-hour sine/cosine encodings.
- Six-hour-ahead target `pm25_t_plus_6`; model feature sets Base, Base + PM10, and Full.
- Daily PM2.5 mean EDA uses days with at least 18 observed hours.

## Data source attribution and usage

- **Air pollution measurements:** provided by **Hanoi Air Quality Monitoring Network**, accessed through the **OpenAQ API** (Bach Khoa station; [location details](https://explore.openaq.org/locations/4946813)). See [OpenAQ Terms of Use](https://docs.openaq.org/about/terms), including source-specific attribution and redistribution provisions. This repository does not assert a specific license for the Hanoi monitoring network's measurements.
- **Historical weather:** accessed through the **Open-Meteo Historical Weather API** ([documentation](https://open-meteo.com/en/docs/historical-weather-api)), with acknowledgement of Open-Meteo and underlying weather providers. Open-Meteo identifies its weather data as licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/); see [terms](https://open-meteo.com/en/terms).
- The project merges hourly air-pollution and meteorological series and derives lagged, cyclic, and forecast-target variables; these are project transformations and not original measurements by the authors.

No API credentials are committed to this repository.
