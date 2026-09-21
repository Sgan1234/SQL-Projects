# SQL-Projects
NYC Air Quality and Health Impacts: A SQL Analysis

Overview

This project analyzes the relationship between air pollution exposure and respiratory health outcomes across New York City neighborhoods. Using the NYC Open Data "Air Quality and Health Impacts" dataset, I built a PostgreSQL database to explore how pollutant levels (PM2.5, NO2, O3) correlate with asthma hospitalizations, cardiac outcomes, and traffic density at the neighborhood level.

The analysis investigates which neighborhoods bear the highest combined environmental and health burden, and why — revealing that both physical geography (coastal wind patterns) and socioeconomic factors (income, healthcare access, housing quality) shape pollution-related health disparities.

Research Questions

Which NYC neighborhoods show the highest air pollution burden?
Where do asthma and cardiac hospitalizations due to air quality concentrate?
Does traffic density explain the pollution patterns, or do other factors play a role?
Why do pollution levels and health outcomes sometimes diverge across neighborhoods?
Data Sources

Source	Description	Access
NYC Open Data	Air Quality and Health Impacts — winter, summer, and annual averages for PM2.5, NO2, and O3, plus asthma/cardiac hospitalizations and deaths due to air quality, and traffic density by neighborhood	NYC Open Data
Note: The dataset is publicly available and does not contain personally identifiable health information.

Repository Structure

text
nyc-air-quality-health/
├── README.md
├── sql/
│   └── analysis.sql           # Core analytical queries
├── data/
│   └── sample_data.csv        # Anonymized sample subset
├── docs/
│   └── findings.md            # Extended analysis notes
└── results/
    └── screenshots/           # Tableau dashboard previews
Setup and Reproduction

Prerequisites

PostgreSQL 14 or higher
psql command-line tool
Installation

bash
# Clone the repository
git clone https://github.com/yourusername/nyc-air-quality-health.git
cd nyc-air-quality-health

# Create the database
createdb nyc_air_health

# Load schema and data
psql -d nyc_air_health -f sql/analysis.sql
Data Notes

Two data quirks required handling during analysis:

Pollution values were stored as text and needed CAST(data_value AS FLOAT) for numeric sorting.
Hospitalization rates were recorded in the name (pollutant) column rather than a dedicated health metric column, requiring careful filtering.
SQL Techniques Demonstrated

Technique	Application
CAST	Converting text-stored numeric values for proper sorting
Filtering with WHERE	Isolating specific pollutants and health indicators
ORDER BY + LIMIT	Ranking neighborhoods by pollution level or health impact
Column aliasing	Renaming raw columns (geo_place_name, data_value) for readability
Key Findings

Ozone peaks in coastal neighborhoods. The Rockaways and Coney Island recorded the highest average O3 levels, driven by the interaction of polluted air masses with coastal wind patterns.
Traffic density doesn't tell the whole story. Chelsea and Harlem ranked among the top neighborhoods for traffic density, but Midtown ranked highest for PM2.5 — showing that traffic alone doesn't predict particulate pollution.
Pollution and health outcomes diverge. The highest PM2.5 levels were in Manhattan and Chelsea, yet the highest asthma hospitalization rates occurred in Harlem and Hunts Point. This gap likely reflects income-related differences in preventative healthcare access and housing quality.
A multi-faceted problem requires multi-faceted solutions. Physical environment (built and natural) and socioeconomic factors both shape pollution-related health outcomes.
Sample Query

sql
SELECT
    geo_place_name AS neighborhood,
    name AS pollutant,
    time_period,
    data_value AS pollution_level,
    measure_info
FROM
    air_quality_health
WHERE
    name = 'Fine particles (PM 2.5)'
ORDER BY
    CAST(data_value AS FLOAT) DESC
LIMIT 10;
Dashboard Preview

https://results/screenshots/asthma_by_neighborhood.png

Visualization built from SQL analysis results.

Skills Demonstrated

PostgreSQL querying with filtering, sorting, and type conversion
Data cleaning and handling of non-standard column storage
Environmental health data interpretation
Cross-referencing pollution, traffic, and health outcome data
References

NYC Open Data. (2014). Air quality and health impacts. https://data.cityofnewyork.us/Environment/Air-Quality-and-Health-Impacts/c3uy-2p5r/about_data

U.S. Environmental Protection Agency. (2021, February 23). Modeling research shows how salty ocean air impacts ozone pollution. https://www.epa.gov/sciencematters/modeling-research-shows-how-salty-ocean-air-impacts-ozone-pollution

NYC Department of Health and Mental Hygiene. (n.d.). Asthma and the environment in East Harlem. Environment & Health Data Portal. https://a816-dohbesp.nyc.gov/IndicatorPublic/neighborhood-reports/east_harlem/asthma_and_the_environment/
