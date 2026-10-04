# Comprehensive Mutual Funds EDA (India)

Exploratory data analysis of **814 Indian mutual fund schemes** (789 after removing duplicates), built with only **NumPy, pandas and Matplotlib**. No seaborn, scipy or scikit-learn: correlations use rank-based Spearman, confidence intervals come from a manual bootstrap, and K-Means and PCA are implemented from scratch.

## Questions explored

| # | Question | Method |
|---|---|---|
| 1 | How clean is the data? | Missing values, `"-"` placeholders, duplicates, IQR outliers |
| 2 | Does a higher expense ratio buy better returns? | Spearman correlation with bootstrap CI, per category |
| 3 | Which funds are risk-adjusted winners vs overrated? | Rank-shift slope chart on Sharpe vs 3-year return |
| 4 | Do star ratings mean anything? | Boxplots, bootstrap median differences, linear regression via `np.linalg.lstsq` |
| 5 | What natural fund "types" exist? | K-Means from scratch plus PCA via `np.linalg.svd` |
| 6 | Is there a fund-size sweet spot? | Log-size buckets, median alpha with bootstrap bands |
| 7 | Which AMCs are the most consistent? | AMC scorecard, heatmap, bubble chart |
| 8 | Do fund managers show skill? | Performance relative to sub-category median |
| 9 | Do official risk labels match real volatility? | Risk level vs standard deviation, understated-risk flags |
| 10 | Which funds suit a small SIP investor? | Rule-based beginner shortlist |
| 11 | Are returns accelerating or fading? | 1-year vs 5-year trajectory labels |
| 12 | Can the above become a recommender? | Weighted scoring function |

## Selected results

- **Outliers:** the IQR rule flags 88 funds for 1-year returns outside -4.4% to 11.6%.
- **Risk vs return:** Spearman correlation between risk level and 1-year return is -0.33, so higher labelled risk did not mean higher recent returns.
- **Expense ratio:** the relationship depends on category. For Equity, higher fees go with lower 5-year returns (r = -0.42). For Debt, higher fees go with higher 3-year returns (r = +0.58). Do not read it as one universal rule.
- **Ratings:** in the regression, Sharpe ratio carries the largest positive weight on rating, while raw 3-year return carries a negative one, so ratings look risk-adjusted rather than return-driven.
- **Clusters:** four fund groups emerged (313, 164, 167 and 101 funds), from low-volatility debt-like funds to a high-alpha group dominated by sectoral and thematic funds.
- **Trajectory:** of 631 funds with all three horizons, about 68% are fading (short-term return below long-term), 27% steady and 5% accelerating.
- **Risk labels:** 55 funds carry a low risk label (<= 3) but have above-75th-percentile volatility in their category.
- **Beginner shortlist:** only 32 funds pass all filters (SIP <= 500, below-median fees, rating >= 4, risk <= 4, positive alpha).

## Charts

| | |
|---|---|
| ![Outliers](images/01_returns_1yr_outliers.png) | ![Ratings](images/02_rating_vs_metrics.png) |
| ![Clusters](images/03_fund_clusters_pca.png) | ![AMCs](images/04_amc_bubble_chart.png) |
| ![Risk](images/05_risk_level_vs_sd.png) | ![Trajectory](images/06_return_trajectories.png) |

## Project structure

```
mutual-fund-eda/
├── data/
│   ├── comprehensive_mutual_funds_data.csv
│   └── DATA_DICTIONARY.md
├── notebooks/
│   └── comprehensive_mutual_fund_eda.ipynb
├── images/                 # charts used in this README
├── requirements.txt
├── .gitignore
├── LICENSE
└── README.md
```

## How to run

```bash
git clone https://github.com/<your-username>/mutual-fund-eda.git
cd mutual-fund-eda
python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate
pip install -r requirements.txt
cd notebooks
jupyter notebook comprehensive_mutual_fund_eda.ipynb
```

Run the notebook from the `notebooks/` folder so the relative data path (`../data/`) resolves. Matplotlib 3.9 or newer is needed for the `tick_labels` argument.

## Data notes

Column descriptions and known issues are in [`data/DATA_DICTIONARY.md`](data/DATA_DICTIONARY.md). Main caveats: missing values are stored as `"-"`, `fund_age_yr` looks capped at 10, and `rating = 0` is assumed to mean unrated.

## Limitations

- A single snapshot, so no time series; "trajectory" compares return horizons, not history.
- Correlations are descriptive, not causal.
- Fund-level metrics (alpha, beta, Sharpe) depend on each provider's benchmark and window.
- Some analyses drop rows with missing values, so sample sizes vary by section.

## Disclaimer

This project is for learning and exploration only. It is **not** investment advice.

## License

MIT, see [LICENSE](LICENSE). Replace `<YOUR NAME>` in the license file with your name before publishing.
