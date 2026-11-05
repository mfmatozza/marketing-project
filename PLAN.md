# Group 11 – LG Customer Dissatisfaction: Project Plan

**Chain to follow (from guidelines):** Business problem → Research question → Data → Method → Insight → Action

## 1. Where we are

| Item | Status |
|---|---|
| Track | LG Electronics Track (proxy data, shortlisted teams present to LG managers) |
| Business problem | LG needs a basis for deciding which product/service issues to fix first. A low star rating signals dissatisfaction but not *why*. |
| Primary RQ | *Which product and service attributes are most strongly associated with low customer ratings in a selected electronics category, and which should LG prioritise improving?* |
| Open items from interim slides | (1) pick the category + sample size, (2) confirm review text is linked to ratings, (3) define "low rating" |
| Next milestones | Tutorial I Thu 5 Nov · Tutorial II Mon 16 Nov · **Final slides/report/code Wed 25 Nov 23:59** · Presentation Thu 26 Nov · LG managers Mon 30 Nov |

## 2. What the data lets us do (checked)

**`appliance_brand_reviews.csv`** – 20,004 reviews, 14 columns. This is the file that answers the RQ. It has `review_rating` (1–5) linked to `review_text`/`review_title`, plus `brand`, `product_category`, `verified_purchase`, `helpful_votes`, `review_date`, `product_id`.

**`electronics_market_behavior.csv`** – 649,605 shopper events (view/cart/purchase), Oct–Nov 2019, different products and users. It **cannot be joined** to the reviews. Use it only as optional context.

Findings from a first look:

- Ratings are **polarised**: 39% are 1-star, 37% are 5-star, only ~6% are 3-star. The mean is a poor summary, so we model low vs. high instead of the average.
- LG review volume by category: laundry 1,287 · refrigerator_freezer 1,221 · cooking 327 · dishwasher 88 · air conditioner 0.
- Share of LG reviews rated 1–2 stars: dishwasher 83% (n=88, too small), refrigerator 51%, laundry 48%, cooking 38%.
- Reviews were **sampled with a cap per brand × category**, so counts are *not* market share. We must never compare review volumes across brands.
- Reviews span 2004–2023, with roughly 1,100–2,300 per year since 2016.

## 3. Key decisions (recommendations)

### 3.1 Category → **Laundry (washers/dryers)**
- Enough LG reviews (1,287) and 5 competitors present (Samsung 613, Whirlpool 806, Frigidaire 263, Electrolux 230, Bosch 204), so LG can be benchmarked.
- The behaviour file also has `washing_machine` with LG events, so we can add a context slide.
- **Backup / robustness check: refrigerator_freezer** (LG 1,221, Samsung 1,287). If the laundry analysis works, rerun the same notebook on refrigerators with one line changed. This gives a replication check at almost no cost.
- Dishwasher is rejected (LG n=88), and so is air conditioner (no LG data).

### 3.2 Definition of "low rating"
- **Low = 1–2 stars, high = 4–5 stars.** Drop 3-star reviews from the classification model because they are only ~6% and ambiguous.
- Sensitivity check: low = 1-star only.
- Justify it in the report with the polarised distribution above.

### 3.3 Sharpening the RQ (optional, for the report)
Keep the primary RQ unchanged. Add two secondary questions that support it:

1. Do the drivers of low ratings differ for LG vs. competitors in laundry?
2. Which drivers are both *frequent* and *strongly harmful*? This is the prioritisation logic.

## 4. Method (all from the syllabus)

| Step | Course session | What we do |
|---|---|---|
| A. Clean & describe | Marketing research approaches | Dedupe, handle the missing text (6 rows), filter accessories/parts via `product_title`, rating distribution, ratings by brand/year. |
| B. Text → attributes | Text analytics for customer insight | Tokenise, remove stopwords, lemmatise. Build an **attribute dictionary** (see 4.1) as flags per review. Cross-check with TF-IDF top terms in low vs. high reviews, and optionally LDA topics (k = 5–8). |
| C. Link attributes to ratings | Regression foundations / Case: diagnosing churn drivers from customer text | **Logistic regression**: `P(low rating) ~ attribute flags + brand + verified_purchase + log(price) + year`. Report odds ratios with confidence intervals. Optionally an OLS on the 1–5 rating as a check. |
| D. Prioritise | Case: perceptual maps / review mining | **Importance–performance matrix**: x = how often the attribute is mentioned, y = its effect on low-rating odds. The top-right quadrant is the priority list. Add LG vs. competitor comparison per attribute. |
| E. Optional extras | Case: perceptual maps | Perceptual map of brands × attributes (correspondence analysis or PCA of mention rates). Add only if time permits. |

### 4.1 Attribute dictionary (draft, refine after reading ~100 reviews)

| Type | Attribute | Example keywords |
|---|---|---|
| Product | Reliability / breakdown | broke, stopped working, failed, died, defective, lemon |
| Product | Noise / vibration | loud, noise, vibrate, rattle, grinding |
| Product | Cleaning performance | doesn't clean, smell, mold, residue |
| Product | Design / usability | door, lid, display, app, wifi, control panel |
| Product | Value for money | price, expensive, worth, cheap |
| Service | Repair / technician | technician, repair, service call, parts, warranty |
| Service | Customer support | customer service, support, phone, rep, hold |
| Service | Delivery / installation | delivery, damaged, installed, scratched |
| Service | Return / refund | refund, return, replace, exchange |

