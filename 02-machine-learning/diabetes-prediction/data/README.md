# Data

This folder holds the two datasets used in the diabetes prediction project.

## Source

The source of the dataset is not disclosed here. To reproduce the project, place `train.csv` and `test.csv` in this folder (see [How to Run](../README.md#17-how-to-run)).

## Files

| File | Rows | Columns | Labels | Use |
|---|---|---|---|---|
| `train.csv` | 614 | 10 | Yes (`diabetes`) | Model training, tuning, validation, and cross-validation |
| `test.csv` | 154 | 9 | No | Final predictions only |

The two files together match an original dataset of 768 rows split 80% / 20%. Because `test.csv` has no labels, test performance cannot be measured. All reported performance comes from cross-validation and a validation split taken from `train.csv`.

## Column Descriptions

Descriptions are based on the column names and how the columns behave in the data. Confirm them against your source documentation if available.

| Column | Type | Description |
|---|---|---|
| `p_id` | Identifier | Unique patient ID. Excluded from model inputs and kept only to label the test predictions |
| `no_times_pregnant` | Count | Number of times pregnant |
| `glucose_concentration` | Numeric | Plasma glucose concentration (described in the notebook as a 2-hour glucose test) |
| `blood_pressure` | Numeric | Blood pressure |
| `skin_fold_thickness` | Numeric | Skin fold thickness |
| `serum_insulin` | Numeric | Serum insulin |
| `bmi` | Numeric | Body mass index |
| `diabetes pedigree` | Numeric | Diabetes pedigree function. Renamed to `pedigree` in the notebook because the original name contains a space |
| `age` | Count (years) | Age |
| `diabetes` | Target (0 or 1) | 1 = diabetic, 0 = not diabetic. Present in `train.csv` only |

## Target Distribution (`train.csv`)

| Class | Count | Share |
|---|---|---|
| 0 (no diabetes) | 400 | 65.1% |
| 1 (diabetes) | 214 | 34.9% |

A model that always predicts "no diabetes" reaches 65.2% accuracy, so this is the baseline to beat.

## Data Quality Notes

**Hidden missing values.** No column has blank cells, but zeros appear in five columns where zero is not possible for a living patient. These were treated as missing and converted to NaN.

| Column | Zeros in train | Missing share (train) | Zeros in test |
|---|---|---|---|
| `glucose_concentration` | 4 | 0.7% | 1 |
| `blood_pressure` | 31 | 5.0% | 4 |
| `skin_fold_thickness` | 187 | 30.5% | 40 |
| `serum_insulin` | 304 | 49.5% | 70 |
| `bmi` | 10 | 1.6% | 1 |

**Other checks**

| Check | Finding | Treatment |
|---|---|---|
| Duplicate rows (train) | 0 | None |
| Outliers (IQR rule) | Small counts, except `pedigree` (26) and `serum_insulin` (20) | Kept, because they look like real patients |
| Skew | `serum_insulin`, `pedigree`, `no_times_pregnant`, `age` are right-skewed | Log transform on `serum_insulin` and `pedigree` |
| Plausible ranges | Age 21 to 81, pregnancies up to 17 | Kept |

The same cleaning rules were applied to both files so they are treated consistently.

## Notes

- Raw files should not be edited. The notebook works on copies.
- This is patient-level medical data. Do not commit the files if they are subject to licence or privacy restrictions, and consider adding `data/*.csv` to `.gitignore`.