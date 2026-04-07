# Twitch Streamer Engagement & Feature Impact Analysis

> Behavioral analysis of 1,000 top Twitch streamers to identify what drives viewer retention - with a simulated A/B test framework evaluating the impact of content maturity flags by streamer tier.

---

## Project Overview

This project analyzes publicly available Twitch data to uncover patterns in viewer engagement, follow conversion, and retention across streamer tiers and content types. The core deliverable is a **simulated A/B test** evaluating whether mature content designation is associated with higher viewer retention — and critically, whether that effect differs by audience size.

**Why it matters:** Platforms like Twitch face constant product decisions around content policy, discoverability, and creator incentives. This analysis demonstrates how behavioral data can be used to stress-test those decisions before rollout — the same approach applied to customer support or marketplace features in a product analytics context.

---

## Dataset

- **Source:** [Kaggle — Top Twitch Streamers Dataset](https://www.kaggle.com/datasets/aayushmishra1512/twitchdata)
- **Size:** 1,000 rows × 11 columns
- **Key fields:** Watch time, Stream time, Peak viewers, Average viewers, Followers, Followers gained, Views gained, Partnered (bool), Mature (bool), Language

---

## Feature Engineering

Four derived metrics were created to enable richer behavioral analysis:

| Feature | Definition | Purpose |
|---|---|---|
| `viewer_retention_rate` | Average viewers / Peak viewers | Core engagement proxy |
| `followers_per_stream_hour` | Followers gained / Stream hours | Growth efficiency |
| `views_per_follower` | Views gained / Followers | Content stickiness |
| `streamer_tier` | Follower-based segmentation (small / mid / large / mega) | Subgroup analysis |

**Tier breakdown:**
- Small: < 100K followers (n=104)
- Mid: 100K–500K followers (n=559)
- Large: 500K–1M followers (n=204)
- Mega: > 1M followers (n=133)

---

## Exploratory Analysis

EDA surfaced several non-obvious patterns across the streamer population:

- **Retention is not monotonic with tier** — mid and large streamers showed higher retention than mega streamers, suggesting audience loyalty declines at scale
- **Partnered status showed no significant retention advantage** (p=0.67), despite being the platform's primary creator incentive signal
- **English dominates** the top 1,000 streamers, but Portuguese (Gaules) and Spanish-language streamers show competitive retention

**EDA Visualizations:**
- Viewer retention rate by tier (boxplot)
- Followers gained per stream hour by tier
- Views per follower by tier
- Partnered vs. non-partnered retention comparison
- Top 10 language distribution
- Retention rate distribution (histogram)

---

## A/B Test Design & Results

### Experiment: Mature Content Flag → Viewer Retention

**Hypothesis:** Streamers flagged as "Mature" achieve higher viewer retention rates than non-mature streamers.

**Method:**
- Groups: Mature (n=230) vs. Non-Mature (n=770)
- Metric: `viewer_retention_rate` (continuous); threshold-based binary classification at median
- Statistical test: Two-proportion z-test (statsmodels)
- Confidence intervals: 10,000-iteration bootstrap

**Overall Results:**

| Group | Mean Retention | N |
|---|---|---|
| Mature | 0.1874 | 230 |
| Non-Mature | 0.1733 | 770 |
| **Observed Lift** | **+8.13%** | — |

- Z-statistic: 1.80 | P-value: 0.071
- 95% Bootstrap CI: [-0.001, +0.030]
- **Not significant at α=0.05**, but directionally positive — borderline at 90% confidence

---

### Segmented Results by Tier

The aggregate result masked meaningful heterogeneity across tiers:

| Tier | Mature Retention | Non-Mature Retention | Lift | Interpretation |
|---|---|---|---|---|
| Small | 0.1835 | 0.2036 | **-9.9%** | Mature flag may deter small audiences |
| Mid | 0.1931 | 0.1736 | **+11.2%** | Strongest positive effect |
| Large | 0.1890 | 0.1680 | **+12.5%** | Significant positive signal |
| Mega | 0.1482 | 0.1598 | **-7.3%** | Diminishing or negative effect at scale |

**Key Insight:** Mature content is associated with improved retention among **mid and large-tier streamers** (+11–12%), while small and mega streamers show negative effects. A one-size-fits-all content policy would miss this heterogeneity entirely.

---

## Strategic Recommendations

1. **Tier-differentiated content strategy:** Consider allowing mid/large streamers to opt into mature content promotion, while applying more conservative defaults for small and mega-tier creators.
2. **Partnership program signal:** Partnered status alone is not a reliable proxy for retention quality — platform incentives should be revisited.
3. **Experiment power consideration:** The mature-content test is underpowered for small (n=28 mature) and mega (n=19 mature) tiers. A real-world version would require stratified sampling and minimum detectable effect sizing before launch.

---

## Tech Stack

| Category | Tools |
|---|---|
| Language | Python 3 |
| Data manipulation | pandas, numpy |
| Visualization | matplotlib, seaborn |
| Statistical testing | scipy, statsmodels |
| Confidence intervals | numpy (bootstrap) |
| Environment | Google Colab |

---

## Repository Structure

```
twitch-engagement-analysis/
│
├── Twitch_streamer.ipynb     # Full analysis notebook
├── README.md                 # This file
└── images/
    ├── eda_overview.png       # 6-panel EDA visualization
    └── ab_test_segmentation.png  # Tier-level lift analysis
```

---

## How to Run

1. Clone this repository
2. Download the dataset from [Kaggle](https://www.kaggle.com/datasets/aayushmishra1512/twitchdata) and save as `twitchdata-update.csv`
3. Open `Twitch_streamer.ipynb` in Jupyter or Google Colab
4. Run all cells sequentially

---

## Author

**Likhita Nallapati**
