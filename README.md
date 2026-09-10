# Predictive Analytics for Stock Market Trend Forecasting Using Sentiment Analysis

## Overview

This project focuses on **stock market trend forecasting using LSTM-based deep learning models**. It compares a price-based model with a hybrid model that incorporates additional financial, market, volatility, and sentiment-related features.

The project includes:

* Exploratory Data Analysis (EDA)
* Data preprocessing
* Feature encoding and scaling
* Time-series sequence generation
* LSTM model development
* Price-only and hybrid model comparison
* Model evaluation
* Confusion matrix and classification report
* Prediction visualization
* Export of prediction and sentiment results
* Power BI-ready output files

---

## Dataset

The dataset required for this project can be downloaded from the following Kaggle source:

**Kaggle Dataset:**
https://www.kaggle.com/code/samayashar/stock-dl-xgb-95-accuracy/input

Open the Kaggle page and download the required data from the **Input** section.

After downloading the dataset, place the CSV file in the project directory.

The notebook expects the dataset to be named:

```text
synthetic_stock_data.csv
```

If the downloaded file has a different filename, rename it to `synthetic_stock_data.csv` or update the dataset path in the notebook.

> **Note:** You may need to sign in to Kaggle to access and download the dataset.

---

## Notebook

The main project notebook is:

```text
stock_market_trend.ipynb
```

It can be opened using:

* Jupyter Notebook
* JupyterLab
* Google Colab

---

## Project Workflow

The notebook follows the workflow below:

1. Import required Python libraries
2. Load the stock-market dataset
3. Convert and sort dates
4. Check and handle missing values
5. Remove duplicate records
6. Encode categorical variables
7. Perform exploratory data analysis
8. Select model features
9. Scale numerical features
10. Create time-series sequences
11. Split the data into training and testing sets
12. Build LSTM models
13. Train the price-only LSTM model
14. Train the hybrid LSTM model
15. Generate predictions
16. Evaluate model performance
17. Generate a confusion matrix
18. Generate a classification report
19. Visualize actual and predicted trends
20. Export prediction and sentiment results

---

## Dataset Features

The dataset contains information such as:

* Date
* Company
* Sector
* Open
* High
* Low
* Close
* Volume
* Market Cap
* P/E Ratio
* Dividend Yield
* Volatility
* Sentiment Score
* Trend

The target variable is:

```text
Trend
```

Categorical variables such as `Company`, `Sector`, and `Trend` are encoded before being used by the models.

---

## Models

### Price-only LSTM

The baseline model uses:

```text
Open
High
Low
Close
```

This model provides a baseline for comparison.

### Hybrid LSTM

The hybrid model uses:

```text
Open
High
Low
Close
Volume
Market_Cap
PE_Ratio
Dividend_Yield
Volatility
Sentiment_Score
```

The comparison evaluates whether additional financial, market, volatility, and sentiment features improve stock-market trend classification.

---

## Data Preprocessing

### Date Processing

The `Date` column is converted to datetime format and observations are ordered chronologically.

### Missing Values

Missing numerical values are handled using the median of the corresponding feature.

### Duplicate Records

Duplicate rows are removed during preprocessing.

### Categorical Encoding

Categorical variables are converted into numerical representations using `LabelEncoder`.

### Feature Scaling

Numerical features are scaled using `MinMaxScaler`.

---

## Time-Series Sequences

The LSTM models use a **10-observation time window**.

For each prediction, the model receives the previous 10 observations as input.

The resulting input structure follows:

```text
(samples, time steps, features)
```

---

## LSTM Architecture

The models use the following architecture:

```text
Input
  ↓
LSTM (64 units)
  ↓
Dropout (30%)
  ↓
Dense (32 units, ReLU)
  ↓
Dense (3 units, Softmax)
```

### Model Configuration

* **Optimizer:** Adam
* **Loss:** Sparse Categorical Crossentropy
* **Metric:** Accuracy
* **Maximum Epochs:** 30
* **Batch Size:** 32
* **Validation Split:** 20%
* **Early Stopping Patience:** 5
* **Restore Best Weights:** Enabled

---

## Train/Test Split

The data is split chronologically:

```text
80% → Training
20% → Testing
```

The chronological split is used to preserve the time-series nature of the data.

---

## Model Evaluation

The models are evaluated using:

* Accuracy
* Weighted F1 Score
* RMSE
* MAE
* Confusion Matrix
* Classification Report

