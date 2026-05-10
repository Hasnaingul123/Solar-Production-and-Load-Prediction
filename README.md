# Solar Production and Load Prediction

## Project Objective
Develop a model to predict PV (Photovoltaic) generation and load consumption values for 10-minute intervals within a look-ahead period of 1 to 4 hours. The specific look-ahead period will vary each time. The model's performance is evaluated using test data, with the ground truth known only to the competition hosts and the Kaggle Evaluator.

## Project Description
Predicts PV generation and user load at 10-minute intervals using real-world data from SkyElectric and SkyLabs. It applies Machine Learning techniques and is evaluated using Mean Absolute Error (MAE) in a Kaggle competition.

## Data Overview & Patterns
The dataset comprises three core components:
1. **System Metadata (`systems_new.csv`):** Contains static details about each solar installation, including `system_id`, `connection_type` (Residential vs. Commercial), `location`, `panels_capacity`, and `load_capacity`. This allows the model to understand the *scale* of each system.
2. **Time-Series Data (`train_data.csv` & `test_data_masked.csv`):** Contains the historical target variables recorded in exactly **10-minute intervals**.
   - **Generation Pattern:** Follows a diurnal (daily) cycle. It is `0.0` at night, rises at sunrise, peaks near solar noon, and drops back to zero at sunset. Fluctuations during the day correspond to weather conditions (e.g., cloud cover).
   - **Load Pattern:** Continuous 24/7 consumption. Residential systems typically show evening spikes, while commercial systems peak during business hours.
3. **The "Look-Ahead" Window:** The testing goal involves predicting a block of future values (1 to 4 hours ahead). Because data is in 10-minute intervals, a 1-hour look-ahead requires 6 consecutive predictions.

## Process & Methodology
1. **Exploratory Data Analysis (EDA) & Preprocessing**
   - Merged training/test datasets with system-level details to provide scaling context.
   - Performed critical feature engineering by extracting temporal features (`hour`, `day`, `month`, `day_of_week`) from timestamps. This step is vital for a tabular model to understand time-based patterns like the diurnal cycle.
   - Applied One-Hot Encoding to categorical columns like `connection_type`.
2. **Model Development**
   - Trained an **XGBoost Regressor** (`XGBRegressor`) with the `reg:absoluteerror` objective.
   - Designed to handle multi-output regression (predicting both PV generation and load simultaneously) across the required look-ahead periods.
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
- `solar-production-and-load-prediction-competition/`: Contains the competition datasets (`systems_new.csv`, `train_data.csv`, `test_data_masked.csv`, `sample_submissions.csv`).
- `Project Evaluation/`: Contains supplementary materials and details for the project.

---
## Frequently Asked Questions (Interview Prep)

**1. Why is this project important?**
Predicting solar generation and energy consumption helps in better energy management. By accurately forecasting generation and load, we can efficiently balance the power grid, store excess energy, avoid outages, and reduce overall electricity costs.

**2. Why use XGBoost instead of a Time-Series specific model?**
XGBoost is an industry-standard, highly efficient algorithm that handles tabular data extremely well. It is fast and great at finding complex, non-linear relationships. By engineering strong temporal features (hour, day, month), we successfully adapted this tabular model for time-series forecasting.

**3. How did you prevent the model from overfitting?**
I used an 80/20 train-test split to ensure the model was evaluated on unseen validation data. Additionally, I tuned XGBoost hyperparameters such as `learning_rate` (0.05) and `max_depth` (7) to control complexity and encourage steady learning without memorization.
