# App Insights Unlocked: Google Play Store Analysis

An exploratory data analysis (EDA) project investigating Google Play Store apps to understand app ratings, popularity, pricing, app size, content ratings, and category-level performance. The goal is to turn app metadata into practical insights for app developers, product managers, and marketing teams.

## Project Overview

The analysis uses Google Play Store app metadata to explore:

- What app categories are most popular?
- Do installs and ratings move together?
- How do app size, price, and app type relate to performance?
- Which categories and genres have higher average ratings?
- What patterns appear in recorded app update dates?

**Tools:** Python, Pandas, NumPy, Matplotlib, Seaborn, Jupyter Notebook  
**Dataset:** Google Play Store apps dataset  
**Scope:** Data cleaning, exploratory data analysis, visualizations, and business recommendations. Sentiment analysis was intentionally excluded.

## Business Problem

A mobile-app company wants to make data-informed decisions about product development, user experience, pricing, and marketing. This project explores relationships between app characteristics and observed ratings, installs, and review counts to help identify patterns worth investigating further.

## Project Objectives

- Assess overall app ratings and the distribution of app sizes.
- Compare free and paid apps.
- Identify high-install apps and categories.
- Compare average ratings across categories, content ratings, and genres.
- Explore the relationships between installs, ratings, app size, and price.
- Examine the distribution of recorded last-update dates.
- Summarize findings and practical recommendations.

## Dataset and Data Dictionary

The primary dataset contains app-level information. The original columns are:

| Column | Description |
|---|---|
| `App` | App name |
| `Category` | Google Play category |
| `Rating` | Average user rating, on a 0–5 scale |
| `Reviews` | Number of user reviews |
| `Size` | App size, standardized to MB where available |
| `Installs` | Reported install count |
| `Type` | Free or Paid |
| `Price` | Listed app price |
| `Content Rating` | Age/content suitability label |
| `Genres` | App genre label(s) |
| `Last Updated` | Date of the last recorded update |
| `Current Ver` | Current app version |
| `Android Ver` | Minimum Android version requirement |

A separate user-review file was available, but review sentiment analysis was not part of this project's scope.

## Data Cleaning and Preparation

The notebook includes the following preparation steps:

- Inspected column data types and missing values.
- Removed a malformed row with misaligned fields.
- Treated ratings above 5 as invalid.
- Removed exact duplicate rows.
- Standardized whitespace in text columns.
- Converted `Reviews` and `Installs` to numeric values.
- Removed currency symbols from `Price` and converted it to numeric.
- Converted `Last Updated` to datetime.
- Converted usable app-size values to MB; nonnumeric values such as `Varies with device` were left missing for numeric analysis.
- Kept missing values where the correct value could not be reliably inferred.
- Used a separate unique-app view for rankings to avoid listing repeated app names multiple times.

**Cleaned working dataset:** 10,357 rows and 13 columns. Individual analyses use available non-missing values, so denominators vary by question.

## Key Findings

### Dataset Overview

| Metric | Result |
|---|---:|
| Rows after cleaning | 10,357 |
| Unique app categories | 33 |
| Average rating | 4.19 |
| Apps with a valid rating | 8,892 |
| Apps rated 4.0 or above | 6,947 (about 78.13% of apps with valid ratings) |
| Apps with known size | 8,831 |
| Mean app size | 21.29 MB |
| Median app size | 13 MB |
| Apps last updated in 2018 | 6,934 (about 66.95% of cleaned rows) |

### Free vs. Paid Apps

| App type | Count | Share of classified apps |
|---|---:|---:|
| Free | 9,591 | 92.61% |
| Paid | 765 | 7.39% |

One row has a missing app type and is excluded from this comparison. Free apps dominate the dataset.

Average review counts differ substantially by app type:

| App type | Average reviews |
|---|---:|
| Free | 437,373.59 |
| Paid | 11,900.55 |

Free apps have approximately 36.8 times as many reviews on average. This is a descriptive comparison and does not prove that free pricing causes higher engagement; a small number of highly popular apps can strongly affect the mean.

### Categories and Ratings

Categories with the highest average ratings in the notebook:

| Category | Average rating |
|---|---:|
| EVENTS | 4.44 |
| EDUCATION | 4.38 |
| ART_AND_DESIGN | 4.36 |
| BOOKS_AND_REFERENCE | 4.35 |
| PERSONALIZATION | 4.33 |

DATING had the lowest average rating among the listed categories at 3.97. These averages are calculated from apps with available ratings, and category sample sizes differ.

### Most-Installed Apps

The following unique app names are recorded at the highest install tier of 1 billion:

- Facebook
- WhatsApp Messenger
- Instagram
- Messenger – Text and Video Chat for Free
- Subway Surfers

These apps are tied at the maximum recorded install tier, so the list should not be interpreted as a strict ranking.

### Categories by Total Installs

| Category | Total installs (approximately) |
|---|---:|
| GAME | 31.54 billion |
| COMMUNICATION | 24.15 billion |
| SOCIAL | 12.51 billion |
| PRODUCTIVITY | 12.46 billion |
| TOOLS | 11.45 billion |

