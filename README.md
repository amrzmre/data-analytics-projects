# Data Analytics Projects

A collection of data analytics projects covering exploratory data analysis, machine learning, data visualisation, and product usage analytics. Each project has its own folder with a README, the analysis (a notebook or a dashboard), and supporting files.

## Projects at a Glance

| # | Project | Type | Main tools | Headline result |
|---|---|---|---|---|
| 1 | [Loan Default Analysis](01-exploratory-data-analysis/loan-default-analysis/) | Exploratory data analysis | Python, pandas, seaborn | Payment difficulty rates differ by age, gender, contract type, and external credit score |
| 2 | [Customer Segmentation](02-machine-learning/customer-segmentation/) | Clustering | Python, scikit-learn | Five credit card customer segments (K-Means, silhouette 0.249) |
| 3 | [Diabetes Prediction](02-machine-learning/diabetes-prediction/) | Classification | Python, scikit-learn | Gradient Boosting catches about 80% of diabetic patients at a 0.30 threshold |
| 4 | [House Price Prediction](02-machine-learning/house-price-prediction/) | Regression | Python, scikit-learn | Ridge Regression explains about 89% of the variation in log price |
| 5 | [Store Sales Forecasting](02-machine-learning/store-sales-forecasting/) | Time series forecasting | Python, statsmodels | Holt-Winters forecasts 30 days of total daily sales with a 7.0% WAPE |
| 6 | [Google Analytics Dashboard](03-data-visualisation/google-analytics-dashboard/) | Dashboard | Google Sheets, Tableau | California generates 41% of US revenue for the Google Merchandise Store (2021) |
| 7 | [Candidate Search Tool Analytics](04-candidate-search-tool-analytics/) | Product usage analytics | Python, Google Colab | The top 10% of accounts use 91.6% of credits, and 75.1% of accounts never connect |

## 01 Exploratory Data Analysis

### [Loan Default Analysis](01-exploratory-data-analysis/loan-default-analysis/)

**Problem.** Lenders need to understand which customers struggle to repay their loans. Which characteristics differ between customers with and without payment difficulties, and does previous application history add any insight?

**How I solved it.**

1. Inspected two datasets from the Home Credit Default Risk data (current applications and previous applications) and checked missing values, duplicates, and outliers.
2. Created grouped variables such as age, income, and credit amount groups.
3. Compared the two customer groups with univariate, bivariate, and grouped analyses.
4. Examined previous application outcomes alongside payment difficulties.
5. Presented the results in 42 figures.

**Result.** The data covers 307,511 customers, of whom 8.07% had payment difficulties.

- Cash loans had a higher payment difficulty rate (8.35%) than revolving loans (5.48%).
- Customers below 30 had the highest rate (11.46%), and customers aged 60 and above had the lowest (4.92%).
- Customers with payment difficulties had a lower median external credit score (EXT_SOURCE_3: 0.379 vs 0.546).

These are patterns in the data and do not show cause.

## 02 Machine Learning

### [Customer Segmentation](02-machine-learning/customer-segmentation/)

**Problem.** A credit card business sends the same offers to every customer. It wants to group active card holders by how they use their cards, so offers such as saving plans, loans, and wealth management can be targeted.

**How I solved it.**

1. Cleaned 8,950 customer records: filled the few missing values with medians and capped impossible frequency values at 1.
2. Used EDA to find heavy skew and overlapping features, then kept 10 behaviour features.
3. Log-transformed the money columns and scaled all features.
4. Chose k = 5 using the elbow method, silhouette scores, and cluster sizes.
5. Trained K-Means, evaluated each cluster, and profiled the segments in real amounts.

**Result.** Five segments: low-activity, regular buyers with a balance, high-limit heavy users, cash advance users, and full payers. Separation is moderate (silhouette 0.249), so the segments describe tendencies, not sharp boundaries.

### [Diabetes Prediction](02-machine-learning/diabetes-prediction/)

**Problem.** Screen patients for diabetes from eight medical measurements. A missed diabetic patient is a worse error than a false alarm, so the goal is to catch most diabetic patients while keeping false alarms as low as possible.

**How I solved it.**

1. Treated impossible zeros (for example in insulin and skin fold thickness) as missing values.
2. Engineered features, including missing-value flags, log transforms, and age, BMI, and glucose groups.
3. Built pipelines with median imputation and scaling, so no information leaks from validation data.
4. Compared five models against a baseline with stratified cross-validation, then tuned Logistic Regression and Gradient Boosting.
5. Tried an F2-based threshold, found it flagged almost everyone, and switched to a recall-target threshold chosen on out-of-fold predictions.
6. Chose between the two final models with repeated cross-validation and explained the result with permutation importance.

**Result.** Gradient Boosting at a 0.30 threshold, with an expected recall of 0.80 and precision of 0.62 (cross-validated). Glucose is the most important feature. This is a learning project and not a diagnostic tool.

### [House Price Prediction](02-machine-learning/house-price-prediction/)

