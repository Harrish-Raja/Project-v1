# 1. Project explanation

## Project title

**LoanServe – Retail Lending Platform Assistant**

## One-line explanation

LoanServe is an end-to-end lending platform that:

1. Validates and prices loan applications.
2. Calculates EMI and repayment schedules.
3. Exposes loan services through FastAPI.
4. Predicts default risk using machine-learning models.
5. Creates a prioritized work queue for loan officers.
6. Classifies and summarizes customer messages.
7. Detects prompt-injection attacks.
8. Retrieves answers from lending-policy documents using RAG.
9. Provides an authenticated multi-agent AI assistant.

Main configuration is in [config/constants.py](<C:/Users/harri/Downloads/Project-main (3)/Project-main/config/constants.py>).

---

# 2. Business problem statement

LoanServe receives home, vehicle, gold, and personal-loan applications from branches, websites, partners, and sales agents.

Previously:

- Loan officers calculated eligibility manually.
- Applications were not consistently prioritized.
- Risk teams had no common default-risk ranking.
- Servicing teams manually read every customer message.
- Lending policies were spread across 18 PDF documents.
- Customers could receive inconsistent answers.
- There was no central assistant for EMI, application status, risk, or policy questions.

The system solves this by creating one unified lending and customer-service platform.

---

# 3. Business objective

The objective is to build a complete lending platform over **38 development days**:

- Build the loan domain using Python.
- Apply eligibility and pricing rules.
- Clean application data.
- Store applications in SQLite.
- Expose APIs using FastAPI.
- Add authentication and role-based access.
- Train and compare credit-risk models.
- Select a champion model.
- Rank pending applications.
- Process customer messages automatically.
- Detect sentiment, urgency, entities, and escalation cases.
- Build a policy-document RAG system.
- Provide grounded answers with citations.
- Expose the complete assistant through an authenticated endpoint.

---

# 4. End users

## 1. Customer

Can ask:

- EMI-related questions.
- Loan-policy questions.
- Application-status questions.
- Eligibility questions.
- Prepayment and foreclosure questions.

## 2. Loan officer

Uses:

- Loan application API.
- Eligibility result.
- EMI amount.
- Repayment schedule.
- Risk-based work queue.
- Refer/approve decisions.

## 3. Risk analyst

Uses:

- Default-risk predictions.
- Model comparison results.
- Feature importance.
- Threshold-cost analysis.
- Champion model.

## 4. Servicing agent

Uses:

- Message classification.
- Urgency detection.
- Entity extraction.
- Thread summaries.
- Escalation reports.

## 5. Administrator or manager

Uses:

- User and role management.
- Audit logs.
- Human approval for waivers and overrides.
- High-risk customer escalations.

## 6. Data scientist / ML engineer

Uses:

- Training datasets.
- Model artifacts.
- Cross-validation results.
- Prediction files.
- Feature-lift reports.

---

# 5. High-level architecture

```text
Customer / Loan Officer / Servicing Agent
                    |
                    v
              FastAPI Layer
       Authentication, roles, rate limit
                    |
      ------------------------------------
      |              |                   |
 Loan Domain      Risk Models       AI Assistant
      |              |                   |
 Eligibility      Prediction        LangGraph workflow
 Pricing          Work queue        Tools + agents
 EMI              Champion model    Human approval
      |              |                   |
      ------------------------------------
                    |
        SQLite + ChromaDB + Files
                    |
      CSVs, PDFs, PKL models, JSON, reports
```

## Main layers

### Domain layer

Located under [loanserve/core](<C:/Users/harri/Downloads/Project-main (3)/Project-main/loanserve/core>).

Responsibilities:

- Eligibility.
- EMI calculation.
- Debt-to-income calculation.
- Loan-to-value validation.
- Credit-score rules.
- Application entities.
- Validation errors.

### Data-access layer

Located under [loanserve/data_access](<C:/Users/harri/Downloads/Project-main (3)/Project-main/loanserve/data_access>).

Responsibilities:

- CSV cleaning.
- Rejected-record reporting.
- SQLite loading.
- SQL reports.
- File storage.

### API layer

Located under [loanserve/api](<C:/Users/harri/Downloads/Project-main (3)/Project-main/loanserve/api>).

Responsibilities:

- FastAPI application.
- Request and response schemas.
- Authentication.
- User accounts.
- Application CRUD.
- Pagination.
- Rate limiting.
- Audit logs.

