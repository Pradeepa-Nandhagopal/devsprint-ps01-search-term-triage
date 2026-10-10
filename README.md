🎯 Search Term Triage Engine — DevSprint PS-01
An explainable search-term triage dashboard built with Python, Streamlit, and Pandas for the DevSprint PS-01 problem statement. It helps an advertising analyst review search-term reports, understand performance signals, identify potentially irrelevant queries, and export recommendations for human review.
Important: This is a rule-based prototype, not a trained machine-learning model. Its recommendations are decision support only; it does not connect to or make changes to a live advertising account.

✨ Features
- CSV upload first: The dashboard starts empty and analyses a report only after the user uploads a valid CSV.
- PROMOTE / BLOCK / IGNORE recommendations: Combines query relevance, conversion performance, configured thresholds, and available evidence.
- Explainable outputs: Each result includes a decision, confidence label, reason, and human-review flag.
- Performance metrics: Uses impressions, clicks, cost, conversions, conversion value, CTR, conversion rate, CPA, and ROAS where the required columns are present.
- Human-in-the-loop workflow: Ambiguous and low-evidence terms can be flagged for manual review rather than being treated as certain outcomes.
- Triage Copilot: Supports a set of local, read-only dataset tasks such as summarizing data, finding terms, comparing campaigns, investigating a term, and preparing filtered CSV exports.
- Campaign analysis and filtering: Search, filter, and inspect results by campaign and decision.
- CSV export: Download results for further review. Exported files can include additional INR display columns while retaining the original source columns.
- INR display: Financial values are displayed in Indian rupees using a configurable USD-to-INR display rate. The default is an indicative rate of ₹96.50 per USD; change it to the rate required for your use case. It does not change the original data.
🧰 Tech stack
- Python 3.10+
- Streamlit
- Pandas
- NumPy
📁 Project structure
Keep the required files in the same project folder:
PS01_Search_Term_Triage/
├── app.py
├── triage_engine.py
├── agent_runtime.py
├── requirements.txt
├── README.md
├── VALIDATION_REPORT.md              # if included in your package
├── search_terms_report.csv           # organiser-provided dataset, if permitted to include
├── sample_recommendations.csv        # generated example output, if included
└── DevSprint_PS01_Presentation.pptx  # optional presentation
The dashboard imports triage_engine.py and agent_runtime.py. Do not move those modules away from app.py unless you also update the imports.
🚀 Run locally
1. Install Python and Git
Install Python 3.10 or newer and Git for Windows if they are not already installed. During Python installation on Windows, enable the option to add Python to PATH.
2. Open the project folder
Open the folder containing app.py in VS Code, then open Terminal → New Terminal.
3. Create and activate a virtual environment
Windows PowerShell
py -m venv .venv
.\.venv\Scripts\Activate.ps1
If PowerShell blocks activation, open a Command Prompt terminal and run:
.venv\Scripts\activate.bat
macOS / Linux
python3 -m venv .venv
source .venv/bin/activate
4. Install dependencies
python -m pip install -r requirements.txt
5. Start the dashboard
python -m streamlit run app.py
Open the local URL printed in the terminal, usually http://localhost:8501.
📄 Expected CSV columns
Upload a CSV with the required headers used by the triage engine. The supplied report is the reference example. The core fields include:
- search_term
- campaign
- ad_group
- impressions
- clicks
- cost
- conversions
- conversion_value
Use the exact required column names from triage_engine.py / the app's Required CSV columns panel. Files with missing required columns or no data rows are rejected with an error message.
🧠 How decisions work
- PROMOTE: The term appears relevant to a supported business offering and satisfies the configured performance thresholds.
- BLOCK: The term shows strong evidence of an unsupported product or clearly irrelevant intent. Review it before applying a negative keyword.
- IGNORE: Evidence is insufficient, intent is ambiguous, or the promotion criteria are not met.
- Human review: A separate flag for terms that need analyst inspection. It can overlap with any of the three decisions; it is not a fourth decision category.
The thresholds can be adjusted in the sidebar. They are transparent baseline rules and should be validated against the event's rubric and the business context.
🤖 Triage Copilot
The built-in local Copilot can handle supported dataset-oriented prompts, such as:
- Summarize this dataset
- Which campaign has the highest ROAS?
- Show blocked search terms with spend above ₹10,000
- Why was "personal loan" blocked?
- Export BLOCK terms with spend above ₹10,000
The local Copilot uses predefined read-only analysis functions; it is not an unrestricted general-purpose LLM. Unsupported or ambiguous requests may not be understood correctly. Check the returned records and calculations before relying on them.
💱 Currency handling
The supplied report's monetary fields are treated as source-currency values (USD in the current setup). The dashboard converts displayed amounts to INR using the configurable display rate. This is a presentation conversion, not a live exchange-rate feed. Source data remains unchanged; ROAS is a ratio and is not changed by converting both numerator and denominator using the same rate.
✅ Recommended validation before a demo
1. Upload the organiser-provided CSV and confirm the expected number of rows is shown.
2. Search for a few known terms and verify the decision, reason, confidence, and human-review flag.
3. Compare at least one campaign's displayed metrics with the underlying CSV.
4. Test a filtered CSV export and open it to verify its contents.
5. Upload a small test CSV with the required columns to confirm that the row count and dashboard results update.
6. Test invalid input (missing headers or an empty CSV) and confirm that the app shows a helpful validation message.
⚠️ Limitations and responsible use
- The triage engine relies on explicit keyword patterns and configurable thresholds; it does not learn from labelled examples.
- A recommendation is not proof of causation or business value. Conversion attribution may be delayed or noisy, and small samples are uncertain.
- A BLOCK result is only a suggestion for review, not an automatic negative-keyword action.
- Copilot answers should be checked against the underlying data, particularly where intent is ambiguous.
- The currency rate is configurable and may not match the rate on a given date.
- Do not commit passwords, API keys, .env files, virtual environments, or other secrets to GitHub.
- Confirm that the organisers permit publishing their dataset, using AI assistance, and sharing the source code before making the repository public.
👥 Project context
Event: DevSprint 2K26 — PS-01 Search Term Triage Engine
Purpose: Demonstrate an explainable and reviewable workflow for triaging paid-search queries.
