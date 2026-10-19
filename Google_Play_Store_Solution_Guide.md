# Google Play Store App Insights — Answers with Python Code

Use these snippets after running the notebook's data-cleaning cells. They use the cleaned `apps` DataFrame. Sentiment analysis is intentionally excluded.

# A. Basic-Level Questions


## 1. Average app rating

**Answer:** 4.19/5 across 8,892 available ratings.

```python
average_rating = apps["Rating"].mean()
print(round(average_rating, 2))
```

**How it works:** `mean()` averages valid ratings and excludes missing values.


## 2. Number of unique categories

**Answer:** 33 categories.

```python
print(apps["Category"].nunique())
```

**How it works:** `nunique()` counts distinct category labels.


## 3. Distribution of app sizes

**Answer:** 8,831 known sizes; mean 21.29 MB; median 13 MB; minimum 0.0083 MB; maximum 100 MB. Mean above median suggests right skew.

```python
print(apps["Size"].describe())

import matplotlib.pyplot as plt
plt.figure(figsize=(10, 5))
apps["Size"].dropna().hist(bins=30, edgecolor="black")
plt.title("Distribution of App Sizes")
plt.xlabel("App Size (MB)")
plt.ylabel("Number of Apps")
plt.tight_layout()
plt.show()
```

**How it works:** `describe()` gives summary statistics; the histogram shows the distribution. Missing sizes are excluded.


## 4. Free versus paid apps

**Answer:** 9,591 Free (92.61%), 765 Paid (7.39%), and one missing Type.

```python
print(apps["Type"].value_counts())
print((apps["Type"].value_counts(normalize=True) * 100).round(2))
```

**How it works:** `value_counts()` counts each type; normalized counts give percentages.


## 5. Most common content rating

**Answer:** Everyone, with 8,382 apps.

```python
print(apps["Content Rating"].value_counts())
```

**How it works:** Counts apps in each content-rating category.


## 6. Five most-installed apps

**Answer:** Facebook, WhatsApp Messenger, Instagram, Messenger – Text and Video Chat for Free, and Subway Surfers; all are in the 1-billion install tier.

```python
unique_apps = (apps.sort_values(["Installs", "Reviews"], ascending=[False, False])
    .drop_duplicates(subset="App", keep="first"))
top_5_installed = unique_apps.nlargest(5, "Installs")[
    ["App", "Category", "Installs", "Rating"]]
print(top_5_installed.to_string(index=False))
```

**How it works:** Sorts by installs then reviews, keeps one record per app name, and selects the top five. The top five tie at one billion installs.


## 7. Apps rated 4.0 and above

**Answer:** 6,947 apps, approximately 78.13% of 8,892 apps with ratings.

```python
high_count = (apps["Rating"] >= 4.0).sum()
rated_count = apps["Rating"].notna().sum()
print("Count:", high_count)
print("Percentage:", round(high_count / rated_count * 100, 2), "%")
```

**How it works:** Filters ratings of at least 4.0 and calculates the percentage among non-missing ratings.


## 8. Average reviews for free versus paid

**Answer:** Free 437,373.59; Paid 11,900.55 average reviews.

```python
print(apps.groupby("Type")["Reviews"].mean().round(2))
```

**How it works:** Groups by app type and calculates the mean review count. This does not prove pricing causes more reviews.


## 9. Average size by category

**Answer:** GAME has the largest average (44.13 MB); TOOLS the smallest (8.77 MB).

```python
avg_size = apps.groupby("Category")["Size"].mean().sort_values(ascending=False).round(2)
print(avg_size)
```

**How it works:** Groups by category and averages available numeric sizes.


## 10. Apps last updated in 2018

**Answer:** 6,934 apps, about 66.95% of 10,357 cleaned rows.

```python
count_2018 = (apps["Last Updated"].dt.year == 2018).sum()
print("Count:", count_2018)
print("Percentage:", round(count_2018 / len(apps) * 100, 2), "%")
```

**How it works:** Extracts the year from the datetime column and counts rows matching 2018.


# B. Medium-Level Questions


## 1. Correlation between installs and rating

**Answer:** Pearson correlation = 0.0509, an extremely weak positive linear association.

```python
corr = apps[["Rating", "Installs"]].corr().loc["Rating", "Installs"]
print(round(corr, 4))
```

**How it works:** `corr()` calculates Pearson correlation; a value close to zero indicates little linear association.


## 2. Categories with highest average ratings

**Answer:** EVENTS 4.44; EDUCATION 4.38; ART_AND_DESIGN 4.36; BOOKS_AND_REFERENCE 4.35; PERSONALIZATION 4.33.