### Risk-model layer

Located under [loanserve/risk_models](<C:/Users/harri/Downloads/Project-main (3)/Project-main/loanserve/risk_models>).

Responsibilities:

- Feature preparation.
- Training.
- Model comparison.
- Cross-validation.
- Threshold selection.
- Default prediction.
- Work queue generation.

### Message-intelligence layer

Located under [loanserve/message_intelligence](<C:/Users/harri/Downloads/Project-main (3)/Project-main/loanserve/message_intelligence>).

Responsibilities:

- Message cleaning.
- Classification.
- Attention-based extractive summaries.
- LLM summaries.
- Entity extraction.
- Injection protection.
- Escalation.

### Retrieval and assistant layers

Located under [loanserve/retrieval](<C:/Users/harri/Downloads/Project-main (3)/Project-main/loanserve/retrieval>) and [loanserve/assistant](<C:/Users/harri/Downloads/Project-main (3)/Project-main/loanserve/assistant>).

Responsibilities:

- PDF loading.
- Chunking.
- Embeddings.
- ChromaDB search.
- Citation validation.
- Tool selection.
- Multi-agent orchestration.
- Human approval.
- Final response generation.

---

# 6. Week-by-week development plan

## Week 1 – Python lending core

### Objective

Convert lending rules into reusable Python domain logic.

### Problem addressed

Loan officers were manually calculating eligibility, EMI, and repayment schedules.

### Day 1: Eligibility from standard input

File:

- [scripts/day01_loan_eligibility.py](<C:/Users/harri/Downloads/Project-main (3)/Project-main/scripts/day01_loan_eligibility.py>)

Builds:

- Command-line loan input.
- Loan amount validation.
- EMI calculation.
- Opening repayment schedule.
- Eligibility decision.
- Rejection reasons.

### Day 2: Reusable functions and classes

File:

- [scripts/day02_loan_pricing.py](<C:/Users/harri/Downloads/Project-main (3)/Project-main/scripts/day02_loan_pricing.py>)

Builds:

- Reusable eligibility functions.
- Pricing functions.
- Loan-calculation classes.

### Day 3: Python package and storage

Files:

- [loanserve/core/loan_calculations.py](<C:/Users/harri/Downloads/Project-main (3)/Project-main/loanserve/core/loan_calculations.py>)
- [loanserve/core/exceptions.py](<C:/Users/harri/Downloads/Project-main (3)/Project-main/loanserve/core/exceptions.py>)
- [loanserve/data_access/file_storage.py](<C:/Users/harri/Downloads/Project-main (3)/Project-main/loanserve/data_access/file_storage.py>)

Builds:

- Package structure.
- Custom exception hierarchy.
- First file-based persistence.

### Day 4: Entity hierarchy and factory

File:

- [loanserve/core/entities.py](<C:/Users/harri/Downloads/Project-main (3)/Project-main/loanserve/core/entities.py>)

Builds:

- LoanApplication base class.
- SecuredLoanApplication.
- UnsecuredLoanApplication.
- Factory method to select the correct entity.

### Day 5: Validators and decorators

Files:

- [loanserve/core/validators.py](<C:/Users/harri/Downloads/Project-main (3)/Project-main/loanserve/core/validators.py>)
- [loanserve/core/decorators.py](<C:/Users/harri/Downloads/Project-main (3)/Project-main/loanserve/core/decorators.py>)

Builds:

- PAN validation.
- Mobile validation.
- Email validation.
- Execution-time measurement.
- Loading entities from storage.

### Key Week 1 rules

- Applicant age: 21–60.
- Maximum EMI-to-income ratio: 50%.
- Secured loan LTV maximum: 80%.
- Personal loans require credit score at least 700.
- Loan product limits:
  - Home: ₹1.5 crore.
  - Vehicle: ₹25 lakh.
  - Gold: ₹10 lakh.
  - Personal: ₹15 lakh.

### Week 1 output

- Reusable Python lending domain.
- EMI calculation.
- Eligibility decision.
- Rejection explanations.
- Repayment schedule.

---

## Week 2 – Data, SQL, and initial APIs

### Objective

Convert raw applications into reliable data and expose the lending domain as an API.

### Problem addressed

Raw application data contained missing values, duplicates, invalid dates, and invalid credit scores.

