# Climate Monitoring Workflow

## Introduction

This workflow helps you to monitor climate conditions, including temperature and precipitation, to understand long-term climate patterns, assess variability and extremes, forecast climate-related risks (such as heatwaves, floods, and droughts), and support climate adaptation, resilience, and environmental planning.

**What this workflow does:**
- Downloads climate observation data captured by **TAMHO** sensors from EarthRanger.
- Processes observations including temperature and precipitation measurements
- Calculates daily summaries (total precipitation and average temperature)
- Exports data in multiple formats (CSV, GeoParquet, GPKG)
- Creates interactive charts showing precipitation and temperature trends over time
- Generates a downloadable Climate Report (Word document) with charts and data summary

**Who should use this:**
- Conservation managers monitoring climate conditions in protected areas
- Researchers analyzing environmental data
- Anyone needing to export and visualize TAMHO sensor information stored in EarthRanger

## Prerequisites

Before using this workflow, you need:

1. **Ecoscope Desktop** installed on your computer
   - If you haven't installed it yet, please follow the installation instructions for Ecoscope Desktop

2. **EarthRanger Data Source** configured in Ecoscope Desktop
   - You must have already set up a connection to your EarthRanger server
   - Your data source should be configured with proper authentication credentials
   - You'll need to know the name of your configured data source (e.g., "gmmf")

3. **Subject Group with Weather Stations** set up in EarthRanger
   - You need to have at least one subject group configured with TAMHO sensor subjects in your EarthRanger system
   - You'll need to know the exact name of the subject group containing your weather stations
   - You can find them at https://<your-site>.pamdas.org/admin/observations/subjectgroup/

## Installation

1. Open Ecoscope Desktop
2. Select "Workflow Templates"
3. Click "+ Add Template"
4. Copy and paste this URL https://github.com/wildlife-dynamics/wt-climate-monitoring and wait for the workflow template to be downloaded and initialized
5. The template will now appear in your available template list

## Configuration Guide

Once you've added the workflow template, you'll need to configure it for your specific needs. The configuration form is organized into several sections.

### Basic Configuration

These are the essential settings you'll need to configure for every workflow run:

#### 1. Workflow Details
Give your workflow a name and description to help you identify it later.

- **Workflow Name** (required): A descriptive name for this workflow run and the dashboard title
  - Example: `"Climate Workflow"`
- **Workflow Description** (optional): Additional details about this analysis
  - Example: `"A workflow for climate monitoring."`

#### 2. Time Range
Specify the time period for the weather data you want to download.

- **Timezone** (required): Use the dropdown to select your timezone
- **Since** (required): Use the calendar picker to select the start date and time
  - Example: `12/20/2025, 12:00 AM`
- **Until** (required): Use the calendar picker to select the end date and time
  - Example: `12/25/2025, 11:59 PM`

#### 3. Data Source
Select your EarthRanger connection.

- **Data Source** (required): Choose from your configured data sources
  - Example: Select `gmmf` from the dropdown

#### 4. Subject Group
Choose which subject group contains your TAMHO sensors.

- **Subject Group Name** (required): Enter the exact name of the subject group as it appears in EarthRanger. You can find them at https://<your-site>.pamdas.org/admin/observations/subjectgroup/
  - Example: `"Subjects"`
  - Note: Using a group with mixed subtypes could lead to unexpected results

#### 5. Select Weather Stations (Optional)
Filter which weather stations to include in your analysis.

- **Weather Station** (optional): Select specific weather stations by their subject names
  - Leave empty to include all weather stations in the subject group
  - Example: Select `["TA00569- Mara Triangle", "TA00570- Naboisho Conservancy", "TA00825- Mara North Conservancy"]`

#### 6. Group Data (Optional)
Organize your data into separate views based on time periods, weather stations, or spatial regions.

- **Group by**: Create separate outputs grouped by:
  - Time: Year, Month, Date, Day of week, Hour, etc.
  - Category: Weather Station - creates separate views for each weather station
  - Spatial Regions: Group by geographic regions (if configured)
  - You can select multiple groupers to create nested groupings (e.g., group by weather station AND month)

#### 7. Persist Observations
Choose how to save your raw observation data with normalized observation details

- **Filetypes**: Select one or more output formats
  - **CSV**: Standard spreadsheet format, opens in Excel
  - **GeoParquet**: Efficient format for geospatial data
  - **GPKG**: GeoPackage format, opens in GIS software like QGIS
  - Example: Select `CSV` (default)

#### 8. Persist Daily Summary
The workflow automatically creates a daily summary CSV file with aggregated precipitation and temperature data.

- This file is always created and includes:
  - Total daily precipitation by weather station
  - Average daily temperature by weather station

