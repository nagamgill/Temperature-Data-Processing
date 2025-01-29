# Temperature Data Processing

## Overview
This project fetches, processes, and cleans temperature data from USDA NRCS AWDB API. The script removes outliers, applies quality control (QC) flags, and exports the cleaned dataset to a CSV file.

## Features
- Fetches temperature data (`TOBS`, `TMAX`, `TMIN`, `TAVG`)
- Cleans data by:
  - Removing values outside `(-50, 130)`°F
  - Flagging anomalies using period-of-record (POR) medians
  - Comparing data against nearby stations
- Stores original values, cleaned values, and QC flags
- Exports cleaned dataset to `temperature_cleaned.csv`

## Usage
1. Install dependencies:
