# 🎬 Netflix Data Analytics — Big Data Analysis with Python


**An end-to-end Exploratory Data Analysis (EDA) of Netflix's movie and TV show catalog using Python, Pandas, and Seaborn.**

## 📖 What Is This Project?

Netflix has thousands of movies and TV shows from all over the world. But have you ever wondered:

- 📅 *Which year had the most content added to Netflix?*
- 🌍 *Which country produces the most TV Shows?*
- 🎭 *Who are the top directors on Netflix?*
- ⭐ *What content ratings does Netflix use?*

This project answers all of these questions — and many more — by analyzing a real Netflix dataset using Python. It's built as a **Jupyter Notebook** that walks through every step of the analysis clearly, from loading raw data to filtering, cleaning, and visualizing insights.


## 📁 Repository Structure

```
📦 netflix-data-analytics/
 ┣ 📓 Netflix_Data_Analytics.ipynb   ← Main analysis notebook (all code + outputs)
 ┣ 📄 Netflix_Dataset.csv            ← Raw dataset (7,787 records)
 ┗ 📘 README.md                      ← You are here
```


### 🗂️ Column-by-Column Breakdown

| # | Column | Type | Description | Example |
|---|---|---|---|---|
| 1 | `Show_Id` | Text | Unique ID for each title | `s1`, `s2` |
| 2 | `Category` | Text | Type of content | `Movie` or `TV Show` |
| 3 | `Title` | Text | Name of the show/movie | `House of Cards` |
| 4 | `Director` | Text | Director(s) — has **2,388 nulls** | `David Fincher` |
| 5 | `Cast` | Text | Main actors — has **718 nulls** | `Kevin Spacey, Robin Wright` |
| 6 | `Country` | Text | Country of origin — has **507 nulls** | `United States` |
| 7 | `Release_Date` | Text → Date | Date content was added to Netflix | `August 14, 2020` |
| 8 | `Rating` | Text | Content rating — has **7 nulls** | `TV-MA`, `PG-13`, `R` |
| 9 | `Duration` | Text | Length in mins or seasons | `93 min`, `4 Seasons` |
| 10 | `Type` | Text | Genre/sub-category | `Dramas`, `Comedies` |
| 11 | `Description` | Text | Short synopsis | `"In a future where..."` |

> ⚠️ **Note:** The `Director` column has the most missing values (2,388 out of 7,787 rows), mostly because many TV Shows don't list a single director.

---

## 🛠️ Tech Stack & Tools

| Tool | Version | Purpose |
|---|---|---|
| 🐍 **Python** | 3.14 | Core programming language |
| 🐼 **Pandas** | Latest | Data loading, cleaning, filtering, grouping |
| 🎨 **Seaborn** | Latest | Statistical visualizations (heatmap, countplot) |
| 📊 **Matplotlib** | Latest | Backend plotting engine |
| 📓 **Jupyter Notebook** | Latest | Interactive development environment |

---

## ⚙️ How to Run This Project

### Step 1 — Prerequisites

Make sure you have Python installed. Then install the required libraries:

```bash
pip install pandas seaborn matplotlib jupyter
```

### Step 2 — Clone the Repository

```bash
git clone https://github.com/your-username/netflix-data-analytics.git
cd netflix-data-analytics
```

### Step 3 — Launch Jupyter Notebook

```bash
jupyter notebook
```

### Step 4 — Open the Notebook

In the Jupyter browser window, click on `Netflix_Data_Analytics.ipynb`.

### Step 5 — Fix the File Path ⚠️ Important!

In the first code cell, update the CSV path to match your system:

```python
# ❌ Original (hardcoded local machine path — won't work on your computer)
data = pd.read_csv(r"A:\PROJECT_BY_ASHOKA\Netflix Data Analysis\Netflix Dataset.csv")

# ✅ Replace with this (works if CSV is in the same folder as the notebook)
data = pd.read_csv("Netflix_Dataset.csv")
```

### Step 6 — Run All Cells

Go to **Kernel → Restart & Run All** to execute the full notebook from top to bottom.

---

## 🧹 Data Cleaning Steps

Before doing any analysis, the raw data was cleaned in two important steps:

### ✅ Step 1: Removing Duplicate Records

```python
data[data.duplicated()]           # Detect duplicate rows
data.drop_duplicates(inplace=True)  # Remove them permanently
data.shape                         # Confirm the new shape
```

**What happened:** 2 exact duplicate rows were found and removed:

