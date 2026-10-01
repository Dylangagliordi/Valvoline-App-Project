# Valvoline App Market Analysis

What drives installs for apps in Google Play's **Auto & Vehicles** category, and how does Valvoline's Instant Oil Change app compare to its competitors? A class project analyzing ~988,000 Google Play Store apps with Python, pandas, scikit-learn and Plotly.

## Interactive charts

- [Star ratings: recurring vs one-time apps](https://dylangagliordi.github.io/Valvoline-App-Project/slide9_star_ratings.html)
- [Engagement intensity and ratings](https://dylangagliordi.github.io/Valvoline-App-Project/slide10_engagement_and_ratings.html)

## Key findings

- **Model log installs, not raw installs.** On raw install counts, test R² swings from 0.18 to 0.63 depending on the random split, because a handful of apps have 100M+ installs. On log installs the model is stable at R² ≈ 0.81.
- **Rating Count dominates.** It accounts for ~0.89 of Random Forest feature importance across all apps and ~0.78 within Auto & Vehicles. Permissions barely register, and a permissions-only model explains almost nothing (R² ≈ 0.04). Rating Count partly results from installs, so it signals traction rather than a lever to pull.
- **Recurring-use apps engage more.** Maintenance trackers and service reminders collect about twice as many ratings per install as one-time apps such as store locators (median 0.025 vs 0.012), and are rated higher on average (4.22 vs 3.77 stars).
- **Valvoline's app is well rated.** At 4.40 stars it sits above both group averages; Firestone's app is at 3.50.

## How it's built

```
Google-Playstore.csv  (2.3M apps)  ─┐
                                    ├─ 01_data_cleaning_and_join.ipynb ─→ df_clean.parquet ─→ 02_analysis.ipynb ─→ charts
googleplay-app-permission.json     ─┘     (988K apps × 44 columns)
```

| Notebook | What it does |
|---|---|
| [`01_data_cleaning_and_join.ipynb`](01_data_cleaning_and_join.ipynb) | Streams the large permissions JSON and joins it to the app metadata, drops apps with fewer than 10 ratings, adds a log-installs target, and engineers size, monetization and permission features. |
| [`02_analysis.ipynb`](02_analysis.ipynb) | Trains Random Forest models to rank what predicts installs (all apps, Auto & Vehicles only, permissions only), predicts installs for Valvoline, Firestone and Jiffy Lube, and compares recurring-use vs one-time apps on engagement and ratings. |

## Running it

Both notebooks run in Google Colab and read their data from Google Drive (`MyDrive/Colab Notebooks/`). The raw data files aren't included in this repo because of their size. Run notebook 01 once to create `df_clean.parquet`, then run notebook 02.

## Limitations

- Feature importance measures association, not cause.
- The recurring vs one-time gap has not been tested for statistical significance.
- Recurring/One-Time tags come from keyword matching on app names, and the groups are small (35 and 26 apps).
- "Minimum Installs" is Google Play's install bucket (for example 100,000+), not an exact count.
