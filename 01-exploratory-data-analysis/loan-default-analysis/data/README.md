# Data

This folder holds the dataset used in the customer segmentation project.

## Source

The dataset is **not included in this repository**, and its source is not disclosed here. To reproduce the project, place your copy of the file in this folder (see [How to Run](../README.md#how-to-run)).

Expected file name: `CustomerData.csv` (update the path in the notebook if yours differs).

## Overview

| Item | Value |
|---|---|
| Rows | 8,950 (one per credit card holder) |
| Columns | 18 (1 identifier + 17 numerical) |
| Period covered | Last 6 months |
| Unit of analysis | Active credit card holder |
| Target variable | None (unsupervised learning) |

## Column Descriptions

Descriptions are based on the column names and how the columns behave in the data. Confirm them against your source documentation if available.

| Column | Type | Description |
|---|---|---|
| `CUST_ID` | Identifier | Unique customer ID. Not used for clustering |
| `BALANCE` | Amount | Balance left on the account |
| `BALANCE_FREQUENCY` | Ratio (0 to 1) | How often the balance is updated |
| `PURCHASES` | Amount | Total purchase amount |
| `ONEOFF_PURCHASES` | Amount | Largest single purchase amount |
| `INSTALLMENTS_PURCHASES` | Amount | Purchase amount made in instalments |
| `CASH_ADVANCE` | Amount | Cash advanced by the customer |
| `PURCHASES_FREQUENCY` | Ratio (0 to 1) | How often purchases are made |
| `ONEOFF_PURCHASES_FREQUENCY` | Ratio (0 to 1) | How often one-off purchases are made |
| `PURCHASES_INSTALLMENTS_FREQUENCY` | Ratio (0 to 1) | How often instalment purchases are made |
| `CASH_ADVANCE_FREQUENCY` | Ratio (0 to 1) | How often cash advances are taken |
| `CASH_ADVANCE_TRX` | Count | Number of cash advance transactions |
| `PURCHASES_TRX` | Count | Number of purchase transactions |
| `CREDIT_LIMIT` | Amount | Credit limit of the customer |
| `PAYMENTS` | Amount | Total payments made |
| `MINIMUM_PAYMENTS` | Amount | Minimum payments due |
| `PRC_FULL_PAYMENT` | Ratio (0 to 1) | Share of months the full balance was paid |
| `TENURE` | Count (months) | Months as a card holder (6 to 12) |

## Data Quality Notes

| Issue | Detail | Treatment |
|---|---|---|
| Missing `CREDIT_LIMIT` | 1 row (0.01%) | Filled with the median |
| Missing `MINIMUM_PAYMENTS` | 313 rows (3.50%), of which 240 had `PAYMENTS` = 0 | Filled with the median |
| `CASH_ADVANCE_FREQUENCY` above 1 | 8 rows (maximum 1.5) | Capped at 1 |
| Duplicate rows or IDs | None | No action |
| Negative amounts or counts | None | No action |
| Skewed money columns | Skew from 1.52 to 13.85 | Log transform (`log1p`) before scaling |
| `TENURE` concentrated at 12 | 84.7% of customers | Dropped from clustering |

## Notes

- Raw data should not be edited. The notebook keeps an untouched copy (`df_original`) and works on a cleaned copy.
- Do not commit the raw file if it is subject to licence or privacy restrictions. Consider adding `data/*.csv` to `.gitignore`.