# A/B Testing: Spotify Premium Free Trial Campaign

Bootcamp case study (Dibimbing Data Analyst Bootcamp). Tests whether offering a 7-day Premium trial to active Free users increases Premium conversion without hurting listening time, across 100,000 users (50,000 per arm).

**Full portfolio write-up:** [Notion](https://app.notion.com/p/A-B-Testing-Spotify-Premium-Free-Trial-Campaign-3de3c020afa8801b9db2c083df26724f)

## Experiment Design

**Goal:** increase Free-to-Premium conversion (primary metric) without reducing Duration Played (guardrail metric).

**Hypothesis:** offering a 7-day Premium trial increases conversion because Free users experience the value of Premium first-hand.

- **H0:** no difference in Premium conversion rate between Control and Target (p_Control = p_Target)
- **H1:** the conversion rates differ (two-tailed test, α = 0.05)

| Role | Metric |
|---|---|
| Primary | Conversion rate (Free to Premium subscription) |
| Guardrail | Duration Played (minutes) |
| Secondary | Engagement |

**Sample size and duration (planned)**

- Assumed baseline conversion 4.20%, minimum detectable effect 10% (relative), α = 0.05, power = 0.80
- Effect size (Cohen's h) = 0.0205, requiring **37,493 users per group (74,986 total)**
- At 400 users per group per day, the planned duration is **94 days**
- Actual test size: **100,000 users**, above the planned minimum

![MDE vs sample size](<img width="1333" height="802" alt="01_mde_vs_sample_size" src="https://github.com/user-attachments/assets/4d046316-923f-4b4d-94d3-06aca81c0cbd" />)


## Method

- **Data cleaning:** no missing values or duplicates. Two rows with negative Duration Played (both in Control, likely technical errors) were removed, leaving 99,998 rows.
- **Conversion rate:** two-proportion Z-test, 95% confidence interval for the difference, and relative lift.
- **Duration Played and Engagement:** Shapiro-Wilk normality check (both groups normal), then a two-tailed independent t-test with a 95% confidence interval for the difference (Welch).

## Result

| Metric | Control | Target | Difference (95% CI) | Relative lift | Test |
|---|---|---|---|---|---|
| Conversion rate | 59.74% | 80.25% | +20.51 pp [19.95, 21.06] | +34.33% | Z = 70.75, p < 0.001 |
| Duration Played | 120.08 min | 159.91 min | +39.84 min [39.46, 40.21] | +33.18% | t = -209.76, p < 0.001 |
| Engagement | 0.600 | 0.800 | +19.98 pp [19.85, 20.10] | +33.30% | t = -315.76, p < 0.001 |

All three metrics moved in the same direction with a similar lift (33-34%). The guardrail metric did not drop; it rose, so the trial lifted conversion without attracting low-quality users who only take the free offer.

![Conversion rate by group](<img width="1334" height="732" alt="02_conversion_rate_by_group" src="https://github.com/user-attachments/assets/5c9e1d3d-6f52-4d81-b4fa-e5fa77fba32a" />)

## Recommendations

1. **Roll out the 7-day trial to all users.** The result is significant and consistent across all three metrics, with narrow confidence intervals.
2. **Monitor after the trial ends.** The data only covers behavior during the trial, so post-trial churn still needs to be tracked.
3. **Run a cost-benefit analysis.** A free trial delays revenue by 7 days; check whether the ~34% conversion lift covers that cost.
4. **Segment the results.** Check whether the effect is equally strong for new vs long-time users.

## Post-Implementation Monitoring

Daily users were tracked for 15 days after rollout (1-15 March 2026, 310,423 users in total).

- Average daily users grew from **16,184 (days 1-7)** to **24,642 (days 8-15)**, up 52.3%
- Trend slope: about **+871 users per day**
- Day 1: 19,304 users, day 15: 24,446 users (+26.6%)

![Daily users after implementation](<img width="1484" height="732" alt="08_post_implementation_daily_users" src="https://github.com/user-attachments/assets/5227c212-3e06-40ae-93a9-e9a9ca06cc8f" />)

**Limitations:** there is no pre-campaign baseline to compare against, Total Users is not the same as Premium conversion, and post-trial retention is not yet visible. The growth is consistent with the A/B result but cannot be attributed to the campaign alone.

## Repository Contents

```
├── README.md
├── ab_testing_analysis.ipynb   # Full analysis (Google Colab notebook)
└── images/                     # Charts used in this README
```

## Tools

Python (pandas, numpy, scipy, statsmodels, matplotlib, seaborn), Google Colab

## Data

Spotify campaign dataset provided as part of the Dibimbing Data Analyst Bootcamp assignment.