The model comparison is exported to:

```text
model_metrics.csv
```

### Evaluation Note

Because the target variable represents categorical trend classes, **accuracy, F1 score, confusion matrix, precision, and recall** are more directly relevant classification metrics.

RMSE and MAE are calculated on encoded class values and should not be interpreted as conventional stock-price prediction errors.

---

## Visualizations

The notebook produces visualizations such as:

* Closing Price Over Time
* Market Trend Distribution
* Sentiment Score Distribution
* Feature Correlation Heatmap
* Confusion Matrix
* Actual vs. Predicted Trend

---

## Output Files

After running the notebook, the following output files are generated.

### `model_metrics.csv`

Contains model evaluation results, including:

```text
Model
RMSE
MAE
F1
Accuracy
```

### `predictions.csv`

Contains:

```text
Date
Actual
Predicted
```

This file can be used to compare actual and predicted market trends.

### `sentiment_summary.csv`

Contains the average sentiment score grouped by market trend.

---

## Power BI

The generated CSV files can be imported into **Microsoft Power BI** for additional visualization and dashboard development.

The main prediction file is:

```text
predictions.csv
```

It can be used to create visualizations for:

* Actual vs. predicted trends
* Trend distribution
* Prediction performance over time

The `sentiment_summary.csv` file can be used to examine sentiment patterns across different market trends.

---

## Requirements

The project requires Python and the following libraries:

```text
pandas
numpy
matplotlib
seaborn
scikit-learn
tensorflow
```

Install the dependencies using:

```bash
pip install pandas numpy matplotlib seaborn scikit-learn tensorflow
```

---

## How to Run

### Step 1: Download the Dataset

Download the required dataset from:

https://www.kaggle.com/code/samayashar/stock-dl-xgb-95-accuracy/input

Download the data from the **Input** section of the Kaggle page.

### Step 2: Place the Dataset in the Project Folder

Place the downloaded dataset in the same project directory as the notebook.

Rename the dataset to:

```text
synthetic_stock_data.csv
```

if necessary.

### Step 3: Open the Notebook

Open:

```text
stock_market_trend.ipynb
```

using Jupyter Notebook, JupyterLab, or Google Colab.

### Step 4: Check the Dataset Path

The notebook uses:

```python
pd.read_csv("/content/synthetic_stock_data.csv")
```

If you are running the project locally, change this path to the location of your downloaded dataset.

### Step 5: Run the Notebook

Run all cells from beginning to end.

The notebook will perform preprocessing, model training, evaluation, visualization, and output-file generation.

---

## Project Structure

```text
Predictive-Analytics-for-Stock-Market-Trend-Forecasting-Using-Sentiment-Analysis/
│
├── stock_market_trend.ipynb
├── synthetic_stock_data.csv
├── model_metrics.csv
├── predictions.csv
├── sentiment_summary.csv
└── README.md
```

---

## Limitations

The project has several limitations:

* The dataset used by the project is synthetic.
* The model predicts trend classes rather than actual future stock prices.
* RMSE and MAE are not primary metrics for categorical classification.
* Scaling the full dataset before splitting can introduce data leakage.
* The validation approach should be considered carefully for time-series data.
* The project does not establish that predictions can generate profitable trading strategies.
* Results obtained from synthetic data should not be treated as evidence of real-world trading performance.
* No systematic hyperparameter optimization is performed.

---

## Future Improvements

Possible improvements include:

* Use real historical stock-market data.
* Fit preprocessing scalers only on the training data.
* Use a strictly chronological validation dataset.
* Add technical indicators such as RSI and moving averages.
* Compare LSTM with GRU, CNN-LSTM, XGBoost, Random Forest, and other models.
* Perform systematic hyperparameter tuning.
* Test different sequence lengths.
* Address class imbalance where required.
* Use additional classification metrics such as macro F1, precision, recall, and balanced accuracy.
* Perform walk-forward validation.
* Perform out-of-sample testing.
* Develop a proper trading-strategy backtesting framework.

---

## Conclusion

This project presents an LSTM-based approach for stock-market trend forecasting using historical market information and sentiment-related features.

A **price-only LSTM** is compared with a **hybrid LSTM** that incorporates additional financial, market, volatility, and sentiment variables.

The project combines data preprocessing, exploratory analysis, time-series modelling, deep learning, model evaluation, visualization, and Power BI-ready outputs in a single workflow.
