# Data

This folder is for the dataset used in the candidate search tool analytics project.

## Source

The dataset comes from a case study and is **not shared**. It is not included in this repository, and its source is not disclosed here. It contains account and user identifiers, so it should not be published.

To run the notebook, you need your own copy of the file. The notebook expects an Excel file named `Case_Study_Data.xlsx` and loads it by upload in Google Colab (see [Loading the Data in Colab](#loading-the-data-in-colab)).

## Overview

| Item | Value |
|---|---|
| File | `Case_Study_Data.xlsx` (not included) |
| Rows | 1,531 |
| Columns | 12 |
| Period | November 2014 to January 2015 (3 months) |
| Unique accounts | 241 |
| Unique users | 507 |
| Unique job ads | 1,288 |
| Target variable | None (descriptive analysis) |

The data records how hirers used a candidate search tool. Each row appears to cover one account, user, and job ad in one month. This is inferred from the repeated account and user IDs and the one-per-row job ad ID, so confirm it against your source documentation.

## Column Descriptions

Descriptions are based on the column names and how the columns behave in the data. The notebook renames the columns to snake case.

| Original column | Notebook name | Type | Description |
|---|---|---|---|
| `Month` | `month` | Date | First day of the month (2014-11-01, 2014-12-01, 2015-01-01) |
| `Account ID` | `account_id` | Identifier | Customer account (241 accounts) |
| `User ID` | `user_id` | Identifier | User within an account (507 users) |
| `Job Ad (Role) ID` | `job_ad_id` | Identifier | Job advert or role (1,288 job ads) |
| `Sessions` | `sessions` | Count | Number of sessions (minimum 1, maximum 80) |
| `Message Sends` | `msg_sends` | Count | Messages sent to candidates |
| `Job Sends` | `job_sends` | Count | Job adverts sent to candidates |
| `CVs Accessed` | `cv_accessed` | Count | Candidate CVs accessed |
| `Message Credits Used` | `msg_credits` | Count | Credits used on messages |
| `Send Job Credits Used` | `job_credits` | Count | Credits used on job sends |
| `Access CV Credits Used` | `cv_credits` | Count | Credits used on CV access |
| `Total Credits Used` | `total_credits` | Count | Total credits used |

## Credit Rules

The credit columns follow fixed prices, which the notebook checks on every row:

| Action | Credits |
|---|---|
| Message send | 200 |
| Job send | 100 |
| CV access | 500 |

For every row, `msg_sends` x 200 + `job_sends` x 100 + `cv_accessed` x 500 equals `total_credits`. No row breaks this rule.

## Data Quality Notes

| Check | Finding | Treatment |
|---|---|---|
| Missing values | None in any column | None |
| Duplicate rows | None | None |
| Negative values | None | None |
| Credit math | 0 rows where the total does not match the action counts | None |
| Rows with zero sessions | None | None. Every row has at least one session, so a row exists only when a user had a session |
| Column names | Spaces and mixed case | Renamed to snake case |

Because every row has at least one session, there are no rows for users with zero sessions. This means a "session to connection" conversion rate cannot be measured from this data (see the main README).

## Derived Columns

Four columns were added in the notebook:

| Column | Definition |
|---|---|
| `month_label` | Readable month, for example "Nov 2014" |
| `connection_made` | 1 if `total_credits` is above 0 in that row, otherwise 0 |
| `total_connections` | `msg_sends` + `job_sends` + `cv_accessed` |
| `dominant_method` | The connection type used most in the row, or `none` if no connection was made |

An account is called **active** if it made at least one connection in the three months (credits above 0), and **passive** otherwise.

## Loading the Data in Colab

The notebook runs in Google Colab and loads the file by upload:

```python
from google.colab import files
import pandas as pd

uploaded = files.upload()          # choose Case_Study_Data.xlsx
file_name = next(iter(uploaded))   # use the name Colab actually saved
df = pd.read_excel(file_name)
```

Using the uploaded file name avoids a common problem: if a file with the same name already exists in the Colab session, Colab saves the new upload as `Case_Study_Data (1).xlsx`, and reading `Case_Study_Data.xlsx` would load the old copy.

## Output Tables

The last cell of the notebook exports five summary tables to a Google Sheets workbook, for use in a dashboard:

| Sheet | Content |
|---|---|
| `main` | The cleaned main table with the derived columns |
| `credits_per_account` | Total credits, segment, and unique job ads per account |
| `monthly_summary` | Credits, active accounts, active users, and connections per month |
| `credits_per_user` | Credits per user within each account |
| `method_summary` | Credits and connections by method |

This export is optional. It needs Google authentication in Colab and writes to your own Google Drive.

## Notes

- The raw file should not be edited. The notebook works on the loaded table and adds derived columns.
- Do not commit the data file or the exported workbook, because they contain account and user identifiers. Consider adding `*.xlsx` and `*.csv` to `.gitignore`.