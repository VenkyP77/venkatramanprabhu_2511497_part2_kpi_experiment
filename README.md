# Part 2: KPI Framework, Business Experiment Analysis & Decision Recommendation

## Business Context

A subscription-based digital product company launched a new onboarding and activation campaign to improve user conversion and early engagement. Users were divided into two groups:

- **Control group (n = 690):** Existing onboarding experience
- **Treatment group (n = 710):** New campaign experience

Leadership needs to know whether the Treatment experience should be launched to all users. This analysis frames the business problem, defines the right success metrics, analyzes experiment results, evaluates guardrail metrics, and delivers a data-backed decision recommendation.

---

## Business Problem Statement

**Decision:** Should the new onboarding and activation campaign be launched to all users, a selected segment, or held for further testing?

**Impacted stakeholders:** New users, customer support teams, revenue and finance teams, product teams

**Metric to improve:** Paid Conversion Rate — the proportion of total users who upgrade to a paid subscription within 30 days

**Risks to monitor:**
- Support ticket rate (operational cost)
- Refund rate (revenue quality and satisfaction)
- Average revenue per converted user (revenue quality)
- Segment-level conversion decline (ensure no group is harmed)

**Evidence required:** Statistically significant improvement in paid conversion rate, with no meaningful deterioration in guardrail metrics, and consistent results across key user segments.

---

## Dataset Description

**File:** `data/campaign_experiment_data.xlsx`  
**Source:** Internal experiment tracking system  
**Rows:** 1,408 (raw) → 1,400 (after de-duplication)  
**Experiment period:** January – May 2025

| Column | Description |
|---|---|
| user_id | Unique user identifier |
| signup_date | Date user signed up |
| experiment_group | Control or Treatment |
| region | Geographic region (North, South, East, West) |
| device_type | Mobile, Desktop, or Tablet |
| traffic_source | Organic, Paid Search, Social, Referral, Email |
| plan_type | Free, Basic, or Premium |
| visited_landing_page | Binary: 1 if user visited landing page |
| started_trial | Binary: 1 if user started a trial |
| completed_onboarding | Binary: 1 if user completed onboarding |
| converted_to_paid | Binary: 1 if user became a paying subscriber |
| revenue_30d | Revenue generated in first 30 days ($) |
| support_tickets_30d | Number of support tickets submitted |
| refund_requested | Binary: 1 if user requested a refund |
| days_to_convert | Days from signup to paid conversion (null if not converted) |
| engagement_score | Behavioural engagement score (0–100) |

---

## Data Quality Handling

| Check | Finding | Action |
|---|---|---|
| Missing device_type | 18 rows | Retained; excluded from device segment analysis |
| Missing traffic_source | 24 rows | Retained; excluded from traffic segment analysis |
| Missing engagement_score | 14 rows | Retained; excluded from engagement averages |
| Missing days_to_convert | 1,336 rows | Expected — only converted users have a conversion date |
| Duplicate user_ids | 8 pairs (16 rows) | Removed 8 duplicates; first occurrence kept |
| Invalid binary values | None found | All binary columns contain only 0 and 1 |
| Revenue outliers (>3σ) | Present in Control (max $8,610.72) | Retained — legitimate high-value users; noted in analysis |
| Segment balance | All four regions represented in both groups | No rebalancing required |

All analysis uses the clean deduplicated dataset of 1,400 users (690 Control, 710 Treatment).

---

## North Star Metric

**Selected: Paid Conversion Rate**

Paid conversion rate is the primary revenue event for a subscription business. It measures whether users cross the most critical threshold — from free/trial to paying customer. All activation and engagement metrics are valuable only insofar as they contribute to this outcome.

**Result:**
- Control: 3.19% (22 / 690)
- Treatment: 7.04% (50 / 710)
- Lift: +3.86 percentage points (+121% relative improvement)

**Optimisation risk:** Blindly maximising conversion could attract low-quality users with high refund rates or low revenue, or overload customer support — as partially observed in this experiment.

---

## KPI Tree Summary

The KPI tree breaks the North Star into three primary drivers:

**Driver 1 — Activation**
- Landing Page Visit Rate (63.6% → 72.4% ✅)
- Trial Start Rate (25.1% → 29.0% ✅)
- Sub-drivers: Traffic source mix, Device type optimisation

**Driver 2 — Engagement**
- Onboarding Completion Rate (15.7% → 21.1% ✅)
- Engagement Score (57.0 → 62.9 ✅)
- Sub-drivers: 7-day retention, Feature adoption rate

**Driver 3 — Revenue Quality**
- Avg Revenue per Converted User ($1,630 → $770 ⚠️)
- Days to Convert (8.9 → 6.4 days ✅)
- Sub-drivers: Plan upgrade rate, Revenue per user segment

