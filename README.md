# Solar Production and Load Prediction

## Project Objective
Develop a model to predict PV (Photovoltaic) generation and load consumption values for 10-minute intervals within a look-ahead period of 1 to 4 hours. The specific look-ahead period will vary each time. The model's performance is evaluated using test data, with the ground truth known only to the competition hosts and the Kaggle Evaluator.

## Project Description
Predicts PV generation and user load at 10-minute intervals using real-world data from SkyElectric and SkyLabs. It applies Machine Learning techniques and is evaluated using Mean Absolute Error (MAE) in a Kaggle competition.

### Process & Methodology
1. **Exploratory Data Analysis (EDA) & Preprocessing**
   - Merged training/test datasets with system-level details (e.g., panel capacity, connection type).
   - Performed feature engineering by extracting temporal features (`hour`, `day`, `month`, `day_of_week`) from timestamps.
   - Applied One-Hot Encoding to categorical columns like `connection_type`.
2. **Model Development**
   - Trained an **XGBoost Regressor** (`XGBRegressor`) with the `reg:absoluteerror` objective.
   - Handled multiple predictions across the look-ahead period (e.g., 6 predictions each for PV generation and load for a 1-hour look-ahead).
3. **Evaluation**
   - **Metric:** Mean Absolute Error (MAE), comparing the model's predictions to actual PV generation and load consumption.
   - **Current Model Performance:** Achieved an MAE of `1178.7056` on the validation set.

## Submission Format
For each ID and timestamp in the test set, the model predicts the power generation and load consumption. The submission file includes a header and follows this format:
```csv
test_id,system_id,timestamp,generation_W,load_W
W6GEQ51E,39,2023-08-12 23:10:00,10968.3,6497.07
RBYF8A9F,39,2023-08-12 23:20:00,3445.44,17995.29
```

## Repository Structure
- `psv-model.ipynb`: Contains the executed code for data loading, preprocessing, feature engineering, model training (XGBoost), and evaluation.
- `Project Evaluation/`: Contains supplementary materials and details for the project.
