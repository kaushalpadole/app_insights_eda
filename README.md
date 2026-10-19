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

## Business Insights and Recommendations

1. **Use multiple success metrics.** Ratings and installs measure different aspects of app performance. Track user satisfaction alongside installs and review volume rather than treating popularity as a proxy for quality.
2. **Investigate high-install categories.** GAME and COMMUNICATION lead aggregate installs in this dataset. Study successful apps in these categories to understand their features, user experience, and positioning before making investment decisions.
3. **Optimize app size thoughtfully.** The size–install correlation is weak. Prioritize performance, download experience, and product requirements rather than assuming larger apps attract more installs.
4. **Interpret free-versus-paid comparisons carefully.** Free apps have far more reviews on average, while paid apps have a slightly higher average rating (4.26 versus 4.18). These are observational differences, not evidence of causation.
5. **Use category and genre ratings with sample sizes.** High average ratings can help identify areas for further research, but the number of rated apps and market demand should also be considered.
6. **Avoid overinterpreting historical update dates.** Complete app update histories would be needed to evaluate update cadence or seasonality reliably.

## Answers to All Analysis Questions

The sections below answer every in-scope question from the project brief: 10 Basic, 10 Medium, and 4 Advanced questions. The sentiment-analysis question was intentionally excluded as requested.

### A. Basic-Level Questions

#### 1. What is the average rating of apps?
**Answer:** The average rating is **4.19 out of 5**, calculated from **8,892 apps with valid ratings**.

#### 2. How many unique app categories are present?
**Answer:** There are **33 unique categories** in the cleaned dataset.

#### 3. What is the distribution of app sizes?
**Answer:** App size is available for **8,831 records**. The mean size is **21.29 MB**, the median is **13 MB**, the minimum is approximately **0.0083 MB**, and the maximum is **100 MB**. Since the mean is higher than the median, the size distribution is right-skewed.

#### 4. How many apps are free versus paid?
**Answer:** **9,591 apps are Free (92.61%)** and **765 are Paid (7.39%)** among records with a known type. One record has a missing app type.

#### 5. What is the most common content rating?
**Answer:** **Everyone** is the most common content rating, with **8,382 apps**. Other counts are Teen (1,146), Mature 17+ (447), Everyone 10+ (377), Adults only 18+ (3), and Unrated (2).

#### 6. Which five apps have the most installs?
**Answer:** Facebook, WhatsApp Messenger, Instagram, Messenger – Text and Video Chat for Free, and Subway Surfers. Each is recorded in the **1 billion install tier**. These apps are tied at the highest recorded tier, so this is not a strict rank order.

#### 7. How many apps have a rating of at least 4.0?
**Answer:** **6,947 apps**, or approximately **78.13% of the 8,892 apps with valid ratings**, are rated 4.0 or higher.

#### 8. What is the average number of reviews for free versus paid apps?
**Answer:** Free apps average **437,373.59 reviews**, while paid apps average **11,900.55 reviews**. Free apps therefore have about **36.8 times** as many reviews on average. This is a descriptive comparison and does not show that price causes review volume; a few highly popular apps can strongly affect the mean.

#### 9. What is the average app size for each category?
**Answer:** **GAME** has the largest average app size at **44.13 MB**, while **TOOLS** has the smallest at **8.77 MB**. Other large category averages include FAMILY (27.93 MB), TRAVEL_AND_LOCAL (24.52 MB), and SPORTS (24.18 MB). Category means are based only on records with usable size values.

#### 10. How many apps were last updated in 2018?
**Answer:** **6,934 records**, approximately **66.95% of the 10,357 cleaned rows**, have a last-update date in 2018.

### B. Medium-Level Questions

#### 1. What is the correlation between installs and app rating?
**Answer:** The Pearson correlation is **0.0509**, an extremely weak positive linear association. Install count alone does not meaningfully explain rating differences.

#### 2. Which categories have the highest average ratings?
**Answer:** The five highest category averages are:
1. EVENTS — **4.44**
2. EDUCATION — **4.38**
3. ART_AND_DESIGN — **4.36**
4. BOOKS_AND_REFERENCE — **4.35**
5. PERSONALIZATION — **4.33**

DATING has the lowest category average in this analysis at **3.97**. Category sample sizes differ, so averages should not be interpreted without considering the number of rated apps.

#### 3. How does app price relate to average rating among paid apps?
**Answer:** The overall average rating is **4.26 for paid apps** versus **4.18 for free apps**, but this comparison alone does not show how ratings vary by price. For the paid-only analysis, the Pearson correlation between listed price and rating is approximately **−0.113** across **613 paid apps with valid positive prices and ratings**. This indicates a weak negative linear association, not a strong price effect. Price points have very different sample sizes; for example, $0.99 apps average 4.30 (105 rated apps), $2.99 apps average 4.28 (110), and $4.99 apps average 4.17 (68). Do not infer that raising or lowering a price will cause ratings to change.

#### 4. How are ratings distributed across content-rating groups?
**Answer:** Mean ratings by content rating are:
- Adults only 18+ — **4.30**
- Everyone 10+ — **4.25**
- Teen — **4.24**
- Everyone — **4.18**
- Mature 17+ — **4.12**
- Unrated — **4.10**

The Adults only 18+ result is based on very few records, so it is especially uncertain. These are group averages; a boxplot is useful for comparing medians, spread, and outliers.

