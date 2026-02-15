Track 1: Where does the data come from?

In a professional setting at TruSource, we wouldn't start with a clean CSV. The data used for our XGBoost model is a "final feature set" synthesized from a complex database ecosystem. To build this pipeline from scratch, I would pull from the following internal sources.

1. The Raw Ingredients: Source Tables

If I were building the TruSource data pipeline, I would expect to integrate these five source tables:

Customer Master

What one row represents: One unique customer.

Importance: Provides static demographics such as Age, Gender, and Marital Status.

Billing & Transactions

What one row represents: One monthly invoice or transaction.

Importance: Captures financial health, total_billed, and pay_method history.

Service Inventory

What one row represents: One active plan, contract, or add-on.

Importance: Tracks the contract_term, internet_plan, and add_on_security features.

Usage Metrics (CDR)

What one row represents: One daily or monthly usage summary.

Importance: The "behavioral heart" of the model: avg_gb_download and long_dist_fees_total.

CRM / Support Tickets

What one row represents: One customer interaction or support case.

Importance: Captures "frustration signals" like technical support calls or refund requests.

2. Connecting the Data (Keys)

To create a unified "Customer 360" view, we use specific keys to join these disparate tables:

cust_ref (Customer ID): The primary unique identifier for a human being. This is the "gold standard" key for joining demographic and billing data.

acct_ref (Account ID): Used to link multiple services (e.g., a home line and a business line) to a single billing profile. We model at the account level because churn usually happens when an account is closed.

fiscal_qtr / Timestamps: Crucial for the temporal join. We must align usage data with the correct billing cycle to ensure the model sees the "snapshot" of the customer exactly as they were before they made the decision to stay or leave.

3. Real-World Risks & Mitigation

Live data is inherently messy. I have identified two primary risks and the strategies to reduce them:

Risk A: Data Leakage (The "Future Information" Risk)

The Problem: Including data that only exists after a customer has already started the churn process (e.g., a "Closing Statement Fee" or "Equipment Return Date"). If the model sees these, it will appear 100% accurate in training but will fail in the real world because that data won't exist for active customers.

The Fix: I would implement a Point-in-Time (PIT) Join. This involves "freezing" the data at a specific observation date (e.g., 30 days prior to the left_flag event) and only allowing the model to "see" data that was physically present in the database before that date.

Risk B: ID Fragmentation (The "Ghost" Customer Risk)

The Problem: Customers who churn and then return as a "new" customer often get a new cust_ref. This splits their history across two records, hiding their previous churn behavior and making the tenure_mo variable inaccurate.

The Fix: I would use Fuzzy Matching and Identity Resolution. By running a script that matches records based on hashed email addresses, physical addresses, and phone numbers, we can create a "Golden Record" that preserves the customer's true long-term history with TruSource.

4. Why This Architecture Matters

By moving from a single file to this multi-source architecture, TruSource can move past obvious churn drivers (like contracts) and find the non-obvious patterns/ deeper—such as a specific sequence of "Service Interactions" followed by a drop in "Usage Metrics"—that indicate a customer is silently detaching from the brand.