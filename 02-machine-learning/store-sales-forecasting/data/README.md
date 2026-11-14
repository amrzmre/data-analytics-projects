# Data

This folder is where the datasets for the store sales forecasting project are placed.

## Source

The data comes from the Kaggle competition **Store Sales - Time Series Forecasting** (Corporación Favorita, a large grocery retailer in Ecuador):
<https://www.kaggle.com/competitions/store-sales-time-series-forecasting/data>

The files are **not included in this repository**. `train.csv` alone has about 3 million rows, which is too large to commit comfortably, and the data is subject to the competition rules. To reproduce the project:

1. Sign in to Kaggle and accept the competition rules.
2. Download the files from the competition's Data tab.
3. Place them in this folder.

Check the competition rules for the licence and terms of use. Consider adding `data/*.csv` to `.gitignore` so the files are not committed by mistake.

## Files

| File | Rows | Columns | Used in this project |
|---|---|---|---|
| `train.csv` | 3,000,888 | 6 | Yes. Source of the target and the daily series |
| `stores.csv` | 54 | 5 | Yes, for EDA (store type) |
| `oil.csv` | 1,218 | 2 | Yes, for EDA only (not a model feature) |
| `transactions.csv` | 83,488 | 3 | Yes, for EDA only (not a model feature) |
| `holidays_events.csv` | 350 | 6 | Loaded and checked, but not used in the analysis or models |
| `test.csv` | | | No |
| `sample_submission.csv` | | | No. This project makes no Kaggle submission |

## Column Descriptions

Descriptions are based on the column names and how the columns behave in the data. See the Kaggle data page for the official definitions.

**`train.csv`**

| Column | Description |
|---|---|
| `id` | Row identifier |
| `date` | Date of the record |
| `store_nbr` | Store number (54 stores) |
| `family` | Product family (33 families) |
| `sales` | Sales for that store, family, and date (the target). Reported in units |
| `onpromotion` | Number of items on promotion for that store, family, and date |

**`stores.csv`:** `store_nbr`, `city`, `state`, `type`, `cluster`.

**`oil.csv`:** `date` and `dcoilwtico` (daily oil price). Only weekdays are present.

**`transactions.csv`:** `date`, `store_nbr`, and `transactions` (daily transaction count per store).

**`holidays_events.csv`:** `date`, `type`, `locale`, `locale_name`, `description`, `transferred`.

## Coverage

| Item | Value |
|---|---|
| First date | 2013-01-01 |
| Last date | 2017-08-15 |
| Unique dates | 1,684 (out of 1,688 calendar days) |
| Stores | 54 |
| Product families | 33 |

Row-level `sales` is heavily skewed: median 11, 75th percentile about 196, mean about 358, and maximum 124,717. There are no negative values.

## Data Quality Notes

| Issue | Finding | Treatment |
|---|---|---|
| Missing values and duplicates in `train.csv` | None, including none for (`date`, `store_nbr`, `family`) | None |
| Missing dates | 4 days, all December 25 (2013 to 2016), when stores are closed | Not treated as zero sales. Filled by interpolation in the modelling calendar and flagged |
| Zero sales | 939,130 rows (31.3%). Many are product families that sell rarely, or stores that had not opened yet | Kept, since they are real "nothing sold" values |
| 2013-01-01 | Only 1 of 54 stores reported sales | Removed from the daily series |
| Late-opening stores | Several stores recorded their first sale well after 2013 (for example store 53 in May 2014 and store 52 in April 2017) | No action. The number of stores with sales rises from about 46 to 54 over the period |
| Promotions | `onpromotion` is 0 until 2014-04-01, so before that date 0 means "not recorded" | Promotions are not used as a model feature |
| Oil prices | 43 missing values in the file. Weekends are absent, which produces 481 gaps once joined to a daily calendar | Merge to the daily calendar first, then forward- and back-fill. Used for EDA only |
| Transactions | 2 days (2016-01-01 and 2016-01-03) have no transactions after joining to the daily series | Not used in models |
| Holidays | 38 repeated dates, since national and local events can fall on the same day | Not used |

## Derived Tables

| Table | Rows | Description |
|---|---|---|
| Daily series (`daily`) | 1,683 | Total sales and total items on promotion per date, 2013-01-02 to 2017-08-15 |
| Modelling calendar (`model_df`) | 1,687 | One row per day, including the 4 filled December 25 rows, with `time_index`, `weekday`, and `month` |
| Training part | 1,657 | 2013-01-02 to 2017-07-16 |
| Validation part | 30 | 2017-07-17 to 2017-08-15 |

## Notes

- Raw files should not be edited. All changes go into the derived tables.
- The same date range and cleaning rules apply to every model, so results are comparable.