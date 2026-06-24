# Recommendation Memo

**To:** Leadership / Product & Growth Team  
**From:** Business Analyst  
**Date:** June 23, 2026  
**Re:** New Onboarding & Activation Campaign — Launch Decision  
**Classification:** Internal Decision Document

---

## 1. Business Problem Statement

**Decision to be made:**  
Should the new onboarding and activation campaign be launched to all users, rolled out to a selected segment, or held for further testing?

**Who this impacts:**  
- New users signing up to the subscription-based digital product (primary)
- Customer support teams (operational workload)
- Revenue and finance teams (subscription revenue, refunds)
- Product teams (activation funnel investment)

**What metric should improve:**  
Paid Conversion Rate — the proportion of users who upgrade from free/trial to a paying subscription within the first 30 days. This is the North Star metric because it directly connects user activation to revenue.

**Risks to monitor:**  
- Support ticket rate (operational cost per user)
- Refund rate (revenue quality and user satisfaction)
- Average revenue per converted user (revenue quality — are we converting lower-value users?)
- Segment-level conversion decline (ensuring no user group is harmed)

**Evidence required before recommendation:**  
- Statistically significant improvement in the North Star metric
- No meaningful deterioration in any guardrail metric, and
- Consistent positive signal across key user segments.

---

## 2. Executive Summary

The new onboarding and activation campaign **significantly improves paid conversion rate** (3.19% → 7.04%, +3.86 pp, p = 0.0005), representing a 2.2× improvement over the control experience.

However, the campaign also triggers **elevated support ticket rates** (+10 pp) and a **sharp decline in average revenue per converted user** ($1,630 → $770). These guardrail signals indicate that while more users are converting, the quality and sustainability of those conversions must be addressed before a full launch.

**Recommendation: Launch with targeted fixes (phased rollout).**  
Roll out the campaign to the Free plan segment, which shows the strongest conversion benefit (9.3% vs 3.1%) with acceptable guardrail risk. Simultaneously investigate and address the support ticket surge and revenue quality decline before scaling to other segments.

---

## 3. North Star Metric

**Selected North Star: Paid Conversion Rate**

| Metric | Control | Treatment | Lift |
|---|---|---|---|
| Paid Conversion Rate | 3.19% | 7.04% | **+3.86 pp (+121%)** |

**Why this is the North Star:**  
For a subscription business, converting free or trial users to paid subscribers is the primary revenue event. It effectively addresses both user activation and revenue generation. Every other metric (landing page visits, trial starts, onboarding completion, engagement) is valuable only as far as it contributes to paid conversion, directly or indirectly. Revenue, retention, and long-term growth all flow from this single trigger - conversion to paid user.

**Why other metrics are supporting metrics:**
- *Landing page visit rate* measures reach but not commitment
- *Trial start rate* measures intent but not follow-through
- *Onboarding completion rate* measures activation quality but not monetisation
- *Engagement score* is a leading indicator but not a financial outcome
- *Average revenue per user* is downstream of conversion

**Optimisation risk:**  
Blindly maximising conversion rate could attract low-quality users (low engagement, high refund rate, low revenue) or overload support capacity — exactly what the guardrail data suggests is occurring in the current experiment.

---

## 4. KPI Tree Explanation

The KPI tree structures the North Star into three primary drivers and their sub-drivers:

```
North Star: Paid Conversion Rate
│
├── Driver 1: ACTIVATION
│   ├── Landing Page Visit Rate (63.6% → 72.4% ✅)
│   ├── Trial Start Rate (25.1% → 29.0% ✅)
│   └── Traffic Source Mix (Referral shows highest Treatment lift)
│       └── Device Type Optimisation (Mobile: largest user group, strong gain)
│
├── Driver 2: ENGAGEMENT
│   ├── Onboarding Completion Rate (15.7% → 21.1% ✅)
│   ├── Engagement Score (57.0 → 62.9 ✅)
│   └── 7-Day Retention (not measured — recommended for next iteration)
│       └── Feature Adoption Rate (not measured — recommended for next iteration)
│
├── Driver 3: REVENUE QUALITY
│   ├── Avg Revenue per Converted User ($1,630 → $770 ⚠️)
│   ├── Days to Convert (8.9 → 6.4 days ✅ faster)
│   └── Plan Upgrade Rate (Free plan shows strongest conversion)
│       └── Revenue per User Segment (Premium segment underperforms in Treatment)
│
└── GUARDRAIL METRICS
    ├── Refund Rate (0.0% → 0.4% ⚠️ minor)
    ├── Support Ticket Rate (14.8% → 24.8% 🚨 major)
    └── Segment-Level Conversion Decline (no group shows decline ✅)
```

