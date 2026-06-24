# priyankavobalaboina_2511666_part2_kpi_experiment

#  README — A/B Experiment Analysis Project

---

##  Project Overview

This project contains a complete **A/B experiment analysis** evaluating a new onboarding experience for a SaaS product. The experiment ran from January–May 2025 with 1,408 users randomly assigned to Control (existing experience) or Treatment (new experience).

**Primary Goal:** Determine if the new onboarding experience increases paid conversion rates.

**North Star Metric:** Paid Conversion Rate

---

```

---

##  Data Files Explained

### experiment_summary.xlsx (6 sheets)

| Sheet | What It Contains | Why It Matters |
|-------|-----------------|----------------|
| Summary_Metrics | Top-level KPIs for Control vs. Treatment | Quick snapshot of experiment results |
| Segment_by_Region | Conversion rates by North/South/East/West | Identifies geographic patterns |
| Segment_by_Device | Conversion rates by Desktop/Mobile/Tablet | Shows device-specific effects |
| Segment_by_Traffic_Source | Conversion rates by Email/Organic/Paid/Referral/Social | Reveals which channels benefit most |
| Guardrail_Analysis | Risk metrics (revenue, tickets, refunds) | Flags unintended negative effects |
| Recommendation | Final decision and action plan | Business conclusion |

### experiment_analysis .xlsx (5 sheets)

| Sheet | What It Contains | Why It Matters |
|-------|-----------------|----------------|
| Cleaned_Data | 1,408 user-level records with 16 variables | The raw dataset for all analysis |
| Group_Summary_Stats | Means, variances, proportions by group | Foundation for hypothesis tests |
| Hypothesis_Test | 4 complete statistical tests with results | Proves whether differences are real |
| Data_Quality_Checks | Validation of data integrity | Ensures analysis is trustworthy |
| Segment_Distribution | Cross-tabulations by group | Confirms balanced randomization |

---

##  Variables in the Dataset

| Variable | Type | Description |
|----------|------|-------------|
| user_id | ID | Unique user identifier (USR-100001 to USR-101400) |
| signup_date | Date | When the user signed up (Jan–May 2025) |
| experiment_group | Categorical | "Control" or "Treatment" |
| region | Categorical | North, South, East, West |
| device_type | Categorical | Desktop, Mobile, Tablet |
| traffic_source | Categorical | Email, Organic, Paid Search, Referral, Social |
| plan_type | Categorical | Free, Basic, Premium |
| visited_landing_page | Binary (0/1) | Did user visit the landing page? |
| started_trial | Binary (0/1) | Did user start a trial? |
| completed_onboarding | Binary (0/1) | Did user complete onboarding? |
| converted_to_paid | Binary (0/1) | **North Star** — Did user become a paying customer? |
| revenue_30d | Continuous ($) | Revenue generated in first 30 days |
| support_tickets_30d | Count | Number of support tickets filed in 30 days |
| refund_requested | Binary (0/1) | Did user request a refund? |
| days_to_convert | Continuous | Days from signup to paid conversion |
| engagement_score | Continuous (0–100) | User engagement metric |

---

##  Experiment Design

- **Type:** Randomized Controlled Trial (A/B Test)
- **Unit of Randomization:** Individual user
- **Control Group (n=693):** Existing onboarding experience
- **Treatment Group (n=715):** New onboarding experience
- **Duration:** January 1 – May 31, 2025
- **Significance Level:** α = 0.05 (two-tailed)
- **Balance Check:** Groups are balanced across regions, devices, traffic sources, and plan types (verified in Segment_Distribution sheet)

---

##  Key Results Summary

| Metric | Control | Treatment | Lift | Significant? |
|--------|---------|-----------|------|--------------|
| Paid Conversion Rate | 3.17% | 6.99% | +120.5% | ✅ Yes (p=0.0011) |
| Engagement Score | 57.03 | 62.93 | +10.3% | ✅ Yes (p<0.0001) |
| Support Tickets/User | 0.22 | 0.37 | +68.2% | ⚠️ Yes (p<0.0001) |
| Revenue per User | $51.75 | $53.88 | +4.1% | ❌ No (p=0.9163) |

**Final Decision:** LAUNCH WITH MONITORING (exclude social traffic segment)

---

##  Statistical Methods Used

1. **Chi-Square Test of Independence** — For comparing conversion rates (binary proportions)
2. **Welch's Two-Sample t-Test** — For comparing means of continuous variables (engagement, revenue, tickets)
3. **IQR Method** — For outlier detection in revenue data
4. **Cross-Tabulation** — For verifying randomization balance

---

##  How to Reproduce This Analysis

### Step 1: Data Quality Checks
- Verify no missing values (COUNTA across all columns)
- Check for duplicate user IDs (COUNTIF > 1)
- Validate binary columns contain only 0/1
- Check engagement scores are within 0–100 range
- Flag revenue outliers using IQR method

### Step 2: Compute Summary Statistics
- Calculate means, standard deviations, min/max for continuous variables by group
- Calculate proportions for binary variables by group
- Compute relative lift: (Treatment − Control) / Control × 100

### Step 3: Run Hypothesis Tests
- **Conversion Rate:** Chi-Square test on 2×2 contingency table
- **Engagement Score:** Two-sample t-test (unequal variances)
- **Support Tickets:** Two-sample t-test (unequal variances)
- **Revenue per User:** Two-sample t-test (unequal variances)

### Step 4: Segment Analysis
- Break down conversion rates by region, device, and traffic source
- Identify segments with negative or unusually low lift

### Step 5: Guardrail Assessment
- Evaluate revenue per converter (not just per user)
- Check support ticket burden
- Monitor refund rate emergence
- Flag any segment with negative treatment effect

### Step 6: Form Recommendation
- Weigh primary metric gains against guardrail concerns
- Define kill-switch thresholds
- Propose phased rollout plan

###  KPI Tree: KPI tree created using [Canvas] and exported as PNG 
---

## ⚠️ Limitations & Assumptions

- **No long-term LTV data:** 30-day revenue window may not capture full customer value
- **Refund sample too small:** Only 3 refunds in Treatment — need more data to assess true refund risk
- **Social traffic anomaly unexplained:** Requires qualitative research to understand why Treatment underperforms for social users
- **Unequal variances:** Revenue variance is 3.6× higher in Control (due to one $8,610 outlier) — Welch's t-test accounts for this
- **No multiple testing correction:** Four tests at α=0.05 each; Bonferroni-adjusted α would be 0.0125 — all significant results still pass this stricter threshold

---

