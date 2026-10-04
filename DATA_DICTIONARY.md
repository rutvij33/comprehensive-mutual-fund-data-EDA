# Data Dictionary

File: `comprehensive_mutual_funds_data.csv` (814 rows, 20 columns, one row per mutual fund scheme)

| Column | Type | Description |
|---|---|---|
| scheme_name | text | Name of the mutual fund scheme (25 names repeat in the raw file) |
| min_sip | int | Minimum SIP amount (INR) |
| min_lumpsum | int | Minimum lumpsum investment (INR) |
| expense_ratio | float | Annual fund management fee (% of assets) |
| fund_size_cr | float | Assets under management (INR crore) |
| fund_age_yr | int | Fund age in years (appears capped at 10, so read it as "10+") |
| fund_manager | text | Name of the fund manager |
| sortino | float | Sortino ratio (return per unit of downside risk) |
| alpha | float | Excess return versus the benchmark |
| sd | float | Standard deviation of returns (volatility) |
| beta | float | Sensitivity to market movements |
| sharpe | float | Sharpe ratio (return per unit of total risk) |
| risk_level | int | Risk label from 1 (lowest) to 6 (highest) |
| amc_name | text | Asset management company (39 in the data) |
| rating | int | Star rating, 0 to 5 (0 most likely means unrated) |
| category | text | Equity, Debt, Hybrid, Solution Oriented, Other |
| sub_category | text | Finer classification (38 values) |
| returns_1yr | float | 1-year return (%) |
| returns_3yr | float | 3-year return (%) |
| returns_5yr | float | 5-year return (%) |

## Known data issues

- `sortino`, `alpha`, `sd`, `beta`, `sharpe` use `"-"` for missing values, so they load as text until converted with `pd.to_numeric(errors="coerce")`.
- `returns_3yr` has 21 missing values and `returns_5yr` has 167, mostly for young funds.
- 25 duplicate `scheme_name` rows; the notebook keeps the first occurrence (789 unique schemes remain).
- `returns_1yr` has extreme values (from -19.7% to 130.8%); 88 fall outside the IQR bounds.
- The meaning of `rating = 0` is an assumption (unrated); the source does not document it.
