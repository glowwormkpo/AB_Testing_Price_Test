# Pricing A/B Test Outputs - GitHub Upload Version

This folder is designed to avoid GitHub upload failures.

It does **not** include the full cleaned dataset because `cleaned_pricing_ab_data.csv` is about 45 MB and GitHub's browser uploader may reject it. Instead, this folder includes:

- `cleaned_pricing_ab_data_sample_1000.csv` - small sample for previewing schema/data
- `pricing_ab_test_report.pdf` - final report
- `pricing_ab_test_report.md` - Markdown report
- `pricing_ab_test_analysis_notebook.ipynb` - runnable notebook/code
- summary and segment CSV outputs

If you need to store the full cleaned dataset in GitHub, use Git LFS or upload it to cloud storage and link to it from the README.

Avoid committing these files:

- `cleaned_pricing_ab_data.csv`
- `cleaned_pricing_ab_data_part1.csv`
- `cleaned_pricing_ab_data_part2.csv`
- `cleaned_pricing_ab_data_part3.csv`
- `cleaned_pricing_ab_data_part4.csv`
- `all_outputs.zip`
- `github_ready_upload.zip`
- `github_ready_upload_small_parts.zip`