**Problem.** Predict the sale price of a home from its size, quality, location, and age, so buyers, sellers, and lenders can judge whether a price is reasonable. The project also asks which features matter most and where the model makes its largest errors.

**How I solved it.**

1. Cleaned the Kaggle data: separated "no such feature" values from real gaps and removed two extreme outliers from the training set.
2. Explored the target and the features most related to price, and built three new features (`TotalSF`, `HouseAge`, `TotalBath`).
3. Modelled `log(SalePrice)` to handle the skew, with one-hot encoded categories.
4. Compared five models on the same validation split and studied the residuals and largest errors.
5. Refit the best model on all training data, checked the test predictions, and submitted them to Kaggle.

**Result.** Ridge Regression had the lowest validation error (RMSE 0.1367 on the log scale, R² 0.8891). Its largest errors are overestimates of low-priced houses. The Kaggle public score was 14,227.48.

### [Store Sales Forecasting](02-machine-learning/store-sales-forecasting/)

**Problem.** Forecast total daily sales for a large grocery retailer 30 days ahead, to help plan stock, staff, and promotions.

**How I solved it.**

1. Summed store-level sales into one daily series, removed an incomplete first day, and filled the four closed December 25 dates to make a regular calendar.
2. Explored the trend, weekly and yearly patterns, promotions, and other signals to decide what could be used.
3. Held out the last 30 days as a time-based validation set, so no future information reaches the models.
4. Built five models (Naive, Seasonal Naive, Moving Average, Linear Regression, and Holt-Winters) and scored them with MAE, RMSE, WAPE, and bias.
5. Chose the best model and stated how close the top two were.

**Result.** Holt-Winters had the lowest error (WAPE 7.0%, about 58,000 units on an average of about 833,000 per day), though Seasonal Naive came within 3.8%. The project forecasts total sales only, not each store or product family, and makes no Kaggle submission.

## 03 Data Visualisation

### [Google Analytics Dashboard](03-data-visualisation/google-analytics-dashboard/)

**Problem.** An ecommerce team wants to know which US states drive traffic and sales for the Google Merchandise Store, and whether more visitors always means more revenue.

**How I solved it.**

1. Imported a Google Analytics location report into Google Sheets and cleaned it on a copy, with every action logged.
2. Removed the totals row and the unassignable `(not set)` row, and converted revenue, rates, and durations to numbers.
3. Validated the cleaned file against the raw file.
4. Built a Tableau dashboard with four charts: a revenue map, a users vs revenue scatter plot, and top 10 bar charts for users and revenue.

**Result.** California generates 41% of revenue, and the top five states generate about 66%. Users and revenue are very strongly correlated (about 0.99), but conversion differs by state. Washington has the highest conversion rate (8.49%) among states with at least 1,000 users.

**Live dashboard:** [view on Tableau Public](https://public.tableau.com/views/GoogleAnalyticsUS2021_17941114680870/GoogleAnalyticsUnitedStates2021VisualisationbyAmar?:language=en-US&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link)

## 04 Product Usage Analytics

### [Candidate Search Tool Analytics](04-candidate-search-tool-analytics/)

Analyses how hirers used a candidate search tool over three months (November 2014 to January 2015): engagement, how they connect with candidates, how credits are spent, and which accounts drive that spend.

- **Data:** 1,531 usage rows covering 241 accounts and 507 users. The dataset comes from a case study and is not shared.
- **Approach:** data checks, engagement overview, connection behaviour, credit consumption, account segmentation by credit use, user behaviour within accounts, monthly trends, and a risk summary. The notebook runs in Google Colab.
- **Findings:**
  - 24.9% of accounts made at least one connection, and 75.1% never did.
  - CV access is 83.1% of connections and 95.4% of credits.
  - The top 10% of accounts (24) used 91.6% of credits, and one account used 44.4%.
- **Report:** [view the presentation](https://docs.google.com/presentation/d/e/2PACX-1vTIiT7MfzYMBrBr64b9Ah-sVCsdEefAASwz3RcwxQYz-KTT954hWZz7FhUaz2gY0GfE52SXxShVE1g2/pub?start=false&loop=false&delayms=3000&slide=id.p)

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
| Product usage analysis, segmentation by rules, and risk signals | Candidate Search Tool Analytics |

## Tools and Technologies

| Category | Tools |
|---|---|
| Language | Python |
| Data handling | pandas, NumPy |
| Visualisation | Matplotlib, seaborn, Tableau Public |
| Machine learning | scikit-learn |
| Forecasting | statsmodels |
| Data preparation and export | Google Sheets, gspread |
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
├── 03-data-visualisation/
│   └── google-analytics-dashboard/
│       ├── README.md
│       ├── dashboard/
│       ├── data/
│       └── docs/
│
└── 04-candidate-search-tool-analytics/
    ├── README.md
    ├── candidate-search-tool-analytics.ipynb
    ├── figures/
    └── data/
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

The Google Analytics dashboard uses Tableau Public, and its README explains how to open the workbook or the live dashboard. The candidate search tool notebook is written for Google Colab and loads its data by file upload.

## Author

**Amar**