### Day 1: Data cleaning

File:

- [loanserve/data_access/cleaning_pipeline.py](<C:/Users/harri/Downloads/Project-main (3)/Project-main/loanserve/data_access/cleaning_pipeline.py>)

Validates:

- Duplicate application IDs.
- Missing modelling fields.
- Non-positive income.
- Invalid dates.
- Credit scores outside 300–900.
- Inconsistent category case and whitespace.

Outputs:

- `processed_data/loan_applications_cleaned.csv`
- `processed_data/loan_applications_predict_cleaned.csv`
- `output/rejected_records.csv`

### Day 2: SQLite and reports

Files:

- [loanserve/data_access/database_loader.py](<C:/Users/harri/Downloads/Project-main (3)/Project-main/loanserve/data_access/database_loader.py>)
- [loanserve/data_access/database_reports.py](<C:/Users/harri/Downloads/Project-main (3)/Project-main/loanserve/data_access/database_reports.py>)

Builds:

- SQLite application database.
- Application loading.
- SQL reports.
- Application lookup.

Database:

- [database/loanserve.db](<C:/Users/harri/Downloads/Project-main (3)/Project-main/database/loanserve.db>)

### Day 3: API schemas and application factory

Files:

- [loanserve/api/schemas.py](<C:/Users/harri/Downloads/Project-main (3)/Project-main/loanserve/api/schemas.py>)
- [loanserve/api/application_factory.py](<C:/Users/harri/Downloads/Project-main (3)/Project-main/loanserve/api/application_factory.py>)

Builds:

- Pydantic request validation.
- FastAPI application factory.
- CORS.
- Health endpoint.
- Domain exception handling.

### Day 4: Application repository and endpoints

Files:

- [loanserve/api/repository.py](<C:/Users/harri/Downloads/Project-main (3)/Project-main/loanserve/api/repository.py>)
- [loanserve/api/routers/loan_applications.py](<C:/Users/harri/Downloads/Project-main (3)/Project-main/loanserve/api/routers/loan_applications.py>)

Endpoints include:

```text
POST   /api/v1/applications
GET    /api/v1/applications
GET    /api/v1/applications/{application_id}
PUT    /api/v1/applications/{application_id}
DELETE /api/v1/applications/{application_id}
GET    /api/v1/applications/{application_id}/schedule
GET    /health
```

### Day 5: ORM tables

File:

- [loanserve/api/orm_models.py](<C:/Users/harri/Downloads/Project-main (3)/Project-main/loanserve/api/orm_models.py>)

Builds:

- SQLAlchemy models.
- Database-backed application repository.
- Persistent application storage.

---

## Week 3 – Service hardening and baseline model

### Objective

Secure and productionize the service, then create the first risk model.

### Problem addressed

The initial API had no authentication, rate limiting, auditability, or predictive risk scoring.

### Day 1: Repository abstraction

Builds:

- Common repository interface.
- In-memory repository for testing.
- SQL repository for production.
- Factory selection based on configuration.

### Day 2: Rate limiting and password storage

File:

- [loanserve/api/middleware.py](<C:/Users/harri/Downloads/Project-main (3)/Project-main/loanserve/api/middleware.py>)
- [loanserve/api/password_hashing.py](<C:/Users/harri/Downloads/Project-main (3)/Project-main/loanserve/api/password_hashing.py>)

Builds:

- Sliding-window rate limiter.
- Request logging.
- Argon2 password hashing.

### Day 3: Tokens, roles, audit trail

File:

- [loanserve/api/access_control.py](<C:/Users/harri/Downloads/Project-main (3)/Project-main/loanserve/api/access_control.py>)

Builds:

- JWT token generation.
- Role claims.
- Administrator-only delete access.
- Audit logging.

### Day 4: Accounts and login

File:

- [loanserve/api/routers/user_accounts.py](<C:/Users/harri/Downloads/Project-main (3)/Project-main/loanserve/api/routers/user_accounts.py>)

Endpoints:

```text
POST /api/v1/auth/signup
POST /api/v1/auth/login
```

Roles:

- `loan_officer`
- `administrator`

### Day 5: Baseline model

Files:

- [loanserve/risk_models/baseline_model.py](<C:/Users/harri/Downloads/Project-main (3)/Project-main/loanserve/risk_models/baseline_model.py>)
- [loanserve/risk_models/feature_pipeline.py](<C:/Users/harri/Downloads/Project-main (3)/Project-main/loanserve/risk_models/feature_pipeline.py>)

