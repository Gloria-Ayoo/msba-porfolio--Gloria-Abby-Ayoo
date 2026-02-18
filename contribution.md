Individual Contribution Report

Student Name: Gloria Abby Ayoo

Project: TruSource Churn Analysis (Spring 2026 Case Competition)

1. What did you personally contribute to the most?

My primary contribution was leading the modeling strategy and technical implementation.
I was responsible for moving the team beyond basic exploratory analysis into a robust machine-learning pipeline focused on Ensemble Methods with Decision Trees.

Interpretability with SHAP: I implemented the SHAP (SHapley Additive Explanations) framework to interpret our XGBoost model.
This turned complex ensemble predictions into the "non-obvious" business insights requested by the CEO, such as identifying specific interaction effects between service usage and tenure.

2. What is one decision or approach you personally advocated for and why?

I personally advocated for using XGBoost (Extreme Gradient Boosting) as our primary ensemble method over simpler models like Logistic Regression or standalone Decision Trees.

Why I advocated for this Ensemble Approach:

The Power of Boosting: Unlike a single Decision Tree, which is prone to high variance and overfitting, XGBoost is an ensemble method that builds trees sequentially. Each new tree corrects the errors of the previous ones, leading to a much more stable and accurate prediction.

Handling Non-Linear Interactions: TruSource's churn behavior is rarely driven by a single factor. XGBoost’s gradient boosting framework is exceptionally good at capturing non-linear patterns, such as how the impact of a "Late Fee" changes exponentially as a customer nears the end of their contract.

Predictive Power (AUC): We settled on XGBoost because it consistently delivered a higher AUC (Area Under the Curve) in our cross-validation tests. A higher AUC ensured that our "churn probability" scores were reliable enough for the retention team to use in financial forecasting.

Efficiency and Regularization: XGBoost has built-in $L1$ and $L2$ regularization. I used this to "prune" the trees automatically, ensuring that our ensemble didn't become too complex or sensitive to noise in the TruSource dataset.

3. Lessons Learned & Future Improvements

Technical Deep Dive: Hyperparameter Tuning for Ensembles
To ensure the model was both accurate and generalizable, I conducted extensive hyperparameter tuning using randomized search. The key parameters I "tricked" and tuned included:

n_estimators (e.g., 500-1000 with Early Stopping): I used a large ensemble of trees but implemented Early Stopping. This allowed the model to stop adding trees as soon as the validation error plateaued, preventing the ensemble from overfitting.

max_depth (3-6): I intentionally kept the depth of the individual decision trees shallow. In an ensemble, the "strength" comes from the combination of many "weak" learners. Keeping depth low ensures the model learns general trends rather than memorizing specific outliers.

learning_rate (eta: 0.01 - 0.1): I opted for a lower learning rate. This requires the ensemble to use more trees to reach an answer, which typically results in a more robust model that generalizes better to new data.

gamma and lambda (Regularization): I tuned these to penalize tree complexity. This was my primary tool for managing the bias-variance tradeoff, ensuring the ensemble remained stable even with high-dimensional input data.

scale_pos_weight: This was crucial for handling class imbalance. By up-weighting the minority "Churn" class, I ensured the ensemble prioritized catching potential churners (High Recall).

Lessons Learned:

Interpretability is as important as Accuracy: I learned that even the most complex ensemble is only valuable if you can explain it. SHAP values allowed us to "deconstruct" the XGBoost decisions for the executive team.

The Importance of Holdout Strategy: Seeing our ensemble maintain its AUC on the holdout set validated our tuning and regularization choices.

What I would add/improve with more time:

External Data Integration: I would join internal billing data with external market data to see if competitive pressure is a bigger driver of churn than service quality.

Automated Re-training Pipeline: I would transition this from a static notebook to a scheduled cloud workflow that automatically retrains the ensemble as new billing cycles are completed.