Validate the dictionary by hand-labelling ~100 random reviews and reporting precision. This is a cheap, credible check.

## 5. Python workflow (reproducible, relative paths – required by guidelines)

```
marketing-project/
├── data_raw/                    # extracted course data (never edited)
├── data_clean/reviews_laundry.csv
├── 01_clean_explore.ipynb       # cleaning, filtering, descriptive charts
├── 02_text_features.ipynb       # preprocessing, dictionary flags, TF-IDF/LDA
├── 03_model_priorities.ipynb    # logistic regression, odds ratios, importance-performance chart
├── figures/                     # PNGs used in slides/report
└── requirements.txt
```

- Libraries: `pandas`, `numpy`, `matplotlib`/`seaborn`, `nltk` or `spacy`, `scikit-learn` (TF-IDF, LDA), `statsmodels` (logit with odds ratios and p-values).
- Notebook 01 writes `data_clean/reviews_laundry.csv`. Notebook 02 reads that exact filename and writes `data_clean/reviews_laundry_features.csv`. Notebook 03 reads that one. This is what the guidelines require.
- Final deliverable: zip of notebooks + clean data (if small) + `requirements.txt`, named `30420_Files_Group11.zip`.

## 6. Expected insight → recommendation

The output we want is a ranked list like: *"In laundry, reliability/breakdown and repair-service issues have the highest odds ratios for a low rating and appear in X% of LG's low reviews. LG should prioritise these over noise and design complaints."* Then:

- Specific action per top-2 attribute (e.g. a repair-experience improvement programme, a reliability audit on named product lines).
- Where LG is *worse* than competitors on an attribute, it is a competitive gap. Where it is equal, it is a category-wide problem.
- Trade-offs: cost of each fix vs. size of the effect.

**Optional context slide (behaviour file):** LG vs. competitor view→cart→purchase funnel in `washing_machine`, and the price position. It adds a commercial angle but does not answer the RQ, so keep it to one slide, not the core.

## 7. Limitations to state (graders look for these)

- **Proxy data**: reviews are of LG and competing products on a marketplace, not LG's own customer data. Conclusions are *category-level priorities to validate with LG evidence* (as already said in the interim slides).
- Reviews are self-selected and skew to extremes. The stratified sampling means the sample is not market share.
- Association is not causation. Odds ratios show correlation with low ratings.
- 2004–2023 time span. Product generations differ, so use year as a control or restrict to 2016+ as a robustness check.
- Some products may still be accessories or parts; filter by title.
- Dictionary-based text coding misses sarcasm and negation ("not loud"). Mention it, and handle simple negations if time allows.
- Optional ethics note: reviews are public and we analyse them in aggregate. No personal data is reported.

## 8. Timeline

| When | Task | Who (suggest) |
|---|---|---|
| **Thu 5 Nov** (Tutorial I) | Present: category = laundry, low = 1–2★, method = text attributes + logistic regression, ask for feedback | whole group |
| 5–9 Nov | Notebook 01: cleaning + descriptive stats. In parallel, read ~100 reviews to draft the dictionary | 2 people / 2 people |
| 9–13 Nov | Notebook 02: text features, TF-IDF/LDA, dictionary validation | 2 people |
| 13–16 Nov | Notebook 03 first version: logistic regression and importance–performance chart | 2 people |
| **Mon 16 Nov** (Tutorial II) | Show first results, get feedback on recommendation | whole group |
| 16–20 Nov | Robustness (refrigerator replication, 1★ definition, 2016+). Write the report draft by hand | split by section |
| 20–23 Nov | Slides (8 min presentation, clear visuals, no text walls) and polish report (max 12 pages, 1.5 spacing, 12-pt, APA) | all |
| 24 Nov | Rehearse (no reading from notes), final code run from a clean folder | all |
| **Wed 25 Nov 23:59** | Upload `30420_Slides_Group11.pdf`, `30420_Report_Group11.pdf`, `30420_Files_Group11.zip` (one person uploads) | agreed uploader |
| Thu 26 Nov / Mon 30 Nov | Final presentation, then LG managers if shortlisted | all |

## 9. Report skeleton (maps to guideline Section 6)

1. Managerial problem · 2. RQ · 3. Data (unit = one review, variables, cleaning, proxy assumptions) · 4. Methods (why text coding + logit, assumptions) · 5. Findings (only what answers the RQ) · 6. Managerial interpretation · 7. Recommendation (action, target, trade-offs) · 8. Limitations and next evidence (e.g. LG's own CRM/service tickets, an A/B test of a fix) · 9. Optional ethics/privacy · **AI-use disclosure**.

## 10. Rules to remember

- **AI policy**: GenAI is allowed for coding, debugging and grammar. It is **not** allowed to draft the final report. Include a short AI-use disclosure.
- Interim slides and tutorials are not graded, but missing them as a team costs points.
- Deadlines are strict, files must be PDF and named exactly as in the guidelines.
- Cite dataset by its course name: *"LG Electronics & appliances: market behaviour and brand reviews"*.

## 11. Decisions we need from the group

1. Laundry as the category (my recommendation), or refrigerator instead?
2. OK with low = 1–2★ and dropping 3★?
3. Do we include the behaviour-file context slide, or keep the project 100% on reviews?
4. Who uploads the final files?
