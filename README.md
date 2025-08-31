 User Input (Transactions/Questions)
            |
            v
   Data Storage (CSV/In-Memory)
            |
            v
     Feature Engineering
            |
            v
 -----------------------------
 |  Multi-Model ML Pipeline  |
 |---------------------------|
 | Anomaly Detection         |
 | Category Prediction       |
 | Spending Forecast         |
 -----------------------------
            |
            v
       API Endpoints
            |
            v
   Actionable Insights / Answers


Low-Level Design (LLD)

Modules & Classes:

Transaction (Pydantic Model): Validates transaction input.

Question (Pydantic Model): Validates finance-related questions for LLM.

make_features(): Extracts features like log of amount, hour of transaction for ML models.

Endpoints:

/transactions: Add a transaction

/predict_anomaly: Predict whether a transaction is anomalous

/predict_category: Auto-predict transaction category

/predict_forecast/{user_id}: Forecast next month's spending

/budget/{user_id}: Recommend monthly budgets

/summary/{user_id}: Summarize transaction stats

/category-spending/{user_id}: Spending per category

/ask_finance: Answer finance questions using LLM

/health: Check server and model status

ML Models:

Anomaly Detection: IsolationForest (detect fraud / unusual transactions)

Category Prediction: XGBoost / RandomForest (categorize transactions automatically)

Forecasting: Rolling average / Prophet (predict next month's spend)

| Task                      | Model Type            | Features Used       | Output                     |
| ------------------------- | --------------------- | ------------------- | -------------------------- |
| Fraud / Anomaly Detection | IsolationForest       | amount\_log, hour   | Anomaly score, is\_anomaly |
| Category Prediction       | XGBoost / RF          | amount\_log, hour   | Predicted category         |
| Spending Forecast         | Rolling Avg / Prophet | amount over time    | Next month forecast amount |
| Finance Q\&A              | LLM (GPT / mock)      | Transaction history | Text advice / answer       |


What is Model Context Protocol (MCP)

MCP (Model Context Protocol) is a design approach to:

Standardize the input/output of ML models

Maintain context about user, time, and features

Enable multiple models to collaborate seamlessly

Make models plug-and-play and traceable

Why use MCP here?

Currently, your /predict, /budget, /summary endpoints handle raw data inconsistently.

Using MCP, you create a central context object that stores transaction features, user info, and computed features, which every model can access.