#### 9. Create Climate Report
Generate a downloadable Word document (.docx) containing your climate analysis.

- **Template Path** (required): Path or URL to a Word template file with Jinja2 placeholders
  - You can use the default template or provide your own customized template
  - Supports both local file paths and remote URLs (http://, https://)
- The report includes:
  - Report title and date range
  - Temperature chart visualization
  - Precipitation chart visualization
  - Summary data table

### Advanced Configuration

These optional settings provide additional control over your workflow:

#### Filename Prefixes
Customize the names of your output files.

- **Persist Observations - Filename Prefix**: Custom prefix for raw observation files
  - Default: `"observations"`
  - Example: `"weather_observations"` will create files like `weather_observations_abc123.csv`

- **Persist Daily Summary - Filename Prefix**: Custom prefix for daily summary files
  - Default: `"daily_summary"`
  - Example: `"climate_summary"` will create files like `climate_summary_abc123.csv`

## Running the Workflow

Once you've configured all the settings:

1. **Review your configuration**
   - Double-check your time range, data source, and subject group name

2. **Save and run**
   - Click the "Submit" and the workflow will show up in "My Workflows" table button in Ecoscope Desktop
   - Click on "Run" and the workflow will begin processing

3. **Monitor progress and wait for completion**
   - You'll see status updates as the workflow runs
   - Processing time depends on:
     - The size of your date range
     - Number of weather stations
     - Number of observations in the system
   - The workflow completes with status "Success" or "Failed"

## Understanding Your Results

After the workflow completes successfully, you'll find your outputs in the designated output folder.

### Data Outputs

Your climate data will be saved in the format(s) you selected:

#### Raw Observations Data

- **File formats**: CSV, GeoParquet, and/or GPKG (based on your selection)
- **Opens in**: Microsoft Excel, Google Sheets (CSV), Python/R (GeoParquet), QGIS/ArcGIS (GPKG)
- **Best for**:
  - CSV: Quick data review and analysis
  - GeoParquet: Large datasets, programmatic analysis
  - GPKG: Spatial analysis in GIS software
- **Contents**: All weather observation data with normalized observation details including:
  - `weather_station`: Name of the weather station (subject name)
  - `recorded_at`: Timestamp when the observation was recorded
  - `date`: Date extracted from recorded_at
  - `precipitation`: Precipitation measurement (mm)
  - `surface_air_temperature`: Temperature measurement (Celsius)
  - `geometry`: Geographic point location
  - Additional columns from observation details

#### Daily Summary Data

- **File format**: CSV (always created)
- **Filename**: `daily_summary_*.csv` or custom prefix you specified
- **Opens in**: Microsoft Excel, Google Sheets, or any spreadsheet application
- **Contents**: Aggregated daily data with these columns:
  - `weather_station`: Name of the weather station
  - `date`: Date of the observations
  - `daily_precipitation`: Total precipitation for that day (mm)
  - `daily_temperature`: Average temperature for that day (Celsius)

### Visual Outputs (Dashboard)

The workflow creates an interactive dashboard with two main visualizations:

#### Daily Precipitation Chart
- **Format**: Interactive line chart
- **Features**:
  - X-axis: Date
  - Y-axis: Precipitation (mm)
  - Multiple lines: One for each weather station (if multiple stations selected)
  - Interactive hover: Shows exact values when you mouse over data points
  - Step-line style: Shows precipitation as steps (hvh shape)
  - Legend: Identifies each weather station by color

#### Daily Temperature Chart
- **Format**: Interactive line chart
- **Features**:
  - X-axis: Date
  - Y-axis: Average Daily Temperature (Celsius)
  - Multiple lines: One for each weather station (if multiple stations selected)
  - Interactive hover: Shows exact values when you mouse over data points
  - Smooth spline: Shows temperature trends as smooth curves
  - Legend: Identifies each weather station by color

### Climate Report Output

The workflow generates a downloadable Word document (.docx) containing:
- **Report title and date range**: Summary of the analysis period
- **Temperature chart**: Visual representation of temperature trends
- **Precipitation chart**: Visual representation of precipitation data
- **Summary table**: Aggregated daily climate data

If grouping is configured, separate reports are generated for each group.

### Grouped Outputs

If you configured data grouping:
- You'll receive separate files for each group (e.g., one per weather station or time period)
- Dashboard visualizations will have multiple views, with each group selectable from the dashboard
- Separate climate reports are generated for each group

## Common Use Cases & Examples

Here are some typical scenarios and how to configure the workflow for each:

### Example 1: Simple Climate Monitoring for All Stations
**Goal**: Download climate data for all weather stations in a subject group

**Configuration**:
- **Time Range**:
  - Since: `2025-12-20T00:00:00`
  - Until: `2025-12-25T23:59:59`
  - Timezone: `UTC (UTC+00:00)`
- **Subject Group Name**: `"Subjects"`
- **Select Weather Stations**: Leave empty (include all stations)
- **Filetypes**: Select `CSV`

**Result**:
- CSV file with all raw observations from all weather stations
- Daily summary CSV with aggregated precipitation and temperature
- Interactive dashboard with precipitation and temperature charts showing all stations
- Climate report (Word document) with charts and summary

---

### Example 2: Filtered by Specific Weather Stations
**Goal**: Monitor only selected weather stations

**Configuration**:
- **Time Range**:
  - Since: `2025-12-20T00:00:00`
  - Until: `2025-12-25T23:59:59`
  - Timezone: `UTC (UTC+00:00)`
- **Subject Group Name**: `"Subjects"`
- **Select Weather Stations**: `["TA00569- Mara Triangle", "TA00570- Naboisho Conservancy", "TA00825- Mara North Conservancy"]`
- **Filetypes**: Select `CSV` and `GeoParquet`

**Result**:
- CSV and GeoParquet files with observations from only the three selected stations
- Daily summary CSV for those stations
- Dashboard charts comparing the three stations
- Climate report with charts and summary for selected stations

---

### Example 3: Grouped by Weather Station
**Goal**: Create separate views and files for each weather station

**Configuration**:
- **Time Range**:
  - Since: `2025-12-20T00:00:00`
  - Until: `2025-12-25T23:59:59`
- **Subject Group Name**: `"Subjects"`
- **Select Weather Stations**: `["TA00569- Mara Triangle", "TA00570- Naboisho Conservancy", "TA00825- Mara North Conservancy"]`
- **Group Data**:
  - Select `weather_station`
- **Filetypes**: Select `CSV`

**Result**:
- Separate CSV files for each weather station (3 observation files, 3 daily summary files)
- Dashboard with multiple views - one for each station, selectable from dropdown

---

### Example 4: Multiple Groupers (Weather Station by Month)
**Goal**: Analyze climate data grouped by both weather station AND month

**Configuration**:
- **Time Range**:
  - Since: `2025-12-25T00:00:00`
  - Until: `2026-01-05T23:59:59`
  - Timezone: `Africa/Nairobi (UTC+03:00)`
- **Subject Group Name**: `"Subjects"`
- **Group Data**:
  - Select `weather_station`
  - Also select `"%B"` (Month name: January, February, etc.)
- **Filetypes**: Select `CSV`

**Result**:
- Separate output files for each station-month combination
- Dashboard with nested views for each station and month
- Separate climate reports for each group combination

## Troubleshooting

### Common Issues and Solutions

#### Workflow fails to start
**Problem**: The workflow won't run or immediately fails

**Solutions**:
- Verify your EarthRanger data source is properly configured
- Check that you have network connectivity to the EarthRanger server
- Ensure your credentials haven't expired
- Confirm the data source name matches exactly

#### No observations returned
**Problem**: Workflow completes but produces empty results

**Solutions**:
- Verify the date range is correct (start date should be before end date)
- Check that the subject group name is spelled exactly as it appears in EarthRanger
- Visit `https://<your-site>.pamdas.org/admin/observations/subjectgroup/` to confirm subject group names
- Verify the subject group contains TAMHO sensor subjects with observations during the specified time range
- Try a broader date range to verify observations exist
- Check if the selected weather stations have any data in the time period

#### Workflow runs very slowly
**Problem**: The workflow takes an extremely long time to complete

**Solutions**:
- Reduce the date range to smaller time periods
- Process data in smaller batches (by week or month instead of year)
- Consider using only CSV format instead of multiple formats
- Limit the number of weather stations if you have many in the subject group
- The first run may take longer as the environment gets warmed up. The following ones should be faster.

#### Authentication errors
**Problem**: Errors related to login or permissions

**Solutions**:
- Re-configure your EarthRanger data source in Ecoscope Desktop
- Verify your user account has permission to access subject data in EarthRanger
- Check that your account has permission to view the specific subject group

#### Charts are empty or not showing data
**Problem**: Dashboard displays but charts are blank

**Solutions**:
- Verify your observations have valid data in the precipitation and surface_air_temperature fields
- Check that the daily summary was created successfully
- Ensure the time range contains enough data points for meaningful visualization
- Check if observations were filtered out completely

#### Missing precipitation or temperature data
**Problem**: Some values are missing in the output

**Solutions**:
- Verify the TAMHO sensors are recording both precipitation and temperature measurements
- Check the observation_details field in the raw data to see which measurements are present
- Some sensors may only provide partial data - this is normal and will show as empty/null values
- The daily summary will calculate averages only from available data points