Model:

- Logistic Regression baseline.

Features:

- Age.
- Monthly income.
- Employment type.
- Credit score.
- Debt-to-income ratio.
- Existing loan count.
- Late payments.
- Loan type.
- Loan amount.
- Tenure.

Output:

- [artifacts/baseline_model.pkl](<C:/Users/harri/Downloads/Project-main (3)/Project-main/artifacts/baseline_model.pkl>)

---

## Week 4 – Risk models and work queue

### Objective

Compare multiple models, select the champion, choose a cost-based threshold, and rank waiting applications.

### Problem addressed

A lender needs to identify risky applications consistently and prioritize officer attention.

### Day 1: Tree models

File:

- [loanserve/risk_models/tree_models.py](<C:/Users/harri/Downloads/Project-main (3)/Project-main/loanserve/risk_models/tree_models.py>)

Models:

- Decision Tree.
- Random Forest.

Output artifacts:

- [artifacts/decision_tree.pkl](<C:/Users/harri/Downloads/Project-main (3)/Project-main/artifacts/decision_tree.pkl>)
- [artifacts/random_forest.pkl](<C:/Users/harri/Downloads/Project-main (3)/Project-main/artifacts/random_forest.pkl>)
- `output/class_weight_comparison.json`

Class weighting is used because default cases are only about 6.27% of cleaned records.

### Day 2: Fair model comparison

File:

- [loanserve/risk_models/cross_validation.py](<C:/Users/harri/Downloads/Project-main (3)/Project-main/loanserve/risk_models/cross_validation.py>)

Models compared:

- Logistic Regression.
- Decision Tree.
- Random Forest.
- Linear SVM.

Metric:

- ROC-AUC.
- Five-fold stratified cross-validation.

Observed results:

| Model | Mean ROC-AUC |
|---|---:|
| Random Forest | 0.8746 |
| Linear SVM | 0.8660 |
| Logistic Regression | 0.8659 |
| Decision Tree | 0.8546 |

### Day 3: Feature engineering

File:

- [loanserve/risk_models/feature_engineering.py](<C:/Users/harri/Downloads/Project-main (3)/Project-main/loanserve/risk_models/feature_engineering.py>)

New features:

1. `monthly_installment_inr`
2. `installment_to_income`
3. `credit_band`

Credit bands:

- Poor.
- Fair.
- Good.
- Excellent.

Outputs:

- [processed_data/loan_applications_enriched.csv](<C:/Users/harri/Downloads/Project-main (3)/Project-main/processed_data/loan_applications_enriched.csv>)
- [processed_data/loan_applications_predict_enriched.csv](<C:/Users/harri/Downloads/Project-main (3)/Project-main/processed_data/loan_applications_predict_enriched.csv>)
- `output/feature_lift.json`

Feature lift:

```text
Without engineered features: 0.8746
With engineered features:    0.8800
Lift:                        0.0054
```

### Day 4: Champion and threshold

File:

- [loanserve/risk_models/champion_model.py](<C:/Users/harri/Downloads/Project-main (3)/Project-main/loanserve/risk_models/champion_model.py>)

Champion:

- Random Forest.
- Mean ROC-AUC: 0.8746.
- Maximum depth: 8.
- Minimum samples per leaf: 20.

Output:

- [artifacts/champion.pkl](<C:/Users/harri/Downloads/Project-main (3)/Project-main/artifacts/champion.pkl>)
- [output/champion_search.json](<C:/Users/harri/Downloads/Project-main (3)/Project-main/output/champion_search.json>)
- [output/threshold_choice.json](<C:/Users/harri/Downloads/Project-main (3)/Project-main/output/threshold_choice.json>)

Chosen threshold:

```text
Default probability >= 0.60  -> Refer
Default probability <  0.60  -> Approve
```

Cost at threshold 0.60:

```text
₹86,000,000
```

Cost at traditional threshold 0.50:

```text
₹91,720,000
```

The threshold is selected based on business cost, not simply 0.5.

### Day 5: Work queue

File:

- [loanserve/risk_models/work_queue.py](<C:/Users/harri/Downloads/Project-main (3)/Project-main/loanserve/risk_models/work_queue.py>)

Output:

