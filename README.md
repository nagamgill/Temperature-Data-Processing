Temperature Data Processing

Overview
This script is designed to fetch, clean, analyze, and visualize
temperature data from the USDA NRCS AWDB API.
It retrieves daily temperature data, processes it for quality control,
and removes outliers. Additionally,
the script plots both raw and cleaned data to visualize the impact of
quality control measures.
Workflow:
1. Initialize the Processor: Define stations and date range.
2. Fetch Data: Retrieve data via API calls.
3. Preprocess Data: Convert API JSON response into a structured
DataFrame.
4. Clean Data: Identify and remove outliers based on predefined
thresholds.
5. Plot Raw and Cleaned Data: Visualize how the cleaning process
affects the dataset.
6. Export Data: Save the cleaned dataset to a CSV file.
1. Initialization
Class Definition:
The script uses a class-based structure (TemperatureDataProcessor)
for efficient data handling.
- station_triplets: List of station identifiers from NRCS AWDB.
- start_date: Start of the time period for data retrieval (YYYY-MM-DD).
- end_date: End of the time period for data retrieval (YYYY-MM-DD).
- db_name: Name of the SQLite database (optional, used for local
storage).
Temperature Variables Tracked:
- TOBS: Observed Temperature
- TMAX: Maximum Temperature
- TMIN: Minimum Temperature
- TAVG: Average Temperature
2. Fetching Data
API Request:
The script sends GET requests to:
https://wcc.sc.egov.usda.gov/awdbRestApi/services/v1/data
Each request includes:
- Station ID (stationTriplets)
- Start & End Date (beginDate, endDate)
- Elements (TOBS, TMAX, TMIN, TAVG)
- Duration (DAILY)
If successful (200 OK), it extracts temperature readings.
3. Preprocessing Data
Converting JSON to DataFrame:
The API response contains nested JSON objects.
This function flattens and structures them into a pandas DataFrame.
- Date Formatting: Converts timestamps into proper datetime format
(YYYY-MM-DD).
- Indexing: Sets date as the DataFrame index.
- Data Type Casting: Ensures temperature values are stored as float
for numerical operations.
4. Cleaning Data
Quality Control Measures:
1. Preserving Raw Values
- Original data is stored in new columns as "TMAX_raw",
"TMIN_raw", etc.
- This allows before-and-after comparison.
2. Identifying Outliers
- Extreme temperature thresholds:
- Lower Limit = -50°F
- Upper Limit = 130°F
- Any value outside this range is flagged as an outlier (E).
Why use NaN?
- Leaving the cell empty maintains the structure of the dataset.
- It prevents the introduction of incorrect values.
5. Visualization
Plotting Raw & Cleaned Data:
The script generates interactive line charts using Plotly.
The plots visually highlight:
- Temperature trends
- Gaps created by removed outliers
- Differences between raw and cleaned data
6. Exporting Data
Saving to CSV:
After cleaning, the final dataset is saved in temperature_cleaned.csv.
CSV Columns:
| Date | TMAX_raw | TMAX | TMAX_qc_flag | TMIN_raw | TMIN |
TMIN_qc_flag | ... |
|------|---------|------|-------------|---------|------|-------------|-----|
| 2024-01-01 | 33.4 | 33.4 | V | 7.5 | 7.5 | V | ... |
| 2024-01-02 | 160.0 | NaN | E | 5.2 | 5.2 | V | ... |
7. Execution & Summary
Main Execution Flow:
1. Fetches temperature data from NRCS AWDB API
2. Converts JSON response into structured data
3. Identifies and removes extreme outliers
4. Preserves original data for transparency
5. Generates visual plots for comparison
6. Saves cleaned dataset to CSV
Key Takeaways:
- Accurate Data Handling: Keeps raw and processed data separate.
- Outlier Management: Flags extreme values and removes them.
- Visualization: Helps users understand data trends.
- Exportable Data: Ready-to-use cleaned temperature dataset.
Future Improvements:
- Add more advanced anomaly detection using statistical methods.
- Implement machine learning models for predictive analysis.
- Allow for custom threshold settings per station.
