# Predicting Ectomycorrhizal (ECM) Percentage Using Remote Sensing and Machine Learning

## Project Objective

The goal of this project is to predict the **percentage of ectomycorrhizal (ECM) trees per NEON plot** using remotely sensed imagery and machine learning models.

Two NEON sites are used:

- **HARV** – Harvard Forest
- **BART** – Bartlett Experimental Forest

Model performance is evaluated using:

- **R² (Coefficient of Determination)** — Higher is better
- **MAE (Mean Absolute Error)** — Lower is better

---

# Research Timeline

| Method   | Description                                        | Status    |
| -------- | -------------------------------------------------- | --------- |
| Method 1 | Sentinel-2 baseline models                         | Completed |
| Method 2 | Combined HARV + BART Sentinel-2 model              | Completed |
| Method 3 | Replace Sentinel-2 with NEON hyperspectral imagery | Completed |
| Method 4 | Expand field datasets (2015–2023)                  | Completed |
| Method 5 | Single cloud-free Sentinel-2 scene                 | Completed |

---

# Method 1 — Sentinel-2 Baseline Models

## Objective

Train machine learning models using Sentinel-2 spectral features.

---

## HARV → HARV

| Model             |        R² |       MAE |
| ----------------- | --------: | --------: |
| Linear Regression |     0.154 |     14.88 |
| Random Forest     |     0.186 |     14.66 |
| XGBoost           |    -0.251 |     17.64 |
| SVR               |    -0.094 |     14.94 |
| HistGradientBoost | **0.316** | **14.62** |
| CNN               |    -0.502 |     23.07 |

**Best model:** HistGradientBoost

---

## HARV → BART

| Model             |        R² |       MAE |
| ----------------- | --------: | --------: |
| Linear Regression |     0.019 |     16.52 |
| Random Forest     | **0.219** | **15.79** |
| XGBoost           |    -0.094 |     18.80 |
| SVR               |    -0.083 |     18.63 |
| HistGradientBoost |    -0.270 |     20.22 |
| CNN               |    -0.111 |     18.81 |

**Best model:** Random Forest

---

## BART → BART

| Model             |        R² |       MAE |
| ----------------- | --------: | --------: |
| Linear Regression |    -1.158 |     25.76 |
| Random Forest     |     0.085 |     14.79 |
| XGBoost           |     0.132 | **14.37** |
| SVR               |    -0.133 |     16.31 |
| HistGradientBoost |    -0.216 |     17.72 |
| CNN               | **0.137** |     15.13 |

**Best R²:** CNN

**Best MAE:** XGBoost

---

## BART → HARV

| Model             |        R² |       MAE |
| ----------------- | --------: | --------: |
| Linear Regression |    -3.427 |     30.88 |
| Random Forest     | **0.188** | **15.72** |
| XGBoost           |    -0.106 |     17.94 |
| SVR               |    -0.036 |     18.37 |
| HistGradientBoost |     0.056 |     17.46 |
| CNN               |     0.079 |     16.80 |

**Best model:** Random Forest

---

## Summary

### Observations

- Within-site prediction was modest.
- Cross-site prediction remained poor.
- CNN did not outperform traditional ML models.
- Random Forest was the most consistent model.

---

# Method 2 — Combined HARV + BART Sentinel-2 Model

## Objective

Train a single model using both HARV and BART data.

| Model             |     R² |   MAE |
| ----------------- | -----: | ----: |
| Linear Regression | -1.026 | 23.51 |
| Random Forest     | -0.366 | 19.81 |
| XGBoost           | -0.395 | 19.12 |

### Summary

Combining both sites reduced overall model performance.

---

# Method 3 — Replace Sentinel-2 with NEON Hyperspectral Data

## Objective

Evaluate whether higher spectral resolution improves ECM prediction.

---

## HARV → HARV

| Model             |        R² |       MAE |
| ----------------- | --------: | --------: |
| Linear Regression |    -1.676 |     31.37 |
| Random Forest     |     0.478 |     11.59 |
| XGBoost           | **0.658** | **10.06** |

---

## HARV → BART

| Model         |     R² |   MAE |
| ------------- | -----: | ----: |
| Random Forest | -0.288 | 16.69 |

**Note:** Linear Regression and XGBoost currently fail because of null values.

---

## BART → BART

| Model             |        R² |      MAE |
| ----------------- | --------: | -------: |
| Linear Regression | **0.699** | **8.82** |
| Random Forest     |     0.151 |    14.64 |
| XGBoost           |     0.261 |    13.96 |

---

## BART → HARV

| Model             |        R² |       MAE |
| ----------------- | --------: | --------: |
| Linear Regression |    -8.242 |     55.69 |
| Random Forest     | **0.170** | **14.62** |
| XGBoost           |     0.058 |     15.19 |

---

## HARV + BART → HARV + BART

| Model             |        R² |       MAE |
| ----------------- | --------: | --------: |
| Linear Regression |     0.130 |     15.28 |
| Random Forest     |     0.061 |     16.09 |
| XGBoost           | **0.184** | **14.70** |

---

## Summary

### Improvements

- Hyperspectral imagery significantly outperformed Sentinel-2.
- HARV Random Forest improved from **0.186 → 0.478**.
- HARV XGBoost improved from **negative R² → 0.658**.

### Remaining Issues

