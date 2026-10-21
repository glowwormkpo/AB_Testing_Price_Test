# Pricing A/B Test Report

## Executive conclusion

The $59 test price reduced conversion but increased revenue per visitor. Control conversion was 1.99% versus 1.56% for test, a -0.43% point drop. Revenue per visitor increased from $0.776 to $0.918, a lift of $0.142 (18.25%), with p-value 1.10e-08.

Recommendation: do not ship solely on the raw result. The revenue lift is statistically strong and remains similar after post-stratifying by source, device, OS, and geo-match status, but the experiment shows randomization imbalance, especially by city among matched users. Treat $59 as promising, then rerun with stratified randomization or run an adjusted holdout before broad rollout.

## Data cleaning

- Raw experiment rows: 316,800
- Removed treatment/price mismatch rows: 365
- Flagged invalid timestamp rows retained for primary test: 5,180
- Clean experiment rows used for primary test: 316,435
- Rows without matching geography: 41,141, retained with geography marked Unknown.

## Main results

| metric                              | control   | test      | absolute_lift | relative_lift | p_value     | ci_low      | ci_high     |
| ----------------------------------- | --------- | --------- | ------------- | ------------- | ----------- | ----------- | ----------- |
| Conversion rate                     | 0.0198996 | 0.015555  | -0.00434452   | -0.218322     | 0           | -0.00528598 | -0.00340305 |
| Revenue per visitor                 | 0.776083  | 0.917748  | 0.141665      | 0.182538      | 1.09559e-08 | 0.0930818   | 0.190248    |
| Post-stratified conversion rate     | 0.0199017 | 0.0156492 | -0.00425254   | -0.213677     |             |             |             |
| Post-stratified revenue per visitor | 0.776167  | 0.923301  | 0.147134      | 0.189565      |             |             |             |

Post-stratified check: control revenue per visitor was $0.776 and test revenue per visitor was $0.923, a lift of $0.147 across 154 source/device/OS/geo-match strata.

## Validity checks

| variable          | cramers_v  | p_value      | chi2     | dof |
| ----------------- | ---------- | ------------ | -------- | --- |
| source            | 0.0099434  | 0.000991907  | 31.2863  | 11  |
| device            | 0.015506   | 2.72064e-18  | 76.0825  | 1   |
| operative_system  | 0.0307813  | 1.09523e-62  | 299.819  | 5   |
| country           | 0.00105823 | 0.551654     | 0.354362 | 1   |
| geo_matched       | 0.00105823 | 0.551654     | 0.354362 | 1   |
| city_matched_only | 0.0899871  | 1.02721e-109 | 2229.24  | 922 |

The source, device, and OS gaps are small in magnitude but statistically significant because the sample is large. City imbalance is the biggest concern: Cramer's V is about 0.09 among matched users.

## Linux sensitivity check

Linux is anomalous: Linux users in the test group have 0% conversion. Linux is 1.31% of the cleaned data. Removing Linux increases the estimated revenue lift: control revenue per visitor becomes $0.778 and test revenue per visitor becomes $0.934, a lift of $0.156 with t-statistic 6.184 and p-value 6.24e-10.

| sample           | control_n | test_n | control_conversion | test_conversion | control_revenue_per_user | test_revenue_per_user | revenue_lift | t_statistic | p_value     |
| ---------------- | --------- | ------ | ------------------ | --------------- | ------------------------ | --------------------- | ------------ | ----------- | ----------- |
| All cleaned rows | 202517    | 113918 | 0.0198996          | 0.015555        | 0.776083                 | 0.917748              | 0.141665     | 5.71522     | 1.09559e-08 |
| Linux removed    | 200313    | 111992 | 0.0199488          | 0.0158226       | 0.778002                 | 0.933531              | 0.155529     | 6.18424     | 6.24023e-10 |

## Novelty and time pattern

The available files do not include a new-vs-returning user flag, so the PDF's recommended novelty test cannot be performed directly. Weekly results do not show one clean fade pattern, but weekly revenue lift is noisy; this is another reason to prefer a follow-up test or a longer holdout.

## Bonus stopping-time calculation

Using alpha 0.05, power 0.80…, and the detectable conversion drop implied by moving from $39 to $59, the required sample size is about 5,548 users per group. Average traffic is about 24356 users per week, so a two-week test would need about 11% of traffic in the test group. The practical recommendation is still to run for at least two weeks to capture weekly patterns.

| alpha | power | raw_control_conversion | target_conversion | required_sample_per_group | avg_users_per_week | min_test_traffic_percent_for_two_weeks | recommended_min_runtime_weeks |
| ----- | ----- | ---------------------- | ----------------- | ------------------------- | ------------------ | -------------------------------------- | ----------------------------- |
| 0.05  | 0.8   | 0.0199036              | 0.0131566         | 5548                      | 24356.4            | 11                                     | 2                             |