- [output/predictions.csv](<C:/Users/harri/Downloads/Project-main (3)/Project-main/output/predictions.csv>)
- [output/work_queue.csv](<C:/Users/harri/Downloads/Project-main (3)/Project-main/output/work_queue.csv>)

The work queue:

1. Scores all waiting applications.
2. Sorts highest-risk applications first.
3. Marks each as `refer` or `approve`.
4. Shows loan amount and affordability indicators.

---

## Week 5 – Customer-message intelligence

### Objective

Convert raw customer messages into structured information.

### Problem addressed

Servicing agents had to manually read thousands of messages.

### Day 1: Message cleaning

File:

- [loanserve/message_intelligence/text_cleaning.py](<C:/Users/harri/Downloads/Project-main (3)/Project-main/loanserve/message_intelligence/text_cleaning.py>)

Removes:

- Greetings.
- Signatures.
- Quoted replies.
- Ticket references.
- URLs.
- Application references.
- Unnecessary whitespace.

Output:

- [processed_data/messages_cleaned.csv](<C:/Users/harri/Downloads/Project-main (3)/Project-main/processed_data/messages_cleaned.csv>)

### Day 2: Text classifiers

File:

- [loanserve/message_intelligence/message_classifier.py](<C:/Users/harri/Downloads/Project-main (3)/Project-main/loanserve/message_intelligence/message_classifier.py>)

Model:

- TF-IDF vectorization.
- Unigrams and bigrams.
- Logistic Regression.
- Balanced class weights.

Two predictions are made:

- Message category.
- Message urgency.

Artifact:

- [artifacts/message_classifier.pkl](<C:/Users/harri/Downloads/Project-main (3)/Project-main/artifacts/message_classifier.pkl>)

### Day 3: Attention

File:

- [loanserve/message_intelligence/attention.py](<C:/Users/harri/Downloads/Project-main (3)/Project-main/loanserve/message_intelligence/attention.py>)

Builds:

- NumPy-based attention scoring.
- Sentence importance ranking.

### Day 4: Extractive summaries

File:

- [loanserve/message_intelligence/thread_summary.py](<C:/Users/harri/Downloads/Project-main (3)/Project-main/loanserve/message_intelligence/thread_summary.py>)

Output:

- [output/thread_summaries.csv](<C:/Users/harri/Downloads/Project-main (3)/Project-main/output/thread_summaries.csv>)

### Day 5: LLM summary

File:

- [loanserve/message_intelligence/llm_client.py](<C:/Users/harri/Downloads/Project-main (3)/Project-main/loanserve/message_intelligence/llm_client.py>)

The LLM receives the extractive summary and returns JSON:

```json
{
  "summary": "...",
  "sentiment": "neutral",
  "action": "..."
}
```

The implementation expects Gemini settings from:

```text
GEMINI_API_KEY
GEMINI_MODEL
```

It also supports SHA-256 prompt caching.

---

## Week 6 – Entities, batching, safety, escalation, and policy loading

### Objective

Create a safe operational pipeline for customer messages and policy documents.

### Problem addressed

Customer messages can be malformed, malicious, urgent, or connected to sensitive applications.

### Day 1: Entity extraction

File:

- [loanserve/message_intelligence/entity_extraction.py](<C:/Users/harri/Downloads/Project-main (3)/Project-main/loanserve/message_intelligence/entity_extraction.py>)

Extracts:

- Application references.
- Ticket references.
- Monetary amounts.
- Dates.

Output:

- [output/message_entities.csv](<C:/Users/harri/Downloads/Project-main (3)/Project-main/output/message_entities.csv>)

### Day 2: Batch processing

File:

- [loanserve/message_intelligence/batch_pipeline.py](<C:/Users/harri/Downloads/Project-main (3)/Project-main/loanserve/message_intelligence/batch_pipeline.py>)

Ensures:

- One bad message does not stop the entire batch.
- Each message receives processed or failed status.
- Failure reason is stored.

Output:

- [output/message_pipeline.csv](<C:/Users/harri/Downloads/Project-main (3)/Project-main/output/message_pipeline.csv>)

### Day 3: Injection guard

File:

- [loanserve/message_intelligence/injection_guard.py](<C:/Users/harri/Downloads/Project-main (3)/Project-main/loanserve/message_intelligence/injection_guard.py>)

Detects:

