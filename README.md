# Car Care Companion: A Data-Driven App Strategy for Valvoline

**Recommendation:** Valvoline should launch a free, ad-free maintenance-tracking app (reminders, a maintenance log, recall alerts) that brings drivers back for service, with booking as the in-app transaction.

A group consulting project (AN 306) analyzing 988,310 Google Play apps to answer four business questions for Valvoline. Built with Python, pandas, scikit-learn and Plotly.

## The opportunity

Valvoline runs about 2,400 service centers and completes 30M+ services a year. Its Instant Oil Change app is well rated (4.4★), but it only books visits: nothing brings drivers back to the app between oil changes. The project asked whether a maintenance-tracking app could close that gap.

## Four questions, four answers

| Business question | Answer | Key evidence |
|---|---|---|
| **1. Is there room for a better car app?** | Yes. Drivers rate this category poorly. | Auto & Vehicles ranks **44th of 48** categories by average rating (3.89★ vs 4.10★ store-wide). |
| **2. Do recurring-use apps earn deeper engagement than one-time apps?** *(primary)* | Yes. | Maintenance trackers and reminder apps earn **2.0×** the median ratings per install of one-time apps such as store locators (0.025 vs 0.012), and rate **0.45★** higher (4.22★ vs 3.77★). |
| **3. What predicts installs, and do permissions matter?** | Engagement does; permissions barely matter. | In a Random Forest model of Auto & Vehicles installs, Rating Count and Rating carry **88%** of the weight; all 12 permission features together carry about **4%**. |
| **4. Which revenue model should the app use?** | Free and ad-free, built around booking. | **97.7%** of 4.0★+ car apps are free to download, and the most common model (58.2%) is free with no ads or in-app purchases. |

## The competitive opening

| | My Firestone | Valvoline Instant Oil Change | Car Care Companion (target) |
|---|---|---|---|
| App type | Recurring | One-time | Recurring |
| Star rating | 3.50★ | 4.40★ | 4.5★+ |
| Ratings per install | 0.0197 | 0.0245 | 0.041+ (top quarter of recurring apps) |

Firestone shows a service brand can run a recurring maintenance app, but drivers rate it near the bottom of the recurring group. Valvoline already has the goodwill (4.40★) with a one-time app. Car Care Companion puts that goodwill into the recurring format, where Firestone is weak.

## Interactive charts

- [Star ratings: recurring vs one-time apps](https://dylangagliordi.github.io/Valvoline-App-Project/slide9_star_ratings.html)
- [Engagement intensity and ratings](https://dylangagliordi.github.io/Valvoline-App-Project/slide10_engagement_and_ratings.html)

## How we got there

- **Data:** 2,312,944 Google Play apps (June 2021 snapshot) joined with each app's requested permissions, then filtered to the 988,310 apps with at least 10 ratings. 7,403 of them are in Auto & Vehicles.
- **Install model:** predicting raw installs was unstable (R² swung from 0.18 to 0.63 across random splits) because a few apps have 100M+ installs. Modeling log installs fixed that (R² 0.81, stable), and every model in the analysis uses it.
- **Recurring vs one-time:** 61 Auto & Vehicles apps were tagged by keyword patterns in their names (35 recurring, 26 one-time), then compared on ratings per install and star rating.

```
Google-Playstore.csv  (2.3M apps)  ─┐
                                    ├─ 01_data_cleaning_and_join.ipynb ─→ df_clean.parquet ─→ 02_analysis.ipynb ─→ charts
googleplay-app-permission.json     ─┘     (988K apps × 44 columns)
```

| Notebook | What it does |
|---|---|
| [`01_data_cleaning_and_join.ipynb`](01_data_cleaning_and_join.ipynb) | Streams the large permissions file and joins it to the app metadata, removes apps with fewer than 10 ratings, and engineers size, monetization and permission features. |
| [`02_analysis.ipynb`](02_analysis.ipynb) | Trains the install models, compares recurring and one-time apps, places Valvoline and Firestone against both groups, and builds the charts. |

Both notebooks run in Google Colab and read their data from Google Drive. The raw data files aren't included because of their size. Run notebook 01 once to create `df_clean.parquet`, then run notebook 02.

## Limitations

- The data is a June 2021 snapshot, so today's market may differ.
- The engagement comparison uses 61 name-tagged apps and shows correlation, not cause. The gap hasn't been tested for statistical significance.
- Rating Count partly reflects installs (more users leave more reviews), so it signals engagement rather than being a lever on its own.
- My Firestone doesn't match the name-tagging patterns, so it was placed with the recurring apps by hand. Valvoline's app is listed under Shopping, so it's compared to Auto & Vehicles apps rather than modeled with them.
- 24,144 apps with no permission record were counted as requesting zero permissions.
- "Minimum Installs" is Google Play's install bucket (for example 100,000+), not an exact count.