#### 5. Which genres have the most apps with over 1 million installs?
**Answer:** After keeping one record per app name and filtering to apps with **more than 1 million installs**, the leading genres by number of qualifying apps are:
1. Tools — **173 apps**
2. Action — **128**
3. Photography — **124**
4. Communication — **99**
5. Productivity — **91**
6. Entertainment — **83**
7. Arcade — **81**
8. Sports — **79**
9. Shopping — **72**
10. Social — **67**

This counts qualifying apps per genre; it is different from summing install counts by genre.

#### 6. How frequently do apps get updated, and what is the average time between updates?
**Answer:** **The average time between updates cannot be calculated from this dataset.** Each app has only one recorded `Last Updated` date, rather than a history of successive update dates. The dataset shows **6,934 records in 2018**, with July 2018 accounting for **2,653** records, but that does not provide an update interval for any individual app.

#### 7. What is the relationship between app size and installs?
**Answer:** The Pearson correlation between size and installs is **0.1689**, a weak positive linear relationship. App size alone is not a strong predictor of installs. Other factors, such as category, features, marketing, and user experience, may also matter but were not tested as causal drivers here.

#### 8. Which apps have the highest review counts, and what are their ratings?
**Answer:** The five highest-review apps in the cleaned unique-app view are:
- Facebook — **78.16 million reviews**, rating **4.1**
- WhatsApp Messenger — **69.12 million**, rating **4.4**
- Instagram — **66.58 million**, rating **4.5**
- Messenger – Text and Video Chat for Free — **56.65 million**, rating **4.0**
- Clash of Clans — **44.89 million**, rating **4.6**

Review volume indicates engagement at scale but is not the same as user satisfaction.

#### 9. How does the content-rating distribution differ between free and paid apps?
**Answer:** **Everyone** is the largest group for both app types: **7,720 Free apps** and **662 Paid apps**. Other counts are:
- Teen — 1,094 Free; 52 Paid
- Mature 17+ — 428 Free; 19 Paid
- Everyone 10+ — 344 Free; 32 Paid
- Adults only 18+ — 3 Free; 0 Paid
- Unrated — 2 Free; 0 Paid

Because free apps are much more numerous overall, raw counts should be supplemented with percentages within each app type when comparing audience composition.

#### 10. What are the five categories with the most total installs?
**Answer:** The highest total recorded installs by category are:
1. GAME — **31.54 billion**
2. COMMUNICATION — **24.15 billion**
3. SOCIAL — **12.51 billion**
4. PRODUCTIVITY — **12.46 billion**
5. TOOLS — **11.45 billion**

These are sums of the dataset's reported install tiers, not counts of unique active users.

### C. Advanced-Level Questions

#### 1. What are the top 10 highest-rated apps, and how do their reviews and installs compare?
**Answer:** To avoid ranking apps with very high ratings but negligible review volume as directly comparable to widely reviewed apps, this comparison uses a threshold of at least **100,000 reviews**. The top results include:
- JW Library — rating **4.9**, **922,752 reviews**, **10 million installs**
- Six Pack in 30 Days – Abs Workout — rating **4.9**, **272,337 reviews**, **10 million installs**
- Tickets + PDA 2018 Exam — rating **4.9**, **197,136 reviews**, **1 million installs**
- Learn Japanese, Korean, Chinese Offline & Free — rating **4.9**, **133,136 reviews**, **1 million installs**

The next group includes apps rated 4.8; Period Tracker is among them, with **4,559,407 reviews** and **100 million installs**. High ratings do not necessarily coincide with the largest review counts or install tiers. The threshold is an analytical choice and should be stated when presenting this ranking.

#### 2. What trends appear in app update dates, and is there seasonality?
**Answer:** **6,934 records** have a last-update date in 2018. July has the highest count across months (**2,950** records), including **2,653 in July 2018**; August has 1,466 and June has 1,226. The data has no recorded update dates for September–December 2018. These results describe the dates in this historical dataset, but they do **not** establish recurring seasonal behavior because coverage is incomplete and each app has only one last-update date.

#### 3. How does average rating change across install ranges?
**Answer:** The average ratings by install range are:
| Install range | Average rating | Apps with ratings |
|---|---:|---:|
| Up to 1K | 4.20 | 1,337 |
| 1K–10K | 4.03 | 1,415 |
| 10K–100K | 4.09 | 1,572 |
| 100K–1M | 4.21 | 2,002 |
| 1M–10M | 4.29 | 1,815 |
| 10M–100M | 4.38 | 641 |
| 100M+ | 4.31 | 110 |

The 10M–100M group has the highest mean rating (**4.38**), while the 1K–10K group has the lowest (**4.03**). The pattern is not consistently increasing, and group sizes differ.

#### 4. What is the relationship between genre and user ratings?
**Answer:** Among genres with at least **20 rated apps**, the highest average ratings include:
- Events — **4.44** (45 rated apps)
- Word — **4.40** (27)
- Puzzle — **4.39** (117)
- Art & Design — **4.36** (56)
- Books & Reference — **4.35** (177)
- Education — **4.30** (446)

This minimum sample-size rule makes the comparison more stable than ranking every genre regardless of how few apps have ratings. Genre ratings are descriptive and do not establish that a genre itself causes higher satisfaction.

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

## References

- Dataset: [Google Play Store Apps dataset on Kaggle](https://www.kaggle.com/datasets/lava18/google-play-store-apps)

---

*This README summarizes the results recorded in the project notebook and solution guide. It distinguishes completed calculations from analyses that require additional data or follow-up work.*
