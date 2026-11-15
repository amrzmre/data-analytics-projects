# Data Analytics Projects

A collection of data analytics projects covering exploratory data analysis, machine learning, and data visualisation. Each project has its own folder with a README, the analysis (a notebook or a dashboard), and supporting files.

## Projects at a Glance

| # | Project | Type | Main tools | Headline result |
|---|---|---|---|---|
| 1 | [Loan Default Analysis](01-exploratory-data-analysis/loan-default-analysis/) | Exploratory data analysis | Python, pandas, seaborn | Payment difficulty rates differ by age, gender, contract type, and external credit score |
| 2 | [Customer Segmentation](02-machine-learning/customer-segmentation/) | Clustering | Python, scikit-learn | Five credit card customer segments (K-Means, silhouette 0.249) |
| 3 | [Diabetes Prediction](02-machine-learning/diabetes-prediction/) | Classification | Python, scikit-learn | Gradient Boosting catches about 80% of diabetic patients at a 0.30 threshold |
| 4 | [House Price Prediction](02-machine-learning/house-price-prediction/) | Regression | Python, scikit-learn | Ridge Regression explains about 89% of the variation in log price |
| 5 | [Store Sales Forecasting](02-machine-learning/store-sales-forecasting/) | Time series forecasting | Python, statsmodels | Holt-Winters forecasts 30 days of total daily sales with a 7.0% WAPE |
| 6 | [Google Analytics Dashboard](03-data-visualisation/google-analytics-dashboard/) | Dashboard | Google Sheets, Tableau | California generates 41% of US revenue for the Google Merchandise Store (2021) |

## 01 Exploratory Data Analysis

### [Loan Default Analysis](01-exploratory-data-analysis/loan-default-analysis/)

Explores customer characteristics and previous loan application records to find patterns associated with loan payment difficulties, using two datasets from the Home Credit Default Risk data.

- **Data:** 307,511 customers, of whom 8.07% had payment difficulties.
- **What it covers:** data quality checks, univariate and bivariate analysis, grouped variables (age, income, credit amount), and previous application history. The project has 42 figures.
- **Findings:**
  - Cash loans had a higher payment difficulty rate (8.35%) than revolving loans (5.48%).
  - Customers below 30 had the highest rate (11.46%), and customers aged 60 and above had the lowest (4.92%).
  - Customers with payment difficulties had a lower median external credit score (EXT_SOURCE_3: 0.379 vs 0.546).
- **Note:** the findings describe patterns in the data and do not show cause.

## 02 Machine Learning

### [Customer Segmentation](02-machine-learning/customer-segmentation/)

Groups 8,950 active credit card holders by usage behaviour with K-Means clustering, to support targeted offers such as saving plans, loans, and wealth management.

- **Approach:** cleaning, feature selection, log transform and scaling, choosing k with inertia and silhouette scores, and per-cluster evaluation.
- **Result:** five segments (low-activity, regular buyers with a balance, high-limit heavy users, cash advance users, and full payers), with moderate separation (silhouette 0.249).

### [Diabetes Prediction](02-machine-learning/diabetes-prediction/)

Predicts whether a patient has diabetes from eight medical measurements, treated as a screening task where a missed patient is the costly error.

- **Approach:** hidden missing values (zeros), feature engineering, leakage-safe pipelines, model comparison and tuning, and threshold selection with a recall target.
- **Result:** Gradient Boosting at a 0.30 threshold, with an expected recall of 0.80 and precision of 0.62 (cross-validated). Glucose is the most important feature.
- **Note:** this is a learning project and is not a diagnostic tool.

### [House Price Prediction](02-machine-learning/house-price-prediction/)

Predicts the sale price of Iowa homes from 79 features, using data from the Kaggle competition *Housing Prices Competition for Kaggle Learn Users*.

- **Approach:** cleaning, feature engineering (`TotalSF`, `HouseAge`, `TotalBath`), a log-transformed target, and five models compared on a validation split.
- **Result:** Ridge Regression had the lowest validation error (RMSE 0.1367 on the log scale, R² 0.8891). The Kaggle public score was 14,227.48.

### [Store Sales Forecasting](02-machine-learning/store-sales-forecasting/)

Forecasts total daily sales for Corporación Favorita, a grocery retailer in Ecuador, 30 days ahead, using data from the Kaggle competition *Store Sales - Time Series Forecasting*.