```python
avg_rating_category = apps.groupby("Category")["Rating"].mean().sort_values(ascending=False).round(2)
print(avg_rating_category.head(10))
```

**How it works:** Groups by category, calculates mean rating, and sorts descending.


## 3. Price and rating (paid apps only)

**Existing result:** Free apps average 4.18 and paid apps 4.26. This is not a price-level analysis. Run the code below for the exact paid-only comparison; the output is generated from your current data.

```python
paid_rated = apps.loc[(apps["Type"] == "Paid") & apps["Price"].notna() & apps["Rating"].notna() & (apps["Price"] > 0)].copy()
price_rating = paid_rated.groupby("Price")["Rating"].agg(["mean", "count"]).sort_index().round(2)
print(price_rating)
print("Price-rating correlation:", round(paid_rated[["Price", "Rating"]].corr().loc["Price", "Rating"], 4))
print("Price points with at least 5 rated apps:")
print(price_rating[price_rating["count"] >= 5])
```

**How it works:** Filters paid apps with valid positive prices and ratings, calculates mean rating per price, and measures correlation. Low-count price points need caution.


## 4. Ratings across content-rating categories

**Answer:** Adults only 18+ 4.30; Everyone 10+ 4.25; Teen 4.24; Everyone 4.18; Mature 17+ 4.12; Unrated 4.10.

```python
print(apps.groupby("Content Rating")["Rating"].mean().sort_values(ascending=False).round(2))

import seaborn as sns
import matplotlib.pyplot as plt
sns.boxplot(data=apps, x="Content Rating", y="Rating")
plt.xticks(rotation=35, ha="right")
plt.tight_layout()
plt.show()
```

**How it works:** The group mean compares averages; the boxplot displays median, spread, and outliers. Adults-only has very few apps.


## 5. Genres with most apps over 1 million installs

**Correction:** The notebook's original genre sum answers total installs by genre, not the count of qualifying apps. This code answers the question as worded.

```python
unique_apps = apps.sort_values(["Installs", "Reviews"], ascending=[False, False]).drop_duplicates("App")
apps_over_1m = unique_apps[unique_apps["Installs"] > 1_000_000]
genre_app_counts = apps_over_1m.groupby("Genres")["App"].nunique().sort_values(ascending=False)
print("Qualifying unique apps:", apps_over_1m["App"].nunique())
print(genre_app_counts.head(15))
```

**How it works:** Filters individual apps above one million installs, then counts unique apps by genre. Run this cell for exact values.


## 6. Update frequency and average time between updates

**Answer:** The average interval cannot be calculated: the dataset has only one `Last Updated` date per app, not a sequence of update dates.

```python
print(apps["Last Updated"].dt.year.value_counts().sort_index())
print("Rows with a last-update date:", apps["Last Updated"].notna().sum())
```

**How it works:** This summarizes the dates available. Multiple update records per app would be needed to calculate intervals.


## 7. Relationship between app size and installs

**Answer:** Pearson correlation = 0.1689, a weak positive linear relationship.

```python
corr = apps[["Size", "Installs"]].corr().loc["Size", "Installs"]
print(round(corr, 4))

plt.figure(figsize=(9, 5))
plt.scatter(apps["Size"], apps["Installs"], alpha=0.3)
plt.yscale("log")
plt.xlabel("App Size (MB)")
plt.ylabel("Installs (log scale)")
plt.title("App Size vs. Installs")
plt.tight_layout()
plt.show()
```

**How it works:** Calculates correlation and visualizes installs on a log scale because counts vary widely.


## 8. Apps with the highest review counts and their ratings

**Answer:** Facebook 78.16M reviews (4.1); WhatsApp Messenger 69.12M (4.4); Instagram 66.58M (4.5); Messenger 56.65M (4.0); Clash of Clans 44.89M (4.6).

```python
top_reviewed = unique_apps.nlargest(10, "Reviews")[
    ["App", "Category", "Reviews", "Rating", "Installs"]]
print(top_reviewed.to_string(index=False))
```

**How it works:** Selects the ten apps with the largest review counts from a one-record-per-app view.


## 9. Content rating distribution for free versus paid

**Answer:** Everyone dominates both types: 7,720 Free and 662 Paid apps.

```python
print(pd.crosstab(apps["Content Rating"], apps["Type"]))
print("
Percentages within each app type:")
print((pd.crosstab(apps["Content Rating"], apps["Type"], normalize="columns") * 100).round(2))
```

**How it works:** Crosstab counts combinations; column normalization compares percentages within Free and Paid.


## 10. Five categories with the most installs

**Answer:** GAME 31.54B; COMMUNICATION 24.15B; SOCIAL 12.51B; PRODUCTIVITY 12.46B; TOOLS 11.45B.

