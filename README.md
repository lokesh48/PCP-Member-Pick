# PCP-Member-Pick
Smart PCP Assignment Engine that uses machine learning to predict the optimal assignment process for matching members to Primary Care Physicians. In healthcare, there are multiple ways to assign members to PCPs - through member choice, prior relationships, family groupings, auto-assignment based on proximity, or default assignments.
My model analyzes PCP characteristics, network details, capacity constraints, and member profiles to recommend which assignment method would result in the best match, improving member satisfaction and reducing reassignment rates
## **Complete Architecture Summary**
```
┌───────────────────────────────────────────────────────────┐
│ Backend Team → Daily CSV Exports                          │
│ (PCP data, member assignments, outcomes)                  │
└─────────────────────┬─────────────────────────────────────┘
                      │
                      ▼
┌───────────────────────────────────────────────────────────┐
│ STEP 1: AWS S3 Data Lake                                  │
│ Tool: boto3 Python SDK                                    │
│ Structure: raw-data/, processed-data/, models/            │
└─────────────────────┬─────────────────────────────────────┘
                      │
                      ▼
┌───────────────────────────────────────────────────────────┐
│ STEP 2: Feature Engineering (SageMaker Studio)            │
│ Tools: Pandas, NumPy, Jupyter Notebooks                   │
│ Tasks:                                                    │
│  • EDA (visualizations, correlations)                     │
│  • Data Cleaning (nulls, outliers, duplicates)            │
│  • Feature Engineering (temporal, capacity, geographic)   │
│  • Encoding (Label Encoding for categorical)              │
│  • Data Quality Validation                                │
└─────────────────────┬─────────────────────────────────────┘
                      │
                      ▼
┌───────────────────────────────────────────────────────────┐
│ STEP 3: Model Training (SageMaker)                        │
│ Tools: scikit-learn, XGBoost, LightGBM                    │
│ Methods:                                                  │
│  • Handle imbalance with SMOTE                            │
│  • Stratified train-test split                            │
│  • Cross-validation (5-fold)                              │
│  • Algorithm benchmarking                                 │
│  • Hyperparameter tuning                                  │
│  • Model versioning with MLflow                           │
└─────────────────────┬─────────────────────────────────────┘
                      │
                      ▼
┌───────────────────────────────────────────────────────────┐
│ STEP 4: Batch Predictions → Redshift                      │
│ Tools: psycopg2, SQLAlchemy                               │
│ Actions:                                                  │
│  • Load new assignment requests                           │
│  • Apply same feature engineering                         │
│  • Generate predictions with confidence scores            │
│  • Load into Redshift table                               │
│  • Create business views for analytics                    │
└─────────────────────┬─────────────────────────────────────┘
                      │
                      ▼
┌───────────────────────────────────────────────────────────┐
│ STEP 5: Monitoring (CloudWatch + Redshift)                │
│ Tools: boto3 CloudWatch SDK, SQL queries                  │
│ Metrics:                                                  │
│  • Model Accuracy (predicted vs actual)                   │
│  • Success Rate (member retained PCP?)                    │
│  • Confidence Scores                                      │
│  • Distribution Drift Detection                           │
│  • SNS Alerts for performance degradation                 │
└───────────────────────────────────────────────────────────┘
