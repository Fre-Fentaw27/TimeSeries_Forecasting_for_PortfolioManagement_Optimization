# TimeSeries_Forecasting_for_PortfolioManagement_Optimization

preprocess financial data, develop time series forecasting models, analyze market trends, and optimize investment portfolios.

# Financial Portfolio Analysis Project

## Project Overview

This project analyzes three key financial assets (TSLA, BND, SPY) from July 2015 to July 2025 to identify trends, build forecasting models, and optimize portfolio allocations.

## Folder Structure

```bash
TimeSeries_Forecasting_for_PortfolioManagement_Optimization/
│
├── data/
│ ├── raw/ # Original downloaded data
│ │ ├── TSLA_historical.csv
│ │ ├── BND_historical.csv
│ │ └── SPY_historical.csv
│ │
│ └── processed/ # Cleaned and processed data
│ ├── TSLA_cleaned.csv
│ ├── BND_cleaned.csv
│ ├── SPY_cleaned.csv
│ ├── combined_cleaned.csv
│ └── metadata.json
│
├── notebooks/
│ ├── 01_data_collection.ipynb # Data fetching from yfinance
│ ├── 02_data_preprocessing.ipynb # Data cleaning and feature engineering
│ └── 03_eda_analysis.ipynb # Exploratory data analysis
│
├── reports/
│ ├── figures/
│ │ ├── eda/ # EDA visualizations
│ │ │ ├── TSLA_returns_analysis.png
│ │ │ ├── SPY_volatility.png
│ │ │ └── ...
│ │ └── ... # Other visualizations
│ ├── risk_metrics.csv # Calculated risk metrics
│ └── eda_insights.json # Key findings from EDA
│
└── README.md # This documentation file
```

## Task 1: Data Pipeline Implementation

### 1. Data Collection (`01_data_collection.ipynb`)

- **Sources**: Yahoo Finance (yfinance) API
- **Assets**:
  - TSLA (Tesla) - High-growth tech stock
  - BND (Vanguard Bond ETF) - Low-risk fixed income
  - SPY (S&P 500 ETF) - Market benchmark
- **Time Period**: 2015-07-01 to 2025-07-31
- **Data Points Collected**:
  - Daily OHLC prices (Open, High, Low, Close)
  - Adjusted Close prices
  - Trading volume
  - Automatic ticker labeling

### 2. Data Preprocessing (`02_data_preprocessing.ipynb`)

**Cleaning Steps**:

1. Missing value handling (forward/backward fill)
2. Data type conversion (numeric/date)
3. Feature engineering:
   - Daily returns (arithmetic/logarithmic)
   - Rolling volatility (5D, 21D, 63D)
   - Moving averages (50D, 200D)
   - Time features (weekday, month, year)

**Outputs**:

- Cleaned CSV files for each asset
- Combined dataset with all assets
- Processing metadata file

### 3. Exploratory Data Analysis (`03_eda_analysis.ipynb`)

**Key Analyses**:

1. **Time Series Analysis**:

   - Price evolution with moving averages
   - Cumulative return trajectories

2. **Return Characteristics**:

   - Return distributions and normality tests (QQ plots)
   - Skewness and kurtosis measurements
   - Extreme return analysis

3. **Risk Metrics**:

   - Value-at-Risk (Historical/Parametric)
   - Conditional VaR (Expected Shortfall)
   - Sharpe ratios

4. **Seasonality Analysis**:

   - Weekly/monthly return patterns
   - Calendar effects visualization

5. **Comparative Analysis**:
   - Correlation matrix of returns
   - Risk-return profile visualization

**Outputs**:

- 15+ visualization files (PNG)
- Risk metrics CSV report
- EDA insights JSON file

## Technical Requirements

- Python 3.8+
- Key Packages:
  - yfinance (0.2.18+)
  - pandas (1.3.0+)
  - matplotlib (3.4.0+)
  - seaborn (0.11.0+)
  - scipy (1.7.0+)

## Usage

1. Run notebooks in sequence:

```bash
jupyter notebook 01_data_collection.ipynb
jupyter notebook 02_data_preprocessing.ipynb
jupyter notebook 03_eda_analysis.ipynb
```

# Task 2: Time Series Forecasting Models

## Objective

Develop and compare forecasting models (ARIMA/SARIMA and LSTM) to predict future prices of TSLA, BND, and SPY using historical data from 2015-2023, evaluating performance on 2024-2025 test data.

### Task 3: Optimize Portfolio Based on Forecast

This part of the project focuses on applying modern portfolio theory to optimize an investment portfolio. The primary goal is to determine the ideal asset allocation (the **weights**) for a portfolio of assets based on their historical performance and a specific forecast for one of the assets. We use **PyPortfolioOpt**, a powerful Python library, for this task.

#### 1. Data Preparation and Alignment

The initial step involves combining historical price data for a bond ETF (**BND**) and a market index ETF (**SPY**) with a forecasted return series for **TSLA**. A crucial part of this step is ensuring that all data is correctly **aligned by date**. Misaligned data can lead to a **singular covariance matrix**, which will cause the optimization to fail. The code addresses this by concatenating the return series into a single, clean DataFrame.

#### 2. Calculating Expected Returns ($\mu$) and Covariance Matrix ($\Sigma$)

Two fundamental inputs for portfolio optimization are:

- **Expected Annual Returns ($\mu$)**: The projected return for each asset over a year.
- **Covariance Matrix ($\Sigma$)**: A matrix that measures how the returns of each asset move in relation to one another.

We calculate these using PyPortfolioOpt's built-in functions, `mean_historical_return` and `sample_cov`. To prevent numerical instability and solver errors, the covariance matrix is **regularized** by adding a small positive value to its diagonal. This ensures the matrix is **positive definite**, a requirement for the optimization.

#### 3. Portfolio Optimization

With the expected returns and covariance matrix, we use PyPortfolioOpt's `EfficientFrontier` class to solve two classic optimization problems:

- **Maximum Sharpe Ratio Portfolio**: This portfolio aims to maximize the return per unit of risk, identifying the most efficient allocation on the frontier.
- **Minimum Volatility Portfolio**: This portfolio aims to minimize the overall risk (volatility) of the portfolio.

#### 4. Plotting the Efficient Frontier

The **efficient frontier** is a graph that shows the set of optimal portfolios that provide the highest expected return for a given level of risk. The code plots this curve, marking the location of the **Max Sharpe** and **Min Volatility** portfolios. This visualization helps in understanding the trade-off between risk and return and provides a visual representation of the optimal portfolios.

#### 5. Summarizing Results

Finally, the optimal weights for both the Max Sharpe and Min Volatility portfolios are printed. These weights represent the percentage of the total portfolio value that should be allocated to each asset to achieve the desired optimization goal.
EOF
