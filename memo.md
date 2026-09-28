# Memo

**To:** Devon Achebe, VP Customer Retention  
**Subject:** Retention campaign test recommendation

I recommend testing a retention campaign using the boosted-trees model that was used to build the contact list.

On the final test, boosted trees reached an AUC of 0.8497, compared with 0.8472 for logistic regression and 0.7373 for the contract rule. Among the top 20% of customers ranked by boosted trees, 69.4% actually churned. That gives us a much more concentrated group to target than the contract rule.

The economics are less certain. With the current cost and retention assumptions, the campaign breaks even at about a 13.54% save rate. A 15% save rate would produce about $670 in net value per 1,000 contacts. At 10%, the campaign would lose money. This is why I would test the campaign before rolling it out more broadly.

There is also little separation between boosted trees and logistic regression. Their final-test AUCs were nearly identical, and both identified a top-20% group with an observed churn rate of 69.4%. I would keep logistic regression as a simpler benchmark for testing.

The main limitation is that these models predict who is likely to churn, not who will stay because we contact them. I would change the recommendation if a randomized test shows that the campaign cannot generate a save rate above the 13.54% break-even point. I would also reconsider the model choice if logistic regression delivers similar campaign results with less complexity.