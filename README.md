# Customer Retention Decision Analysis

This project compares a simple contract-based churn rule with logistic regression and boosted trees to support a customer retention decision.

The goal is to determine whether predictive models can identify a more concentrated group of likely churners than a simple contract rule can, and whether that improvement is sufficient to support a targeted retention test.

## Methods

Three approaches were compared:

- Contract rule
- Logistic regression
- Boosted trees

The predictive models use seven customer-level inputs: `tenure`, `MonthlyCharges`, `TotalCharges`, `Contract`, `InternetService`, `PaperlessBilling`, and `PaymentMethod`.

The data were split into 60% for training, 20% for validation, and 20% for final testing. The validation set was used to select a method before the final test results were examined.

## Results

| Method | Validation AUC | Final-test AUC | Top-20% observed churn |
| --- | ---: | ---: | ---: |
| Contract rule | 0.7427 | 0.7373 | 39.86% |
| Logistic regression | 0.8384 | 0.8472 | 69.40% |
| Boosted trees | 0.8456 | 0.8497 | 69.40% |

The boosted trees model was selected on validation. On the final test, its performance was very close to logistic regression, so logistic regression remains a useful, simpler benchmark.

## Business interpretation

The boosted-trees top-20% contact list had an observed churn rate of 69.4%.

| Assumed save rate | Net value per 1,000 contacts |
| --- | ---: |
| 10% | -$1,619.93 |
| 15% | $670.11 |
| 20% | $2,960.14 |

The estimated break-even savings rate is approximately 13.54%.

The models estimate churn risk, not the effect of a retention offer. A randomized retention test would be needed to determine whether contacting high-risk customers actually reduces churn.

## Run the analysis

Install the dependencies listed in `VD1_requirements.txt`.

Validation comparison:

```bash
python VD1_analysis.py compare --csv churn.csv --out outputs