GAME and COMMUNICATION lead in aggregate installs in this cleaned analysis. Aggregated install figures are not the same as unique active users.

### Relationship Between Installs and Ratings

The Pearson correlation between installs and rating is **0.0509**, indicating an extremely weak positive linear association. Install count alone does not meaningfully explain rating variation.

Average ratings across install bands show a non-linear pattern:

| Install range | Average rating |
|---|---:|
| Up to 1K | 4.20 |
| 1K–10K | 4.03 |
| 10K–100K | 4.09 |
| 100K–1M | 4.21 |
| 1M–10M | 4.29 |
| 10M–100M | 4.38 |
| 100M+ | 4.31 |

The 10M–100M install group has the highest average rating at 4.38. The 100M+ group averages 4.31, so ratings do not increase consistently with install count.

### App Size and Installs

The Pearson correlation between app size and installs is **0.1689**, a weak positive relationship. This suggests app size alone is not a strong predictor of install count.

App sizes are right-skewed: the mean (21.29 MB) is higher than the median (13 MB). GAME has the largest mean app size among categories at 44.13 MB, while TOOLS has the smallest at 8.77 MB.

### Content Ratings

The most common content rating is **Everyone**, with 8,382 apps. Other counts include Teen (1,146), Mature 17+ (447), and Everyone 10+ (377).

Average ratings by content rating were:

| Content rating | Average rating |
|---|---:|
| Adults only 18+ | 4.30 |
| Everyone 10+ | 4.25 |
| Teen | 4.24 |
| Everyone | 4.18 |
| Mature 17+ | 4.12 |
| Unrated | 4.10 |

The Adults only 18+ group contains very few apps, so its mean should be interpreted cautiously.

### App Update Dates

A total of 6,934 apps in the cleaned dataset were last updated in 2018. July has the highest overall count of recorded update dates, with 2,950 records; July 2018 alone accounts for 2,653.

The dataset contains only one last-update date per app and has no records for September–December 2018. Therefore, it cannot establish the average time between successive updates or confirm a recurring seasonal update pattern.

## Challenges Faced

1. High number of duplicate rows: The source contained 1,025 rows, of which 723 were flagged as duplicates. Removing repeated rows left 302 rows. Without patient IDs or source provenance, it was not possible to confirm whether identical rows represented repeated records for the same person.

2. Unexpected category codes: The data contained ca = 4 even though the brief described a 0–3 range, and thal = 0 even though the brief listed categories 1–3. These values were retained and flagged for investigation rather than silently changed.

3. Comparing groups of different sizes: Stacked bar charts show raw counts, but categories have unequal sample sizes. Within-group percentages are needed for fairer comparisons of proportions.

4. Interpreting correlations: Some variables are encoded categories. Their numeric correlations are exploratory summaries and can be misleading if interpreted as continuous clinical measurements.

5. Limited model evaluation: Logistic regression was evaluated on a single 61-record test set. A single split provides only a limited estimate of generalization performance.

6. Unavailable survival information: The dataset has no survival status, mortality outcome, or follow-up time, so the requested survival-rate analysis could not be performed.

## Business Insights and Recommendations

1. **Use multiple success metrics.** Ratings and installs measure different aspects of app performance. Track user satisfaction alongside installs and review volume rather than treating popularity as a proxy for quality.
2. **Investigate high-install categories.** GAME and COMMUNICATION lead aggregate installs in this dataset. Study successful apps in these categories to understand their features, user experience, and positioning before making investment decisions.
3. **Optimize app size thoughtfully.** The size–install correlation is weak. Prioritize performance, download experience, and product requirements rather than assuming larger apps attract more installs.
4. **Interpret free-versus-paid comparisons carefully.** Free apps have far more reviews on average, while paid apps have a slightly higher average rating (4.26 versus 4.18). These are observational differences, not evidence of causation.
5. **Use category and genre ratings with sample sizes.** High average ratings can help identify areas for further research, but the number of rated apps and market demand should also be considered.
6. **Avoid overinterpreting historical update dates.** Complete app update histories would be needed to evaluate update cadence or seasonality reliably.


## Limitations

- The dataset is historical and should not be used to describe the current Google Play Store without newer data.
- Missing ratings and sizes reduce the number of observations available for some analyses.
- Install counts are reported in tiers in the source dataset and are not exact measures of active users.
- Repeated app names exist in the source; a separate unique-app view was used for app-level rankings.
- Correlation does not imply causation.
- Average review counts can be skewed by a small number of highly popular apps.
- Sentiment analysis was excluded due to data limitations.
- The data contains one last-update date per app, not a full update history.

## Tools and Skills Demonstrated

- Python for exploratory data analysis
- Pandas and NumPy for data cleaning, transformation, grouping, and correlation
- Matplotlib and Seaborn for visual analysis
- Data quality checks and missing-value handling
- Descriptive statistics and category-level comparisons
- Business insight development and analytical communication
- GitHub for project documentation and version control

## References

- Dataset: [Google Play Store Apps dataset on Kaggle](https://www.kaggle.com/datasets/lava18/google-play-store-apps)


