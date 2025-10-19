{\rtf1\ansi\ansicpg1252\cocoartf2822
\cocoatextscaling0\cocoaplatform0{\fonttbl\f0\fswiss\fcharset0 Helvetica;}
{\colortbl;\red255\green255\blue255;}
{\*\expandedcolortbl;;}
\paperw11900\paperh16840\margl1440\margr1440\vieww11520\viewh8400\viewkind0
\pard\tx720\tx1440\tx2160\tx2880\tx3600\tx4320\tx5040\tx5760\tx6480\tx7200\tx7920\tx8640\pardirnatural\partightenfactor0

\f0\fs24 \cf0 # Replication of Sagaceta-Mej\'eda et al. Methodology for Stock Market Prediction\
\
## Project Overview\
This repository contains a complete replication of the methodology presented in the paper "An Intelligent Approach for Predicting Stock Market Movements in Emerging Markets Using Optimized Technical Indicators and Neural Networks" by Sagaceta-Mej\'eda et al. (2024). The project implements a data-driven pipeline to predict directional movements of the iShares MSCI Chile ETF (ECH) using feature engineering, feature selection via Dispersion Ratio, and a Multilayer Perceptron (MLP) classifier.\
\
## 1. Objectives\
The primary objectives of this replication study are:\
*   To faithfully reconstruct the analytical pipeline described by Sagaceta-Mej\'eda et al.\
*   To engineer a set of widely-used technical indicators from raw stock data.\
*   To identify the most predictive features using the Dispersion Ratio filter method.\
*   To train and evaluate an MLP model using 10-fold stratified cross-validation.\
*   To quantitatively and visually compare the replication results with the original study's findings.\
\
## 2. Environment and Dependencies\
This project requires a Python 3 environment with the following libraries installed:\
\
*   `yfinance`\
*   `pandas`\
*   `numpy`\
*   `matplotlib`\
*   `seaborn`\
*   `scikit-learn`\
\
All warnings in the code have been suppressed to ensure clean execution logs.\
\
## 3. Data Acquisition and Preparation\
*   **Data Source:** Yahoo Finance (`yfinance`)\
*   **Ticker Symbol:** `ECH` (iShares MSCI Chile ETF)\
*   **Period:** 2009-12-12 to 2020-01-01\
*   **Preparation:** Raw data was cleaned by converting all columns to numeric data types and removing rows with `NaN` values.\
\
**Summary Statistics of ECH Opening Price:**\
*   **Minimum:** $21.49\
*   **Mean:** $36.00\
*   **Median:** $34.73\
*   **Maximum:** $54.53\
\
The final cleaned dataset consists of **2,510 trading days**.\
\
## 4. Feature Engineering and Target Variable Creation\
The target variable, **\uc0\u915  (Gamma)**, is a binary indicator defined as:\
*   **1** if today's opening price is greater than yesterday's (`Open_t > Open_\{t-1\}`).\
*   **0** otherwise.\
\
The following technical indicators were computed as features:\
*   Simple Moving Average (SMA_20)\
*   Exponential Moving Average (EMA_12)\
*   Relative Strength Index (RSI_14)\
*   Bollinger Bands (BB_upper, BB_middle, BB_lower)\
*   Williams %R (14-day)\
*   Price-based features (Price_Range, Price_Change)\
*   Volume indicator (Volume_SMA)\
\
**Target Variable Distribution:**\
*   **Up Days (\uc0\u915  = 1):** 1,247 (49.7%)\
*   **Down Days (\uc0\u915  = 0):** 1,263 (50.3%)\
*   The dataset is well-balanced, mitigating potential model bias.\
\
## 5. Feature Selection using Dispersion Ratio\
Feature selection was performed using the Dispersion Ratio (Ferreira & Figueiredo, 2012) to identify the most relevant predictors. The top 5 features with the highest scores were selected for model training.\
\
| Rank | Feature        | Dispersion Ratio |\
|------|----------------|------------------|\
| 1    | Williams_R_14  | 1.6623           |\
| 2    | Price_Range    | 1.1236           |\
| 3    | RSI_14         | 1.0968           |\
| 4    | Volume_SMA     | 1.0799           |\
| 5    | BB_lower       | 1.0224           |\
\
## 6. Model Training and Validation\
*   **Model:** Multilayer Perceptron (MLP) Classifier\
*   **Architecture:** 50 neurons in a single hidden layer, logistic activation function.\
*   **Solver:** L-BFGS (optimizer for small datasets).\
*   **Validation:** 10-Fold Stratified Cross-Validation.\
*   **Preprocessing:** Features were normalized using `MinMaxScaler`.\
\
### Cross-Validation Results\
The model was evaluated across 10 folds, yielding the following accuracies:\
\
| Fold | Accuracy |\
|------|----------|\
| 1    | 61.75%   |\
| 2    | 63.35%   |\
| 3    | 62.15%   |\
| 4    | 67.33%   |\
| 5    | 62.15%   |\
| 6    | 71.31%   |\
| 7    | 64.94%   |\
| 8    | 69.72%   |\
| 9    | 65.74%   |\
| 10   | 68.53%   |\
| **Mean** | **65.70%** |\
| **Std**  | **3.24%** |\
\
### Comparison with Original Paper\
| Study                   | Methodology                              | Accuracy |\
|-------------------------|------------------------------------------|----------|\
| Sagaceta-Mej\'eda et al.   | 5 Selected Features + MLP                | 80.27%   |\
| **This Replication**    | **5 Selected Features + MLP**            | **65.70%** |\
\
**Performance Gap:** -14.57 percentage points.\
\
## 7. Visualization\
The project includes a suite of visualizations to aid in the interpretation of results:\
1.  **Price History:** Historical closing price of the ECH ETF.\
2.  **Feature Importance:** Bar chart of Dispersion Ratios for all technical indicators.\
3.  **Model Performance:** Box plot and strip plot of cross-validation accuracies.\
4.  **Target Distribution:** Bar chart showing the balance of the target variable.\
\
## 8. Conclusion\
This replication successfully confirms the core methodological premise of the original paper: a compact set of features selected via Dispersion Ratio can be used with an MLP to generate above-random predictions for stock market movements. While the absolute performance metric achieved in this replication was lower than that reported in the original study, the pipeline validates the effectiveness of the feature selection strategy and model architecture. The difference in performance may be attributed to variations in data sourcing, market conditions, or specific hyperparameter implementations.\
\
## 9. References\
1.  Sagaceta-Mej\'eda, Alma Roc\'edo, et al. \'93An Intelligent Approach for Predicting Stock Market Movements in Emerging Markets Using Optimized Technical Indicators and Neural Networks.\'94 *Economics*, vol. 18, 2024, pp. 1\'9614.\
2.  Ferreira, A. J., and M. A. Figueiredo. \'93Efficient Feature Selection Filters for High-Dimensional Data.\'94 *Pattern Recognition Letters*, vol. 33, no. 13, 2012, pp. 1794\'96804.}