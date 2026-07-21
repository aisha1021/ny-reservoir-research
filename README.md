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
| Method 6 | Expanded Sentinel-2 Features + Canopy Height Model | Completed |
| Method 7 | Hyperspectral Feature Selection                    | Completed |
| Method 8 | Hyperspectral Principal Component Analysis (PCA)   | Completed |
| Method 9 | Sentinel-2 + NEON Canopy Height Model              | Completed |
| Method 10 | Combined HARV + BART with Sentinel-2 + NEON CHM    | Completed |
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

## A. Hyperspectral Models

### Updated HARV Dataset

#### With Nov–Dec Filter

| Model             |        R² |       MAE |
| ----------------- | --------: | --------: |
| Linear Regression |    -1.677 |     31.54 |
| Random Forest     |     0.477 |     11.53 |
| XGBoost           | **0.598** | **10.67** |

---

#### Without Nov–Dec Filter

| Model             |        R² |   MAE |
| ----------------- | --------: | ----: |
| Linear Regression | **0.125** | 15.63 |
| Random Forest     |     0.059 | 14.98 |
| XGBoost           |    -0.178 | 17.10 |

---

### Updated BART Dataset

#### With Nov–Dec Filter

| Model             |        R² |      MAE |
| ----------------- | --------: | -------: |
| Linear Regression | **0.691** | **8.39** |
| Random Forest     |     0.167 |    14.70 |
| XGBoost           |     0.244 |    14.19 |

---

#### Without Nov–Dec Filter

Results are identical because BART contains no observations after October.

---

## B. Sentinel-2 Models

### Updated HARV Dataset

#### With Nov–Dec Filter

| Model             |        R² |       MAE |
| ----------------- | --------: | --------: |
| Linear Regression |    -3.123 |     37.02 |
| Random Forest     | **0.051** | **15.65** |
| XGBoost           |    -0.658 |     20.51 |

---

#### Without Nov–Dec Filter

| Model             |        R² |       MAE |
| ----------------- | --------: | --------: |
| Linear Regression |    -0.555 |     19.75 |
| Random Forest     | **0.201** | **15.60** |
| XGBoost           |    -0.404 |     17.19 |

---

### Updated BART Dataset

#### With Nov–Dec Filter

| Model             |     R² |       MAE |
| ----------------- | -----: | --------: |
| Linear Regression | -0.295 |     16.55 |
| Random Forest     | -0.326 |     13.98 |
| XGBoost           | -0.200 | **13.20** |

---

#### Without Nov–Dec Filter

Results are identical because BART contains no November or December observations.

---

### Summary

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
---

# Method 6 — Expanded Sentinel-2 Features + Canopy Height Model

## Objective

Improve Sentinel-2 ECM prediction by expanding the feature space while controlling multicollinearity.

Changes from previous Sentinel-2 baseline:

- Added back spectral standard deviation features.
- Added additional GLCM texture features.
- Removed highly correlated predictors (>0.70 correlation threshold).
- Added canopy height features:
  - canopy_height_mean
  - canopy_height_stdDev

Cross-validation was performed using 5-fold K-fold CV on training set.

---

## A. Expanded Features Without Canopy Height

### HARV → HARV

Feature set:
- 22 uncorrelated Sentinel-2 features

| Model | Test R² | Test MAE | CV Mean R² |
|---|---:|---:|---:|
| Linear Regression | -0.265 | 17.67 | -32579.24 |
| Random Forest | 0.194 | 15.38 | -0.032 |
| Tuned Random Forest | 0.179 | 15.18 | 0.026 |
| XGBoost | 0.082 | 18.21 | -0.087 |
| Tuned XGBoost | **0.275** | **14.80** | **0.103** |
| SVR | -0.009 | 14.35 | - |
| HistGradient Boosting | 0.120 | 15.76 | - |

#### Summary

Adding back standard deviation and texture features improved XGBoost performance compared to the previous Sentinel-2 baseline.

- XGBoost improved from negative/low R² values to R² = 0.275.
- Random Forest remained similar to previous experiments.
- Linear Regression remained unstable due to high dimensionality relative to sample size.

---

### BART → BART

