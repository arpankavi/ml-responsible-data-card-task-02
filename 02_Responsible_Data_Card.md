# Responsible Data Card

## Dataset purpose
The supplied dataset can support a **prototype analysis of customer churn prediction** and can be used to explore whether customer attributes are associated with the `churned` label.

It must **not** by itself be used to make irreversible customer decisions, automatically deny services, or determine customer treatment without validated business rules, human oversight, and a documented prediction/action window.

This follows the provided template's requirement to state supported and prohibited decisions. fileciteturn0file0L3-L4

## Provenance and permission
- File supplied: `customer-churn-training.csv`
- Records: 12
- Columns: 7
- The supplied materials do **not** document who created the data, collection method, source systems, consent status, licensing, or permitted downstream uses.
- Therefore, provenance, consent, and licensing are **unknown and must be verified before external sharing or production use**.

The template specifically requires provenance, permission, creator/use rights, and consent/licensing restrictions to be documented. fileciteturn0file0L6-L7

## Population and representation
- Observations: 12 customers.
- Plan groups: Basic = 5, Standard = 4, Pro = 3.
- Churn labels: 5 churned (41.7%), 7 non-churned (58.3%).
- No demographic/sensitive attributes are present in the supplied columns.
- Because the dataset is only 12 rows, it is not adequate evidence that the population is representative of a real customer base.
- Sampling method and missing population groups are not documented.

The template calls for represented groups, missing groups, sampling limitations, and imbalance to be documented. fileciteturn0file0L9-L10

## Features and target

| Feature | Type | Role | Responsible-use note |
|---|---|---|---|
| `customer_id` | Identifier | ID only | Exclude from model features. |
| `tenure_months` | Numeric | Feature | Plausibly available before intervention; timing must be verified. |
| `support_tickets` | Numeric | Feature | Verify that the count is measured before the prediction cutoff. |
| `monthly_spend_inr` | Numeric | Feature | Verify timestamp and currency/business definition. |
| `last_login_days` | Numeric | Feature | Potentially useful engagement signal; must be measured before cutoff. |
| `plan_type` | Categorical | Feature | Strong separation in this sample; investigate whether it is a business policy/proxy variable. |
| `churned` | Binary | Target | Outcome label; must be defined with a time window for real prediction. |

### Leakage check
No obvious direct copy of `churned` is present among the feature columns. However, **temporal leakage cannot be ruled out** because the dataset has no timestamps or prediction cutoff.

### Sensitive proxies
No direct sensitive attributes are supplied. Proxy risk cannot be assessed adequately without domain knowledge and additional population attributes.

The supplied template explicitly requires feature definitions, target, leakage, and sensitive-proxy review. fileciteturn0file0L12-L13

## Quality checks
- Missing values: **0 in every column**.
- Duplicate rows: **0**.
- `customer_id`: 12 unique values.
- Target balance: 5 churned / 7 non-churned.
- Numeric ranges observed:
  - `tenure_months`: 1–30
  - `support_tickets`: 0–5
  - `monthly_spend_inr`: ₹499–₹1,499
  - `last_login_days`: 1–30
- Train/test separation: **not established in the source file** because there are no timestamps or predefined split.
- Outlier review: no missing or obviously extreme values were detected from the supplied 12-row sample, but the sample is too small for robust outlier conclusions.

These checks align with the provided template's required missingness, duplicates, outliers, class-balance, and train/test-separation checks. fileciteturn0file0L15-L16

## Risks and safeguards

| Risk | Evidence/issue | Safeguard |
|---|---|---|
| Small-sample overfitting | Only 12 rows | Collect a substantially larger, time-split dataset before deployment. |
| Temporal leakage | No timestamps/cutoff | Define observation and outcome windows; build features only from pre-cutoff data. |
| Sampling bias | Sampling process undocumented | Document population, sampling frame, inclusion/exclusion rules. |
| Privacy/provenance | Consent/licensing not documented | Obtain data-governance approval and document lawful/authorized use. |
| False positive | Unnecessary retention contact/incentive | Use cost-sensitive thresholding and human review. |
| False negative | Missed retention opportunity | Monitor recall and business loss, not accuracy alone. |
| Plan-type dependence | Basic group has 100% churn in this sample | Investigate business meaning and validate on a larger representative dataset. |
| Misuse | Model could be treated as ground truth | Restrict use to decision support and document intended use. |

The template requires bias, privacy, misuse, FP/FN risks and mitigations. fileciteturn0file0L18-L19

## Intended evaluation
Before training/deployment, define:
- **Baseline:** operational rule and/or historical retention policy.
- **Performance:** precision, recall, F1, confusion matrix; accuracy only as a secondary measure.
- **Calibration:** reliability/calibration curve and Brier score when probability outputs are used.
- **Fairness:** subgroup error-rate comparisons if appropriate demographic/sensitive attributes become available and lawful to evaluate.
- **Error analysis:** inspect false positives/false negatives by plan type and other relevant operational segments.
- **Drift:** monitor feature and prediction distributions over time.

The provided template explicitly asks for baseline, performance, calibration, fairness, and error-analysis measures before training. fileciteturn0file0L21-L22

## Overall data-card status
**Prototype / educational use only.**

The current file is useful for demonstrating responsible ML workflow, but it is not sufficient evidence for production deployment.
