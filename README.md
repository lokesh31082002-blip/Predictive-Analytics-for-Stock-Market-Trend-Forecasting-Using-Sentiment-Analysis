# Stock Market Trend Prediction Using LSTM

## Overview

This project uses **Long Short-Term Memory (LSTM)** neural networks to predict stock market trends from historical stock data.

Two models are developed and compared:

1. **Price-only LSTM** – uses historical Open, High, Low, and Close prices.
2. **Hybrid LSTM** – combines price information with market, financial, volatility, and sentiment-related features.

The project also performs exploratory data analysis, data preprocessing, model evaluation, confusion-matrix analysis, and exports prediction results for further analysis in tools such as Power BI.

---

## Project Workflow

The notebook follows these main steps:

1. Import required libraries
2. Load the stock dataset
3. Convert and sort dates
4. Handle missing values and duplicates
5. Encode categorical variables
6. Perform exploratory data analysis
7. Select model features
8. Scale numerical features
9. Create time-series sequences
10. Split data into training and testing sets
11. Build LSTM models
12. Train the price-only model
13. Train the hybrid model
14. Generate predictions
15. Evaluate model performance
16. Generate a confusion matrix
17. Generate a classification report
18. Visualize actual vs. predicted trends
19. Export prediction data
20. Export sentiment summaries

---

## Dataset Download

The dataset used for this project can be downloaded from the following Kaggle notebook:

**Kaggle:** https://www.kaggle.com/code/samayashar/stock-dl-xgb-95-accuracy/input

Please download the required dataset from the Kaggle **Input** section and place the downloaded CSV file in the project directory.

The notebook expects the dataset to be available as:

```text
synthetic_stock_data.csv
```

If the downloaded file has a different filename, either rename it to `synthetic_stock_data.csv` or update the dataset path in `Lokesh_Final.ipynb`.

> **Note:** You may need to sign in to Kaggle to access and download the dataset. The linked Kaggle notebook is the source referenced for obtaining the input data.

## Dataset

The notebook expects a CSV file named:

```text
synthetic_stock_data.csv
```

The dataset contains information used for stock-market trend prediction, including:

- Date
- Company
- Sector
- Open
- High
- Low
- Close
- Volume
- Market Cap
- P/E Ratio
- Dividend Yield
- Volatility
- Sentiment Score
- Trend

The target variable is:

```text
Trend
```

The categorical variables `Company`, `Sector`, and `Trend` are encoded using `LabelEncoder`.

> **Note:** The notebook uses a synthetic dataset. The results should therefore not be interpreted as evidence of performance on real-world stock-market data.

---

## Features

### Price-only Model

The baseline model uses:

```text
Open
High
Low
Close
```

### Hybrid Model

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

The hybrid model incorporates additional market, financial, volatility, and sentiment information rather than relying only on historical prices.

---

## Data Preprocessing

The following preprocessing operations are performed:

### Date Processing

The `Date` column is converted to datetime format and the dataset is sorted chronologically.

### Missing Values

Missing numerical values are replaced with the median of their respective columns.

### Duplicate Removal

Duplicate rows are removed from the dataset.

### Categorical Encoding

`Company`, `Sector`, and `Trend` are converted into numerical representations using `LabelEncoder`.

### Feature Scaling

Numerical features are scaled using `MinMaxScaler`.

---

## Time-Series Sequence Creation

The project uses a **10-day time window** for creating LSTM sequences.

For each prediction, the model receives the previous 10 observations as input.

```text
Window size = 10
```

The resulting data is structured in the form required by an LSTM:

```text
(samples, time steps, features)
```

---

## LSTM Architecture

Both models use the same basic architecture:

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

The models are compiled using:

- **Optimizer:** Adam
- **Loss:** Sparse Categorical Crossentropy
- **Metric:** Accuracy

The output layer contains three classes corresponding to the encoded market trend categories.

---

## Training

The data is divided chronologically:

```text
80% → Training
20% → Testing
```

The models are trained with:

- Maximum epochs: `30`
- Batch size: `32`
- Validation split: `20%`

An `EarlyStopping` callback is used with:

```text
patience = 5
restore_best_weights = True
```

This stops training when validation loss stops improving and restores the best-performing model weights.

---

## Model Evaluation

The notebook evaluates the models using:

- RMSE
- MAE
- Weighted F1 Score
- Accuracy

The comparison is stored in a pandas DataFrame and exported as:

```text
model_metrics.csv
```

### Evaluation Note

