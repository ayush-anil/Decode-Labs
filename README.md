# DecodeLabs — Project 1: Advanced EDA & Feature Engineering

**Program:** DecodeLabs Data Science Industrial Training Kit, Batch 2026
**Author:** Ayush Badgujar

## Overview

This project transforms a raw e-commerce orders dataset (1,200 rows, 14 columns)
into a clean, feature-engineered dataset ready for machine learning — covering
statistical imputation, outlier handling, and feature engineering.

## Dataset

`Dataset_for_Data_Analytics.xlsx` — e-commerce order records including order date,
customer ID, product, quantity, pricing, shipping, payment method, order status,
coupon usage, and referral source.

## What This Project Does

### 1. Missing Data Handling
- `CouponCode` was missing for 309 of 1,200 orders (~26%).
- Rather than applying blind statistical imputation, the missingness was diagnosed
  first: this is a categorical field where "missing" almost certainly means
  *no coupon was used* — not a data quality issue.
- Handled by filling with an explicit `"NoCoupon"` category, and adding a derived
  `has_coupon` binary flag.

### 2. Outlier Detection & Treatment (IQR Method)
- Checked `Quantity`, `UnitPrice`, `ItemsInCart`, and `TotalPrice` for outliers
  using the Interquartile Range method.
- Only `TotalPrice` had outliers (8 rows) — the other columns were naturally
  bounded with no extreme values.
- Outliers were **capped (winsorized)**, not dropped, to preserve the full
  dataset size (1,200 rows retained).

### 3. Feature Engineering
Four new features were engineered from the existing columns:
| Feature | Description |
|---|---|
| `has_coupon` | Binary flag — whether a coupon was applied to the order |
| `order_month` | Month extracted from the order date (seasonality signal) |
| `order_dayofweek` | Day of week extracted from the order date |
| `cart_fill_ratio` | `Quantity` ÷ `ItemsInCart` — how much of the cart was purchased |
| `customer_order_count` | Number of orders placed by that customer (repeat-buyer signal) |

### 4. Categorical Encoding
`Product`, `PaymentMethod`, `OrderStatus`, and `ReferralSource` were one-hot
encoded to prepare the dataset for downstream ML models.

## Files

- `decodelabs_project1.ipynb` — full notebook, run top to bottom
- `Dataset_for_Data_Analytics.xlsx` — original raw dataset
- `ecommerce_cleaned_features.csv` — cleaned, feature-engineered output

## Tools Used

Python, pandas, NumPy, matplotlib, seaborn

## Key Takeaway

Not all missing data should be imputed the same way — recognizing *why* data is
missing (random noise vs. a meaningful business signal) changes the correct
handling strategy. Outlier treatment (capping vs. dropping) was chosen to
preserve sample size on a moderately-sized dataset.
