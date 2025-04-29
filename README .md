
# Zillow Property Price Prediction

## Objective
The goal of this project is to predict property prices using real estate features from the Zillow dataset. 
The project focuses on building a clean, machine learning-ready dataset through a structured pipeline 
and applying predictive models to accurately estimate property values.

## Data Source
The data was obtained from Kaggle:  
[Zillow Real Estate Data](https://www.kaggle.com/datasets/tonygordonjr/zillow-real-estate-data/data)

The dataset contains various CSV files related to property listings, price history, school proximity, taxes, 
and mortgages, all packaged in a `Zillow.zip` archive.

## Pipeline
The project follows a structured data processing pipeline:

**Main File:**  
- `Zillow.zip`

**Files Inside and Their Usage:**  
- `listing_property.csv` — Main property listing information  
- `subtype.csv` — Subtype classification of properties  
- `price_history.csv` — Historical price trends of properties  
- `price_nearbyhomes.csv` — Pricing details of nearby homes  
- `nearby_schools.csv` — Information about schools near properties  
- `tax.csv` — Tax history and information  
- `mortgage.csv` — Mortgage-related data  

**Steps:**
1. **Merge** — Loaded individual CSV files, merged Listings with Subtype on 'zpid', engineered time-based features (timeOnZillow_hours, days_since_lastUpdated, days_since_datePosted), merged in Tax and Mortgage data after cleaning and deduplication, optimized datatypes, dropped irrelevant columns, and integrated key historical pricing metrics (recent, average, median prices and change rates).

2. **Cleaning** — Dropped rows missing critical values (e.g., 'bedrooms', 'bathrooms'), imputed missing mortgage rates, consolidated rare mortgage categories, standardized formats across all features, and finalized a cleaner, more reliable DataFrame.

3. **Outlier Removal** — Created four price segments (Low: ≤$300K, Medium: $300K–$1M, High: $1M–$10M, Luxury: >$10M), visualized their distribution to ensure balance, and reserved the Luxury group for specialized downstream modeling and analysis.

```
## Libraries Used
- pandas — Data manipulation and analysis
- numpy — Numerical computations
- scikit-learn — Machine learning models and preprocessing
- matplotlib — Data visualization
- seaborn — Statistical data visualization

## 📊 Modeling Results

| Model                  | MAE       | MSE             | RMSE     | R²     |
|-------------------------|-----------|-----------------|----------|--------|
| LightGBM                | 55,965.24 | 54,448,101,294.07| 233,341.17 | 0.9411 |
| Random Forest           | 51,846.47 | 50,856,911,312.41| 225,514.77 | 0.945  |
| CatBoost                | 43,845.69 | 48,369,009,611.77| 219,929.56 | 0.9477 |
| XGBoost                 | 52,876.48 | 77,483,181,029.42| 278,358.01 | 0.9162 |
| ExtraTrees              | 43,882.08 | 28,865,383,195.56| 169,898.16 | 0.9688 |
| HistGradientBoosting    | 60,330.06 | 73,237,138,802.65| 270,623.61 | 0.9208 |
| GradientBoosting        | 66,972.37 | 56,245,023,559.34| 237,160.33 | 0.9391 |
| Stacking                | 48,968.63 | 42,459,828,417.25| 206,057.83 | 0.9541 |

## ⭐ Feature Importance Summary

Key important features across models:

- **hist_average_price**
- **hist_price_change_rate**
- **priceComp**
- **hist_median_price**
- **pricePerSquareFoot**
- **livingArea**

Other features such as `propertyValue`, `bathrooms`, `zipcode`, `yearBuilt`, `rate`, `pageViewCount`, and school-related features also contributed to the models' performance but with relatively lower importance.

## 🔁 Cross-Validation Results

| Model               | Mean RMSE | Std Dev (RMSE) | Mean R² | Std Dev (R²) | Mean MAE | Std Dev (MAE) |
|---------------------|-----------|---------------|---------|--------------|----------|---------------|
| Random Forest        | 311,102.04 | ±83,364.63    | 0.9193  | ±0.0220      | 73,297.61| ±7,235.35     |
| XGBoost              | 393,128.98 | ±91,355.31    | 0.8731  | ±0.0149      | 63,391.80| ±9,596.76     |
| LightGBM             | 362,145.18 | ±113,447.01   | 0.8929  | ±0.0319      | 74,901.58| ±11,261.09    |
| CatBoost             | 383,367.20 | ±105,310.31   | 0.8802  | ±0.0256      | 82,072.12| ±9,916.62     |
| ExtraTrees           | 274,643.73 | ±72,241.72    | 0.9378  | ±0.0131      | 97,201.52| ±7,515.99     |
| GradientBoosting     | 299,984.96 | ±85,373.54    | 0.9183  | ±0.0420      | 63,715.33| ±7,347.63     |
| HistGradientBoosting | 458,984.98 | ±153,381.36   | 0.8275  | ±0.0584      | 88,684.56| ±15,749.08    |
| StackingRegressor    | 5,038,531.28 | ±3,521,500.51 | -26.0337| ±21.1063     | 2,277,379.97| ±1,470,753.7 |

