# Risk Register — Customer Churn ML

| ID | Risk | Likelihood | Impact | Detection / Evidence | Mitigation | Owner |
|---|---|---|---|---|---|---|
| R1 | Very small dataset causes overfitting | High | High | Only 12 records | Collect larger historical dataset; use time-based validation | ML/Data owner |
| R2 | Temporal leakage due to missing timestamps | High | High | No date/cutoff fields | Define feature cutoff and future churn window | Data owner |
| R3 | Unknown provenance/consent/licensing | Medium | High | Source metadata absent | Obtain governance approval and document permission | Data owner |
| R4 | Sampling bias / poor representation | High | High | Sampling process undocumented | Define population and sampling frame; validate on representative data | Data owner |
| R5 | Plan-type dependence | High | Medium/High | Basic has 5/5 churn; Standard/Pro have 0/7 | Validate on larger sample; assess whether plan_type reflects policy or eligibility | ML + business owner |
| R6 | False-positive retention actions | Medium | Medium | Incorrectly flagged non-churners | Cost-sensitive threshold; human review for incentives | Business owner |
| R7 | False negatives | Medium | High | Missed churners | Monitor recall and business loss | Business owner |
| R8 | Feature drift | Medium | Medium | Distribution changes over time | Automated drift monitoring and alert thresholds | ML/Data owner |
| R9 | Model drift | Medium | High | Performance declines on labeled outcomes | Scheduled evaluation and rollback criteria | ML owner |
| R10 | Misuse as an automatic decision engine | Medium | High | Model used without review | Document intended use; enforce human approval for consequential actions | Product owner |
| R11 | Privacy exposure | Medium | High | Customer identifier present | Remove `customer_id` from modeling; restrict access; minimize data | Security/Data owner |
| R12 | Missing/out-of-range inputs | Low/Medium | Medium | Schema/range validation failures | Abstain and route to fallback/human review | ML owner |

## Abstention / fallback conditions
The system should abstain and use the approved fallback when:
- required feature values are missing;
- schema or value ranges fail validation;
- data freshness/timestamp checks fail;
- drift exceeds an approved threshold;
- model monitoring indicates material degradation;
- the requested action is outside the documented intended use.

## Rollback
Rollback to the approved non-ML baseline if:
1. production data quality fails validation,
2. model performance materially falls below the approved threshold,
3. calibration becomes unacceptable for the intended decision,
4. material fairness/error-rate concerns are detected,
5. or a serious pipeline/model defect is identified.
