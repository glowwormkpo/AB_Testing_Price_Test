# Pricing A/B Test: Should the Price Go from $39 to $59?

An end-to-end analysis of a pricing experiment across **316,800 user sessions** (Mar–May 2015), testing whether a higher price increases revenue enough to offset lost conversions.

## Result

| Metric | Control ($39) | Test ($59) | Change | p-value |
|---|---|---|---|---|
| Conversion rate | 1.99% | 1.56% | −0.43 pts (−22%) | < 0.001 |
| Revenue per visitor | $0.776 | $0.918 | +$0.142 (+18%) | 1.1e-08 |

**$59 converts fewer users but earns 18% more per visitor.** The lift holds after post-stratification ($0.923 across 154 strata) and after removing Linux users, whose test-group conversion was anomalous ($0.934, p = 6.2e-10).

**Recommendation:** treat $59 as promising, but re-run with stratified randomization or roll out with a holdout. The randomization showed significant imbalance, largest by city (Cramér's V ≈ 0.09).

## Method

1. **Data cleaning:** removed 365 rows where test assignment and price disagreed; flagged 5,180 invalid timestamps (retained); kept 41,141 rows with unmatched geography as "Unknown"
2. **Hypothesis tests:** two-proportion z-test for conversion and Welch's t-test for revenue per visitor, with 95% confidence intervals
3. **Randomization audit:** chi-square and Cramér's V across source, device, OS, country and city
4. **Robustness checks:** post-stratification by source, device, OS and geo-match; Linux-exclusion sensitivity test
5. **Test design:** sample-size and minimum-runtime calculation (α = 0.05, power = 0.80): about 5,548 users per group and at least 2 weeks to capture weekly patterns

## Segment highlights

- Mobile users showed a stronger revenue lift than web users
- iOS and Mac users responded better than most other OS groups
- Friend referrals, Google ads and Facebook ads were the highest-value channels

## Files

| File | Contents |
|---|---|
| `pricing_ab_test_analysis_notebook.ipynb` | Full analysis code |
| `pricing_ab_test_report.md` | Written report |
| `summary_metrics.csv`, `balance_tests.csv` | Core results and randomization checks |
| `*_segments.csv`, `*_distribution_gaps.csv` | Segment results by source, device, OS, city and week |
| `cleaned_pricing_ab_data_sample_1000.csv` | 1,000-row sample of the cleaned data (the full file is about 45 MB) |

**Tools:** Python · pandas · NumPy · Jupyter

