# 🔍 Landing Page A/B Testing and Revenue Analysis (Python)

## 🎯 Business Objective

Determine whether **landing page B** outperforms **page A (control)** in an e-commerce experiment, and translate the statistical evidence into a practical rollout recommendation.

The analysis covers **40,000 users over January 1–28, 2026**. It evaluates **conversion rate**, **average spending among buyers**, and **revenue per exposed user**, alongside exploratory comparisons by traffic source and user type.

**Main finding:** page B achieved a **26.92% relative increase in conversion rate** and **42.83% higher revenue per exposed user**. The recommendation is a monitored rollout, subject to verifying the experiment’s assignment process and implementation costs.

## 🛠️ Tools and Methodology

| Area | Approach |
|---|---|
| Language and environment | Python in Jupyter Notebook. |
| Libraries | pandas, NumPy, Matplotlib, Seaborn, SciPy, statsmodels, and IPython. |
| Data validation | Checked required columns, missing values, unique user identifiers, dates, expected categories, finite nonnegative spending, and consistency between conversion and spending. |
| Data preparation | Standardized column names and category labels to English, parsed dates, and exported a clean CSV without dropping records or changing spending amounts. Verified the CSV round trip. |
| Allocation checks | Sample-ratio mismatch test under an assumed 50:50 allocation, plus composition checks by region, device, traffic source, and user type. |
| Buyer spending | Two-sided Welch’s t-test and 95% confidence interval; distribution diagnostics, Levene’s test, bootstrap inference, Mann–Whitney U, and sensitivity analysis excluding upper IQR outliers. |
| Conversion | Two-proportion z-test, normal-approximation checks, absolute and relative lift, and a 95% interval for the rate difference. |
| Segment analysis | Chi-square independence tests, expected-frequency checks, and Cramér’s V for association strength. |
| Revenue per exposed user | Included zero-spend users; used 10,000 within-group bootstrap resamples for difference and relative-lift intervals, with Welch’s test as a complementary mean comparison. |
| Visualization | Spending histograms, box plots, normal Q–Q plots, grouped counts, 100% stacked bars, and an expanded traffic-rate chart with pointwise 95% intervals. |

Tests use **α = 0.05**. Reported treatment effects consistently use **B − A**. Bootstrap calculations use **seed 42**. Monetary values are expressed in **currency units (CU)** because the dataset does not specify a currency.

Mann–Whitney provides a complementary rank/distribution comparison, not another test of the same mean difference. Outlier exclusion is a sensitivity check; the primary analysis retains all valid observations. Bootstrap percentile intervals are not presented as null-hypothesis p-values.

## 📊 Experiment Results

| Metric | Page A | Page B | Observed effect, B − A |
|---|---:|---:|---:|
| Exposed users | 19,982 | 20,018 | +36 users |
| Conversions | 2,512 | 3,194 | +682 conversions |
| Conversion rate | 12.57% | 15.96% | +3.38 pp; +26.92% relative |
| Average buyer spending (CU/buyer) | 61.09 | 68.75 | +7.66 CU; approximately +12.5% relative |
| Revenue per exposed user (CU/user) | 7.68 | 10.97 | +3.29 CU; +42.83% relative |

**pp** means percentage points. Differences are calculated before rounding. Conversion rates account for the slightly different group sizes; the raw conversion-count difference is not a normalized treatment effect.

## 📈 Key Business Insights

1. **Page B converts a higher share of visitors.** Conversion increased from **12.57% to 15.96%**, a difference of **3.38 pp**, with a 95% confidence interval of **[2.70, 4.07] pp** and **p = 3.76 × 10⁻²²**. At the observed rates, this corresponds to approximately **34 additional conversions per 1,000 exposed users**, if performance persists.

2. **Buyers on page B spend more.** Average buyer spending increased from **61.09 CU to 68.75 CU**. The Welch interval for B − A is **[6.08, 9.24] CU**, with **p = 3.63 × 10⁻²¹**. The bootstrap interval **[6.10, 9.23] CU** and sensitivity checks support the same direction. This comparison conditions on purchase and does not independently establish an unconditional causal effect.

