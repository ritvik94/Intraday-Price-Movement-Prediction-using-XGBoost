# Intraday Stock Price Movement Prediction using XGBoost

This project aims to predict the short-term (minute-by-minute) price movement of a stock using historical intraday data. The objective is to build a binary classification model that predicts whether the closing price of the next minute will be higher ('Up') or lower/same ('Down') compared to the current minute's closing price.

The analysis is performed on minute-level stock data for ABB (`ABB_minute.csv`) and utilizes the powerful **XGBoost** algorithm for classification.

-----

## 📊 Dataset

  * **Source File**: `ABB_minute.csv`
  * **Description**: The dataset contains minute-level Open, High, Low, Close, and Volume (OHLCV) data for the stock ABB.
  * **Columns**:
      * `date`: Timestamp for the data point.
      * `open`: The opening price for the minute.
      * `high`: The highest price during the minute.
      * `low`: The lowest price during the minute.
      * `close`: The closing price for the minute.
      * `volume`: The number of shares traded during the minute.

-----

## ⚙️ Methodology

The script follows a structured approach to building the predictive model:

#### 1\. Data Loading and Exploration

The `ABB_minute.csv` dataset is loaded, and initial checks for missing values and duplicates are performed.

#### 2\. Data Preprocessing and Cleaning

  * **Column Dropping**: Any non-numeric columns are dropped.
  * **Outlier Removal**: The Interquartile Range (IQR) method is used to identify and remove outliers from the `open`, `high`, `low`, `close`, and `volume` columns.

#### 3\. Feature Engineering

  * **Target Variable Creation**: The core of the prediction problem is defined here.
    1.  A `next_close` column is created by shifting the `close` price column up by one period.
    2.  A binary target variable, `price_move`, is created.
          * If `next_close > close`, `price_move` is set to `1` (price went up).
          * Otherwise, `price_move` is set to `0` (price went down or stayed the same).

#### 4\. Model Preparation

  * **Feature Scaling**: Features are scaled using `StandardScaler` to normalize their range.
  * **Data Splitting**: The dataset is split into training (80%) and testing (20%) sets, stratified to maintain the target variable's distribution.

#### 5\. Model Training and Prediction

An **XGBoost Classifier** (`xgb.XGBClassifier`) is trained on the training data and then used to make predictions on the test data.

-----

## 📈 Results and Analysis

#### Performance Metrics

  * **Accuracy**: \~63.8%
  * **ROC-AUC Score**: \~0.56

**Classification Report:**

```
              precision    recall  f1-score   support

           0       0.64      0.65      0.64     38118
           1       0.64      0.63      0.63     37533

    accuracy                           0.64     75651
   macro avg       0.64      0.64      0.64     75651
weighted avg       0.64      0.64      0.64     75651
```

#### Feature Importance

The model identifies the most influential features in determining the next price movement. As expected, the OHLC prices are the most important predictors.

#### Key Takeaway

Predicting high-frequency price movements is extremely challenging. The results reflect this reality:

> Short-term intraday predictions are stuck near 60–65% accuracy, ROC-AUC \~0.55–0.60 because markets are efficient, data is noisy, features are weak, and most price changes are random micro-fluctuations rather than predictable patterns.

An ROC-AUC score of \~0.56 is only slightly better than a random guess (0.5), indicating that the model has very limited predictive power.

-----

## 🚀 How to Run the Project

#### Prerequisites

Ensure you have Python and the following libraries installed:

  * pandas
  * numpy
  * xgboost
  * scikit-learn
  * seaborn
  * matplotlib

You can install them using pip:

```bash
pip install pandas numpy xgboost scikit-learn seaborn matplotlib
```

#### Execution

1.  Place the `ABB_minute.csv` file in the same directory as the Python script.
2.  Run the script from your terminal:
    ```bash
    python intraday_price_movement_prediction_using_xgboost.py
    ```