Feature set:
- 21 uncorrelated Sentinel-2 features

| Model | Test R² | Test MAE | CV Mean R² |
|---|---:|---:|---:|
| Linear Regression | -1.280 | 25.47 | -9.663 |
| Random Forest | 0.189 | 15.06 | -0.192 |
| Tuned Random Forest | 0.179 | 15.03 | -0.012 |
| XGBoost | **0.271** | **14.44** | -0.190 |
| Tuned XGBoost | 0.116 | 15.66 | -0.038 |
| SVR | -0.149 | 16.68 | - |
| HistGradient Boosting | 0.096 | 15.95 | - |

#### Summary

BART models showed similar performance to previous Sentinel-2 experiments.

- XGBoost achieved the highest test R² (0.271).
- Cross-validation remained unstable, indicating limited generalization with the small sample size.

---

## B. Expanded Features Including Canopy Height

### HARV → HARV

Feature set:
- 25 uncorrelated features
- Added:
  - canopy_height_mean
  - canopy_height_stdDev

| Model | Test R² | Test MAE | CV Mean R² |
|---|---:|---:|---:|
| Linear Regression | -3.306 | 36.68 | -13.594 |
| Random Forest | 0.207 | 14.28 | -0.037 |
| Tuned Random Forest | 0.208 | 14.73 | 0.017 |
| XGBoost | 0.141 | 15.00 | -0.061 |
| Tuned XGBoost | **0.350** | **12.76** | 0.033 |
| SVR | -0.009 | 14.35 | - |
| HistGradient Boosting | 0.120 | 15.76 | - |

#### Summary

Adding canopy height improved HARV prediction performance.

Key improvements:

- Tuned XGBoost increased from:
  - R² = 0.275 → **0.350**
  - MAE = 14.80 → **12.76**

This suggests canopy structure provides additional ecological information not captured by Sentinel-2 spectral features alone.

---

### BART → BART

Feature set:
- 23 uncorrelated features
- Added:
  - canopy_height_mean
  - canopy_height_stdDev

| Model | Test R² | Test MAE | CV Mean R² |
|---|---:|---:|---:|
| Linear Regression | -1.538 | 27.16 | -42.209 |
| Random Forest | 0.201 | 14.77 | -0.178 |
| Tuned Random Forest | 0.139 | 14.84 | -0.032 |
| XGBoost | **0.433** | **12.43** | -0.223 |
| Tuned XGBoost | 0.305 | 13.88 | -0.062 |
| SVR | -0.149 | 16.68 | - |
| HistGradient Boosting | 0.195 | 14.61 | - |

#### Summary

Canopy height substantially improved BART XGBoost performance.

Compared with the expanded Sentinel-2-only model:

- XGBoost improved:
  - R² = 0.271 → **0.433**
  - MAE = 14.44 → **12.43**

This indicates forest structural information may be important for predicting ECM composition.

---

## Method 6 Overall Summary

| Dataset | Best Model | Best Test R² | MAE |
|---|---|---:|---:|
| HARV → HARV (expanded Sentinel-2) | Tuned XGBoost | 0.275 | 14.80 |
| HARV → HARV (+ canopy height) | Tuned XGBoost | **0.350** | **12.76** |
| BART → BART (expanded Sentinel-2) | XGBoost | 0.271 | 14.44 |
| BART → BART (+ canopy height) | XGBoost | **0.433** | **12.43** |

---

## Interpretation

Adding canopy height improved within-site ECM prediction, particularly for XGBoost models.

The improvement suggests that structural forest characteristics provide complementary information to Sentinel-2 spectral measurements.

However, cross-validation scores remained variable due to the limited number of field plots (~35–40 samples), indicating that model generalization remains challenging.

---

# Method 7 — Hyperspectral Feature Selection

## Objective

Evaluate whether removing redundant hyperspectral predictors improves ECM prediction.

### Feature Selection

* Removed zero-variance features
* Removed highly correlated features (|r| > 0.70)
* Added back standard deviation features
* Included canopy height statistics

---

## Feature Reduction

