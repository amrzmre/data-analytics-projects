# Data Cleaning Log

This log records every change made between the raw export and the cleaned file used in Tableau. Cleaning was done in Google Sheets on a copy of the raw file, which was left unchanged.

| | File | Rows |
|---|---|---|
| Raw | `data/raw/Analytics_Master_View_Location_2021.csv` | 52 regions plus 1 totals row |
| Clean | `data/processed/Google_Analytics_US_2021_Cleaned.csv` | 51 (50 states plus District of Columbia) |

## 1. Initial inspection

| # | Check | Finding |
|---|---|---|
| 1 | File header | The export begins with comment lines (`# 1 Master View`, `# Location`, `# 20210101-20211231`) and a blank line before the real header row |
| 2 | Row count | 52 regions plus a totals row (the totals row has a blank region name) |
| 3 | Regions | 50 states, District of Columbia, and a `(not set)` row |
| 4 | Duplicates | None found. Every region appears once |
| 5 | Missing values | None found in any column |
| 6 | Data formats | Percentages, durations and revenue were stored as text (`29.51%`, `00:04:47`, `$746,386.06`) |

## 2. Cleaning actions

| # | Action | Before | After | Reason |
|---|---|---|---|---|
| 1 | Removed header comment lines and the blank row above the table | Comment lines at the top | Header in row 1 | Needed for the table to import correctly |
| 2 | Froze and formatted the header row, added borders, resized columns | Unformatted | Formatted | Readability while checking data |
| 3 | Removed the **totals row** | Blank region, 265,762 users | Row deleted | It is a summary row, not a region, and would double-count in charts |
| 4 | Removed the **`(not set)`** row | 3,352 users, 4,189 sessions, 128 transactions, $11,760.68 revenue | Row deleted | Google could not assign this traffic to a state, so it cannot be mapped or compared |
| 5 | Kept **District of Columbia** | 1 row | 1 row | It is a valid US location that Tableau can map. This is why the file has 51 rows |
| 6 | Renamed `Region` | `Region` | `State` | Clearer, and used for the Tableau geographic role |
| 7 | Renamed `Pages / Session` | `Pages / Session` | `Pages per Session` | Removed the slash for safer use in tools |
| 8 | Renamed `Ecommerce Conversion Rate` | `Ecommerce Conversion Rate` | `Conversion Rate` | Shorter |
| 9 | Converted **Bounce Rate** to a decimal | `29.51%` | `0.2951` | Numeric value, formatted as a percentage in Tableau |
| 10 | Converted **Conversion Rate** to a decimal | `7.26%` | `0.0726` | Same as above |
| 11 | Converted **Average Session Duration** to seconds and renamed it `Avg Session Duration Sec` | `00:04:47` | `287` | A number can be averaged and charted. The unit is in the column name |
| 12 | Split the currency symbol from **Revenue**, rounded to whole dollars, and stored it as a number | `$746,386.06` | `746386` | Text could not be summed or charted. Cents were dropped for readability |
| 13 | Checked data type of every column | Mixed text and numbers | See the [data dictionary](data_dictionary.md) | Required before importing to Tableau |
| 14 | Exported the result as CSV | | `Google_Analytics_US_2021_Cleaned.csv` | Input for Tableau |

## 3. Validation (raw vs clean)

The two files were compared state by state.

| Check | Result |
|---|---|
| All 50 states present in the clean file | Yes |
| States in the clean file that are not US states | 1 (District of Columbia, kept deliberately) |
| Duplicate states in the clean file | 0 |
| Missing values in the clean file | 0 |
| Leading or trailing spaces in state names | 0 |
| Users, New Users, Sessions, Transactions vs raw | Identical for all 51 states |
| Bounce Rate and Conversion Rate vs raw | Identical (differences are floating point only) |
| Average Session Duration (seconds) vs raw | Identical for all 51 states |
| Revenue vs raw | Within $0.50 per state. Two values differ by half a dollar from standard rounding, likely a rounding-rule difference in Google Sheets |
| Spot check: California | 74,623 users, 746,386 revenue, matches the raw file and the dashboard |

### Reconciliation to the report's totals row

Adding the removed `(not set)` row back to the 51 states reproduces the totals row exactly, which confirms nothing else was lost.

| Metric | 51 states (clean) | `(not set)` | Sum | Raw totals row |
|---|---|---|---|---|
| Users | 262,410 | 3,352 | 265,762 | 265,762 |
| Sessions | 398,867 | 4,189 | 403,056 | 403,056 |
| Transactions | 22,621 | 128 | 22,749 | 22,749 |
| Revenue | $1,842,344.35 (raw) | $11,760.68 | $1,854,105.03 | $1,854,105.03 |

The clean file's revenue sums to **$1,842,342** because of whole-dollar rounding.

## 4. Known limitations of the cleaned data

- `(not set)` traffic (about 1.3% of users and 0.6% of revenue) is excluded.
- Revenue has no cents.
- The Users, New Users, Sessions and Transactions columns are stored with thousands separators in the CSV (for example `"74,623"`). Tableau may read them as numbers, but they may need to be re-exported without separators for other tools.
- Rates for states with very few users (for example North Dakota, 1 transaction) are unreliable.