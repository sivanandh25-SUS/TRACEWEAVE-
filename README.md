# TRACEWEAVE-

# TRACEWEAVE FINANCE - Working Model

Team CodeNexus · CRIMEATHON 4.0 · Financial Security · Approach: DEFENSE

TRACEWEAVE checks whether a vendor bank-detail change made through an API integration can be trusted. It separates authentication (the key is recognised) from authorisation (the key may do this, and an independent approval exists).

## What is included

- `app/engine.py` - the decision engine: vendor change detection, API identity and permission checks, approval verification, correlation of change records with API logs, and the trust decision with explanations.
- `app/cli.py` - runs every dataset and writes CSV reports to `reports/`.
- `dashboard.py` - Streamlit investigation dashboard (7 pages).
- `data/evidence_excerpt.json` - the investigation evidence (E04, E18, E19).
- `data/synthetic/` - Scenario A (legitimate) and Scenario C (unauthorised). Both are labelled SYNTHETIC.
- `tests/test_engine.py` - 18 automated tests covering evidence results, scenarios, edge cases and bad input.

## Running

1. Install the requirements: `pip install -r requirements.txt`
2. Run the tests: `python -m unittest tests.test_engine`
3. Generate reports: `python -m app.cli`
4. Open the dashboard: `streamlit run dashboard.py`

## Decision rules

- **REJECT / HIGH-RISK ALERT:** the key is not registered, lacks the required scope, or the approval reference exists but does not match vendor, bank details or timing, or was approved by the same identity that made the change.
- **MANUAL REVIEW:** the approval is blank, the key has broad permissions, no IP restriction is recorded, no expiry is recorded, or no matching API log exists.
- **VERIFIED:** the key is registered and in scope, and a valid independent approval exists.

No payment is blocked automatically. Results recommend verification or a simulated hold.

## Limitations

- The approval registry is not in the evidence, so the investigation record is reported as missing approval. Scenario A demonstrates a valid approval using synthetic data.
- Only the excerpts of E04, E10, E18 and E19 are used. The full evidence files should be checked against these fields.
- The engine proves an authorisation gap, not intent. It does not show that fraud, credential theft or payment occurred.
- Rules are transparent and fixed. Anomaly detection is a future enhancement.
