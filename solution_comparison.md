# Comparison: Our Outputs vs. Official Solution

## Bottom Line

Our outputs match the official solution on the core data cleaning and the main pricing conclusion:

- Remove 365 rows where `test` and `price` disagree.
- Use 316,435 rows for the primary analysis.
- Conversion falls at the $59 price.
- Revenue per visitor rises at the $59 price.
- The revenue lift is statistically significant.

The biggest differences are that our report adds stricter validity checks and post-stratification, while the official solution adds a Linux-exclusion sensitivity check and a bonus sample-size/stopping-time discussion.

## Core Metrics

| Item | Official solution | Our outputs | Match? |
|---|---:|---:|---|
| Raw experiment rows | 316,800 | 316,800 | Yes |
| Invalid test/price rows removed | 365 | 365 | Yes |
| Clean rows after price cleanup | 316,435 | 316,435 | Yes |
| Missing geography rows after join | 41,184 before cleanup | 41,141 after price cleanup | Consistent |
| Control conversion | 0.019900 | 0.019900 | Yes |
| Test conversion | 0.015555 | 0.015555 | Yes |
| Control revenue/user | 0.776083 | 0.776083 | Yes |
| Test revenue/user | 0.917748 | 0.917748 | Yes |
| Revenue t-statistic | 5.715224666 | 5.715224666 | Yes |
| Revenue p-value | 1.097e-08 | 1.096e-08 | Yes, rounding only |

## Conclusion Comparison

Official solution:

- Says the price increase is successful for revenue.
- Notes roughly 25% customer loss due to lower conversion.
- Recommends considering longer-term effects on growth, referrals, paid marketing, and segmentation.

Our report:

- Also says the $59 price is promising because revenue per visitor is higher and statistically significant.
- Adds a stronger caution: do not ship solely on the raw result because observed randomization balance is imperfect, especially by city.
- Recommends rerunning with stratified randomization or rolling out with a holdout.

## Segment Comparison

The segment findings are directionally aligned:

- Friend referral, Google ads, and Facebook ads are important high-value channels.
- Mobile users show a stronger revenue lift than web users.
- iOS and Mac users respond more positively than many other OS groups.
- Linux is anomalous: the official solution flags the 0% conversion rate in the Linux test group and removes Linux for a sensitivity test. Our `os_segments.csv` also shows Linux test conversion as 0% and revenue lift as negative.

Official Linux sensitivity check:

- Linux share: 1.305%.
- After removing Linux, control revenue/user = 0.778002.
- After removing Linux, test revenue/user = 0.933531.
- New t-statistic = 6.184240.
- New p-value = about 6.25e-10.

Our outputs did not remove Linux in the primary report, but the notebook/data support the same sensitivity calculation.

## Differences In Scope

Included in the official solution but not emphasized in our report:

- Visual bar plots by source, device, OS, and city.
- Linux-exclusion sensitivity test as a formal re-run.
- Bonus sample-size/stopping-time calculation:
  - Required sample size per group: about 5,548.
  - Average users per week: about 24,356.
  - Suggested minimum test traffic over two weeks: about 11%.
  - Practical recommendation: run at least two weeks to capture weekly patterns.

Included in our outputs but not in the official solution:

- Invalid timestamp flagging. We retained those rows for the primary conversion/revenue test.
- Cramer's V randomization/balance checks.
- Post-stratified source/device/OS/geo-match revenue check.
- A reusable notebook and cleaned CSV exports.
- Split cleaned CSV files and zipped output bundles.

## Recommended Update

Completed. The report and notebook have been updated to:

1. Add the Linux-exclusion sensitivity result.
2. Add the bonus sample-size/stopping-time section.
3. Keep our randomization/post-stratification checks as an extra quality-control section.
