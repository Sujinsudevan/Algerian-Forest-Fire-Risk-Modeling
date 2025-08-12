# Algerian Forest Fires – Regression Analysis on FWI

## Project Overview
This project predicts the **Fire Weather Index (FWI)** — a continuous measure of fire risk — from daily meteorological and fire-weather variables for two regions in Algeria.

**Objective:**  
Build and evaluate regression models to estimate **FWI** given meteorological and fire index data, while dealing with strong feature correlations.

**Dataset:**  
- Year: 2012  
- Regions: **Bejaia** (coastal) and **Sidi-Bel Abbes** (inland)  
- Source: Algerian Forest Fires Dataset (UCI Repository)  
- Features include:  
  - Temperature, Relative Humidity (RH), Wind Speed (Ws), Rainfall (Rain)  
  - Fire-weather indices: FFMC, DMC, DC, ISI, BUI  
  - Region code (0 or 1)  

Target variable (**y**): `FWI` (Fire Weather Index)

---

## Data Cleaning & Preparation
Steps performed:
1. Removed repeated header rows from the merged dataset.
2. Dropped extra text rows and NaN entries.
3. Added `Region` column → **0** = Bejaia, **1** = Sidi-Bel Abbes.
4. Converted `Classes` to numeric (not used as `y` in this regression task).
5. Ensured all features were numeric.
6. Verified no duplicate rows remained.

**Final dataset shape**: `243 rows × 13 columns`

---

## Exploratory Data Analysis – Key Insights
- Strong positive correlation with `FWI`: `ISI`, `BUI`, `FFMC`, and `Temperature`
- Negative correlation: `RH` (humidity) and `Rain` — higher moisture reduces fire risk
- High multicollinearity among predictors → suitable for regularized regression models
- Inland region exhibits slightly higher extreme FWI values

---

## Model Building

### Why Regularized Linear Models?
- Strong predictor correlations → plain Linear Regression can overfit
- **Lasso (L1)** → feature selection
- **Ridge (L2)** → keeps all variables, shrinks coefficients
- **Elastic Net (L1 + L2)** → balances both approaches

### Models Used:
1. Linear Regression
2. Lasso Regression (`Lasso` & `LassoCV`)
3. Ridge Regression (`Ridge` & `RidgeCV`)
4. Elastic Net (`ElasticNet` & `ElasticNetCV`)

---

## Model Performance

**Target:** `FWI`  
**Metrics:** MSE (lower is better), R² (closer to 1 is better)

| Model                      | MSE       | R²       | Notes |
|----------------------------|-----------|----------|-------|
| Linear Regression          | 0.5460    | 0.9847   | High accuracy; no regularization |
| Lasso Regression           | 1.1332    | 0.9492   | Removed some useful features |
| LassoCV (α=0.572)           | 0.6190    | 0.9820   | Better tuning improved accuracy |
| Ridge Regression           | 0.5642    | 0.9842   | Stable results with collinear features |
| **RidgeCV**                | **0.5042**| 0.9842   | **Best overall performance** |
| Elastic Net                | 1.8822    | 0.8753   | Over-penalized → weak performance |
| ElasticNetCV (α=0.0043)    | 0.6570    | 0.9814   | Improved but still below Ridge |

---

## Analysis & Key Takeaways
- **Best Model:** `RidgeCV` → Lowest MSE (0.5042), highest stability
- **Lasso** reduced some predictive power by dropping relevant variables
- **Elastic Net** needed very low penalties to be competitive
- **Linear Regression** fit well but may be unstable on unseen data due to multicollinearity

---

## Conclusion
- Objective achieved: Accurate regression model for predicting **FWI** from weather data
- Data cleaning was crucial due to merged multi-region data inconsistencies
- Multicollinearity made **Ridge Regression with CV tuning** the best choice
- Key influencing features:
  - Increase FWI: `ISI`, `FFMC`, `BUI`, `Temperature`  
  - Decrease FWI: `RH`, `Rain`

**Final Recommendation:** Use **RidgeCV** for production-ready predictions of FWI.