**Guardrail Metrics:**
- Refund Rate (0.0% → 0.4% ⚠️)
- Support Ticket Rate (14.8% → 24.8% 🚨)
- Segment-Level Conversion Decline (none observed ✅)

See `outputs/kpi_tree.png` for the full visual.

---

## Experiment Analysis Approach

See `analysis/experiment_analysis.xlsx` for full workbook with three sheets:

1. **Raw Data** — original 1,408 rows as provided
2. **Quality Checks** — documented findings for all 8 quality checks performed
3. **Clean Data** — deduplicated 1,400-row dataset used for all analysis

See `outputs/experiment_summary.xlsx` for all summary metrics across five sheets:
- Overall Summary (Control vs Treatment comparison with hypothesis test block)
- Region Segment
- Device Segment
- Traffic Source Segment
- Plan Type Segment

---

## Hypothesis Test Summary

**Test:** Two-Proportion Z-Test (one-tailed)  
**Metric:** Paid Conversion Rate  
**H₀:** Conversion rate Treatment = Conversion rate Control  
**H₁:** Conversion rate Treatment > Conversion rate Control  
**Significance level:** α = 0.05

| Statistic | Value |
|---|---|
| Z-Statistic | 3.264 |
| P-Value (one-tailed) | 0.0005 |
| Critical Value | 1.645 |
| Result | **Reject H₀** |

The conversion improvement is statistically significant at the 99.95% confidence level. The new campaign demonstrably improves paid conversion rate and the result is not attributable to random chance.

See `analysis/hypothesis_test_notes.md` for full test documentation.

---

## Guardrail Metrics Considered

| Guardrail | Control | Treatment | Status |
|---|---|---|---|
| Refund Rate | 0.00% | 0.42% | ⚠️ Minor concern — monitor at scale |
| Support Ticket Rate | 14.8% | 24.8% | 🚨 Major concern — investigate before full launch |
| Avg Revenue per Converted User | $1,630.10 | $770.41 | ⚠️ Significant — investigate revenue quality |
| Segment-Level Decline | — | — | ✅ No segment harmed |

---

## Final Recommendation

**Phased Launch** — Roll out the campaign to Free plan users immediately while investigating guardrail issues before full launch.

**Supporting evidence:**
- Conversion improvement is statistically significant (p = 0.0005) and practically large (2.2× lift)
- All user segments show positive conversion movement — no group is harmed
- Free plan users show the strongest benefit (9.3% vs 3.1%)
- Mobile and referral segments show outsized positive response

**Conditions for full launch:**
- Resolve support ticket rate increase (+10 pp) — UX root cause analysis required
- Investigate and address revenue quality decline (avg revenue per converted user halved)
- Monitor refund rate over 60–90 day window

See `outputs/recommendation_memo.md` for the full decision memo.

---

## Assumptions and Limitations

- Analysis assumes proper randomisation in group assignment (not independently verified)
- 30-day revenue window may understate LTV for users with longer conversion timelines
- Missing device_type (18 users) and traffic_source (24 users) reduce precision of those segment analyses
- Experiment period (Jan–May 2025) may introduce seasonal effects not visible in the data
- 8 duplicate user IDs were treated as data quality issues; first occurrence was retained

---

## Screenshots Included

| File | Contents |
|---|---|
| `screenshots/summary_metrics.png` | Control vs Treatment comparison table for all key metrics |
| `screenshots/hypothesis_test_output.png` | Two-proportion Z-test calculation and result |
| `screenshots/kpi_tree_preview.png` | KPI tree visual showing North Star, drivers, and guardrails |
| `screenshots/kpi_tree_guardrail_metrics.png` | Guardrail KPI Tree visual showing key guardrail metrics |

---

## Repository Structure

```
part2_kpi_experiment/
├── data/
│   └── campaign_experiment_data.xlsx       # Raw experiment dataset
├── analysis/
│   ├── experiment_analysis.xlsx            # Data quality checks + clean data
│   └── hypothesis_test_notes.md            # Full hypothesis test documentation
├── outputs/
│   ├── kpi_tree.png                        # KPI tree diagram
│   ├── experiment_summary.xlsx             # Summary metrics + segment analysis
│   └── recommendation_memo.md              # Full decision recommendation memo
├── screenshots/
│   ├── summary_metrics.png                 # Key metrics comparison screenshot
│   ├── hypothesis_test_output.png          # Hypothesis test output screenshot
│   ├── kpi_tree_preview.png                # KPI tree preview screenshot
|   └── kpi_tree_preview.png                # Guardrail KPI Tree screenshot
└── README.md                               # This file
```