- Prompt-injection instructions.
- Attempts to override system instructions.
- Attempts to expose confidential data.
- Attempts to manipulate the AI model.

Output:

- [output/injection_report.csv](<C:/Users/harri/Downloads/Project-main (3)/Project-main/output/injection_report.csv>)

### Day 4: Escalation

File:

- [loanserve/message_intelligence/escalation.py](<C:/Users/harri/Downloads/Project-main (3)/Project-main/loanserve/message_intelligence/escalation.py>)

Escalation can be based on:

- Urgency.
- Sentiment.
- Financial exposure.
- Thread-level risk.

Output:

- [output/escalation_report.csv](<C:/Users/harri/Downloads/Project-main (3)/Project-main/output/escalation_report.csv>)

### Day 5: Policy loading and chunking

Files:

- [loanserve/retrieval/document_loader.py](<C:/Users/harri/Downloads/Project-main (3)/Project-main/loanserve/retrieval/document_loader.py>)
- [loanserve/retrieval/chunking.py](<C:/Users/harri/Downloads/Project-main (3)/Project-main/loanserve/retrieval/chunking.py>)

Builds:

- PDF text extraction.
- Section-based chunks.
- Document IDs.
- Policy headings.
- Effective dates.
- Product metadata.

---

## Week 7 – Retrieval-augmented generation

### Objective

Answer customer policy questions from trusted documents with citations.

### Problem addressed

The policy book contains 18 different documents, and the assistant must not hallucinate policy answers.

### Day 1: Embeddings and vector store

File:

- [loanserve/retrieval/vector_store.py](<C:/Users/harri/Downloads/Project-main (3)/Project-main/loanserve/retrieval/vector_store.py>)

Technology:

- Sentence Transformer: `all-MiniLM-L6-v2`.
- ChromaDB.
- Cosine similarity.
- Persistent vector database.

Store:

- [database/policy_store](<C:/Users/harri/Downloads/Project-main (3)/Project-main/database/policy_store>)

### Day 2: Grounded answers

File:

- [loanserve/retrieval/grounded_answer.py](<C:/Users/harri/Downloads/Project-main (3)/Project-main/loanserve/retrieval/grounded_answer.py>)

Rules:

- Answer only from retrieved chunks.
- Every factual answer must contain a citation.
- Citation must match a retrieved chunk ID.
- Invalid citations produce an `UngroundedAnswer` error.
- Low-confidence questions receive a refusal message.

### Day 3: Product-filtered search

File:

- [loanserve/retrieval/filtered_search.py](<C:/Users/harri/Downloads/Project-main (3)/Project-main/loanserve/retrieval/filtered_search.py>)

Examples:

- “Home loan foreclosure” searches home plus universal policies.
- “Gold loan purity” searches gold plus universal policies.
- “Personal loan eligibility” searches personal plus universal policies.

### Day 4: Tools

File:

- [loanserve/assistant/tools.py](<C:/Users/harri/Downloads/Project-main (3)/Project-main/loanserve/assistant/tools.py>)

Available tools:

1. `calculate_installment`
2. `lookup_application`
3. `predict_default_risk`
4. `search_policy_documents`

---

## Week 8 – Multi-agent assistant and deployment

### Objective

Combine all previous functionality into an intelligent assistant.

### Problem addressed

Customers should not need separate systems for EMI, applications, risk, and policy questions.

### Day 1: Planner and resolver

File:

- [loanserve/assistant/agents.py](<C:/Users/harri/Downloads/Project-main (3)/Project-main/loanserve/assistant/agents.py>)

Planner:

- Understands the question.
- Selects the correct tool.
- Extracts arguments.

Resolver:

- Combines tool results.
- Creates a draft response.
- Identifies whether approval is required.

### Day 2: Approval and graph

Files:

- [loanserve/assistant/graph.py](<C:/Users/harri/Downloads/Project-main (3)/Project-main/loanserve/assistant/graph.py>)
- [loanserve/assistant/nodes.py](<C:/Users/harri/Downloads/Project-main (3)/Project-main/loanserve/assistant/nodes.py>)

Graph flow:

```text
Triage
  ↓
Plan
  ↓
Execute tools
  ↓
Draft answer
  ↓
Approval gate
  ↓
Critic validation
  ↓
Final response
```

Human approval is required for:

- Policy waivers.
- Overrides.
- Exceptions.
- Sensitive actions.

