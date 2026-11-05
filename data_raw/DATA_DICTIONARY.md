# Data Dictionary

## Dataset overview
- Course dataset name: LG Electronics & appliances: market behaviour and brand reviews
- Industry/context: Consumer electronics and home appliances (retail e-commerce and consumer reviews)
- Unit of observation: electronics_market_behavior.csv: one shopper event (a product view, an add-to-cart, a purchase) on one product in one browsing session; appliance_brand_reviews.csv: one consumer review of one appliance product
- Number of rows: electronics_market_behavior.csv: 649,605; appliance_brand_reviews.csv: 20,004
- Number of columns: electronics_market_behavior.csv: 8; appliance_brand_reviews.csv: 14
- Time period, if applicable: electronics_market_behavior.csv: 2019-10-01 to 2019-11-30; appliance_brand_reviews.csv: 2004-07-12 to 2023-09-01
- Direct brand/company names present? Yes — brand names are observed in both files (LG and competing brands).
- Proxy note, if applicable: electronics_market_behavior.csv: This is a curated multi-brand electronics/appliance retail behavior dataset used as a market-behavior proxy. It is not proprietary LG customer data. LG is one observed brand among competing brands.
  appliance_brand_reviews.csv: This course dataset contains public consumer reviews of LG and selected competing products in appliance categories. It is not proprietary LG data. Brand-specific conclusions are valid only for brands/products actually represented in the file.

## Files and relationships
- `electronics_market_behavior.csv` — one row = one shopper event (a product view, an add-to-cart, a purchase) on one product in one browsing session. Keys: user_id (shopper), session_id (browsing session), product_id (product). Events of one session share session_id; sessions of one shopper share user_id. The file is a stand-alone table (no join to the review file: products and users are different).
- `appliance_brand_reviews.csv` — one row = one consumer review of one appliance product. Keys: review_id (one row); product_id groups the reviews of the same product (product_* columns repeat within a product).

## Variables

### electronics_market_behavior.csv

| Variable | Type | Description | Missing values / notes |
|---|---|---|---|
| event_time | datetime (UTC) | Time of the event, YYYY-MM-DD HH:MM:SS, UTC | none missing |
| event_type | categorical | view, cart (added to cart) or purchase; the types present in the data | none missing |
| user_id | key | Anonymised shopper code (U…), stable across sessions | none missing |
| session_id | key | Anonymised browsing session (S…); every event of a kept session is included | none missing |
| product_id | key | Anonymised product code (P…) | none missing |
| product_category | categorical | Product category of the item (tv, washing_machine, refrigerator, …) | none missing |
| brand | categorical | Brand as recorded by the store, lower case (lg, samsung, …) | none missing |
| price | numeric | Listed price of the product at the time of the event, in the store's currency units | none missing |

### appliance_brand_reviews.csv

| Variable | Type | Description | Missing values / notes |
|---|---|---|---|
| review_id | key | Review code (RV…) | none missing |
| product_id | key | Product code (AP…); several reviews per product | none missing |
| brand | categorical | Brand of the product: LG, Samsung, Whirlpool, GE, Frigidaire, Bosch, Electrolux, Haier (only brands with enough reviews are present) | none missing |
| product_category | categorical | Product family: refrigerator_freezer, laundry, dishwasher, cooking, air_conditioner, other_appliance | none missing |
| product_title | text | Listing title of the product | none missing |
| price | numeric | Listed price (USD) at collection time | 15,883 missing (79.4%); missing when the listing showed no price |
| product_average_rating | numeric | Average star rating of the product over all its reviews on the marketplace (1–5) | none missing |
| product_rating_count | integer | Number of ratings the product had on the marketplace (all reviews, not only those in this file) | none missing |
| review_rating | integer | Star rating given in this review (1–5) | none missing |
| review_title | text | Title written by the reviewer | 2 missing (0.0%) |
| review_text | text | Review text, unedited | 6 missing (0.0%) |
| review_date | date | Date of the review, YYYY-MM-DD | none missing |
| verified_purchase | boolean | Whether the marketplace recorded the reviewer as having bought the product | none missing |
| helpful_votes | integer | Number of helpful votes the review received | none missing |

## Curation performed by the instructor
- Behaviour file: two months of events from a large multi-category online store, restricted to the product categories where LG has meaningful activity; in each category LG and its five largest competitors (by purchases) were kept; events without a brand or session were dropped; a random sample of whole sessions was taken so that funnel rates (view → cart → purchase) are not distorted; shopper, session and product IDs were anonymised.
- Review file: products of eight appliance brands in finished-appliance families (accessories and replacement parts excluded as far as possible); reviews were sampled with a cap per brand × category cell, so the file is curated/stratified: raw review counts must not be interpreted as brand market share.
- No imputation, scaling, text processing or scoring was done.

## Important limitations
- The behaviour file is a market-behaviour proxy: it is not LG's own customer data, and the store, country and currency are not identified.
- The behaviour file covers two months only; seasonality beyond that window cannot be studied.
- Events are for the kept brands and categories only; the store's total traffic is larger, so shares are relative to the kept set.
- Review counts per brand/category reflect the sampling cap, not popularity; product_rating_count gives the marketplace-wide number of ratings.
- Brand-specific conclusions are valid only for brands/products actually represented; reviews are self-selected and skew to extreme experiences.
- Some products may still be accessories or parts despite the filtering; check product_title when a product matters for your question.
