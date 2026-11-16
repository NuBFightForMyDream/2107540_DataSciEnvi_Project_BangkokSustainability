# Green Space and PM2.5 Across Bangkok's Districts (2021–2024)

Course project for **2107540 Data Science for Environment and Sustainability**, Academic Year 2026 Semester 1.

## Environmental Problem

Bangkok regularly experiences high PM2.5 levels, especially in the dry season (November–April). PM2.5 is linked to respiratory and cardiovascular disease, and Thailand tightened its 24-hour PM2.5 standard to **37.5 µg/m³** in 2023. Green space in Bangkok is unevenly distributed: some outer districts have more than 50 m² of green space per person, while inner-city districts have only a few m².

**Research question:** Is green space per capita associated with lower PM2.5 levels in Bangkok's districts?

## Dataset Source

| File | Content | Coverage | Source |
|---|---|---|---|
| `bangkok_green_space_by_district.csv` | Green space area, district area, population | 50 districts | [insert source + URL] |
| `bangkok_pm25_2021.csv` – `bangkok_pm25_2024.csv` | Daily PM2.5 (µg/m³) per monitoring station | 2021 – 2024 (2023: Jan–Jun only), 96 stations | [insert source + URL] |
| `station_district.csv` | Station code → district lookup | 96 stations | Air4Thai station list, Pollution Control Department |

Files in `data/` that are not used: `pm25_aqi.csv` (Suphanburi province, not Bangkok) and `hii_station_metadata.csv` (rain-gauge and water-level stations, no PM2.5). `bangkok_population_by_district.csv` (2005–2018) is optional.

## Data Cleaning and Transformation

- **Green space:** removed the Bangkok total row, removed the `เขต` prefix from district names, calculated green space per capita (m²/person) and population density (persons/km²).
- **PM2.5:** handled Thai encoding (`cp874`) in 2021–2022, different date formats (2024 is day/month/year), merged the old and new 72T station columns, removed a duplicate date, reshaped from wide (one column per station) to long format, and combined all four years.
- **Merge:** joined PM2.5 with the station lookup table, averaged stations within each district per day, then summarised each district as mean PM2.5, dry-season PM2.5, and % of days above 37.5 µg/m³.
- All steps are done in pandas; SQL (SQLite) is used to cross-check the aggregations.

## Exploratory Data Analysis (EDA)

[Summary: distributions, missing values, number of stations per year, outliers above 200 µg/m³ and how they were handled]

## Data Analysis and Insights Extraction

[Key findings: highest/lowest districts, % of days above the standard, correlation between green space per capita and PM2.5 (Spearman ρ), dry-season result, priority districts]

## Data Visualization

Figures are saved in `figures/`:

| Figure | Shows |
|---|---|
| `green_per_capita.png` | Green space per capita by district |
| `pm25_monthly.png` | Monthly mean PM2.5, 2021–2024, with the Thai standard |
| `green_vs_pm25.png` | Green space per capita vs. mean PM2.5 |
| `correlation_heatmap.png` | Spearman correlations between indicators |

## Basic Machine Learning (Optional)

- **K-Means clustering** of districts by green space per capita, population density, and PM2.5. [Describe the clusters]
- **Linear regression** of mean PM2.5 on green space per capita and population density, evaluated with leave-one-out cross-validation. [R², MAE]

## Environmental Interpretation

[Answer the research question, explain the result, and give policy implications for green space planning in Bangkok]

**Limitations:** no temperature data; PM2.5 is measured at stations, not across whole districts; station numbers change between years; green space data is a single snapshot; correlation does not imply causation.

## Repository Structure

```
├── README.md
├── data/                 raw CSV files
├── notebooks/
│   └── DS_analysis.ipynb
├── figures/              saved charts
└── requirements.txt
```

## How to Run

```bash
pip install -r requirements.txt
jupyter notebook notebooks/DS_analysis.ipynb
```