| Removed Title | Show ID |
|---|---|
| Backfire | s684 |
| The Lost Okoroshi | s6621 |

The dataset went from **7,789 → 7,787 rows** after cleanup.

---

### ✅ Step 2: Detecting Null (Missing) Values

```python
data.isnull().sum()
```

**Full missing value report:**

```
Show_Id           0   ✅ Complete
Category          0   ✅ Complete
Title             0   ✅ Complete
Director       2388   ⚠️ 30% missing
Cast            718   ⚠️ Missing in some rows
Country         507   ⚠️ Missing in some rows
Release_Date     10   ✅ Almost complete
Rating            7   ✅ Almost complete
Duration          0   ✅ Complete
Type              0   ✅ Complete
Description       0   ✅ Complete
```

A **Seaborn heatmap** was plotted to visually see where data is missing. Bright horizontal lines in the `Director`, `Cast`, and `Country` columns show the gaps clearly at a glance:

```python
import seaborn as sns
sns.heatmap(data.isnull())
```

---

## 🔬 Analysis Walkthrough — All 13 Questions

Below is every question answered in the notebook, with the **pandas method used**, the **actual result from the output**, and a plain English explanation of what was learned.

---

### 📌 Q1 — House of Cards: Show ID & Director

> *"For 'House of Cards', what is the Show ID and who is the Director?"*

**Methods used:** `isin()` and `str.contains()`

```python
# Method 1: Exact match using isin()
data[data['Title'].isin(['House of Cards'])]

# Method 2: Substring search using str.contains()
data[data['Title'].str.contains('House of Cards')]
```

**Actual Output:**

| Show_Id | Category | Title | Director |
|---|---|---|---|
| s2833 | TV Show | House of Cards | Robin Wright, David Fincher, Gerald McRaney... |

**💡 Key Learning:** `isin()` requires the title to match exactly. `str.contains()` is more flexible — it finds the title even if it's embedded in a longer string. Both returned the same result here, but `str.contains()` is better for user-style search queries.

---

### 📌 Q2 — Which Year Had the Most Releases?

> *"In which year were the highest number of TV Shows & Movies released? Show with a Bar Graph."*

**Methods used:** `pd.to_datetime()`, `dt.year`, `value_counts()`

```python
# Convert Release_Date text → proper datetime
data["Date_N"] = pd.to_datetime(data["Release_Date"], format="%B %d, %Y", errors="coerce")

# Extract just the year and count
data['Date_N'].dt.year.value_counts()
```

**Actual Output:**

| Year | Titles Added |
|---|---|
| **2019** | **2,153** 🏆 |
| 2020 | 2,009 |
| 2018 | 1,685 |
| 2017 | 1,225 |
| 2016 | 443 |
| 2021 | 117 |
| 2015 | 88 |

**💡 Key Learning:** `Release_Date` was stored as plain text like `"August 14, 2020"`. Python doesn't know that's a date until we tell it explicitly using `pd.to_datetime()` with `format="%B %d, %Y"`. The `errors="coerce"` argument safely converts any bad/missing dates to `NaT` instead of throwing an error.

---

### 📌 Q3 — How Many Movies vs. TV Shows?

> *"How many Movies & TV Shows are in the dataset? Show with a Bar Graph."*

**Methods used:** `groupby()`, `sns.countplot()`

```python
data.groupby('Category').Category.count()
sns.countplot(data['Category'])
```

**Actual Output:**

| Category | Count |
|---|---|
| 🎬 Movie | **5,377** |
| 📺 TV Show | **2,410** |

**💡 Key Insight:** Netflix's catalog is **69% Movies and 31% TV Shows**. There are more than double the movies compared to shows — though Netflix has been investing more in original series every year.

---

### 📌 Q4 — Movies Released in Year 2000

> *"Show all the Movies that were released in the year 2000."*

**Methods used:** Creating a new column with `dt.year`, multi-condition filtering with `&`

```python
data['Year'] = data['Date_N'].dt.year   # Create a Year column
data[(data['Category'] == 'Movie') & (data['Year'] == 2000)]
```

**Actual Output:** `Empty DataFrame` — no movies were added to Netflix in the year 2000.

*(The notebook also tests for year 2020, which returned hundreds of movies.)*

**💡 Key Learning:** We created a new `Year` column by pulling just the year number from our datetime column. This is a common technique — it makes year-based filtering much simpler than parsing the full date string each time.

---

### 📌 Q5 — Indian TV Shows

> *"Show only the Titles of all TV Shows released in India only."*

**Methods used:** Multi-condition boolean filtering, single-column selection

