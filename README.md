Search Term Triage Engine
DevSprint 2K26 — Problem Statement 01
An explainable search-term triage dashboard built with Python, Streamlit, and Pandas. The application analyses paid-search query reports and provides reviewable recommendations to help analysts identify relevant queries, potentially irrelevant traffic, and terms that need further investigation.
Project status: Hackathon prototype. Recommendations are decision-support suggestions only. The application does not connect to or make changes in a live advertising account.

Overview
Advertising campaigns can generate thousands of search terms. Reviewing every term manually is time-consuming, and decisions based on small samples can be misleading. This project provides a repeatable workflow for reviewing search terms against advertiser product relevance and available performance evidence.
The application starts without a dataset. The user uploads a CSV report, after which the system validates the file, calculates performance metrics, generates recommendations, and presents the results in a dashboard.
Key Features
- CSV-based workflow: Upload a search-term report to start analysis; the dashboard does not preload the report by default.
- Three triage recommendations: PROMOTE, BLOCK, and IGNORE.
- Explainable decisions: Each processed record includes an explanation and a confidence label.
- Human review flags: Records with ambiguous intent or insufficient evidence can be flagged for manual review.
- Performance metrics: Calculates click-through rate (CTR), conversion rate, cost per acquisition (CPA), and return on ad spend (ROAS) when the necessary fields are available.
- Search and filtering: Explore terms by query, campaign, recommendation, confidence, and review status.
- Campaign insights: Review campaign-level performance and compare monetary metrics visually.
- Triage Copilot: Supports a defined set of local, read-only tasks over the uploaded dataset, including summaries, campaign comparisons, term investigations, and filtered CSV exports.
- CSV exports: Download filtered recommendations or the complete result set. Export files can include additional INR-converted columns while preserving the original source columns.
- INR display: Monetary values are displayed in Indian rupees using a configurable USD-to-INR display rate.
Technology Stack
- Python 3.10 or later
- Streamlit
- Pandas
- NumPy
Project Structure
Keep the application modules in the same directory so their imports resolve correctly.
PS01_Search_Term_Triage/
├── app.py                       # Streamlit user interface and workflow
├── triage_engine.py              # Rule-based recommendation engine
├── agent_runtime.py              # Local dataset-analysis assistant and tools
├── requirements.txt              # Python dependencies
├── README.md                     # Project documentation
├── search_terms_report.csv       # Input dataset, if permitted to distribute
├── sample_recommendations.csv    # Example generated output, if included
├── VALIDATION_REPORT.md          # Validation notes, if included
└── DevSprint_PS01_Presentation.pptx  # Presentation, if included
The exact optional files included may differ by project package. The application requires app.py, triage_engine.py, agent_runtime.py, and the dependencies listed in requirements.txt.
Installation and Setup
1. Install prerequisites
Install Python 3.10 or later and Git if they are not already available on your computer.
- Python: https://www.python.org/downloads/
- Git: https://git-scm.com/downloads
On Windows, enable the Python option to add Python to PATH during installation.
2. Open the project
Open the project directory in Visual Studio Code, then select Terminal → New Terminal.
3. Create a virtual environment
Windows PowerShell
py -m venv .venv
.\.venv\Scripts\Activate.ps1
If PowerShell prevents activation, open a Command Prompt terminal and run:
.venv\Scripts\activate.bat
macOS or Linux
python3 -m venv .venv
source .venv/bin/activate
4. Install dependencies
python -m pip install -r requirements.txt
5. Start the application
python -m streamlit run app.py
Open the local URL printed by Streamlit, usually http://localhost:8501.
Input Dataset
Upload a CSV file containing the required columns below. Column names must match exactly.
Column	Description
row_id	Identifier for the source record
search_term	User search query
match_type	Advertising keyword match type
campaign	Campaign name
ad_group	Ad group name
impressions	Number of ad impressions
clicks	Number of clicks
cost	Recorded advertising cost
conversions	Recorded conversions
conversion_value	Recorded value attributed to conversions


