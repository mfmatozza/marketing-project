# validation/ (Noemi)

Checks how accurate the keyword tags from notebook 02 are. Run `05_validation.ipynb` from inside this folder; it recomputes every validation number in the report.

| File | What |
|---|---|
| `05_validation.ipynb` | Precision, inter-coder agreement (Cohen's kappa), recall, spot check |
| `hand_check_100_coder1.xlsx` | Human coder 1 (Noemi): 172 keyword tags in 100 random reviews, 1 = really a complaint |
| `hand_check_100_AI_coder.xlsx` | AI coder, same 172 tags, labelled blind |
| `hand_check_100_coder2_BLANK.xlsx` | Blank sheet for human coder 2. Save the filled version as `hand_check_100_coder2.xlsx`; the notebook then adds the human-human kappa automatically |
| `ai_labels_200.csv` | AI labels on all 10 categories for 200 further random reviews (1-2 and 4-5 stars) |
| `ai_spot_check_20.xlsx` | Human check of 20 random AI labels |

Coding rules: 1 = the review complains about the category (mild or secondary complaints count); 0 = praise, neutral mention, or the word used in another sense. Price/value = the machine itself is overpriced or not worth the money; repair and warranty costs count as Repair.