| Dataset | Original Features | After Variance Filter | After Correlation Filter |
| ------- | ----------------: | --------------------: | -----------------------: |
| HARV    |               854 |                   796 |                       10 |
| BART    |               854 |                   798 |                        6 |

---

## HARV → HARV

| Model               |   Test R² |       MAE | Mean CV R² |
| ------------------- | --------: | --------: | ---------: |
| Linear Regression   |    -1.806 |     27.81 |     -4.611 |
| Random Forest       |    -0.293 |     18.09 |     -0.101 |
| Tuned Random Forest |     0.008 |     15.40 |     -0.100 |
| XGBoost             | **0.049** | **13.60** |     -0.729 |
| Tuned XGBoost       |    -0.058 |     15.98 |     -0.100 |
| SVR                 |    -0.173 |     16.99 |          — |
| HistGradientBoost   |    -0.239 |     19.53 |          — |

### Summary

Although dimensionality was reduced from **854 to 10 predictors**, all models exhibited poor cross-validation performance. This indicates that aggressive correlation filtering discarded important spectral information.

---

## BART → BART

| Model               | Test R² |   MAE | Mean CV R² |
| ------------------- | ------: | ----: | ---------: |
| Linear Regression   |  -1.046 | 20.82 |     -0.489 |
| Random Forest       |  -0.876 | 21.10 |     -0.663 |
| Tuned Random Forest |  -0.268 | 17.20 |     -0.064 |
| XGBoost             |  -1.024 | 20.84 |     -1.204 |
| Tuned XGBoost       |  -0.182 | 16.75 |     -0.067 |
| SVR                 |  -0.292 | 17.11 |          — |
| HistGradientBoost   |  -0.949 | 23.38 |          — |

### Summary

Feature selection substantially reduced predictive performance for BART, with all cross-validation scores remaining negative.

---

# Method 8 — Principal Component Analysis (PCA)

## Objective

Reduce hyperspectral dimensionality using Principal Component Analysis instead of correlation filtering.

---

## Feature Reduction

| Dataset | Original Features | Principal Components |
| ------- | ----------------: | -------------------: |
| HARV    |               857 |                    6 |
| BART    |               857 |                    5 |

---

## HARV → HARV

| Model               |   Test R² |       MAE | Mean CV R² |
| ------------------- | --------: | --------: | ---------: |
| Linear Regression   |    -0.974 |     22.99 |     -1.218 |
| Random Forest       |     0.335 | **11.62** |     -0.264 |
| Tuned Random Forest |     0.219 |     12.31 |     -0.025 |
| XGBoost             |     0.348 |     12.94 |     -0.261 |
| Tuned XGBoost       |     0.265 |     12.89 |  **0.048** |
| SVR                 |     0.103 |     13.45 |          — |
| HistGradientBoost   | **0.406** |     13.13 |          — |

### Summary

PCA preserved substantially more predictive information than correlation filtering while reducing the dataset from **857 features to only six principal components**.

Compared to Method 7, nearly every model improved, demonstrating that PCA retains information from all spectral bands rather than discarding correlated variables.

---

## BART → BART

| Model               |   Test R² |       MAE | Mean CV R² |
| ------------------- | --------: | --------: | ---------: |
| Linear Regression   |    -0.651 |     18.02 |     -0.290 |
| Random Forest       |     0.034 | **11.67** |     -0.033 |
| Tuned Random Forest |    -0.062 |     13.23 |  **0.247** |
| XGBoost             |     0.028 |     14.31 |     -0.156 |
| Tuned XGBoost       | **0.097** |     13.18 |  **0.139** |
| SVR                 |    -0.097 |     14.89 |          — |
| HistGradientBoost   |     0.071 |     14.80 |          — |

### Summary

PCA consistently outperformed correlation filtering and produced the first positive cross-validation scores for tuned Random Forest and tuned XGBoost on BART.

---

# Method 9 — Sentinel-2 + NEON Canopy Height Model

## Objective

Evaluate whether replacing the global canopy height product with the higher-resolution NEON Canopy Height Model improves Sentinel-2 prediction.

---

## HARV → HARV