Files missing required columns or containing no usable data rows should be corrected before analysis. Numeric fields are converted to numeric values by the triage engine; invalid or missing numeric values are treated as zero in the current implementation.
Data distribution: The organiser-provided dataset may be subject to competition rules or usage restrictions. Do not publish it in a public repository unless distribution is explicitly permitted. A private repository is preferable while those permissions are being confirmed.
Recommendation Logic
The current implementation is a transparent, rule-based decision engine, not a trained machine-learning model. Its baseline rules combine detected query intent with performance signals and configurable thresholds.
Recommendation	Purpose
PROMOTE	The query appears relevant to a supported business-financing product and meets the configured performance criteria.
BLOCK	The query appears to indicate an unsupported product or clearly irrelevant intent; review it as a potential negative keyword.
IGNORE	Evidence is insufficient, intent is ambiguous, or promotion criteria are not met. Further observation or analyst review may be appropriate.


A separate human_review flag identifies records that should be inspected manually. This flag can overlap with any of the three recommendations; it is not a fourth recommendation category.
The baseline engine includes configurable parameters for promotion clicks, promotion conversions, minimum evidence for evaluation, block-spend threshold, and minimum ROAS for promotion. Review the implementation and app settings before interpreting any generated recommendation as a business decision.
Performance Metrics
- CTR: clicks divided by impressions
- Conversion rate: conversions divided by clicks
- CPA: cost divided by conversions
- ROAS: conversion value divided by cost
Where a denominator is zero, the corresponding metric may be undefined. ROAS is a ratio and is not changed by converting both monetary inputs using the same exchange rate.
Currency Display
The current application treats the monetary values in the supplied report as USD source values and converts displayed monetary amounts to INR using a configurable USD-to-INR rate. The default rate is an illustrative display setting, not a live exchange-rate feed or financial quote.
- The uploaded source values are not overwritten by the display conversion.
- CSV exports can include additional fields such as cost_inr, conversion_value_inr, and cpa_inr where applicable.
- Confirm the source currency and update the display rate before using the results for any real-world decision.
Triage Copilot
The local Triage Copilot uses predefined, read-only dataset tools. Example supported requests include:
- Summarize this dataset.
- Which campaign has the highest ROAS?
- Show blocked search terms with spend above ₹10,000.
- Why was "personal loan" blocked?
- Export BLOCK terms with spend above ₹10,000.
The local assistant supports defined dataset operations rather than unrestricted natural-language reasoning. Verify its answers and exported records against the source dataset, especially for ambiguous search intent or financial calculations. It does not modify an advertising account.
Validation Checklist
Before presenting or submitting the project, verify the following:
1. Upload the permitted input CSV and confirm the displayed row count.
2. Test representative relevant, unsupported, and ambiguous search terms.
3. Inspect the recommendation, confidence label, explanation, and human-review flag for each sample.
4. Compare at least one campaign metric with a manual calculation from the source CSV.
5. Export filtered results and the full recommendation report; open the files and check their contents.
6. Upload a small valid test CSV and confirm that the application updates to the new data.
7. Upload a CSV with missing required columns and confirm that a useful validation error is shown.
8. Test the dashboard at the intended presentation screen size.
Counts generated by the rule engine are not an accuracy score. Accuracy, precision, and recall should only be reported if the recommendations are compared against a verified labelled dataset.
Limitations and Responsible Use
- Recommendations are produced by keyword patterns and configurable thresholds; the system does not learn from labelled examples.
- Search intent can be ambiguous. Keyword matching can miss context or misclassify a query, so human review remains important.
- Low-volume performance data may be statistically unreliable. A recorded conversion or lack of conversions alone should not be treated as definitive evidence.
- BLOCK is a recommendation for review, not an instruction to automatically change campaign targeting.
- The Copilot can misunderstand unsupported questions; verify its calculations and explanations.
- Currency conversion is for display and depends on the configured rate.
- Confirm that external tools, code assistance, dataset use, and source-code publication comply with the hackathon rules.
Security and Repository Hygiene
Do not commit credentials or local environment files. A .gitignore file should exclude at least:
.venv/
__pycache__/
*.py[cod]
.env
.streamlit/secrets.toml
Review the complete staged file list with git status before pushing to GitHub. Do not upload API keys, passwords, private tokens, or data that you are not authorised to share.
Project Context
- Event: DevSprint 2K26
- Problem statement: PS-01 — Search Term Triage Engine
- Application type: Streamlit dashboard and local dataset-analysis assistant
- Intended use: Explainable recommendations for human review of paid-search terms
