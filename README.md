# Cookie Cats A/B Test Analysis

## Business Problem
Cookie Cats is a mobile puzzle game that uses "gates" — forced pauses that require players to wait or make an in-app purchase to continue. The product team tested moving the first gate from **level 30 to level 40** to see if delaying the gate would improve player retention.

This analysis answers the question: **should the gate stay at level 30 or move to level 40?**

---

## Dataset
- **Source:** [Kaggle — Cookie Cats Mobile Game A/B Test](https://www.kaggle.com/datasets/yufengsui/mobile-games-ab-testing)
- **Size:** 90,189 players
- **Period:** First week after installation
- **Groups:** gate_30 (control, n=44,700) vs gate_40 (test, n=45,489)

| Column | Description |
|---|---|
| userid | Unique player identifier |
| version | gate_30 (control) or gate_40 (test) |
| sum_gamerounds | Rounds played in first week |
| retention_1 | Did the player return after 1 day? |
| retention_7 | Did the player return after 7 days? |

---

## Tools & Methods
- **Python** — pandas, scipy, statsmodels, matplotlib
- **SQL** — SQLite for data storage and aggregation queries
- **Statistical Test** — Two-proportion z-test on 7-day retention rates

---

## Key Findings

![A/B Test Results](outputs/ab_test_results.png)

| Metric | gate_30 (Control) | gate_40 (Test) | Difference |
|---|---|---|---|
| Day 1 Retention | 44.82% | 44.23% | -0.59% |
| Day 7 Retention | 19.02% | 18.20% | **-0.82%** |
| Avg Rounds Played | 52.46 | 51.30 | -1.16 |
| Z-Statistic | 3.16 | — | — |
| P-Value | 0.0008 | — | — |

**The difference in 7-day retention is statistically significant (p = 0.0008, z = 3.16).** We can conclude with high confidence that gate_30 produces genuinely better retention than gate_40.

---

## Business Impact
- Moving the gate to level 40 costs approximately **740 retained players per week**
- Annualised, this represents **38,457 fewer retained players per year**
- At an industry ARPU benchmark of $0.05/day, this represents a conservative **$13,460 annual revenue risk**
- *Note: This is a floor estimate based on 7-day ARPU only. Full lifetime value impact is likely significantly higher.*

---

## Recommendation
**Keep the gate at level 30.**

The data strongly supports maintaining the current gate position. Players who encounter the gate earlier return at a meaningfully higher rate after 7 days. The most likely explanation is that the gate at level 30 creates a natural early stopping point while player motivation is still high — by level 40, engagement may already be declining.

---

## Assumptions
- ARPU of $0.05/day based on mobile gaming industry benchmarks
- Revenue impact calculated over 7-day window only (conservative estimate)
- Sample treated as representative of the full player population

---

## Project Structure
```
cookie_cats_ab_test/
├── analysis.ipynb        ← main notebook
├── cookie_cats.csv       ← raw data
├── cookie_cats.db        ← SQLite database
├── outputs/
│   ├── ab_test_results.png
│   └── summary_table.csv
└── README.md
```