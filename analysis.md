# Q1 — Generate

# Q2 — Validate the Analysis

# Q3 — Assess Uncertainty and Value
## Independent Value Check

I independently checked the 15% save-rate scenario using the historical churn rate from the boosted-trees contact list:

Net value per 1,000 contacts = 1,000 × (r × 0.15 × $66 − $6.20)
Using r = 0.693950177935943:

1,000 × (0.693950177935943 × 0.15 × $66 − $6.20) = $670.11

Using the unrounded churn rate from the saved output gives approximately $670 per 1,000 contacts, consistent with the script result of $670.11

# Q4 — Explain Your Choice

## Validation Choice — Recorded Before Final Evaluation

Based on the validation results, I chose to carry forward boosted trees. It had the highest validation AUC at 0.8456, compared with 0.8384 for logistic regression and 0.7427 for the contract rule. The difference between boosted trees and logistic regression is small, so at this point I am treating trees as the validation choice rather than as evidence that the model is clearly better.  However, I don't yet know whether that difference will hold in the final test.