```python
data[(data['Category'] == 'TV Show') & (data['Country'] == 'India')]['Title']
```

**Sample Output:**
```
21 Sarfarosh: Saragarhi 1897
7 (Seven)
Agent Raghav
Akbar Birbal
Anjaan: Rural Myths
... and many more
```

**💡 Key Learning:** Adding `['Title']` at the end of a filter returns only that one column instead of the full row. This gives a clean, focused list — very useful when you only need specific fields from filtered data.

---

### 📌 Q6 — Top 10 Directors on Netflix

> *"Show the Top 10 Directors who gave the highest number of TV Shows & Movies to Netflix."*

**Methods used:** `value_counts().head(10)`

```python
data['Director'].value_counts().head(10)
```

**Actual Output:**

| Rank | Director | Titles on Netflix |
|---|---|---|
| 🥇 1 | Raúl Campos, Jan Suter | 18 |
| 🥈 2 | Marcus Raboy | 16 |
| 🥉 3 | Jay Karas | 14 |
| 4 | Cathy Garcia-Molina | 13 |
| 5 | Youssef Chahine | 12 |
| 5 | Jay Chapman | 12 |
| 5 | **Martin Scorsese** | **12** |
| 8 | **Steven Spielberg** | **10** |
| 8 | David Dhawan | 10 |

**💡 Key Insight:** The top spot goes to the collaborative duo Raúl Campos & Jan Suter (Netflix lists them together). Legendary Hollywood directors **Martin Scorsese** and **Steven Spielberg** also crack the top 10, showing that Netflix licenses content from the biggest names in cinema.

---

### 📌 Q7 — Comedy Movies or UK Content

> *"Show all records where Category is Movie & Type is Comedies, OR Country is United Kingdom."*

**Methods used:** Combining `&` (AND) and `|` (OR) operators

```python
# Step 1: Only Comedy Movies
data[(data['Category'] == 'Movie') & (data['Type'] == 'Comedies')]

# Step 2: Comedy Movies OR anything from the United Kingdom
data[(data['Category'] == 'Movie') & (data['Type'] == 'Comedies') | (data['Country'] == 'United Kingdom')]
```

**💡 Key Learning:** In Python's pandas, `&` (AND) is evaluated before `|` (OR) — similar to how `×` comes before `+` in math. Always wrap each condition in its own parentheses `()` to avoid unexpected results. Without parentheses, the filter may not work the way you intend.

---

### 📌 Q8 — Tom Cruise on Netflix

> *"In how many movies/shows was Tom Cruise cast?"*

**Methods used:** Exact match (fails), `str.contains()` with `case=False, na=False`, `dropna()`

```python
# ❌ Returns ZERO results — Cast has multiple actors in one string
data[data['Cast'] == 'Tom Cruise']

# ✅ Correct approach — search within the cast string
data[data["Cast"].str.contains("tom cruise", case=False, na=False)]
```

**Actual Output — Tom Cruise appears in 2 Netflix titles:**

| Title | Director |
|---|---|
| Magnolia | Paul Thomas Anderson |
| Rain Man | Barry Levinson |

**💡 Key Learning:** The `Cast` column stores all actors as one long comma-separated string: `"Tom Cruise, Dustin Hoffman, Valeria Golino..."`. Using `==` for exact match will never find anything because it's looking for a cell that contains *only* "Tom Cruise". The correct approach is `str.contains()`, which searches inside the string. Setting `case=False` handles capitalization differences, and `na=False` skips blank cast cells without crashing.

---

### 📌 Q9 — Netflix Content Ratings

> *"What are the different Ratings defined by Netflix?"*

**Methods used:** `nunique()`, `unique()`

```python
data['Rating'].nunique()   # → 14 unique ratings
data['Rating'].unique()    # → Shows all 14 values
```

**Netflix uses 14 content ratings:**

| Rating | Audience |
|---|---|
| `TV-MA` | Mature Audiences (17+) — most common |
| `TV-14` | Parents Strongly Cautioned (14+) |
| `TV-PG` | Parental Guidance Suggested |
| `TV-G` | General Audience (all ages) |
| `TV-Y` | Designed for Young Children |
| `TV-Y7` | Suitable for Ages 7+ |
| `TV-Y7-FV` | Ages 7+ (Fantasy Violence) |
| `R` | Restricted (17+) |
| `PG-13` | Parents Cautioned (13+) |
| `PG` | Parental Guidance |
| `G` | General Audience |
| `NC-17` | Adults Only |
| `NR` | Not Rated |
| `UR` | Unrated |