3. **Revenue per exposed user provides the strongest economic justification.** Including non-buyers, B generated **10.97 CU per user**, compared with **7.68 CU** for A. The observed lift is **42.83%**; the bootstrap 95% interval is **[35.30%, 50.79%]**. The absolute-difference interval is **[2.79, 3.79] CU**, and Welch’s test gives **p = 1.11 × 10⁻³⁷**. The interval is an uncertainty range, not a guaranteed future gain.

4. **Channel volume and conversion efficiency tell different stories.** Organic contributed the most purchases (**2,480; 43.46% of all conversions**), while Email had the highest observed conversion rate (**14.99%**). Rates for Ads, Referral, and Organic were **14.74%**, **13.88%**, and **13.79%**, respectively. The association is nominally significant (**p = 0.03414**) but very small (**Cramér’s V = 0.0147**). The global test does not establish that Email significantly outperforms each other channel.

5. **User type does not show a statistically significant conversion association.** New users converted at **14.36%** and Returning users at **14.09%**. The continuity-corrected chi-square test gives **p = 0.4736**; standard Pearson-based **Cramér’s V ≈ 0.0037**. B has higher observed conversion rates in both segments, but no formal interaction test was performed to establish whether its effect differs by user type.

6. **Validation found no obvious data-integrity or allocation problems.** The dataset contains **40,000 unique users and nine columns**, with no missing values, duplicate user identifiers, negative spending, or conversion–spending inconsistencies. The SRM test gives **p = 0.8572**, and all reported composition-check p-values exceed 0.05. These checks support the analysis but do not prove correct randomization or independence.

## 💡 Strategic Recommendations

- **Proceed with a monitored rollout of page B**, conditional on confirming valid random assignment and acceptable implementation costs. Prioritize conversion and revenue per exposed user when evaluating the rollout.
- **Assess channels using economic outcomes before changing budgets.** Combine conversion rates with acquisition cost, revenue, and margins; channel rankings alone do not establish profitability or causality.
- **Do not introduce different page strategies by user type based solely on these results.** If differentiated treatment effects matter, plan an adequately powered interaction analysis.
- **Monitor persistence and operational performance.** Track conversion, revenue per exposed user, buyer spending, and relevant guardrails after rollout, including changes in traffic composition and seasonality.

## ⚠️ Limitations

- The A/B interpretation depends on valid random assignment, representative exposure, and independent observations. Dataset checks cannot verify every design assumption.
- The observation window covers only **28 days** and does not establish long-term effects or seasonal stability.
- Buyer-only spending conditions on conversion, which may itself be affected by the page version.
- Traffic-source and user-type comparisons are observational and exploratory. Nonsignificance does not establish equivalence, and no page × segment interaction test was performed.
- Multiple hypotheses were examined without a prespecified multiplicity plan. The traffic-source result is especially sensitive to this issue; the three page metrics are related outcomes, not independent replications.
- Revenue per exposed user is the main economic outcome for this recommendation, but the available material does not establish that it was prespecified as the primary endpoint.
- Acquisition costs, margins, retention, and implementation-risk data are unavailable. Higher revenue does not automatically imply higher profit.
- Confidence intervals describe sampling uncertainty under the analysis assumptions; they do not guarantee future performance.

## 👁️ Visualizations

### Buyer spending by page

Histograms, box plots, and normal Q–Q plots show the right-skewed spending distributions and support the choice of unequal-variance inference and robustness checks.

![Buyer spending distributions and diagnostics by page](assets/buyer_spending_diagnostics.png)

### Traffic-source conversion rates

The expanded vertical axis makes small differences visible. Error bars are **pointwise 95% normal intervals**, not simultaneous intervals or formal pairwise tests. The overall-rate reference includes the observations from each channel.

![Traffic-source conversion rates with pointwise 95% confidence intervals](assets/traffic_conversion_rates.png)

### Conversion by user type

Normalized bars distinguish conversion efficiency from the unequal sizes of the New and Returning user populations.

![Conversion proportions for New and Returning users](assets/user_type_conversion.png)
