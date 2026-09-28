# ML Problem-Framing Memo

## 1. Decision to support
**Proposed decision:** identify customers who may be at risk of churn so that a retention team can decide whether to offer a retention intervention.

**Important boundary:** the dataset itself does not document the actual business intervention, intervention cost, customer-contact policy, or an approved decision threshold. Those items must be confirmed by the business owner before production use.

## 2. Prediction target
- **Target:** `churned`
- **Meaning in the supplied dataset:** binary label where `1` represents churn and `0` represents non-churn.
- **Observed class counts:** 5 churned and 7 non-churned customers (12 rows total).

## 3. Unit of observation
One row represents one customer record. The unique identifier is `customer_id`.

## 4. Action window
**Not specified in the supplied data.** There is no event date, prediction timestamp, or explicit future window. Therefore, this dataset cannot establish whether the model predicts churn in the next 7, 30, 60, or 90 days.

**Required before deployment:** define an observation cutoff and a future outcome window, e.g. “predict churn during the next N days using information available at the cutoff.”

## 5. Candidate features
- `tenure_months` — customer tenure in months.
- `support_tickets` — number of support tickets recorded.
- `monthly_spend_inr` — monthly spend in INR.
- `last_login_days` — days since last login.
- `plan_type` — Basic, Standard, or Pro.
- `customer_id` — identifier; should not be used as a predictive feature.

## 6. Non-ML baseline
A simple operational baseline is:

> **Flag a customer for retention if `plan_type == Basic`.**

In this tiny supplied dataset, this rule happens to classify all 12 rows correctly. This is **not evidence of real-world generalization** because the dataset contains only 12 observations and all five Basic customers are churned while all Standard/Pro customers are non-churned.

## 7. Model baseline
A reproducible logistic-regression baseline can use the four numeric features plus one-hot encoded `plan_type`. The notebook evaluates it using leave-one-out cross-validation because the supplied dataset is too small for a meaningful conventional train/test split.

The observed leave-one-out result on this dataset is perfect classification, but it must be treated as a **dataset-level diagnostic, not a production performance estimate**.

## 8. Error costs
The supplied materials do not provide business costs for false positives or false negatives.

Before deployment, document:
- **False positive:** contacting/discounting a customer who would not churn; possible cost includes unnecessary incentive spend and customer friction.
- **False negative:** failing to identify a customer who churns; possible cost includes lost revenue and missed retention opportunity.

Threshold selection should be based on these business costs rather than accuracy alone.

## 9. Responsible fallback
Recommended safeguards:
1. **Abstain** when required input fields are missing or outside validated ranges.
2. **Human review** for high-impact retention actions such as large discounts or account changes.
3. **Monitoring** for feature drift, prediction distribution changes, subgroup performance, calibration, and error rates.
4. **Rollback** to the approved non-ML baseline if monitoring detects material degradation or data-pipeline problems.

## 10. Go/no-go assessment
ML may be useful for ranking/triage, but the current dataset is **not sufficient for production deployment**. The main blockers are the extremely small sample size, missing time/action-window definition, undocumented provenance/consent, and absence of validated business error costs.

**Decision:** proceed only as a responsible baseline/prototyping exercise until the missing governance and evaluation requirements are resolved.
