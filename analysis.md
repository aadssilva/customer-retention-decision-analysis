# Q1 - Generate
The analysis compares three methods for ranking customers by churn risk: a contract rule, logistic regression, and boosted trees.

The contract rule assigns a score based on the training-set churn rate for each contract type. Logistic regression and boosted trees use seven inputs: `tenure`, `MonthlyCharges`, `TotalCharges`, `Contract`, `InternetService`, `PaperlessBilling`, and `PaymentMethod`. Numeric variables are standardized and categorical variables are one-hot encoded.

The data were split into 60% training, 20% validation, and 20% final test using a stratified split with seed 0. Training data were used to fit the models, validation data to select a method, and the final test only after that choice was recorded.

Validation AUCs were:

| Method | Validation AUC |
| --- | ---: |
| Contract rule | 0.7427 |
| Logistic regression | 0.8384 |
| Boosted trees | 0.8456 |

## Validation choice (recorded before final evaluation)

I selected boosted trees because it had the highest validation AUC. The gap from logistic regression was small, so I treated it as the method to carry forward, not as evidence that it was clearly better.

# Q2 - Validate the Analysis
The dataset contained 7,043 customers and 21 columns. `customerID` was present and unique. Eleven blank `TotalCharges` values occurred for customers with zero tenure and were replaced with 0.0. No rows or outliers were removed, and the analysis confirmed that the model inputs had no remaining missing or non-finite values.

Final-test results were:

| Method | AUC | Top-20% observed churn |
| --- | ---: | ---: |
| Contract rule | 0.7373 | 39.86% |
| Logistic regression | 0.8472 | 69.40% |
| Boosted trees | 0.8497 | 69.40% |

Both predictive models ranked customers better than the contract rule and produced contact lists with much higher observed churn. The difference between boosted trees and logistic regression was small: 0.0025 AUC, with the same 69.4% observed churn rate in their top-20% lists.

The final test was not used for model selection. The method was chosen on validation before these results were examined.

# Q3 - Assess Uncertainty and Value
The final-test AUC intervals were:

| Method | AUC | 95% bootstrap interval |
| --- | ---: | ---: |
| Contract rule | 0.7373 | 0.7163 to 0.7557 |
| Logistic regression | 0.8472 | 0.8254 to 0.8700 |
| Boosted trees | 0.8497 | 0.8283 to 0.8732 |

Paired comparisons were:

| Comparison | ΔAUC | 95% interval |
| --- | ---: | --- |
| Logistic − Contract | 0.1099 | 0.0932 to 0.1285 |
| Trees − Contract | 0.1124 | 0.0959 to 0.1311 |
| Trees − Logistic | 0.0025 | -0.0049 to 0.0102 |

The intervals for both predictive models versus the contract rule stay above zero. The trees-versus-logistic interval includes zero, so the final test does not establish a reliable difference between those two models.

For boosted trees, predicted and observed churn were reasonably close across most probability groups:

| Predicted-risk group | Predicted | Observed |
| --- | ---: | ---: |
| 0.0–0.2 | 7.96% | 7.18% |
| 0.2–0.4 | 29.78% | 25.37% |
| 0.4–0.6 | 49.63% | 52.12% |
| 0.6–0.8 | 67.69% | 69.01% |
| 0.8–1.0 | 83.45% | 91.43% |

The largest gap was in the highest-risk group, which contained only 35 customers.

For the boosted-trees contact list:

| Assumed save rate | Net value per 1,000 contacts |
| --- | ---: |
| 10% | -$1,619.93 |
| 15% | $670.11 |
| 20% | $2,960.14 |

The break-even save rate is about 13.54%.

## Independent value check

Using the unrounded contact-list churn rate:

`1,000 × (0.693950177935943 × 0.15 × $66 − $6.20) = $670.11`

This matches the script result.

# Q4 - Explain Your Choice
| Method | Final-test AUC | Δ vs. contract | 95% interval | Carry forward |
| --- | ---: | ---: | --- | --- |
| Contract rule | 0.7373 | — | — | No |
| Logistic regression | 0.8472 | +0.1099 | 0.0932 to 0.1285 | Benchmark |
| Boosted trees | 0.8497 | +0.1124 | 0.0959 to 0.1311 | Yes |

I would keep boosted trees for the retention test because it was selected on validation before the final test was opened. The final results support using a predictive model instead of the contract rule, but they do not show a clear advantage for boosted trees over logistic regression.

Logistic regression remains a useful benchmark because it achieved almost the same AUC and the same top-20% observed churn rate with a simpler model.

The larger uncertainty is whether contacting these customers will actually prevent churn. The models predict who is likely to churn; they do not estimate the effect of the retention offer.

I would therefore test the campaign experimentally. I would change the recommendation if the measured save rate falls below the 13.54% break-even point. I would also reconsider the production model if logistic regression delivers similar campaign results with less complexity.