| Model               |   Test R² |       MAE | Mean CV R² |
| ------------------- | --------: | --------: | ---------: |
| Linear Regression   |   -16.263 |     63.72 |    -15.532 |
| Random Forest       |     0.050 |     15.92 |     -0.096 |
| Tuned Random Forest |     0.130 |     15.51 |     -0.029 |
| XGBoost             |     0.073 |     15.58 |     -0.139 |
| Tuned XGBoost       | **0.395** | **12.87** |  **0.053** |
| SVR                 |    -0.009 |     14.35 |          — |
| HistGradientBoost   |    -0.002 |     16.45 |          — |

### Summary

Adding the NEON canopy height model produced the best Sentinel-2 result observed for HARV. Tuned XGBoost improved to **R² = 0.395**, while also yielding the first positive mean cross-validation score among the Sentinel-2 experiments.

---

## BART → BART

| Model               |   Test R² |       MAE | Mean CV R² |
| ------------------- | --------: | --------: | ---------: |
| Linear Regression   |    -3.582 |     33.66 |    -13.913 |
| Random Forest       |     0.154 |     15.36 |     -0.170 |
| Tuned Random Forest |     0.103 |     15.10 |     -0.014 |
| XGBoost             | **0.326** | **14.72** |     -0.193 |
| Tuned XGBoost       |    -0.037 |     15.44 |     -0.048 |
| SVR                 |    -0.149 |     16.68 |          — |
| HistGradientBoost   |    -0.190 |     16.97 |          — |

### Summary

The NEON canopy height model improved the test performance of XGBoost for BART, although cross-validation scores remained negative, indicating limited generalization due to the small dataset.

---

# Method 10 — Combined HARV + BART with Sentinel-2 + NEON CHM

## Objective

Determine whether canopy height improves prediction when training a single model using both HARV and BART.

| Model             |    Test R² |       MAE |
| ----------------- | ---------: | --------: |
| Linear Regression |     -0.992 |     24.01 |
| Random Forest     |     -0.329 |     20.50 |
| XGBoost           |     -0.348 |     20.71 |
| SVR               | **-0.060** | **17.73** |
| HistGradientBoost |     -0.589 |     22.23 |

### Summary

Adding canopy height did not improve the combined-site model. While canopy structure enhanced within-site prediction, it did not overcome the ecological differences between HARV and BART that limit cross-site generalization.

---

# Final Best Results Summary

