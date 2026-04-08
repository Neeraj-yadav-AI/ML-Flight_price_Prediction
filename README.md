# ✈️ Flight Price Prediction

A machine learning project that predicts **Indian domestic flight ticket prices** using a Random Forest Regressor with hyperparameter tuning via RandomizedSearchCV.

---

## 📁 Project Structure

```
├── Flight_price_Prediction.ipynb   # Main notebook (cleaned & fixed)
├── Data_Train.xlsx                 # Training dataset (10,683 rows)
├── Test_set.xlsx                   # Test dataset (2,671 rows)
└── README.md
```

---

## 📊 Dataset

| File | Rows | Description |
|------|------|-------------|
| `Data_Train.xlsx` | 10,683 | Labelled flight records with prices |
| `Test_set.xlsx` | 2,671 | Unlabelled records for prediction |

**Features:**

| Column | Description |
|--------|-------------|
| `Airline` | Airline carrier name |
| `Date_of_Journey` | Travel date (DD/MM/YYYY) |
| `Source` | Departure city |
| `Destination` | Arrival city |
| `Route` | Intermediate stops on the route |
| `Dep_Time` | Scheduled departure time |
| `Arrival_Time` | Scheduled arrival time |
| `Duration` | Total flight duration |
| `Total_Stops` | Number of stopovers |
| `Additional_Info` | Extra details (meal, baggage, etc.) |
| `Price` | Ticket price in INR *(target variable)* |

---

## ⚙️ Pipeline Overview

```
Raw Data
   │
   ├─ Date Extraction       (Day / Month / Year from Date_of_Journey)
   ├─ Time Extraction       (Dep_Hour, Dep_Min, Arrival_Hour, Arrival_Minute)
   ├─ Stop Parsing          (Total_Stops → integer Stop count)
   ├─ Route Splitting       (Route → Route_1 … Route_5)
   ├─ Null Handling         (Price mean-fill, Total_Stops mode-fill)
   ├─ Label Encoding        (Airline, Source, Destination, Route_*, etc.)
   ├─ Feature Selection     (Lasso α=0.05 → drop low-importance features)
   └─ Model Training        (RandomForestRegressor + RandomizedSearchCV)
```

---

## 🌲 Model

**Algorithm:** Random Forest Regressor  
**Tuning:** RandomizedSearchCV — 50 iterations, 2-fold CV

| Hyperparameter | Search Space |
|----------------|-------------|
| `n_estimators` | 100 – 1200 (12 values) |
| `max_depth` | 5 – 30 (6 values) |
| `min_samples_split` | 2, 5, 10, 15, 100 |
| `min_samples_leaf` | 1, 2, 5, 10 |
| `max_features` | `sqrt` |

**Best Parameters Found:**

```python
{'n_estimators': 500, 'max_depth': 25,
 'min_samples_split': 2, 'min_samples_leaf': 1, 'max_features': 'sqrt'}
```

---

## 📈 Results

| Metric | Value |
|--------|-------|
| **R² Score** | 0.8748 |
| **MAE** | ₹738.96 |
| **MSE** | ₹2,536,179 |
| **RMSE** | ₹1,592.54 |

The model explains **~87.5%** of the variance in flight prices.

---

## 🐛 Bugs Fixed from Original Notebook

| Cell | Issue | Fix Applied |
|------|-------|-------------|
| Cells 0 & 2 | Used `google.colab.files.upload()` — Colab-only | Replaced with direct `pd.read_excel()` |
| Cell 4 | `df_train.append(df_test)` — deprecated in pandas 2.0 | Replaced with `pd.concat([df_train, df_test])` |
| Cell 26 | `Dep_Min` extracted with `.str[0]` (same as `Dep_Hour`) | Fixed to `.str[1]` to get actual minutes |
| Cell 52 | `max_features='auto'` removed in sklearn 1.2+ | Replaced with `'sqrt'` (equivalent) |
| Cell 12 | `Fraud`/`Normal` variables referenced before definition | N/A (different notebook — see Anomaly Detection) |

---

## 🚀 How to Run

### 1. Clone the repo
```bash
git clone https://github.com/<your-username>/flight-price-prediction.git
cd flight-price-prediction
```

### 2. Install dependencies
```bash
pip install pandas numpy scikit-learn matplotlib seaborn openpyxl
```

### 3. Launch the notebook
```bash
jupyter notebook Flight_price_Prediction.ipynb
```

> Make sure `Data_Train.xlsx` and `Test_set.xlsx` are in the same directory as the notebook.

---

## 🛠️ Requirements

```
pandas >= 1.5
numpy >= 1.23
scikit-learn >= 1.2
matplotlib >= 3.5
seaborn >= 0.12
openpyxl >= 3.0
jupyter
```

---

## 📄 License

This project is open-source and available under the [MIT License](LICENSE).
