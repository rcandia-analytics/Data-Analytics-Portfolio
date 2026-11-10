# 🧪 Landing Page A/B Test: Which Version Should We Launch? (Python)

## 🎯 Business Objective
Evaluate an **A/B test** run on the landing page of an e-commerce platform, where **40,000 users** were randomly assigned to **Version A (control)** or **Version B (variant)** between **January 1 and 28, 2026**. The goal is to determine, with statistical evidence, **which version the business should roll out**, considering conversion rate, spend per buyer and revenue per exposed user.

## 🛠️ Tools and Methodology (Hypothesis Testing)
* **Language and Libraries:** Python (Pandas, NumPy, SciPy, statsmodels, Matplotlib) in Jupyter Notebook / Google Colab.
* **Data Quality and Preparation:**
  * Audit of **40,000 users** and 9 variables: no missing values, no duplicates (unique `user_id`) and only expected categories.
  * Conversion of `date` to `datetime` and validation of the business rule **spend = 0 ⇔ not converted** in both directions.
  * Standardization of column names and labels to English, and export of a **clean dataset** (`landing_experiment_clean.csv`).
* **Experiment Validation:**
  * **Sample Ratio Mismatch (SRM)** test: no imbalance between groups (p = 0.857).
  * Balanced composition by region, device, traffic source and user type (all p > 0.05).
* **Test Selection by Metric:**
  * **Two-proportion z-test:** conversion rate, with assumption checks and a 95% confidence interval.
  * **Welch's t-test:** spend per buyer, with assumption checks (skewness, Levene, IQR outliers, Q-Q plots) and robustness checks (bootstrap, Mann-Whitney U, Welch without outliers).
  * **Bootstrap (10,000 resamples) + Welch:** revenue per exposed user, a metric with ~85% zeros and strong right skew.
  * **Chi-square + Cramér's V:** traffic source and user type vs. conversion.
* **Visualization and Validation:** count and 100% stacked bar charts, zoomed conversion-rate chart with 95% CIs, daily conversion trend by version and an A/B scorecard of the three business metrics.

## 📈 Key Findings (Business Insights)

1. **Version B Converts More Visitors:**
   * Conversion rate rises from **12.57%** to **15.96%** (**+3.38 pp**, **+26.92%**), with a 95% CI of **[+2.70, +4.07] pp** (p = 3.76e-22). That is about **34 more buyers per 1,000 visitors**.
2. **Buyers on Version B Spend More:**
   * Average spend per buyer increases from **61.09** to **68.75** monetary units (**+7.66**, **+12.5%**), 95% CI **[+6.08, +9.24]**. The result holds across bootstrap, Mann-Whitney and Welch without outliers.
3. **Revenue per Visitor Grows by 42.83%:**
   * Revenue per exposed user goes from **7.68** to **10.97** monetary units (**+3.29 per user**). The 95% CI for the lift is **+35.30% to +50.79%**. The effect compounds: **26.92% more buyers × 12.5% more spend per buyer**.
4. **Traffic Source: Detectable but Negligible:**
   * Significant association with conversion (χ² = 8.66, p = 0.034) but with a negligible effect size (**Cramér's V = 0.0147**). **Email** has the highest rate (**14.99%**) and **Organic** brings the most buyers (**43.46%** of conversions); only **1.20 pp** separate the best and worst channel.
5. **User Type Does Not Affect Conversion:**
   * New users convert at **14.36%** and returning users at **14.09%** (p = 0.4736). Version B converts better in both segments (descriptive).

## 💡 Strategic Recommendations
* **Roll Out Version B:** it outperforms Version A on conversion, spend per buyer and revenue per visitor, with statistical support on all three metrics.
* **Evaluate Channels on Economics:** Organic matters for volume and Email for efficiency, but the gaps are small; review acquisition cost and profitability per channel before moving budget.
* **No Segmentation by User Type:** the difference between new and returning users is not significant and does not justify separate strategies.
* **Post-Launch Monitoring:** track conversion rate, revenue per user and spend per buyer, ideally keeping a small holdout on Version A, to confirm the lift persists outside the experiment.

## ⚠️ Limitations
* The conclusions assume correct randomization; SRM and balance checks support it but cannot rule out tracking errors.
* The test lasted **28 days**: novelty effects, seasonality and long-term outcomes (repeat purchases, returns) are not captured.
* No formal page × user type interaction test, and no pairwise channel comparisons.
* No acquisition cost data per channel, and spend is reported in unspecified monetary units.

## 👁️ Results Visualization

![A/B Scorecard](AB_Scorecard.png)

![Conversion Rate by Traffic Source](Conversion_by_Channel.png)
