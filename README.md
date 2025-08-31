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



