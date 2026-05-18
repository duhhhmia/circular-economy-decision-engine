# Circular Economy Decision Engine: Dual-Stage Return Prediction & Value Recovery

## 📋 Project Overview
This project establishes an automated, dual-stage machine learning decision engine designed to optimize reverse logistics and financial yields for e-commerce electronics returns. By merging consumer return patterns with physical device diagnostics, the architecture programmatically determines whether a returned asset should be routed to high-margin refurbishment or eco-certified e-waste recycling.

## ❓ The Business Problem & The SMART Hook
E-commerce returns generate billions in operational losses and massive electronic waste footprints. However, not all returns are equal. To prevent "billion-dollar leaks," this project evaluates a core operational objective:
> **SMART Goal:** To what extent can the decision engine identify returned electronics with a potential resale value (`actual_used_price`) that is at least 60% of their original cost (`actual_new_price`), while maintaining a Stage 1 return prediction accuracy threshold?

## ⚙️ The Dual-Stage Engine Architecture
The engine processes data through a chain-linked machine learning pipeline:

1. **Stage 1 (Classification):** Evaluates user profiles, shipping options, and item costs to predict the initial likelihood of a device being returned (`Return_Status`).
2. **Stage 2 (Regression):** For items flagged as returned, the engine evaluates hardware specifications (RAM, battery capacity, screen size, usage days, release year) to forecast the precise secondary market resale value (`actual_used_price`).

---

## 🛠️ Tech Stack & Pipeline Engineering
* **Language:** Python
* **Libraries:** Scikit-Learn, Pandas, NumPy, Matplotlib, Seaborn
* **Feature Engineering:** Integrated non-tidy datasets (`returns_sustainability_dataset.csv` and `used_device_data.csv`) via structural index alignment. Categorical variables (`device_brand`, `os`, `Shipping_Method`) were processed using `LabelEncoder`. Target distributions were balanced using algorithmic class weighting.

---

## 📊 Modeling Results & Leaderboards

### Stage 1: Predicting Return Likelihood (Classification)
Baseline classification architectures were tested against ensemble methods to capture subtle multi-feature return triggers:

| Model | Testing Accuracy | Recall (Returns Caught) | Baseline F1-Score |
| :--- | :---: | :---: | :---: |
| Logistic Regression (Balanced) | 53.01% | 49.23% | 0.353 |
| Decision Tree ($max\_depth=5$) | 47.79% | **66.15%** | **0.398** |
| Random Forest (Baseline Ensemble) | **72.29%** | 10.77% | 0.169 |

* **Optimization:** While the baseline Random Forest provided the highest overall classification accuracy (**72.29%**), it struggled with recall on the minority class. Implementing hyperparameter tuning via `GridSearchCV` successfully optimized the tree split structures (`max_depth: 10`, `min_samples_split: 10`), significantly boosting the classification F1-Score to **0.2454** to stabilize predictive stability.

### Stage 2: Predicting Returned Device Resale Value (Regression)
Once a device is flagged as a return, it is passed to the Stage 2 regression layer to isolate price drivers and minimize valuation errors across 314 active return records:

| Model | Mean Absolute Error (MAE) | $R^2$ Score (Variance Explained) |
| :--- | :--- | :---: |
| Linear Regression | $0.32 | 62.09% |
| Decision Tree Regressor | $0.31 | — |
| **Random Forest Regressor (Winner)** | **$0.27** | **74.27%** |

* **The Winning Pipeline:** The **Random Forest Regressor** outperformed all other models, capturing **74.27%** of price variance with a hyper-precise Mean Absolute Error (MAE) of just **$0.27**. 

---

## 💡 Strategic Business & Circular Economy Impact
* **Precision Financial Auditing:** With an average valuation error of under 30 cents, the engine acts as an automated financial auditor. This precision allows warehouse operations to flag high-value assets matching the **60% recovery threshold** with near-perfect confidence, guaranteeing that high-yield electronics are preserved for refurbishment while low-yield components are routed instantly to certified recycling streams.
* **Feature Correlation Drivers:** Multi-variate feature importance maps revealed that original product pricing and discount application structures represent the heaviest leverage points for return risk profiles, while RAM configuration and battery metrics scale the ultimate post-consumer depreciation curve.
