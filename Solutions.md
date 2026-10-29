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
