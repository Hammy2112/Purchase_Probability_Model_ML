# Retail Demand Forecasting: From Sparsity to Strategy

## 📌 Project Overview

This project presents a machine learning solution designed to optimize retail operations, inventory planning, and marketing strategies for **Play Ringo**, an e-commerce brand operating on Shopify.

The initial objective was to build a Stratified Demand Forecasting model to predict exact weekly quantities sold and prevent stockouts. However, due to the extreme sparsity of the transactional data (a "Long Tail" of niche, intermittent-demand products), standard regression models (Linear Regression, Random Forest, XGBoost) failed to perform.

To solve this, the problem was strategically reframed from a quantitative regression task ("How many will sell?") to a probability-based classification task ("Will any sell?"). This pivot transformed a passive inventory management tool into a proactive, data-driven engine for commercial growth.

## 🗄️ Dataset & Confidentiality Notice

* **Confidentiality Notice:** Due to Non-Disclosure Agreements (NDA), the raw Shopify transactional data used in this analysis cannot be shared publicly in this repository.
* **Results Report:** In lieu of the raw dataset, a comprehensive Word document (`Retail Demand Forecasting Report(Playringo).docx`) has been included. This report details the full methodology, feature engineering processes, model evaluation metrics, and final business results.
* **Data Context:** The original dataset consisted of a Shopify Orders CSV Export containing over 10,000 real transaction records and an 80-column schema (customer identifiers, billing regions, pricing, and order financials).

## 🛠️ Data Processing & Feature Engineering

To capture consumer demand dynamics without the raw data, the following features were engineered from the transactional logs:

* **Momentum (Lag Features):** `Qty_Week_Minus_1` and `Qty_Week_Minus_2` to capture short-term sales momentum.
* **Trend (Rolling Statistics):** A 4-week moving average (`Rolling_Mean_4`) to smooth out the noise inherent in sparse data.
* **Current State:** The immediate context of `Qty_Week_0`.
* **Heterogeneity:** Label encoding product categories to allow the model to differentiate between item types.

## ⚙️ Model Specification & Performance

Framing the challenge as a Binary Classification task, **Logistic Regression** was selected for its strong interpretability, specifically the ability to generate transparent Odds Ratios to explain why a product was flagged.

**Testing Performance (at default 0.5 threshold):**

* **Accuracy:** 75%
* **Precision:** 79% (Highly reliable when predicting a sale)
* **Recall:** 54% (Conservative; misses some opportunities to avoid false alarms)
* **F1 Score:** 64%
* **AUC-ROC / AUC-PR:** 0.79 / 0.78 (Robust ability to handle class imbalance)

The confusion matrix revealed a highly precise but cautious model (only 27 False Positives, but 87 False Negatives).

## 💡 Key Business Insights

### 1. Momentum is the Strongest Predictor

By converting the Logistic Regression coefficients into Odds Ratios, the feature importance became clear. `Qty_Week_Minus_1` is the dominant predictor with an Odds Ratio of ~9.5. This means a product is nearly **10 times more likely to sell this week if it sold last week**. Demand is driven heavily by immediate momentum rather than long-term seasonality.

### 2. Optimizing the Decision Threshold

Because "Play Ringo" products often hover in the 35-45% sales probability range, a standard 0.5 decision threshold was too conservative, resulting in zero forecasted sales for the upcoming week. Through rigorous Sensitivity Analysis, the threshold was lowered to **0.30 - 0.35**. This adjustment perfectly balances risk and reward, capturing missed revenue opportunities without flooding the warehouse system with false positives.

## 🚀 Actionable Forecasts & Recommendations

Using the optimized threshold, the model actively isolates "dormant" items with underlying momentum, signaling that they are primed for conversion if targeted with marketing nudges.

**Next Week's High-Potential Targets:**

* **Scorekeeping Pegs:** 42% Sales Probability
* **Red Ringo:** 39% Sales Probability
* **Ash Grey Ringo:** 38% Sales Probability
* **Natural Maple Ringo:** 36% Sales Probability

**Recommendation:** Instead of utilizing average-based heuristics that tie up working capital, Play Ringo should allocate targeted promotional spend to these specific high-probability SKUs to drive immediate conversion.

## 💻 How to Review This Project

Because the raw data is restricted under NDA, you can review the technical approach and business outcomes through the provided documentation:

1. Clone this repository:
```bash
git clone https://github.com/Hammy2112/Purchase_Probability_Model_ML.git

```
2. Open `Retail Demand Forecasting Report(Playringo).docx` to view the complete analytical breakdown, sensitivity analysis charts, and confusion matrices.
3. Review the provided Jupyter Notebooks to examine the Python code, modeling pipeline, and feature engineering logic used to achieve these results.
