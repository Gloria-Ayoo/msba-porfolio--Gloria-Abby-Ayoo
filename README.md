# **TruSource Customer Churn Analysis

### MSBA Portfolio Project | Spring 2026 Case Competition**

##  Business Problem

Customer retention is the primary driver of revenue stability and long-term growth for TruSource.
This project analyzes multi-dimensional customer data—including account plans, service usage, and billing history—to identify behavioral patterns that predict churn risk. 
Our objective was to move beyond standard model performance and surface early warning indicators that allow the TruSource retention team to intervene proactively with high-value, customer-centric actions.

##  Key Results

* **Advanced Predictive Modeling:** Developed an **XGBoost** classification pipeline using SHAP-based feature importance to ensure the model focuses on the most impactful behavioral drivers.  
* **Non-Obvious Insights:** While "Month-to-Month" contracts are a known driver, our analysis revealed a significant churn correlation in long_distance_fees, Customers with high international usage but low technical support engagement, suggesting a segment that feels underserved rather than dissatisfied.  
* **Operational Strategy:** We recommended a "High-Touch" onboarding phase for the first 90 days of a contract, specifically targeting users whose average gb consumption e.g, data consumption spikes and month to month subscribers to automatic payments to increase their length in tru source to indicate a shift in usage habits.

## *How to Run the Code

Follow these steps to replicate the analysis and scoring:

1. **Clone the Repository:** Use GitHub Desktop or run:  
   git clone https://github.com/\[your-username\]/msba-portfolio-yourname.git  
2. **Install Dependencies:** Ensure you have Python installed, then run:  
   pip install pandas xgboost scikit-learn joblib matplotlib seaborn  
3. **Run Model Training:** Open and execute GROUP\_8\_XGBOOST\_MODEL 1.ipynb to see the full pipeline from EDA to model evaluation.  
4. **Generate Predictions:** Use GROUP 8\_SCORED\_HOLDOUT\_CODES.ipynb to apply the saved model bundle (group8\_model\_bundle.pkl) to new holdout data.  
5. **Output:** The final predictions will be saved as group8\_holdout\_scored.csv in the root directory.

## Limitations & Risks

**Model Decay & Market Shifts:** A key risk for stakeholders is **Data Latency**. Because telecommunications is a highly competitive market, pricing changes or new service launches by competitors can rapidly change customer behavior. This model requires a quarterly "Refresh & Retrain" cycle to ensure the features used for prediction still reflect the current market reality.