All activation and engagement drivers show positive movement in Treatment. The revenue quality driver is mixed — faster conversion but lower per-user revenue. All three guardrails require attention before full launch.

---

## 5. Experiment Result Summary

| Metric | Control | Treatment | Change | Assessment |
|---|---|---|---|---|
| Users | 690 | 710 | — | Balanced groups |
| Landing Page Visit Rate | 63.6% | 72.4% | +8.8 pp | Positive |
| Trial Start Rate | 25.1% | 29.0% | +3.9 pp | Positive |
| Onboarding Completion Rate | 15.7% | 21.1% | +5.5 pp | Positive |
| **Paid Conversion Rate** | **3.19%** | **7.04%** | **+3.86 pp** | **Positive** |
| Avg Revenue per User | $51.97 | $54.25 | +$2.28 | Slightly positive |
| Avg Revenue per Converted User | $1,630.10 | $770.41 | -$859.69 | Warning |
| Refund Rate | 0.0% | 0.4% | +0.4 pp | Minor warning |
| Support Ticket Rate | 14.8% | 24.8% | +10.0 pp | Warning |
| Avg Engagement Score | 57.0 | 62.9 | +5.9 | Positive |
| Avg Days to Convert | 8.9 days | 6.4 days | -2.5 days | Positive |

---

## 6. Hypothesis Test Interpretation

**Test:** Two-Proportion Z-Test (one-tailed) for Paid Conversion Rate  
**H₀:** Conversion rate Treatment = Conversion rate Control  
**H₁:** Conversion rate Treatment > Conversion rate Control  
**α:** 0.05

| Result | Value |
|---|---|
| Z-Statistic | 3.264 |
| P-Value | 0.0005 |
| Critical Value | 1.645 |
| Decision | **Reject H₀** |

The conversion improvement is statistically significant at the 99.95% confidence level — far exceeding the 95% threshold. This is not a random result. The probability of observing a lift this large by chance is only 0.05%. The Treatment campaign genuinely improves paid conversion.

---

## 7. Guardrail Analysis

### Guardrail 1: Refund Rate
| Group | Refund Rate |
|---|---|
| Control | 0.00% |
| Treatment | 0.42% |

**Assessment:** Minor concern. 3 users in the Treatment group requested refunds vs 0 in Control. The absolute rate is small, but the signal is non-zero. If the campaign is scaled to 100,000 users, this implies ~420 refund requests. Recommend monitoring closely in production.

### Guardrail 2: Support Ticket Rate
| Group | Support Ticket Rate |
|---|---|
| Control | 14.8% |
| Treatment | 24.8% |

**Assessment:** Major concern. A 10 pp increase in support ticket rate is operationally significant. The new experience may be creating confusion or unmet expectations, particularly during onboarding. The support team must be prepared for higher volume, and the product team should identify which steps in the new flow trigger support requests before full launch.

### Guardrail 3: Average Revenue per Converted User
| Group | Avg Rev / Converted User |
|---|---|
| Control | $1,630.10 |
| Treatment | $770.41 |

**Assessment:** Significant concern. Treatment converts more users but at roughly half the revenue per converted user. This could mean:
- The new campaign attracts lower-intent users who choose cheaper plans
- The friction reduction accelerates conversion but pulls in users who would otherwise have churned or chosen a higher-tier plan
- The 30-day revenue window may not capture long-term value (LTV must be monitored)

Total revenue per group: Control ~$35,862 (22 converters × $1,630) vs Treatment ~$38,521 (50 × $770). The aggregate revenue edge is slight (+$2,659) despite converting 2.3× more users. This trade-off needs to be understood through LTV analysis.

