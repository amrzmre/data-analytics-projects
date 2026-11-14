# Data Dictionary

**File:** `data/processed/Google_Analytics_US_2021_Cleaned.csv`
**Grain:** one row per US state (50 states plus District of Columbia), 51 rows in total
**Period:** 1 January 2021 to 31 December 2021
**Source:** Google Analytics demo account, Google Merchandise Store (web data), Location report for the United States

## Columns

| Column | Type | Unit / format | Raw column | Description | Example (California) |
|---|---|---|---|---|---|
| `State` | Text | State name | `Region` | US state or District of Columbia. Unique per row. Given the *State/Province* geographic role in Tableau | California |
| `Users` | Integer | Count | `Users` | Number of users who had at least one session in the state during 2021 | 74,623 |
| `New Users` | Integer | Count | `New Users` | Users visiting the store for the first time | 71,925 |
| `Sessions` | Integer | Count | `Sessions` | Total visits. One user can have many sessions | 127,192 |
| `Bounce Rate` | Decimal | Proportion from 0 to 1 (format as %) | `Bounce Rate` | Share of sessions that were not engaged, such as a single-page visit with no interaction | 0.2951 (29.51%) |
| `Pages per Session` | Decimal | Pages | `Pages / Session` | Average number of pages viewed per session | 8.1 |
| `Avg Session Duration Sec` | Integer | Seconds | `Avg. Session Duration` | Average length of a session, converted from `hh:mm:ss` | 287 (4 min 47 s) |
| `Transactions` | Integer | Count | `Transactions` | Number of completed purchases | 9,230 |
| `Revenue` | Integer | US dollars, whole dollars | `Revenue` | Total ecommerce revenue. Cents were rounded away | 746386 |
| `Conversion Rate` | Decimal | Proportion from 0 to 1 (format as %) | `Ecommerce Conversion Rate` | Transactions divided by sessions | 0.0726 (7.26%) |

## Notes on interpretation

- **Users are counted per state.** The state rows add up to the report total in this export, but a person who visits from more than one state would be counted in each, so treat the sum as "users across states".
- **Conversion Rate** is based on sessions, not users. For California: 9,230 transactions divided by 127,192 sessions is 7.26%.
- **Revenue** is the Merchandise Store's demo revenue in US dollars, rounded to the nearest dollar.
- Rates for states with few users are unreliable. For example, North Dakota has only 1 transaction.

## Derived fields (calculated in the analysis, not stored in the CSV)

| Field | Formula | Purpose |
|---|---|---|
| Revenue per User | `Revenue / Users` | Shows which states earn more than their traffic suggests (the median state earns about $3.64) |
| Share of Revenue | `State Revenue / sum of all state revenue` | Shows concentration (California is about 41%) |

## Data quality summary

| Check | Result |
|---|---|
| Missing values | None |
| Duplicate states | None |
| Rows | 51 (50 states plus District of Columbia) |
| Excluded from the clean file | Totals row and the `(not set)` row |

See the [data cleaning log](data_cleaning_log.md) for every change made.

## Storage note

In the CSV, `Users`, `New Users`, `Sessions` and `Transactions` contain thousands separators in quotes for values of 1,000 or more (for example `"74,623"`). Most tools read these as numbers, but if a tool imports them as text, remove the commas first.