| Method                                          | Dataset     | Best Model          |   Test R² |       MAE | Mean CV R² | Notes                                                         |
| ----------------------------------------------- | ----------- | ------------------- | --------: | --------: | ---------: | ------------------------------------------------------------- |
| **1. Sentinel-2 Baseline**                      | HARV → HARV | HistGradientBoost   |     0.316 | **14.62** |          — | Best baseline Sentinel-2 model                                |
|                                                 | HARV → BART | Random Forest       |     0.219 | **15.79** |          — | Best cross-site performance                                   |
|                                                 | BART → BART | CNN                 |     0.137 |     15.13 |          — | Highest R² (XGBoost had lower MAE = 14.37)                    |
|                                                 | BART → HARV | Random Forest       |     0.188 | **15.72** |          — | Cross-site remained difficult                                 |
| **2. Combined Sentinel-2**                      | HARV + BART | Random Forest       |    -0.366 |     19.81 |          — | Combining sites reduced performance                           |
| **3. Original Hyperspectral**                   | HARV → HARV | **XGBoost**         | **0.658** | **10.06** |          — | Best overall model                                            |
|                                                 | HARV → BART | Random Forest       |    -0.288 |     16.69 |          — | Poor transferability                                          |
|                                                 | BART → BART | Linear Regression   | **0.699** |  **8.82** |          — | Highest within-site accuracy                                  |
|                                                 | BART → HARV | Random Forest       |     0.170 | **14.62** |          — | Cross-site remained difficult                                 |
|                                                 | HARV + BART | XGBoost             |     0.184 | **14.70** |          — | Best combined hyperspectral model                             |
| **4. Expanded Field Dataset**                   | HARV → HARV | XGBoost             |     0.598 | **10.67** |          — | More field observations did not consistently improve accuracy |
|                                                 | BART → BART | Linear Regression   | **0.691** |  **8.39** |          — | Similar to original results                                   |
| **5. Single Cloud-Free Sentinel-2 Scene**       | HARV → HARV | Random Forest       |     0.223 | **14.39** |          — | Slight improvement over median composite                      |
|                                                 | BART → BART | Random Forest       |     0.107 | **12.72** |          — | Minor improvement                                             |
| **6. Expanded Sentinel-2 Features**             | HARV → HARV | Tuned XGBoost       |     0.275 |     14.80 |  **0.103** | Added standard deviations and texture features                |
|                                                 | BART → BART | XGBoost             |     0.271 | **14.44** |     -0.190 | Best Sentinel-2-only BART model                               |
| **6. Expanded Features + Global Canopy Height** | HARV → HARV | Tuned XGBoost       |     0.350 | **12.76** |  **0.033** | Canopy height improved HARV prediction                        |
|                                                 | BART → BART | XGBoost             | **0.433** | **12.43** |     -0.223 | Largest Sentinel-2 improvement                                |
| **7. Hyperspectral Feature Selection**          | HARV → HARV | XGBoost             |     0.049 | **13.60** |     -0.729 | Correlation filtering removed useful information              |
|                                                 | BART → BART | Tuned Random Forest |    -0.268 | **17.20** |     -0.064 | Feature selection substantially reduced performance           |
| **8. Hyperspectral PCA**                        | HARV → HARV | HistGradientBoost   | **0.406** |     13.13 |          — | PCA retained more information than feature selection          |
|                                                 | BART → BART | Tuned XGBoost       |     0.097 |     13.18 |  **0.139** | First consistently positive CV scores                         |
| **9. Sentinel-2 + NEON CHM**                    | HARV → HARV | Tuned XGBoost       | **0.395** | **12.87** |  **0.053** | Best Sentinel-2 model using NEON canopy structure             |
|                                                 | BART → BART | XGBoost             |     0.326 | **14.72** |     -0.193 | Moderate improvement with NEON CHM                            |
| **10. Combined Sentinel-2 + NEON CHM**          | HARV + BART | SVR                 |    -0.060 | **17.73** |          — | Canopy height did not improve combined-site prediction        |

---

# Overall Conclusions

### Best Performing Models

| Rank | Method                                     | Dataset     | Model             |   Test R² |
| ---: | ------------------------------------------ | ----------- | ----------------- | --------: |
| 🥇 1 | Original Hyperspectral                     | BART → BART | Linear Regression | **0.699** |
| 🥈 2 | Original Hyperspectral                     | HARV → HARV | XGBoost           | **0.658** |
| 🥉 3 | Expanded Field Dataset                     | HARV → HARV | XGBoost           | **0.598** |
|    4 | Expanded Sentinel-2 + Global Canopy Height | BART → BART | XGBoost           | **0.433** |
|    5 | Hyperspectral PCA                          | HARV → HARV | HistGradientBoost | **0.406** |
|    6 | Sentinel-2 + NEON CHM                      | HARV → HARV | Tuned XGBoost     | **0.395** |
|    7 | Expanded Sentinel-2 + Global Canopy Height | HARV → HARV | Tuned XGBoost     | **0.350** |
|    8 | Single Cloud-Free Sentinel-2               | HARV → HARV | Random Forest     | **0.223** |

---

# Key Findings

* **Hyperspectral imagery consistently outperformed Sentinel-2**, achieving the highest prediction accuracy across nearly all experiments.
* **Aggressive correlation-based feature selection** substantially reduced model performance, indicating that many correlated hyperspectral bands still contain useful ecological information.
* **Principal Component Analysis (PCA)** was a more effective dimensionality reduction technique than feature selection, preserving predictive information while reducing the feature space from **857 bands to only 5–6 principal components**.
* **Canopy height metrics** improved within-site Sentinel-2 models, particularly for XGBoost, demonstrating that forest structural information complements spectral information.
* **Cross-site prediction (HARV ↔ BART)** remained consistently poor across all experiments, suggesting strong ecological and spectral differences between the two forests.
* **Cross-validation scores** were generally much lower than test-set R² values, reflecting the limited sample size (35–41 plots per site) and indicating that model generalization remains a primary challenge.