### Day 3: Checkpoint and endpoint

Files:

- [loanserve/assistant/workflow.py](<C:/Users/harri/Downloads/Project-main (3)/Project-main/loanserve/assistant/workflow.py>)
- [loanserve/api/routers/assistant.py](<C:/Users/harri/Downloads/Project-main (3)/Project-main/loanserve/api/routers/assistant.py>)

Endpoint:

```text
POST /api/v1/assistant/ask
```

The assistant uses:

- LangGraph.
- SQLite checkpointing.
- Thread IDs.
- Human-in-the-loop interruption.
- Authenticated API access.

---

# 7. Data files used

Located under [data](<C:/Users/harri/Downloads/Project-main (3)/Project-main/data>).

## `loan_applications_train.csv`

- 33,000 records.
- Contains default outcome.
- Used for model training.
- Contains deliberately dirty data.
- 6.29% default rate before cleaning.

Important fields:

```text
application_id
applicant_id
loan_type
age_years
monthly_income_inr
employment_type
credit_score
debt_to_income_ratio
existing_loan_count
late_payments_last_12_months
loan_amount_inr
tenure_months
defaulted
loss_given_default_inr
```

## `loan_applications_predict.csv`

- 5,000 waiting applications.
- Does not contain the `defaulted` target.
- Used for scoring and work-queue creation.

## `customer_messages.csv`

- 30,045 messages.
- 30,000 labelled messages.
- 45 messages have missing labels.
- Used for category and urgency classification.

## `message_threads.csv`

- 520 conversations.
- 2,508 conversation turns.
- Used for thread summarization and escalation.

## `adversarial_messages.csv`

- 75 security-test messages.
- Includes 34 genuine prompt-injection attempts.
- Used to evaluate false positives and false negatives.

## `policy_documents`

Contains 18 PDF policy documents covering:

- Interest rates.
- Eligibility.
- KYC.
- Prepayment.
- Late payment.
- Grievance handling.
- Insurance.
- Top-up loans.
- Co-applicants.
- Balance transfers.
- NRI lending.
- Moratorium.
- Home loans.
- Personal loans.
- Gold loans.
- Vehicle loans.
- General FAQs.
- Servicing FAQs.

## `policy_documents_manifest.json`

Contains metadata for each policy:

- Document ID.
- Product type.
- Category.
- Effective date.
- Title.

## `retrieval_evaluation_set.json`

Contains 75 test questions:

- 60 answerable from policy.
- 15 intentionally outside the policy.
- Used to validate retrieval and refusal behavior.

---

# 8. Processed data and generated files

## Processed data

Located under [processed_data](<C:/Users/harri/Downloads/Project-main (3)/Project-main/processed_data>).

| File | Purpose |
|---|---|
| `loan_applications_cleaned.csv` | Clean training applications |
| `loan_applications_predict_cleaned.csv` | Clean waiting applications |
| `loan_applications_enriched.csv` | Training data with engineered features |
| `loan_applications_predict_enriched.csv` | Prediction data with engineered features |
| `messages_cleaned.csv` | Cleaned customer messages |

## Model artifacts

Located under [artifacts](<C:/Users/harri/Downloads/Project-main (3)/Project-main/artifacts>).

| File | Purpose |
|---|---|
| `baseline_model.pkl` | Logistic Regression baseline |
| `decision_tree.pkl` | Decision Tree model |
| `random_forest.pkl` | Random Forest model |
| `champion.pkl` | Selected production champion |
| `message_classifier.pkl` | Category and urgency classifiers |

## Reports and outputs

Located under [output](<C:/Users/harri/Downloads/Project-main (3)/Project-main/output>).

| File | Purpose |
|---|---|
| `rejected_records.csv` | Invalid applications and rejection reasons |
| `predictions.csv` | Default probabilities |
| `work_queue.csv` | Risk-ranked application queue |
| `champion_search.json` | Model comparison and selected champion |
| `fold_scores.json` | Cross-validation results |
| `threshold_choice.json` | Cost-based threshold selection |
| `feature_lift.json` | Engineered-feature performance improvement |
| `class_weight_comparison.json` | Balanced versus unbalanced model comparison |
| `messages_cleaned.csv` | Cleaned message output |
| `message_pipeline.csv` | Batch-processing result |
| `message_entities.csv` | Extracted references, amounts, dates |
| `thread_summaries.csv` | Conversation summaries |
| `injection_report.csv` | Prompt-injection detection report |
| `escalation_report.csv` | Conversations requiring human attention |