RMSE and MAE are calculated on numerically encoded trend classes. Since the target is categorical, these metrics should not be interpreted in the same way as continuous stock-price prediction errors.

For the classification task, **accuracy, F1 score, confusion matrix, and the classification report** are more directly relevant.

---

## Confusion Matrix

A confusion matrix is generated for the hybrid LSTM model to compare actual and predicted trend classes.

A classification report is also generated to provide class-level precision, recall, and F1-score information.

---

## Visualizations

The project produces visualizations including:

- Closing Price Over Time
- Market Trend Distribution
- Sentiment Score Distribution
- Feature Correlation Heatmap
- Hybrid Model Confusion Matrix
- Actual vs. Predicted Trend

These visualizations are used to understand the dataset and evaluate model predictions.

---

## Output Files

### `model_metrics.csv`

Contains evaluation results for the Price LSTM and Hybrid LSTM models, including:

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

The predicted and actual trend values are converted back from encoded numerical values to their original categories.

### `sentiment_summary.csv`

Contains the average sentiment score grouped by market trend.

---

## Power BI Integration

The exported prediction data can be used for further visualization and analysis in Power BI.

The main file is:

```text
predictions.csv
```

It can be used to visualize:

- Actual trends
- Predicted trends
- Prediction performance over time
- Trend distributions

The following file can also be used:

```text
sentiment_summary.csv
```

to analyze the relationship between sentiment scores and market trends.

---

## Requirements

The project uses Python and the following libraries:

```text
pandas
numpy
matplotlib
seaborn
scikit-learn
tensorflow
```

Install the required packages with:

```bash
pip install pandas numpy matplotlib seaborn scikit-learn tensorflow
```

---

## How to Run

### 1. Prepare the Dataset

Place the dataset in the working directory:

```text
synthetic_stock_data.csv
```

### 2. Open the Notebook

Open:

```text
Lokesh_Final.ipynb
```

using Jupyter Notebook, JupyterLab, or Google Colab.

### 3. Update the Dataset Path

The notebook uses:

```python
pd.read_csv("/content/synthetic_stock_data.csv")
```

If running locally, update this path to the location of the dataset.

### 4. Run All Cells

Execute the notebook from beginning to end.

The notebook generates:

```text
model_metrics.csv
predictions.csv
sentiment_summary.csv
```

---

## Model Comparison

The project compares whether additional financial, market, volatility, and sentiment features improve stock-market trend classification compared with using price information alone.

| Model | Features |
|---|---|
| Price LSTM | Open, High, Low, Close |
| Hybrid LSTM | Price + Volume + Market Cap + P/E + Dividend Yield + Volatility + Sentiment |

The actual performance values should be taken from the generated `model_metrics.csv` file after running the notebook.

---

## Limitations

The project has several limitations:

- The dataset is synthetic rather than real-world market data.
- The model predicts trend classes rather than actual future stock prices.
- RMSE and MAE are not ideal primary metrics for a categorical classification problem.
- The notebook scales the full dataset before the train/test split, which can introduce **data leakage** because test-period information can influence the scaling transformation.
- The validation split used during training should also be considered carefully for time-series data.
- No systematic hyperparameter search is implemented.
- A fixed 10-observation sequence length is used.
- The model does not establish that predictions can produce profitable trading strategies.
- Performance on synthetic data should not be interpreted as evidence of performance in live financial markets.

---

## Future Improvements

Possible improvements include:

- Use real historical stock-market data.
- Fit scalers only on the training data.
- Use a strictly chronological validation set.
- Compare LSTM with GRU, CNN-LSTM, Random Forest, XGBoost, and other models.
- Perform systematic hyperparameter tuning.
- Test different sequence/window sizes.
- Handle class imbalance where necessary.
- Add technical indicators such as moving averages and RSI.
- Use classification-focused metrics such as macro F1, precision, recall, and balanced accuracy.
- Perform walk-forward and out-of-sample testing.
- Develop a proper backtesting framework before considering trading applications.

---

## Project Structure

```text
project/
│
├── Lokesh_Final.ipynb
├── synthetic_stock_data.csv
├── model_metrics.csv
├── predictions.csv
├── sentiment_summary.csv
└── README.md
```

---

## Conclusion

This project implements an LSTM-based approach for classifying stock-market trends using historical market information.

It compares a **price-only LSTM baseline** with a **hybrid LSTM model** that incorporates additional financial, market, volatility, and sentiment features.

The project combines machine learning, time-series sequence modelling, exploratory data analysis, model evaluation, and Power BI-ready data exports into a single workflow.
