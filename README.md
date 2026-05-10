# Solar Production and Load Prediction

## 📌 Project Overview
The objective of this project is to develop a robust machine learning model to predict PV (Photovoltaic) generation and user load consumption for 10-minute intervals within a look-ahead period of 1 to 4 hours. The model's performance was evaluated in a Kaggle competition using real-world data from SkyElectric and SkyLabs.

## ⚠️ Problem Statement
Predicting solar power generation and user load consumption is highly complex due to:
- **Diurnal Cycles & Weather:** Solar generation heavily depends on the time of day and unpredictable weather conditions (e.g., cloud cover).
- **Varied Load Profiles:** Power consumption fluctuates based on whether the system is Residential or Commercial, with each having distinct peak usage hours.
- **Time-Series Complexity:** Predicting a continuous window (1 to 4 hours ahead) requires a model that can capture both immediate sequential dependencies and long-term seasonality without degrading in accuracy over longer look-ahead periods.

## 💡 Proposed Solution & Methodology
To tackle these challenges, I implemented a robust **Multi-Output Regression** pipeline using **XGBoost**. Instead of relying solely on traditional time-series forecasting methods, I engineered strong temporal features to allow a highly efficient tabular model to capture the complex sequential patterns.

### 🔍 Important Work & Implementation Details
1. **Data Integration & Scaling:** 
   - Merged the core time-series data with system-specific metadata (`panels_capacity`, `load_capacity`, `location`, etc.). This gave the model the necessary physical context to scale its predictions accurately for different installations.
2. **Feature Engineering:** 
   - Extracted `hour`, `day`, `month`, and `day_of_week` from the raw timestamp. This was critical for the model to inherently understand diurnal cycles (e.g., knowing generation must be `0.0` at night) and weekly consumption behaviors.
3. **Categorical Encoding:** 
   - Applied One-Hot Encoding to categorical variables such as `connection_type` (Commercial vs. Residential) to ensure mathematical compatibility with the XGBoost algorithm.
4. **Model Training:**
   - Designed a Multi-Output **XGBoost Regressor** (`XGBRegressor`) using the `reg:absoluteerror` objective to predict both generation and load simultaneously.
   - Tuned hyperparameters (`learning_rate=0.05`, `max_depth=7`, `n_estimators=100`) to prevent overfitting and ensure steady, generalized learning.

## 📊 Results & Evaluation
The model was evaluated using **Mean Absolute Error (MAE)**, which calculates the average magnitude of prediction errors across both target variables.

- **Validation MAE:** `1178.7056`
- **Significance:** This result demonstrates that the model successfully learned the complex relationships between the physical system limits, temporal patterns, and the resulting generation/load values, providing a highly reliable forecast for the 1 to 4 hour look-ahead periods.

## 📂 Repository Structure
- `psv-model.ipynb`: Core Jupyter Notebook containing the full pipeline (Data Loading, EDA, Preprocessing, Feature Engineering, Training, and Evaluation).
- `solar-production-and-load-prediction-competition/`: Directory containing the real-world datasets (*Note: Not included in this repository due to confidentiality and data sharing prohibitions*).
- `Project Evaluation/`: Supplementary materials and details regarding the Kaggle competition evaluation criteria.

## 🚀 How to Run
1. Clone the repository.
2. Place the authorized, private datasets into the `solar-production-and-load-prediction-competition/` folder (data is prohibited from public sharing).
3. Install required dependencies (`pandas`, `xgboost`, `scikit-learn`, `matplotlib`, `seaborn`).
4. Run all cells in `psv-model.ipynb` to reproduce the preprocessing, training, and evaluation results.