---

# 9. Models and techniques used

## Business-rule models

- Rule-based eligibility.
- EMI calculation using reducing-balance formula.
- Debt-to-income calculation.
- Loan-to-value calculation.
- Credit-score banding.

## Loan-risk models

- Logistic Regression.
- Decision Tree.
- Random Forest.
- Linear SVM for comparison.

## Message-classification model

- TF-IDF.
- Unigram and bigram features.
- Logistic Regression.
- Balanced class weights.

## Summarization

- NumPy attention scoring.
- Extractive sentence selection.
- Gemini LLM for abstractive summary.

## Retrieval model

- Sentence Transformer `all-MiniLM-L6-v2`.
- ChromaDB vector store.
- Cosine similarity.
- Metadata filtering.

## Assistant orchestration

- LangGraph.
- Planner agent.
- Tool executor.
- Resolver/drafter.
- Approval node.
- Critic node.
- Responder node.
- SQLite checkpointing.

---

# 10. Complete runtime workflow

## Loan application workflow

```text
User submits application
        ↓
Pydantic validation
        ↓
PAN/mobile/email validation
        ↓
Loan entity factory
        ↓
Eligibility rules
        ↓
Interest-rate lookup
        ↓
EMI calculation
        ↓
Debt-to-income and LTV checks
        ↓
Response with eligibility and reasons
        ↓
Application saved in repository/database
```

## Risk workflow

```text
Raw training CSV
        ↓
Data cleaning
        ↓
Rejected-record report
        ↓
Feature engineering
        ↓
Train multiple models
        ↓
Cross-validation
        ↓
Select Random Forest champion
        ↓
Choose threshold based on business cost
        ↓
Score pending applications
        ↓
Sort by default probability
        ↓
Create refer/approve work queue
```

## Message workflow

```text
Raw customer message
        ↓
Clean greetings/signatures/URLs
        ↓
Prompt-injection detection
        ↓
Category prediction
        ↓
Urgency prediction
        ↓
Reference, amount, and date extraction
        ↓
Thread summarization
        ↓
Sentiment and action extraction
        ↓
Escalation decision
```

## Policy assistant workflow

```text
PDF documents
        ↓
Extract text
        ↓
Split into policy chunks
        ↓
Add metadata
        ↓
Create embeddings
        ↓
Store in ChromaDB
        ↓
User asks question
        ↓
Detect product
        ↓
Filter policy documents
        ↓
Semantic search
        ↓
Retrieve top passages
        ↓
Generate grounded answer
        ↓
Validate citations
        ↓
Return answer or safe refusal
```

## Multi-agent workflow

```text
Customer question
        ↓
Triage and injection check
        ↓
Classify category and urgency
        ↓
Planner chooses tool
        ↓
Tool executes
        ↓
Resolver drafts response
        ↓
Approval if waiver/override/exception
        ↓
Critic validates answer
        ↓
Responder returns final answer
```

---

# 11. Interview-ready explanation

You can explain the project like this:

> “LoanServe is an end-to-end retail lending platform developed in Python. It starts with a rule-based loan domain that calculates EMI, validates age, affordability, LTV, credit score, and product limits. The platform then cleans loan data and stores it in SQLite. A FastAPI layer exposes application, authentication, repayment-schedule, and assistant APIs.
>
> For credit risk, I implemented Logistic Regression, Decision Tree, Random Forest, and Linear SVM models using a shared preprocessing pipeline. I compared them using stratified five-fold ROC-AUC, selected Random Forest as the champion with a mean ROC-AUC of 0.8746, and selected a 0.60 operating threshold based on business cost rather than using the default 0.50 threshold.
>
> For customer servicing, I cleaned messages, classified their category and urgency using TF-IDF and Logistic Regression, extracted entities, summarized message threads, detected prompt injection, and escalated high-risk conversations.
>
> Finally, I created a RAG-based policy assistant using Sentence Transformers and ChromaDB. It retrieves policy chunks with product metadata and validates citations before responding. LangGraph orchestrates triage, planning, tool execution, drafting, human approval, criticism, and final response generation. This provides a complete lending, risk, servicing, and AI-assistant platform.”
