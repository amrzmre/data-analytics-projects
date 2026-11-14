# Data

This folder is where the datasets for the house price prediction project are placed.

## Source

The data comes from the Kaggle competition **Housing Prices Competition for Kaggle Learn Users**:
<https://www.kaggle.com/competitions/home-data-for-ml-course/data>

The files are **not included in this repository**. To reproduce the project:

1. Sign in to Kaggle and accept the competition rules.
2. Download `train.csv` and `test.csv` from the competition's Data tab.
3. Place both files in this folder.

Check the competition rules for the licence and terms of use. The competition page also provides a data description file with the full definition of every column and category code.

Consider adding `data/*.csv` to `.gitignore` so the files are not committed by mistake.

## Files

| File | Rows | Columns | Target (`SalePrice`) | Use |
|---|---|---|---|---|
| `train.csv` | 1,460 | 81 | Yes | Training, validation, and EDA |
| `test.csv` | 1,459 | 80 | No | Final predictions for the Kaggle submission |

The 81 columns of `train.csv` are `Id`, 79 explanatory variables describing the homes, and the target `SalePrice`. `test.csv` has the same columns without `SalePrice`. Of the 81 columns, 35 are integer, 3 are float, and 43 are text.

## Column Overview

The 79 explanatory variables cover almost every aspect of a home. Descriptions below are based on the column names. See the Kaggle data description for exact definitions.

| Group | Example columns |
|---|---|
| Identifier and target | `Id`, `SalePrice` |
| Lot and location | `LotArea`, `LotFrontage`, `Neighborhood`, `MSZoning`, `Street`, `Alley` |
| Building type and style | `MSSubClass`, `BldgType`, `HouseStyle`, `YearBuilt`, `YearRemodAdd` |
| Quality and condition | `OverallQual`, `OverallCond`, `ExterQual`, `KitchenQual`, `HeatingQC` |
| Size and layout | `GrLivArea`, `1stFlrSF`, `2ndFlrSF`, `TotRmsAbvGrd`, `BedroomAbvGr`, `FullBath`, `HalfBath` |
| Basement | `TotalBsmtSF`, `BsmtQual`, `BsmtFinType1`, `BsmtFullBath` |
| Garage | `GarageType`, `GarageYrBlt`, `GarageCars`, `GarageArea`, `GarageQual` |
| Extras | `Fireplaces`, `PoolArea`, `PoolQC`, `Fence`, `MiscFeature`, `WoodDeckSF` |
| Sale | `MoSold`, `YrSold`, `SaleType`, `SaleCondition` |

## Target (`SalePrice`)

| Statistic | Value |
|---|---|
| Minimum | 34,900 |
| Median | 163,000 |
| Mean | about 180,921 |
| Maximum | 755,000 |
| Skewness | 1.88 (strongly right-skewed) |

After a log transform the skewness is 0.12, so the project models `log(SalePrice)`.

## Data Quality Notes

**Missing values.** Most missing values do not mean the data is absent. They mean the house has no such feature.

| Columns (train missing count) | Meaning of missing | Treatment |
|---|---|---|
| `PoolQC` (1,453), `MiscFeature` (1,406), `Alley` (1,369), `Fence` (1,179), `FireplaceQu` (690) | No pool, feature, alley, fence, or fireplace | Filled with `None` |
| `MasVnrType` (872) | pandas reads the text "None" as empty, so most are not true gaps | Filled with `None` |
| `GarageType`, `GarageFinish`, `GarageQual`, `GarageCond` (81 each) | No garage | Filled with `None` |
| `BsmtQual`, `BsmtCond`, `BsmtFinType1` (37 each), `BsmtExposure`, `BsmtFinType2` (38 each) | No basement | Filled with `None` |
| `MasVnrArea` (8) and basement or garage numeric columns that are missing in test | No veneer, basement, or garage | Filled with 0 |
| `GarageYrBlt` (81) | No garage, so no garage year | Filled with `YearBuilt` |
| `LotFrontage` (259), `Electrical` (1), and a few test columns such as `MSZoning` and `KitchenQual` | True gaps | Text: most common value in train. Numbers: median of train |

After cleaning, neither file has missing values. All fill values come from the training data.

**Other issues**

| Issue | Finding | Treatment |
|---|---|---|
| Duplicates | None (rows or `Id`) | None |
| Type | `MSSubClass` is a building type code, not a quantity | Converted to text |
| Outliers in train | `Id` 524 (`GrLivArea` 4,676, price 184,750) and `Id` 1299 (`GrLivArea` 5,642, price 160,000): very large and cheap | Removed from train (1,460 to 1,458 rows) |
| Negative `HouseAge` in test | `Id` 2550 was built in 2008 and sold in 2007 (`SaleType` is `New`) | Age set to 0 |
| Impossible `GarageYrBlt` in test | `Id` 2593 has a garage year of 2207 (house built 2006, sold 2007) | Replaced with `YearBuilt` |
| Unusual test house | `Id` 2550 has a `TotalSF` of 10,190, far above any house in train (maximum 6,872) | Prediction capped at the highest train price (755,000) |

No rows were removed from `test.csv`, because Kaggle needs a prediction for every `Id`.

## Notes

- Raw files should not be edited. The notebook works on copies (`train_clean`, `test_clean`).
- The same cleaning rules are applied to both files so they are treated consistently.