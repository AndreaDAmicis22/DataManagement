# Air Pollution and Weather Data Analysis

## Overview
This project explores the relationship between air pollution and weather conditions in Europe, with a particular focus on Italy and generally west european states:
* Italy (IT)
* Switzerland (CH)
* Spain (ES)
* France (FR)
* Belgium (BE)
* Netherlands (NL)
* Germany (DE)
* Portugal (PT)
* Great Britain (GB)
* Ireland (IE)
* Austria (AT)
* Norway (NO)
* Finland (FI)
* Sweden (SE) 
The study analyzes the dynamics of pollution levels based on various meteorological factors and examines their impact on human health. 

Key pollutants studied include PM10, PM2.5, O3, NO2, CO, NO, NOx, and SO2. Weather parameters such as temperature, precipitation, wind speed, and snow accumulation are integrated to provide a comprehensive view.

## Research Questions
The analysis is guided by research questions, split into European and Italian contexts:
- **European Analysis**:
  - Average O3 levels across maximum temperature ranges.
  - Average NO2 levels across precipitation ranges.
  - Average PM10 levels across wind speed ranges.
  - Seasonal variations in PM10 levels.
- **Italian Analysis**:
  - Monthly trends of NO2, O3, and PM2.5 in 2022.
  - Identification of the highest PM2.5 records, including location and meteorological context.

## Data Sources
The project uses data from two APIs:
1. [OpenAQ API](https://openaq.org): Air pollution data.
2. [Open-Meteo API](https://open-meteo.com): Meteorological data.

Data is stored in MongoDB collections for efficient handling and analysis.

# Methodology
## Data Acquisition
- **Pollution Data**: Collected daily for 2022 for major European cities using the OpenAQ API. 
- **Weather Data**: Matched with pollution data by geographical coordinates using the Open-Meteo API.

## Data Profiling
- **Completeness**: Addressed missing values (`NaN` and `0`) in the datasets, ensuring reliable averages.
- **Consistency**: Verified ranges for pollutants and meteorological parameters to identify anomalies.

## Data Integration and Cleaning
- Removed entries with excessively high pollution parameter values.
- Excluded entries with negative pollutant values.
- Dropped columns for `CO`, `NOx`, and `NO` due to almost all values being `NaN`.
- Replaced remaining `NaN` values with the mean value for each state individually.
- Merged weather datasets to create a coherent daily view of parameters for each city.
- Removed entries with inconsistent or improbable measurements:
  - Minimum temperature > Maximum temperature.
  - Apparent minimum temperature > Apparent maximum temperature.
  - Minimum temperature > 50°C OR Apparent minimum temperature > 0°C.
- Excluded entries with inconsistent WindSpeed and Precipitation values, which corresponded to temperature inconsistencies.

## Data Enrichment

- **WMO Code Descriptions**  
  Added descriptive labels for `WMO_code` to improve interpretability.  
  Example: `WMO = 1` corresponds to "Cloud development not observed or not observable."

- **Season Attribute**  
  Included a `Season` attribute categorized as Winter, Summer, Spring, or Autumn.

- **Region Attribute**  
  Added a `Region` attribute based on the United Nations Geoscheme for Europe:  
  - **Northern Europe:** Norway, Finland, Sweden, Great Britain, and Ireland.  
  - **Western Europe:** Germany, Austria, Switzerland, Netherlands, Belgium, and France.  
  - **Southern Europe:** Italy, Spain, and Portugal.

## Data Storage
Data is stored in a document-oriented MongoDB database for flexibility:
- `Pollution Collection`: Contains raw pollution data. 180’768 documents.
- `Weather Collection`: Contains raw weather data. 180’768 documents.
- `Pollution Cleaned`: Contains cleaned pollution data. 99'048 documents
- `Weather Cleaned`: Contains cleaned weather data. 99'048 documents
- `Collection Merged`: Contains weather and pollution data merged together. 99'048 documents

## Final Data Structure (Collection Merged)

- **_id:** Unique identifier for the document.  
- **State:** Country code (e.g., "CH").  
- **City:** Name of the city (e.g., "Basel-Landschaft").  
- **Date:** Date of the record (e.g., "2022-01-11").  
- **Latitude:** Latitude coordinate (e.g., 47.5410842849654).  
- **Longitude:** Longitude coordinate (e.g., 7.583269599999999).

### Measurements
- **Pm10:** Particulate matter 10 µm concentration.  
- **Pm25:** Particulate matter 2.5 µm concentration.  
- **O3:** Ozone concentration.  
- **No2:** Nitrogen dioxide concentration.  
- **So2:** Sulfur dioxide concentration.  
- **Unit:** Measurement unit (e.g., "µg/m³").

### WMO
- **Code:** Weather code (e.g., 3).  
- **Description:** Description of the weather condition (e.g., "Clouds generally forming or developing").

### Temperature
- **Min:** Minimum temperature (e.g., -2.5°C).  
- **Max:** Maximum temperature (e.g., 3.4°C).  
- **Unit:** Temperature unit (e.g., "°C").

### Apparent Temperature
- **Min:** Minimum apparent temperature (e.g., -5.7°C).  
- **Max:** Maximum apparent temperature (e.g., 0.4°C).  
- **Unit:** Apparent temperature unit (e.g., "°C").

### WindSpeed
- **Value:** Wind speed (e.g., 10.8 km/h).  
- **Unit:** Wind speed unit (e.g., "km/h").

### Precipitation
- **Sum:** Total precipitation (e.g., 0 mm).  
- **Rain:** Rainfall amount (e.g., 0 mm).  
- **Snowfall:** Snowfall amount (e.g., 0 mm).  
- **Unit:** Precipitation unit (e.g., "mm").

### Additional Attributes
- **Region:** Geographic region based on the UN Geoscheme (e.g., "Western Europe

## Tools Used
- **Programming**: Python for API querying, data cleaning, and integration.
- **Database**: MongoDB for data storage.
- **Visualization**: Matplotlib and Seaborn for plots.
- **Document Preparation**: LaTeX for report writing.
