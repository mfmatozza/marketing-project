# findings/ (Findings & charts, Michele)

| File | What |
|---|---|
| `04_findings_charts.ipynb` | Builds every chart from `../data_clean/reviews_laundry_features.csv`. Run it from inside `findings/`. It uses the same model formula as notebook 03. |
| `figures/chart1_low_share_by_brand.png` | Chart 1: share of 1-2 star reviews by brand |
| `figures/chart2_odds_ratios.png` | Chart 2: odds ratios with 95% CI, sorted (all brands) |
| `figures/chart3_lg_vs_competitors.png` | Chart 3: LG vs competitors, share of 1-2 star reviews mentioning each issue |
| `figures/chart4_priority_matrix.png` | Chart 4: priority matrix (LG frequency vs LG-only odds ratio). Top-right = fix first |
| `figures/chart5_refrigerator_replication.png` | Chart 5: only appears once `or_refrigerator.csv` exists (see below) |
| `quotes.md` | 9 short anonymous LG quotes, 3 per top issue |

If the tags or the model change, rerun the notebook (Run All, under 1 minute). Every number and chart title updates by itself.

Issue names and colours follow the Tutorial II deck: blue = LG / service issue, orange = product failure, grey = other or not significant.

## Numbers for the Findings section (current tags, 16 Nov)
These are facts to write from, not the section text. The report rule is that each person writes their own section by hand.

1. **LG already has the fewest low ratings.** 52% of LG laundry reviews are 1-2 stars vs 58-82% for competitors, the lowest of 7 brands. This counts 1-2 vs 4-5 stars, 3-star left out as in the model (n = 3,737). It is the same definition as the Tutorial II deck. → Chart 1
2. **Service issues drive low ratings** (all brands, n = 3,737, controls for brand, year and verified purchase):
   - Customer support: OR 4.95 [3.22-7.60]
   - Return / refund: OR 3.73 [2.19-6.34]
   - Breakdown / reliability: OR 3.38 [2.62-4.36]
   - Repair / technician: OR 3.33 [2.68-4.14]
   - Design / usability (0.56) and noise / vibration (0.70) go with *fewer* low ratings. → Chart 2
3. **LG's gap vs competitors is support, not the machines.**
   - In 1-2 star reviews, customer support is mentioned 22.3% (LG) vs 12.8% (competitors), p < 0.001.
   - Breakdown / reliability (34.6% vs 34.9%) and repair / technician (48.4% vs 49.1%) are the same as competitors, a category problem rather than an LG one.
   - Drying / clothes care is slightly higher for LG (12.8% vs 8.9%, p = 0.005).
   - Noise / vibration is lower for LG (10.5% vs 15.5%).
   - Delivery / install is not shown (1% of reviews). → Chart 3
4. **Priorities for LG** (LG-only model, n = 1,173):

   | Issue | OR | Share of LG low reviews | Quadrant |
   |---|---|---|---|
   | Issue | OR | Share of LG low reviews | Quadrant |
   |---|---|---|---|
   | Customer support | 6.89 | 22% | fix first (service) |
   | Repair / technician | 3.32 | 48% | fix first (service) |
   | Breakdown / reliability | 3.33 | 35% | top-right, but the same rate for all brands (product, category-wide) |
   | Return / refund | 4.95 | 9% | rare but harmful |

   The chart title focuses on support + repair to match the two recommended pilots. → Chart 4
5. **Robustness** (notebook 03, Angelo): support, returns, breakdowns and repair stay significant with 1-star only, 2016+, verified only and OLS. → refer to his table. A refrigerator replication would be chart 5.

The old tutorial headline "delivery/installation 19% vs 12%" is gone after the tag clean-up. It is now 1.0% vs 1.3% and not significant, so don't use it.

## Waiting on others
- **Angelo, refrigerator replication.** Save his main-model odds ratios as `findings/or_refrigerator.csv` with columns `attribute,OR,lo,hi,p` (attribute = `a_support`, ...), then rerun 04. Chart 5 is drawn automatically. Check the title still holds.
- **Angelo, interaction `a_support × is_lg`.** Its OR and p-value would confirm finding 3 inside the model. Send the number.
- **Noemi, tags.** `repair_service` still contains plain `parts` (`\bparts?\b` in notebook 02), which the task breakdown said to drop. If she changes it, rerun 04.
