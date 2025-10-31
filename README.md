# Temperature Prediction with Apache Spark

## Project Overview
This project implements a large-scale temperature prediction system using Apache Spark and meteorological data from global weather stations. The system processes massive datasets from Google Cloud Storage and builds machine learning models to predict temperature based on various weather features.

## Features
- **Large-scale Data Processing**: Handles global meteorological data using Spark distributed computing
- **End-to-End ML Pipeline**: Complete workflow from data preparation to model evaluation
- **Multiple Algorithms**: Implements and compares Linear Regression and Random Forest models
- **Cloud Integration**: Designed to run on Google Cloud Dataproc clusters

## Dataset
- **Source**: Meteorological observation data from GCS bucket
- **Size**: Over 120 million records total (85M training, 36M testing)
- **Location**: `gs://dsa5208--project--2--data/csv_data/*.csv`

## Technical Stack
- Apache Spark 3.x
- PySpark MLlib
- Google Cloud Storage
- Distributed Computing on Dataproc

## Project Structure

### 1. Data Preparation
- Reads CSV files from GCS
- Supports both CLOUD and LOCAL modes
- Data caching for performance optimization

### 2. Data Cleaning
- Parses complex meteorological string formats
- Handles missing values and quality codes
- Processes wind direction, speed, and type
- Cleans temperature, dew point, and pressure data

### 3. Feature Engineering
- Temporal features: hour, month, cyclic time encoding
- Geographical features: latitude, elevation
- Meteorological features: temperature-dew point spread
- Periodic transformations: sine/cosine of hour

### 4. Quality Filtering
- Applies quality control filters
- Reasonable value range validation
- Missing value imputation

### 5. Feature Selection
Final 8 selected features:
- `dew_final` (Dew point temperature)
- `hour_sin` (Sine of hour)
- `hour_cos` (Cosine of hour) 
- `month` (Month of year)
- `LATITUDE` (Geographical latitude)
- `ELEVATION` (Altitude)
- `wind_speed_final` (Wind speed)
- Target: `TMP_value` (Temperature)

### 6. Model Training
- **Linear Regression** with cross-validation
- **Random Forest** with cross-validation
- Hyperparameter tuning
- 70/30 train-test split

## Model Performance

### Linear Regression
- **Test RMSE**: 5.4948°C
- **Test R²**: 0.7926
- **Test MAE**: 3.7965°C
- **Training Time**: 1339.18 seconds

### Random Forest (Best Model)
- **Test RMSE**: 3.7826°C
- **Test R²**: 0.9017
- **Test MAE**: 2.7040°C

## Feature Importance (Random Forest)
1. Dew point: 62.43%
2. Latitude: 21.20%
3. Month: 11.88%
4. Elevation: 1.82%
5. Wind speed: 1.35%
6. Hour (sine): 0.92%
7. Hour (cosine): 0.41%

## Installation & Setup

### Prerequisites
- Apache Spark 3.x
- Python 3.x
- PySpark
- Google Cloud Account (for CLOUD mode)

### Running the Project
1. Set `RUN_MODE = "CLOUD"` or `"LOCAL"`
2. Configure GCS paths for CLOUD mode
3. Execute cells in the Jupyter notebook sequentially
4. Monitor training progress and evaluate results

## Key Configuration
```python
RUN_MODE = "CLOUD"  # or "LOCAL"
GCS_PATH = "gs://dsa5208--project--2--data/csv_data/*.csv"