### Guardrail 4: Segment-Level Conversion Decline
No segment showed a decline in conversion rate under Treatment. All regions, device types, traffic sources, and plan types showed higher conversion in Treatment than Control. This is a positive guardrail finding — no user group is being harmed.

---

## 8. Segment-Level Insights

### By Region
All four regions show Treatment outperforming Control. North region shows the strongest absolute lift (8.9% vs 3.5%). West shows the smallest lift (5.1% vs 3.4%). No region shows a negative outcome.

### By Device Type
Mobile (the largest segment, 61% of users) shows strong improvement: 7.4% vs 2.6%. Tablet also shows strong relative improvement (7.1% vs 1.8%). Desktop shows a smaller but positive lift (6.6% vs 4.5%). The campaign appears especially effective on mobile — this is notable given that mobile users represent the majority of new signups.

### By Traffic Source
- **Referral users** show the highest Treatment conversion rate (11.0% vs 2.5%) — the campaign is highly effective for users arriving via referral
- **Social media** users are the only segment where Treatment slightly underperforms Control (6.1% vs 7.8%) — this is an anomaly worth investigating; social-acquired users may respond differently to the new flow
- **Organic and Paid Search** both show strong improvements

### By Plan Type
- **Free plan users** show the largest absolute benefit: Treatment converts at 9.3% vs Control 3.1% — a 3× improvement
- **Basic plan** shows minimal difference (3.9% vs 3.6%) — the campaign has limited impact on users already on a paid plan
- **Premium plan** shows moderate improvement (6.3% vs 2.8%)

The strongest case for launch is among **Free plan users** and **Referral traffic** — these segments show the clearest and most meaningful treatment effect.

---

## 9. Final Recommendation

### Decision: **Phased Launch — Roll Out to Free Plan Segment Immediately; Address Guardrails Before Full Launch**

**Rationale:**

1. The conversion improvement is real, statistically significant, and practically large (2.2× lift, p = 0.0005)
2. All user segments benefit — no group is harmed by the new experience
3. The engagement and activation metrics all move in the right direction
4. The campaign reaches its most impactful population (Free plan users, Mobile users, Referral traffic)

**Conditions before full launch:**
- Investigate and resolve the support ticket rate increase (+10 pp) — identify which steps trigger tickets and fix the UX friction
- Investigate the revenue quality decline (lower avg revenue per converted user)
- Monitor refund rate over 60 and 90-day windows to assess if it stabilises

**Specifically NOT recommended:**
- Immediate full launch without addressing support and revenue quality issues
- Abandoning the campaign — the conversion signal is too strong to ignore

---

## 10. Risks and Limitations

| Risk | Likelihood | Mitigation |
|---|---|---|
| Support ticket surge persists at scale | High | UX review, pre-launch support readiness |
| Revenue per user stays low (Lifetime Value dilution) | Medium | 90-day LTV analysis before full rollout |
| Social traffic segment conversion drops | Low-medium | Investigate creative/messaging fit for social audience |
| Experiment ran for limited time period | Medium | Consider re-running for 4–6 weeks to capture seasonal effects |
| Refund rate increases at scale | Low | Monitor first cohort after phased launch |

**Limitations of this analysis:**
- 30-day revenue window may understate LTV for users with longer conversion cycles
- Missing device type (18 users) and traffic source (24 users) data reduces precision of segment analysis
- Experiment duration and timing (Jan–May 2025) may introduce seasonal biases
- No network or referral effects have been measured

---

## 11. Next Steps

1. **Immediate (Week 1):** Launch Treatment to Free plan users only; instrument support ticket categorisation to identify drop-off points in new onboarding flow
2. **Short-term (Weeks 2–4):** Monitor support ticket rate daily; target reduction to ≤18% before expanding rollout
3. **Medium-term (Month 2):** Pull 60-day LTV data for Treatment converters; compare to Control cohort
4. **Medium-term (Month 2–3):** Investigate social traffic segment anomaly — consider A/B testing campaign messaging for social-acquired users separately
5. **Full launch decision (Month 3):** If support tickets normalise and LTV holds, proceed with full rollout to all user segments
6. **Follow-on experiment:** Test campaign variants specifically for Desktop users and Paid Search traffic (currently showing room for improvement)
