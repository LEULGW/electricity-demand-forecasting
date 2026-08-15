# Predicting U.S. Regional Electricity Demand Using Weather Data
Forecasted hourly electricity demand across 8 major U.S. grid regions using historical demand, weather, and calendar data, applying and comparing four machine learning approaches: Linear Regression, Random Forest, XGBoost, and LSTM, all within AI4ALL's Ignite accelerator.

## Problem Statement 
Grid operators need accurate demand forecasts to balance electricity supply and maintain reliability. Underestimating demand risks shortages and blackouts, while overestimating it wastes generation capacity and drives up emissions and cost. As grids increasingly integrate variable renewable sources like solar and wind, reliable forecasting becomes even more critical to balancing supply in real time.

## Key Results 
1. Collected and processed ~70,000 hourly observations across 8 U.S. grid regions over one year
2. Built and compared four forecasting models: Linear Regression, Random Forest, XGBoost, and LSTM
3. XGBoost delivered the strongest performance, reaching 3.70% MAPE and R² = 0.995 in the deployed model
4. Found that forecast accuracy degrades gradually as the horizon lengthens
   - 4.30% MAPE at 1 day out
   - 6.90% MAPE at 1 week out
   - 7.48% MAPE at 1 month out
5. Identified regional bias in forecast reliability
   - The model is measurably less reliable for some regions (MISO, CISO, BPAT at 1 month) than others (ERCO)
   - A single pooled accuracy score can mask meaningfully worse forecasts for specific operators
6. Deployed a self-contained, interactive demo app for exploring typical demand scenarios by region, season, and temperature

## Methodologies
We collected hourly electricity demand, solar generation, and wind generation data from the EIA Grid Monitor API across 8 U.S. grid regions, and paired it with hourly weather data (temperature, apparent temperature, humidity, precipitation, wind speed) from the Open-Meteo Historical Weather API, using a representative city per region. After merging energy and weather data by region and timestamp, we engineered cyclical time features (sine/cosine transforms for hour, day, and month) along with lag and rolling-average demand features to capture recurring patterns.

We trained and evaluated four models. Linear Regression served as a fast, interpretable baseline but struggled with the nonlinear structure of demand. Random Forest and XGBoost used multi-horizon direct forecasting, with three separate models per method, each trained to predict 1 day, 1 week, or 1 month ahead, using lag (24h/168h) and rolling mean/std demand features, and were evaluated on a held-out 45-day test window with MAE, RMSE, MAPE, and R² computed overall and per region. LSTM used 24-hour sequences of prior demand alongside weather and time features to model temporal dependencies directly, trained and evaluated under a time-based split rather than the multi-horizon setup.

For deployment, we retrained simplified models on user-provided inputs only, including region, hour, day of week, month, weekend flag, and apparent temperature, with no lag features and no specific date or year, so the app produces a "typical" demand scenario rather than a live forecast. The deployed XGBoost model achieved 1,362 MWh MAE, 3.70% MAPE, and R² = 0.995; the deployed Random Forest model achieved 1,941 MWh MAE, 6.12% MAPE, and R² = 0.992.

## Data Sources 
- EIA Grid Monitor: [Link to EIA Grid Monitor](https://www.eia.gov/electricity/gridmonitor/)
- Open-Meteo Historical Weather API: [Link to Open-Meteo](https://open-meteo.com/en/docs/historical-weather-api)

## Technologies Used 
- Python
- pandas
- scikit-learn
- XGBoost
- TensorFlow / Keras (LSTM)
- Streamlit
- EIA Grid Monitor API
- Open-Meteo Historical Weather API

## Authors 
This project was completed in collaboration with:
- Diego Lozano ([diegolozano@tamu.edu](mailto:diegolozano@tamu.edu))
- Irene Gallini ([igallini@macalester.edu](mailto:igallini@macalester.edu))
- Leul Wolderufael ([Leul.Wolderufael@bison.howard.edu](mailto:Leul.Wolderufael@bison.howard.edu))
- Sujjal Chapagain ([Sujjal.Chapagain@usm.edu](mailto:Sujjal.Chapagain@usm.edu))