- **Approach:** daily aggregation, a regular calendar, a time-based split, and five models (Naive, Seasonal Naive, Moving Average, Linear Regression, and Holt-Winters).
- **Result:** Holt-Winters had the lowest error (WAPE 7.0%), though Seasonal Naive came within 3.8%.
- **Scope:** forecasts total sales only, not each store or product family, and makes no Kaggle submission.

## 03 Data Visualisation

### [Google Analytics Dashboard](03-data-visualisation/google-analytics-dashboard/)

*Where the Revenue Comes From: US Location Analysis of the Google Merchandise Store (2021).* A Tableau dashboard showing where users and revenue come from across US states, built from a Google Analytics location report cleaned in Google Sheets.

- **Approach:** data cleaning with a logged list of actions, validation against the raw file, and four charts (a revenue map, a users vs revenue scatter plot, and top 10 bar charts for users and revenue).
- **Findings:** California generates 41% of revenue, and the top five states generate about 66%. Washington has the highest conversion rate (8.49%) among states with at least 1,000 users.
- **Live dashboard:** [view on Tableau Public](https://public.tableau.com/views/GoogleAnalyticsUS2021_17941114680870/GoogleAnalyticsUnitedStates2021VisualisationbyAmar?:language=en-US&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link)

## Skills Demonstrated

| Area | Where |
|---|---|
| Data cleaning and quality checks | All projects |
| Exploratory analysis and visual storytelling | Loan Default Analysis, Google Analytics Dashboard, EDA sections of every ML project |
| Feature engineering and preprocessing | Customer Segmentation, Diabetes Prediction, House Price Prediction |
| Clustering and cluster evaluation | Customer Segmentation |
| Classification, tuning, and threshold selection | Diabetes Prediction |
| Regression and error analysis | House Price Prediction |
| Time series forecasting and time-based validation | Store Sales Forecasting |
| Dashboard design | Google Analytics Dashboard |

## Tools and Technologies

| Category | Tools |
|---|---|
| Language | Python |
| Data handling | pandas, NumPy |
| Visualisation | Matplotlib, seaborn, Tableau Public |
| Machine learning | scikit-learn |
| Forecasting | statsmodels |
| Data preparation | Google Sheets |
| Environment | Jupyter Notebook, Google Colab |

## Repository Structure

```
data-analytics-projects/
├── README.md
│
├── 01-exploratory-data-analysis/
│   └── loan-default-analysis/
│       ├── README.md
│       ├── loan-default-analysis.ipynb
│       ├── figures/
│       └── data/
│
├── 02-machine-learning/
│   ├── customer-segmentation/
│   │   ├── README.md
│   │   ├── customer-segmentation.ipynb
│   │   ├── figures/
│   │   └── data/
│   ├── diabetes-prediction/
│   │   ├── README.md
│   │   ├── diabetes-prediction.ipynb
│   │   ├── figures/
│   │   └── data/
│   ├── house-price-prediction/
│   │   ├── README.md
│   │   ├── house-price-prediction.ipynb
│   │   ├── figures/
│   │   └── data/
│   └── store-sales-forecasting/
│       ├── README.md
│       ├── store-sales-forecasting.ipynb
│       ├── figures/
│       └── data/
│
└── 03-data-visualisation/
    └── google-analytics-dashboard/
        ├── README.md
        ├── dashboard/
        ├── data/
        └── docs/
```

## Data

Each project's `data/` folder (or its README) explains where the data comes from and how to get it. Some datasets are not included in this repository, because of size, licence, or competition rules, and each project's data README says how to obtain them.

## How to Run

Each project has its own **How to Run** section. In general:

1. Clone the repository:

   ```bash
   git clone https://github.com/<your-username>/data-analytics-projects.git
   cd data-analytics-projects
   ```

2. Open the project folder you are interested in and read its `README.md`.

3. Install the Python libraries used by the notebooks:

   ```bash
   pip install numpy pandas matplotlib seaborn scikit-learn statsmodels jupyter
   ```

4. Download the data as described in the project's data README, then run the notebook from top to bottom.

The Google Analytics dashboard uses Tableau Public, and its README explains how to open the workbook or the live dashboard.

## Author

**Amar**