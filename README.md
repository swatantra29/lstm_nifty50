# NIFTY50 Minute-by-Minute Price & Return Prediction using LSTM

This project uses a pre-trained LSTM (Long Short-Term Memory) model to predict the minute-by-minute closing prices and returns for the NIFTY50 index. It focuses on the most recent 60 minutes of available data, making predictions iteratively for each minute.

## Features

* Loads NIFTY50 historical minute-level data from a CSV file.
* Utilizes a pre-trained LSTM model and a corresponding data scaler.
* Processes the most recent data (last 2 days, focusing on the final 60 minutes for prediction).
* Predicts:
  * Next minute's closing price.
  * Next minute's log return.
  * Next minute's percentage return.
* Compares predictions against actual values for the last 60 minutes.
* Calculates performance metrics: Mean Absolute Error (MAE) and Mean Squared Error (MSE) for log returns.
* Visualizes:
  * Predicted vs. Actual Log Returns.
  * Predicted vs. Actual Percentage Returns.
  * Predicted vs. Actual Prices.

## Requirements

* Python 3.7+
* Google Colab environment (or a local environment with appropriate modifications for file paths and library installations).
* Libraries:
  * numpy
  * pandas
  * joblib
  * tensorflow
  * matplotlib
  * scikit-learn
  * google-colab (if running in Colab)
You can install these (if not in Colab) using pip:
```bash
pip install numpy pandas joblib tensorflow matplotlib scikit-learn
```

## Setup

1. **Google Drive:**
  * Ensure you have a Google Account and access to Google Drive.
  * Create a folder structure in your Google Drive, for example: MyDrive/LSTM/lstm_returns/.
  * Upload the following files to the specified paths (or adjust paths in the script):
  * **NIFTY50 Data CSV:** /content/drive/MyDrive/LSTM/NIFTY50.csv
  * **Pre-trained LSTM Model:** /content/drive/MyDrive/LSTM/lstm_returns/lstm_minute_csv_20250518_25y.h5
  * **Pre-trained Scaler:** /content/drive/MyDrive/LSTM/lstm_returns/scaler_minute_csv_25y.pkl
2. **Data Format (NIFTY50.csv):**
  * The CSV file should contain historical minute-level data for NIFTY50.
  * It **must** have a datetime column. The script attempts to find it under names: Datetime, datetime, Date, or date.
  * It **must** have a closing price column. The script attempts to find it under names: Close, close, or falls back to the 4th column (index 3) if these are not found.
  * The data should be sorted chronologically, though the script includes a sort step.
  * To perform predictions for the last 60 minutes, the CSV should contain at least the last 2 days of data, with at least 61 data points (minutes) in the most recent segment.
Example CSV structure (minimum required columns):
```
Datetime,Open,High,Low,Close,Volume
2023-10-26 09:15:00,19000.50,19010.20,18990.00,19005.75,100000
2023-10-26 09:16:00,19005.75,19015.80,19002.30,19012.40,120000
...
```
Use code with caution.
Csv

## Usage

* Open the Python script (.ipynb or .py) in Google Colab.
* Ensure your Google Drive is organized as per the Setup section, or update the file paths in the script:
  * csv_filename
  * model_path
  * scaler_path
* Run all cells in the notebook.
* When prompted, authorize Google Colab to access your Google Drive.
* The script will:
  * Load and preprocess the data.
  * Load the model and scaler.
  * Perform minute-by-minute predictions for the last 60 minutes.
  * Print the final predicted vs. actual price and performance metrics (MAE, MSE).
  * Display three plots showing predicted vs. actual log returns, percentage returns, and prices.
## Script Overview
* **Imports & Setup:** Imports necessary libraries and mounts Google Drive.
* **Data Loading & Preprocessing:**
  * Loads the NIFTY50 CSV.
  * Converts the date/time column to datetime objects and sets it as the index.
  * Sorts the data by time.
  * Filters the DataFrame to include only data from the last 2 days to optimize processing for the final 60-minute window.
  * Ensures a 'close' column exists (or derives it).
  * Calculates percentage 'Returns'.
  * Selects the last 61 data points (to allow for 60 predictions, as each prediction uses the prior minute's data).
* **Model Loading:** Loads the pre-trained LSTM model and the MinMax scaler.
* **Prediction Loop:**
  * Iterates through the selected 60-minute window (from the 2nd minute to the 61st).
  * For each minute i:
    * Takes the 'close' price and 'Returns' from the previous minute (i-1) as input.
    * Scales this input using the loaded scaler.
    * Reshapes the input for the LSTM model.
    * Predicts the scaled next minute's value (interpreted as a component related to price movement).
    * Inverse transforms the prediction to get an estimated "predicted price". Note: The model was likely trained to predict returns or price changes, so the inverse transform uses a placeholder for the 'Returns' feature to isolate the 'price' component.
    * Calculates predicted log return and percentage return based on this predicted_price and the prev_close.
    * Stores actual and predicted prices, log returns, and percentage returns.
* **Metrics Calculation:** Computes MAE and MSE between actual and predicted log returns.
* **Output & Plotting:**
  * Prints the final predicted vs. actual price for the last minute.
  * Prints the calculated MAE and MSE.
  * Generates and displays plots for log returns, percentage returns, and prices.
## Pre-trained Model & Scaler
Model (lstm_minute_csv_20250518_25y.h5): This is a Keras LSTM model. It was presumably trained on historical NIFTY50 minute data, likely using 'close' prices and 'Returns' as input features to predict future price movements or returns.
Scaler (scaler_minute_csv_25y.pkl): This is a joblib-saved MinMaxScaler (or similar) from scikit-learn. It was fitted on the same features ('close', 'Returns') that the LSTM model was trained on and is necessary to transform new input data into the range the model expects.
## Disclaimer
Financial market predictions are inherently complex and uncertain. This script and the underlying model are for educational and illustrative purposes only. They should not be used as the sole basis for making any investment decisions. Past performance is not indicative of future results. Use at your own risk.
## License
This project is licensed under the [MIT License](LICENSE.md).
