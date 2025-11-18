# mlfs-book
O'Reilly book - Building Machine Learning Systems with a feature store: batch, real-time, and LLMs


# Air Quality Monitoring System

This project extends the mlfs-book air quality tutorial to support **multiple sensors** with automated daily pipelines and a multi-sensor dashboard.

## What Was Added

### Multi-Sensor Support
- **Parameterized notebooks**: All notebooks read sensor configuration from environment variables (`.env` file or GitHub Actions)
- **Sensor-specific models**: One XGBoost model per sensor for improved accuracy
- **Matrix strategy**: GitHub Actions workflow runs in parallel for all configured sensors

### Feature Engineering
- **Rolling 3-day mean feature**: Added `pm25_rolling_3d_mean` feature that calculates the mean of PM2.5 from previous 1-3 days, enabling autoregressive multi-day forecasting

### Current Setup: 5 Sensors in Bergen, Norway
- klosterhaugen
- danmarksplass
- loddefjord
- radal
- asane

### Automated Pipeline
- **GitHub Actions workflow** (`.github/workflows/air-quality-daily.yml`):
  - Scheduled daily runs at 6:11 AM UTC
  - Processes all sensors in parallel
  - Automatically publishes updated dashboard to GitHub Pages

### Multi-Sensor Dashboard
- **Live dashboard**: [https://datskiw.github.io/mlfs-book/air-quality/](https://datskiw.github.io/mlfs-book/air-quality/)
- Displays forecast and hindcast charts for all sensors
- Automatically updates when workflow runs

## Configuration

Sensor configuration via environment variables:
- `AQICN_COUNTRY`, `AQICN_CITY`, `AQICN_STREET`: Sensor location
- `AQICN_URL`: API endpoint for sensor
- `CSV_FILE`: Historical data file path
- `HOPSWORKS_API_KEY`, `AQICN_API_KEY`: API keys

## Setup

Create a conda or virtual environment for your project before you install the requirements:
```bash
pip install -r requirements.txt
```

## Run Pipelines

### Manual Execution
Run notebooks in order:
1. `notebooks/airquality/1_air_quality_feature_backfill.ipynb` - Feature backfill (one-time per sensor)
2. `notebooks/airquality/2_air_quality_feature_pipeline.ipynb` - Daily feature updates
3. `notebooks/airquality/3_air_quality_training_pipeline.ipynb` - Model training (one-time per sensor)
4. `notebooks/airquality/4_air_quality_batch_inference.ipynb` - Batch inference

### Using Make Commands
```bash
make aq-backfill
make aq-features
make aq-train
make aq-inference
make aq-clean
```

Or run all:
```bash
make aq-all
```