```python
category_installs = apps.groupby("Category")["Installs"].sum().sort_values(ascending=False)
print(category_installs.head(5))
category_installs.head(5).sort_values().plot(kind="barh", figsize=(9, 5))
plt.xlabel("Total recorded installs")
plt.tight_layout()
plt.show()
```

**How it works:** Sums recorded install counts by category and sorts descending. These are recorded tiers, not exact active users.


# C. Advanced-Level Questions


## 1. Top 10 highest-rated apps and comparison of reviews/installs

**Answer:** With at least 100,000 reviews, JW Library is rated 4.9 with 922,752 reviews and 10 million installs. Other 4.9-rated apps include Six Pack in 30 Days – Abs Workout, Tickets + PDA 2018 Exam, and Learn Japanese, Korean, Chinese Offline & Free. Several 4.8-rated apps have much larger review counts and install tiers.

```python
top_rated = (unique_apps[unique_apps["Reviews"] >= 100_000]
    .sort_values(["Rating", "Reviews"], ascending=[False, False])
    .head(10)[["App", "Rating", "Reviews", "Installs"]])
print(top_rated.to_string(index=False))
```

**How it works:** Uses a 100,000-review threshold and sorts by rating, then reviews. This is not a ranking of every app regardless of review count.


## 2. Update-date trends and seasonality

**Answer:** 6,934 apps have a recorded last-update date in 2018. July has the highest monthly count (2,950); July 2018 alone has 2,653. This does not prove recurring seasonality.

```python
print(apps["Last Updated"].dt.year.value_counts().sort_index())
updates_by_month = apps.dropna(subset=["Last Updated"]).groupby(apps["Last Updated"].dt.month).size()
updates_by_month.index = ["Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"]
print(updates_by_month.sort_values(ascending=False))
year_month = apps.dropna(subset=["Last Updated"]).groupby([apps["Last Updated"].dt.year, apps["Last Updated"].dt.month]).size().unstack(fill_value=0)
print(year_month)
```

**How it works:** Counts recorded last-update dates by year, month, and year-month. One date per app and incomplete coverage prevent conclusions about intervals or recurring seasonal trends.


## 3. Average rating by install bin

**Answer:** 10M–100M has the highest average rating (4.38); 1K–10K the lowest (4.03); 100M+ averages 4.31.

```python
apps["Install Range"] = pd.cut(apps["Installs"],
    bins=[-1, 1_000, 10_000, 100_000, 1_000_000, 10_000_000, 100_000_000, float("inf")],
    labels=["Up to 1K", "1K–10K", "10K–100K", "100K–1M", "1M–10M", "10M–100M", "100M+"])
rating_by_installs = apps.groupby("Install Range", observed=False)["Rating"].agg(["mean", "count"]).round(2)
print(rating_by_installs)
rating_by_installs["mean"].plot(kind="bar", figsize=(10, 5))
plt.ylabel("Average Rating")
plt.tight_layout()
plt.show()
```

**How it works:** `pd.cut()` creates install bins; grouped mean and count show ratings and sample sizes. The pattern is not perfectly monotonic and does not imply causation.


## 4. Relationship between genre and user ratings

**Answer:** Among genres with at least 20 rated apps, Events averages 4.44, Word 4.40, Puzzle 4.39, and Art & Design 4.36.

```python
genre_ratings = (apps.groupby("Genres")["Rating"]
    .agg(["mean", "count"]).query("count >= 20")
    .sort_values("mean", ascending=False).round(2))
print("Highest average ratings:")
print(genre_ratings.head(15))
print("
Lowest average ratings:")
print(genre_ratings.sort_values("mean").head(15))
```

**How it works:** Calculates average rating and sample count by genre, keeps genres with at least 20 rated apps, and displays highest and lowest means.


# Overall Business Insights

1. Free apps make up most records in this historical dataset.
2. Installs and ratings measure different things; their correlation is extremely weak.
3. GAME and COMMUNICATION lead in total recorded installs.
4. App size has only a weak positive association with installs.
5. Free apps have much higher average review counts, while paid apps have a slightly higher average rating; neither comparison proves causation.
6. Category and genre averages should be considered alongside sample sizes.
7. One last-update date per app cannot establish update frequency or seasonality.

## Limitations

- The dataset is historical, not a live view of Google Play Store.
- Install counts are recorded in tiers and are not exact active-user counts.
- Missing ratings and sizes reduce the observations available for some analyses.
- The dataset has one last-update date per app, not a complete update history.
- For Medium Questions 3 and 5, the code adds the exact paid-only price analysis and the correct count of apps over one million installs by genre. Run those cells to get exact values from your cleaned notebook data.
- Sentiment analysis is intentionally excluded.
