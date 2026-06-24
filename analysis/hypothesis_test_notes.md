# Hypothesis Test Notes

## Business Context

The company has launched a new onboarding and activation campaign for a subscription-based digital product. Users were split into two groups:
- **Control**: Existing onboarding experience
- **Treatment**: New campaign experience

Leadership needs statistical evidence to help them to decide on whether to roll out the new experience to all users.

---

## Metric Selected: Paid Conversion Rate

**Reason for selection:**  
Paid conversion rate is the North Star metric for this experiment. I have selected it primarily because it directly measures whether the new campaign successfully converts users from free/trial status to paying subscribers. This metric also connects directly to revenue and long-term business growth. All activation and engagement metrics are supporting inputs — if conversion to paid, does not improve, then the campaign has not achieved its business objective.

---

## Hypotheses

| | Statement |
|---|---|
| **Null Hypothesis (H₀)** | The paid conversion rate of the Treatment group is same as that of the Control group. There is no meaningful difference attributable to the new campaign. |
| **Alternate Hypothesis (H₁)** | The paid conversion rate of the Treatment group is **greater than** that of the Control group. The new campaign improves conversion. |

---

## Test Specification

| Parameter | Value |
|---|---|
| **Test type** | Two-Proportion Z-Test |
| **Tail** | One-tailed (right-tailed) — testing for improvement, not just difference |
| **Significance level (α)** | 0.05 |
| **Confidence level** | 95% |
| **Critical Z-value** | 1.645 |

**Why one-tailed?**  
The question this test is trying to address is whether the campaign *improves* conversion, not just whether it changes it in either direction. A one-tailed test is more statistically powerful for detecting improvement (one-direction) and hence it is appropriate given the directional business hypothesis.

**Why Z-test (not t-test)?**  
Both samples are large (n > 30) and we are comparing proportions (binary outcome: converted = 1 or 0). The Z-test for two proportions is the standard method for A/B test analysis of conversion rates.

---

## Test Inputs

| Input | Value |
|---|---|
| Control users (n_c) | 690 |
| Treatment users (n_t) | 710 |
| Control conversions | 22 |
| Treatment conversions | 50 |
| Control conversion rate (p_c) | 3.19% |
| Treatment conversion rate (p_t) | 7.04% |
| Observed lift | +3.86 percentage points |

> Note: 8 duplicate user IDs were identified and removed from the raw dataset (1,408 rows → 1,400 clean rows). Each user appears once in either Control or Treatment. All analysis uses the de-duplicated dataset.

---

## Test Calculation

```
Pooled proportion:
  p_pool = (22 + 50) / (690 + 710) = 72 / 1400 = 0.0514

Standard error:
  SE = sqrt[ p_pool × (1 - p_pool) × (1/n_c + 1/n_t) ]
     = sqrt[ 0.0514 × 0.9486 × (1/690 + 1/710) ]
     = sqrt[ 0.0514 × 0.9486 × 0.002836 ]
     = sqrt[ 0.0001384 ]
     = 0.01176

Z-statistic:
  Z = (p_t - p_c) / SE
    = (0.0704 - 0.0319) / 0.01176
    = 0.0385 / 0.01176
    = 3.264

P-value (one-tailed):
  P(Z > 3.264) = 0.0005
```

---

## Test Output

| Result | Value |
|---|---|
| **Z-statistic** | 3.264 |
| **P-value (one-tailed)** | 0.0005 |
| **Critical value (α = 0.05)** | 1.645 |
| **Reject H₀?** | **YES** |

---

## Decision Rule

> **If Z > 1.645 (or equivalently p-value < 0.05), reject H₀.**

**Z = 3.264 > 1.645 → Reject H₀**  
**p-value = 0.0005 < 0.05 → Reject H₀**

The result is statistically significant at the 95% confidence level.

---

## Business Interpretation

The new onboarding and activation campaign produces a **statistically significant improvement in paid conversion rate** (+3.86 percentage points, from 3.19% to 7.04%). This is not attributable to random chance, as shown by the results of the one-tailed test.

The Treatment group converted at **2.2× the rate** of the Control group. With 710 treated users, this translates to 28 additional paying subscribers compared to what would have been expected under the old experience.

**However**, this conclusion must be considered against the guardrail metrics as well, before a final launch is made:

- Support ticket rate increased from 14.8% → 24.8% in Treatment (+10 pp) — this may denote a significant operational cost increase
- Average revenue per converted user dropped from $1,630 → $770 (-$860) — Treatment converts more users but at a significantly lower per-user revenue
- Refund rate increased from 0.0% → 0.4% — This is a small but still a non-zero signal

The conversion improvement is statistically significant, but the full business case requires evaluation of these trade-offs before recommending a full launch.

---

## Interpretation Logic Summary

| Question | Answer |
|---|---|
| Is the conversion lift real? | Yes — statistically significant (p = 0.0005) |
| Is the lift practically meaningful? | Yes — 2.2× improvement (+3.86 pp absolute) |
| Can we attribute it to the campaign? | Yes — randomized experiment design |
| Should we launch immediately? | Requires guardrail evaluation — see recommendation memo |