#### 🔍 Sub-Question 9.1 — TV-14 Movies in Canada

```python
data[(data['Category'] == 'Movie') & (data['Rating'] == 'TV-14')].shape
# → (1272, 15)  — 1,272 TV-14 movies across all countries

data[(data['Category'] == 'Movie') & (data['Rating'] == 'TV-14') & (data['Country'] == 'Canada')].shape
# → (11, 15)  — only 11 of those are from Canada
```

#### 🔍 Sub-Question 9.2 — R-Rated TV Shows After 2018

```python
data[(data['Category'] == 'TV Show') & (data['Rating'] == 'R') & (data['Year'] > 2018)]
```

**Result:** Only **1 show** — *The Hateful Eight: Extended Version* by Quentin Tarantino.

---

### 📌 Q10 — Maximum Movie Duration

> *"What is the maximum duration of a Movie/Show on Netflix?"*

**Methods used:** `str.split(expand=True)`, `pd.to_numeric()`, `max()`, `min()`, `mean()`

The `Duration` column mixes two types of data — movies measured in `minutes` and TV shows measured in `seasons`. All stored as plain text like `"93 min"` or `"4 Seasons"`. The solution was to split this into two separate columns:

```python
data[['Minutes', 'Unit']] = data['Duration'].str.split(' ', expand=True)
# Creates: Minutes = "93", Unit = "min"

data["Minutes"] = pd.to_numeric(data["Minutes"], errors="coerce")
# Converts "93" (text) → 93 (number) so math operations work
```

**Actual Results:**

| Statistic | Value |
|---|---|
| ⬆️ Maximum | 99 |
| ⬇️ Minimum | 1 |
| 📊 Average Movie Duration | **99.31 minutes** |

**💡 Key Insight:** The average Netflix movie is just under **100 minutes** (~1 hour 40 minutes) — right in line with traditional Hollywood movie lengths. This makes sense as Netflix licenses many theatrical films alongside its originals.

---

### 📌 Q11 — Country with Most TV Shows

> *"Which individual country has the highest number of TV Shows?"*

**Methods used:** Boolean filtering to subset, `value_counts()`

```python
data_tvshow = data[data['Category'] == 'TV Show']   # Filter TV Shows only
data_tvshow.Country.value_counts()                   # Count by country
data_tvshow.Country.value_counts().head(1)           # Top country only
```

**Actual Output — Top 5 Countries:**

| Rank | Country | TV Shows |
|---|---|---|
| 🥇 1 | 🇺🇸 United States | **705** |
| 🥈 2 | 🇬🇧 United Kingdom | 204 |
| 🥉 3 | 🇯🇵 Japan | 157 |
| 4 | 🇰🇷 South Korea | 147 |
| 5 | 🇮🇳 India | 71 |

**💡 Key Insight:** The US leads by a massive margin with 705 TV Shows. The strong showing from **Japan (157)** and **South Korea (147)** reflects the global boom in K-dramas and anime on Netflix. India rounds out the top 5, showing Netflix's heavy investment in the South Asian market.

---

### 📌 Q12 — Sorting the Dataset by Year

> *"How can we sort the dataset by Year?"*

**Methods used:** `sort_values()`

```python
# Sort oldest first (ascending)
data.sort_values(by='Year')

# Sort newest first (descending) — show top 10 most recent
data.sort_values(by='Year', ascending=False).head(10)
```

**💡 Key Learning:** `sort_values()` reorders the entire DataFrame by any column you choose. By default it sorts ascending (smallest/oldest first). Use `ascending=False` to flip it. The sort is not saved permanently unless you add `inplace=True` or assign it back: `data = data.sort_values(...)`.

---

### 📌 Q13 — Drama Movies & Kids' TV Shows

> *"Find all instances where: (Category is Movie AND Type is Dramas) OR (Category is TV Show AND Type is Kids' TV)"*

**Methods used:** Complex multi-condition filtering with `&` and `|`

```python
# Check each part separately first
data[(data['Category'] == 'Movie') & (data['Type'] == 'Dramas')]
data[(data['Category'] == 'TV Show') & (data['Type'] == "Kids' TV")]

# Combine both with OR
data[
    (data['Category'] == 'Movie') & (data['Type'] == 'Dramas') |
    (data['Category'] == 'TV Show') & (data['Type'] == "Kids' TV")
]
```

**Sample Results from Combined Filter:**

