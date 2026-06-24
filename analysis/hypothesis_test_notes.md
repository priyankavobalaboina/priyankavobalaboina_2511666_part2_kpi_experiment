
#  Hypothesis Test Notes — A/B Experiment Analysis

> 
> Experiment: Control vs. Treatment (New Onboarding Experience)
> | Sample: 1,408 users (693 Control, 715 Treatment)

---

##  What Are We Testing?

We ran an A/B experiment to see if a **new onboarding experience (Treatment)** performs better than the **existing experience (Control)** at converting users to paid customers. We use hypothesis tests to determine if observed differences are *real* or just *random noise*.

---

##  Key Concepts (Quick Refresher)

- **H₀ (Null Hypothesis):** There is NO difference between Control and Treatment.
- **H₁ (Alternative Hypothesis):** There IS a difference between Control and Treatment.
- **p-value:** The probability of seeing our results if H₀ were true. Smaller = stronger evidence against H₀.
- **α (Alpha) = 0.05:** Our significance threshold. If p < 0.05, we reject H₀.
- **Decision Rule:** If p-value < 0.05 → REJECT H₀ (statistically significant). If p-value ≥ 0.05 → FAIL TO REJECT H₀.

---

## Test 1: Paid Conversion Rate (Chi-Square Test)

### Why Chi-Square?
Conversion is a **binary outcome** (converted = 1, not converted = 0). Chi-Square tests whether two proportions differ significantly.

### Hypotheses
- **H₀:** Conversion rate is the same for Control and Treatment (p_control = p_treatment)
- **H₁:** Conversion rate differs between groups (p_control ≠ p_treatment)

### Observed Frequencies

| Group | Not Converted | Converted | Total |
|-------|--------------|-----------|-------|
| Control | 671 | 22 | 693 |
| Treatment | 665 | 50 | 715 |
| **Total** | **1,336** | **72** | **1,408** |

### Expected Frequencies (Under H₀)

| Group | Not Converted | Converted | Total |
|-------|--------------|-----------|-------|
| Control | 657.56 | 35.44 | 693 |
| Treatment | 678.44 | 36.56 | 715 |

### Results
- **χ² Statistic:** 10.5747
- **Degrees of Freedom:** 1
- **p-value:** 0.0011
- **Decision:** ✅ REJECT H₀

###  What This Means (Plain English)
The Treatment group's conversion rate (6.99%) is significantly higher than Control's (3.17%). This is NOT due to chance — the new experience genuinely doubles conversions. The p-value of 0.0011 means there's only a 0.11% probability this difference happened randomly.

---

##  Test 2: Engagement Score (Two-Sample t-Test)

### Why a t-Test?
Engagement score is a **continuous variable** (0–100 scale). We use Welch's t-test (unequal variances assumed) to compare means.

### Hypotheses
- **H₀:** Mean engagement is the same (μ_control = μ_treatment)
- **H₁:** Mean engagement differs (μ_control ≠ μ_treatment)

### Summary Statistics

| Statistic | Control | Treatment |
|-----------|---------|-----------|
| Mean | 57.03 | 62.93 |
| Variance | 201.13 | 194.04 |
| Observations | 693 | 715 |

### Results
- **t Statistic:** −7.8805
- **Degrees of Freedom:** 1,402.60
- **p-value (two-tail):** < 0.0001 (essentially zero)
- **t Critical (two-tail):** ±1.9617
- **Decision:** ✅ REJECT H₀

###  What This Means (Plain English)
Treatment users scored 5.9 points higher on engagement (a 10.3% lift). The t-statistic of −7.88 is far beyond the critical value of ±1.96, making this extremely significant. The new experience creates meaningfully more engaged users.

---

##  Test 3: Support Tickets — Guardrail Metric (Two-Sample t-Test)

### Why Is This a "Guardrail"?
Guardrail metrics protect against unintended harm. Even if conversions improve, we need to check we're not creating problems elsewhere.

### Hypotheses
- **H₀:** Mean support tickets are the same (μ_control = μ_treatment)
- **H₁:** Mean support tickets differ (μ_control ≠ μ_treatment)

### Summary Statistics

| Statistic | Control | Treatment |
|-----------|---------|-----------|
| Mean | 0.22 | 0.37 |
| Variance | 0.36 | 0.56 |
| Observations | 693 | 715 |

### Results
- **t Statistic:** −4.2287
- **Degrees of Freedom:** 1,354.29
- **p-value (two-tail):** 0.0000251
- **t Critical (two-tail):** ±1.9617
- **Decision:** ⚠️ REJECT H₀ — Support tickets significantly INCREASED

###  What This Means (Plain English)
Treatment users submit 68% more support tickets (0.37 vs. 0.22 per user). This is statistically significant and a genuine concern — the new experience may have UX friction or confusing elements that drive users to seek help. This needs investigation before full rollout.

---

##  Test 4: Revenue per User (Two-Sample t-Test)

### Hypotheses
- **H₀:** Mean revenue per user is the same (μ_control = μ_treatment)
- **H₁:** Mean revenue per user differs (μ_control ≠ μ_treatment)

### Summary Statistics

| Statistic | Control | Treatment |
|-----------|---------|-----------|
| Mean | $51.75 | $53.88 |
| Variance | 222,963.53 | 62,404.36 |
| Observations | 693 | 715 |

### Results
- **t Statistic:** −0.1051
- **Degrees of Freedom:** 1,043.91
- **p-value (two-tail):** 0.9163
- **t Critical (two-tail):** ±1.9622
- **Decision:** ❌ FAIL TO REJECT H₀ — No significant difference

###  What This Means (Plain English)
Despite converting more users, the average revenue per user is virtually identical ($51.75 vs. $53.88). The p-value of 0.92 means we have zero evidence of a revenue difference. More customers are converting, but each spends less — the revenue per *converter* actually dropped 52.7% ($1,630 → $770).

---

##  Summary Table — All Tests at a Glance

| # | Metric | Test | Statistic | p-value | Decision | Meaning |
|---|--------|------|-----------|---------|----------|---------|
| 1 | Paid Conversion Rate | Chi-Square | 10.57 | 0.0011 | Reject H₀ ✅ | Campaign works [+] |
| 2 | Engagement Score | t-Test | −7.88 | < 0.0001 | Reject H₀ ✅ | Higher engagement [+] |
| 3 | Support Tickets | t-Test | −4.23 | 0.0000251 | Reject H₀ ⚠️ | More tickets [!] |
| 4 | Revenue per User | t-Test | −0.11 | 0.9163 | Fail to Reject H₀ ❌ | No revenue change [−] |

---
> *All tests performed at α = 0.05 significance level using two-tailed tests.*
