# Results

## Data preparation

- The supplied analysis required the assigned 7,043-row, 21-column extract and checked the required columns, the Churn class counts, and that `customerID` values were present and unique.
- `TotalCharges` was converted to numeric. The 11 blank `TotalCharges` values (all at zero tenure) were filled with `0.0`; no rows were removed.
- The analysis checked that the numeric and categorical inputs contained no remaining missing values and that numeric inputs were finite.
- No outlier detection, removal, capping, or other outlier treatment was performed.
- For logistic regression and boosted trees, `tenure`, `MonthlyCharges`, and `TotalCharges` were standardized with `StandardScaler`.
- For logistic regression and boosted trees, `Contract`, `InternetService`, `PaperlessBilling`, and `PaymentMethod` were one-hot encoded with unknown categories ignored.

The only missing-data adjustment was filling the 11 blank `TotalCharges` values with 0 because all of them belonged to customers with zero tenure. No rows or outliers were removed; numeric inputs were standardized and categorical inputs were one-hot encoded for the two predictive models.

## AUC across the three methods

| Method | Final-test AUC |
|---|---:|
| Contract rule | 0.7373 |
| Logistic regression | 0.8472 |
| Boosted trees | 0.8497 |

Both predictive models had higher final-test AUC than the contract rule: 0.8472 for logistic regression and 0.8497 for boosted trees, compared with 0.7373 for the contract rule. Boosted trees had the highest AUC, but it was only about 0.0025 higher than logistic regression.

## Uncertainty

| Method | AUC | 95% bootstrap interval |
|---|---:|---:|
| Contract rule | 0.7373 | 0.7163 to 0.7557 |
| Logistic regression | 0.8472 | 0.8254 to 0.8700 |
| Boosted trees | 0.8497 | 0.8283 to 0.8732 |

| Paired AUC comparison | Estimate | 95% bootstrap interval | Interval crosses zero |
|---|---:|---:|---|
| Logistic regression minus contract rule | 0.1099 | 0.0932 to 0.1285 | No |
| Boosted trees minus contract rule | 0.1124 | 0.0959 to 0.1311 | No |

The paired 95% bootstrap intervals for both predictive models relative to the contract rule stayed above zero. This means that the AUC difference versus the contract rule remained positive across the reported intervals for both logistic regression and boosted trees.