- Cross-site prediction remains difficult.
- Null values prevent some models from running.

---

# Method 4 — Updated Field Dataset (2015–2023)

## Objective

Increase the number of individual trees contributing to each plot.

---

## Dataset Summary

| Dataset                        | Number of Plots |
| ------------------------------ | --------------: |
| Updated HARV (Nov–Oct filter)  |              35 |
| Updated HARV (No month filter) |              40 |
| Updated BART                   |              41 |

---

# A. Hyperspectral Models

## Updated HARV Dataset

### With Nov–Dec Filter

| Model             |        R² |       MAE |
| ----------------- | --------: | --------: |
| Linear Regression |    -1.677 |     31.54 |
| Random Forest     |     0.477 |     11.53 |
| XGBoost           | **0.598** | **10.67** |

---

### Without Nov–Dec Filter

| Model             |        R² |   MAE |
| ----------------- | --------: | ----: |
| Linear Regression | **0.125** | 15.63 |
| Random Forest     |     0.059 | 14.98 |
| XGBoost           |    -0.178 | 17.10 |

---

## Updated BART Dataset

### With Nov–Dec Filter

| Model             |        R² |      MAE |
| ----------------- | --------: | -------: |
| Linear Regression | **0.691** | **8.39** |
| Random Forest     |     0.167 |    14.70 |
| XGBoost           |     0.244 |    14.19 |

---

### Without Nov–Dec Filter

Results are identical because BART contains no observations after October.

---

# B. Sentinel-2 Models

## Updated HARV Dataset

### With Nov–Dec Filter

| Model             |        R² |       MAE |
| ----------------- | --------: | --------: |
| Linear Regression |    -3.123 |     37.02 |
| Random Forest     | **0.051** | **15.65** |
| XGBoost           |    -0.658 |     20.51 |

---

### Without Nov–Dec Filter

| Model             |        R² |       MAE |
| ----------------- | --------: | --------: |
| Linear Regression |    -0.555 |     19.75 |
| Random Forest     | **0.201** | **15.60** |
| XGBoost           |    -0.404 |     17.19 |

---

## Updated BART Dataset

### With Nov–Dec Filter

| Model             |     R² |       MAE |
| ----------------- | -----: | --------: |
| Linear Regression | -0.295 |     16.55 |
| Random Forest     | -0.326 |     13.98 |
| XGBoost           | -0.200 | **13.20** |

---

### Without Nov–Dec Filter

Results are identical because BART contains no November or December observations.

---

## Summary

Increasing the number of field observations did not consistently improve prediction accuracy.

---

# Method 5 — Single Cloud-Free Sentinel-2 Scene

## Objective

Determine whether cloud contamination in Sentinel-2 composites reduced model performance.

---

## HARV → HARV (June 26, 2022)

| Model             |        R² |       MAE |
| ----------------- | --------: | --------: |
| Linear Regression |    -1.190 |     20.96 |
| Random Forest     | **0.223** | **14.39** |
| XGBoost           |     0.093 |     14.98 |
| SVR               |    -0.113 |     16.12 |
| HistGradientBoost |     0.140 |     15.53 |

---

## BART → BART (June 6, 2022)

| Model             |        R² |       MAE |
| ----------------- | --------: | --------: |
| Linear Regression |    -1.058 |     19.11 |
| Random Forest     | **0.107** | **12.72** |
| XGBoost           |    -0.331 |     15.83 |
| SVR               |    -0.265 |     17.78 |
| HistGradientBoost |    -0.050 |     14.58 |

---

## Summary

Using a single cloud-free Sentinel-2 image slightly improved Random Forest performance but still did not approach the performance achieved with hyperspectral imagery.

---

# Overall Findings

## Major Results

- Random Forest was the most consistent model across experiments.
- XGBoost performed best when using hyperspectral imagery.
- Sentinel-2 consistently underperformed compared to hyperspectral imagery.
- Cross-site generalization (HARV ↔ BART) remains the largest challenge.
- Expanding the field dataset did not consistently improve performance.
- Using cloud-free Sentinel-2 scenes provided only modest improvements.

---

# Next Steps

- Fix null-value handling for HARV → BART hyperspectral models.
- Continue tuning Random Forest and XGBoost hyperparameters.
- Investigate domain adaptation or normalization techniques to improve cross-site transfer.
- Apply the best-performing hyperspectral model to predict ECM across the target watershed.

---

# Best Results Summary

| Experiment                 | Best Model        |   Best R² |
| -------------------------- | ----------------- | --------: |
| Sentinel-2 HARV → HARV     | HistGradientBoost |     0.316 |
| Sentinel-2 HARV → BART     | Random Forest     |     0.219 |
| Sentinel-2 BART → BART     | CNN               |     0.137 |
| Sentinel-2 BART → HARV     | Random Forest     |     0.188 |
| Hyperspectral HARV → HARV  | XGBoost           | **0.658** |
| Hyperspectral HARV → BART  | Random Forest     |    -0.288 |
| Hyperspectral BART → BART  | Linear Regression | **0.699** |
| Hyperspectral BART → HARV  | Random Forest     |     0.170 |
| Combined Hyperspectral     | XGBoost           |     0.184 |
| Single-Scene Sentinel HARV | Random Forest     |     0.223 |
| Single-Scene Sentinel BART | Random Forest     |     0.107 |
