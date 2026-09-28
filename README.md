# Customer Shopping Trends: A Business Analytics Case Study

A statistically rigorous investigation of the [Customer Shopping Trends](https://www.kaggle.com/datasets/iamsouravbanerjee/customer-shopping-trends-dataset/) dataset (3,900 customer records from a retail business), built to answer a different question than most EDA notebooks ask: not "what patterns can I find," but **what can this dataset actually, responsibly tell a retail business — and what can't it?**

> **Analyst's note.** This project began as a beginner exercise in Pandas syntax (`value_counts`, `groupby`, `mean`). It has been rebuilt from scratch as a business-oriented investigation that follows the evidence wherever it leads, including where it contradicts the original analysis.

## Why This Notebook Is Different

Most walkthroughs of this dataset report group means as findings ("subscribers spend more," "26–45 year-olds spend the most") without testing whether those differences are statistically real or just noise. This project treats every one of those claims as a hypothesis to be tested — with Welch's t-tests, ANOVA, and chi-square tests, each paired with an effect size (Cohen's *d*, eta-squared, or Cramer's V) rather than a p-value alone — and reports negative results (data quality problems and non-findings) with the same weight as positive ones.

## Executive Summary

1. **Transaction value carries no usable signal in this dataset.** `Purchase Amount` was tested against every customer, product, and channel attribute available. Every test is either non-significant or "significant" only due to sample size, with a negligible effect size in every case — this dataset cannot support spend-based segmentation or targeting.
2. **The engagement fields (`Subscription Status`, `Discount Applied`, `Promo Code Used`) are completely confounded with gender.** All 1,248 female customer records show *No* for all three, without exception. This is far more consistent with a data-generation artifact than real market behavior, and is flagged as a data-quality issue rather than a marketing insight.
3. **Product category preference does not differ meaningfully by gender** (χ² p = 0.90), and **geographic demand is essentially uniform across all 50 states** (coefficient of variation ≈ 0.11).
4. **The dataset's real, demonstrable value is descriptive, not predictive.** It reliably describes who the customers are and what they buy, but the absence of timestamps, cost data, and repeat-transaction history puts retention, lifetime value, and campaign ROI structurally out of reach.

## Notebook Structure

1. **Data Understanding** — what each row and column represents, and an early data-quality check (`Discount Applied` and `Promo Code Used` turn out to be the same field recorded twice).
2. **Who Are the Customers?** — demographic and geographic profile (68/32 gender split, uniform age spread, no state-level demand concentration).
3. **What Do They Buy?** — category and item-level demand, tested against gender (chi-square) rather than eyeballed.
4. **Does Purchase Amount Actually Vary by Customer or Purchase Context?** — ten predictors of spend, each formally tested (Welch's t-test / ANOVA + effect size).
5. **Engagement, Subscription, and a Confound That Changes Everything** — the gender/engagement confound, quantified with a chi-square test and Cramer's V.
6. **Loyalty Tenure and Stated Purchase Frequency** — an internal-consistency check between `Previous Purchases` and the self-reported `Frequency of Purchases` label.
7. **Customer Satisfaction** — whether `Review Rating` tracks category, season, shipping type, or payment method.
8. **What This Data Cannot Tell Us** — an explicit account of the dataset's structural limits (no timestamps, no cost/margin data, no non-purchaser data), so nothing in Section 9 is read as more conclusive than the evidence supports.
9. **Business Recommendations** — each one traces back to a specific, tested finding from the sections above, with an explicit evidence → implication → action chain.

## Key Findings

- Purchase Amount is capped in an unusually narrow $20–$100 range with a near-zero skew, unlike the right-skewed shape real transaction values almost always show — a strong signal it may have been generated rather than observed.
- Every pairwise correlation among the four numeric fields (Age, Purchase Amount, Review Rating, Previous Purchases) is under 0.05.
- Only `Season` reaches statistical significance as a predictor of Purchase Amount (p = 0.011), and even then the effect size (η² ≈ 0.003) is negligible — none of the original notebook's headline spend claims survive formal testing.
- `Frequency of Purchases` does not track `Previous Purchases` (ANOVA p = 0.145) and should not be relied on as a real cadence signal without validation against actual order dates.
- Of four purchase-context variables tested against `Review Rating`, only `Shipping Type` reaches p < .05 (p = 0.038), with a negligible effect size (≈0.11 points on a scale with SD 0.72).

## Tools

- Python
- Pandas
- NumPy
- SciPy (`scipy.stats` — chi-square, t-tests, ANOVA)
- Matplotlib
- Seaborn

## Project Structure

```text
shopping-trends-business-analysis/
├── datasets/
│   └── shopping_trends.csv
├── notebooks/
│   └── shopping_trends_business_analysis.ipynb
└── README.md
```

## How to Run

```bash
git clone <your-repository-url>
cd shopping-trends-business-analysis
pip install pandas numpy scipy matplotlib seaborn jupyter
jupyter notebook
```

Open `notebooks/shopping_trends_business_analysis.ipynb` and run the cells sequentially. Download the dataset from [Kaggle](https://www.kaggle.com/datasets/iamsouravbanerjee/customer-shopping-trends-dataset/) into `datasets/` first, and check the notebook's data-loading cell to confirm the file path and name match where you've placed it.

## A Note on Interpreting the Results

The dataset shows several patterns (the exact $20–$100 purchase cap, the perfect gender/engagement confound, the near-uniform distributions throughout) that look more consistent with synthetic or randomly generated data than an organic retail transaction log. This notebook treats that possibility as a finding in its own right rather than papering over it — every conclusion here is scoped to "what this specific dataset shows," not "what real shoppers do."