| Title | Category | Type |
|---|---|---|
| 21 | Movie | Dramas |
| 187 | Movie | Dramas |
| 44 Cats | TV Show | Kids' TV |
| Abby Hatcher | TV Show | Kids' TV |
| Alphablocks | TV Show | Kids' TV |

**💡 Key Learning:** This is the most complex filter in the notebook. It combines two AND conditions joined by an OR. Python evaluates AND before OR automatically, so parentheses around each pair handle the logic correctly. Always build complex filters step by step — test each part alone before combining.

---

## 📈 Key Findings & Insights Summary

| # | Finding | Detail |
|---|---|---|
| 📅 | **2019 was Netflix's biggest year** | 2,153 titles added — the highest of any single year |
| 🎬 | **Movies dominate the catalog** | 5,377 Movies vs. 2,410 TV Shows (69% vs. 31%) |
| 🌍 | **USA leads in TV Shows** | 705 shows — more than 3× the UK (204) |
| 🎭 | **Top Director Pair** | Raúl Campos & Jan Suter co-directed 18 Netflix titles together |
| ⭐ | **14 content ratings** | Netflix uses 14 distinct rating categories globally |
| ⏱️ | **Average movie length** | 99.31 minutes (~1 hour 40 minutes) |
| 🎥 | **Tom Cruise on Netflix** | Only 2 titles: *Magnolia* and *Rain Man* |
| 🇨🇦 | **TV-14 movies in Canada** | Only 11 out of 1,272 total TV-14 movies |
| 🔁 | **2 duplicate rows removed** | *Backfire* and *The Lost Okoroshi* |
| ❓ | **Most missing data** | Director column — 2,388 missing values (30% of rows) |

---

## 🧠 Pandas Concepts Covered — Quick Reference

This project is an excellent reference for the following Pandas and Python techniques:

| Concept | Method | Used For |
|---|---|---|
| Load data | `pd.read_csv()` | Reading the CSV file into a DataFrame |
| Preview top rows | `data.head(n)` | See first n rows |
| Preview bottom rows | `data.tail(n)` | See last n rows |
| Dataset dimensions | `data.shape` | Number of rows and columns |
| Total elements | `data.size` | Rows × Columns |
| Column names | `data.columns` | List all column names |
| Data types | `data.dtypes` | Data type of each column |
| Full summary | `data.info()` | All column info at once |
| Find duplicates | `data.duplicated()` | Detect duplicate rows |
| Remove duplicates | `drop_duplicates()` | Delete duplicate rows permanently |
| Find nulls | `data.isnull().sum()` | Count missing values per column |
| Visualize nulls | `sns.heatmap(data.isnull())` | See missing data as a chart |
| Exact search | `isin(['value'])` | Filter rows matching exact values |
| Text search | `str.contains()` | Find rows containing a substring |
| Date conversion | `pd.to_datetime()` | Convert text → datetime |
| Extract year | `.dt.year` | Get year number from datetime |
| Count occurrences | `value_counts()` | Frequency of each unique value |
| Group & count | `groupby().count()` | Aggregate counts by group |
| Bar chart | `sns.countplot()` | Visual count of categories |
| AND filter | `&` operator | Both conditions must be true |
| OR filter | `\|` operator | Either condition can be true |
| Split column | `str.split(expand=True)` | Break one column into two |
| Convert to numeric | `pd.to_numeric()` | Turn text numbers into real numbers |
| Maximum value | `.max()` | Largest value in a column |
| Minimum value | `.min()` | Smallest value in a column |
| Average value | `.mean()` | Average of a numeric column |
| Sort rows | `sort_values()` | Order rows by a column's values |
| Remove nulls | `dropna()` | Delete all rows with any null values |
| Count unique | `nunique()` | Number of distinct values |
| List unique | `unique()` | All distinct values as an array |

---

## 🤝 Contributing

Want to improve this project or add more analysis? Contributions are welcome!

1. **Fork** this repository
2. **Create** a new branch: `git checkout -b feature/your-analysis`
3. **Add** your changes to the notebook
4. **Commit**: `git commit -m "Add: analysis for [topic]"`
5. **Push**: `git push origin feature/your-analysis`
6. **Open a Pull Request** describing what you added

### 💡 Ideas for Future Analysis
- 🌐 Which genre is most popular in each country?
- 📈 How has the Movie vs. TV Show ratio changed year over year?
- 🔤 What keywords appear most in descriptions (word cloud)?
- 🕐 What is the average number of seasons for TV Shows?
- 🌟 Which actors appear most frequently across all Netflix titles?

