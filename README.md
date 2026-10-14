# Maven Coffee Survey: Data Cleaning with Python

## Situation

Maven Analytics published a large-scale coffee consumption survey dataset as part of a public data challenge. The dataset (sourced from [Maven Analytics](https://mavenanalytics.io/challenges/maven-coffee-challenge/)) captured responses from **4,042 participants** across **111 columns**, covering coffee habits, preferences, spending patterns, and demographic information.

The raw data had significant quality issues that would undermine any downstream analysis:

- **Pervasive missing values** spread across most columns, ranging from 31 missing entries in age to 3,186 in the number of children column
- **Entirely empty columns**: the full flavoring sub-question group had zero non-null values
- **Mixed data types**: 90 object columns and 21 float columns, with some numeric fields stored as objects
- **Wide multi-select columns**: survey responses like "Where do you typically drink coffee?" were split into multiple boolean sub-columns, adding complexity to the structure

The size and breadth of the dataset (111 columns, thousands of rows) made manual cleaning impractical, so Python was the right tool for the job.

---

## Task

The goal was to perform a thorough data cleaning pass on the Maven Coffee survey dataset to produce a reliable, analysis-ready DataFrame. This meant:

1. Understanding the structure, shape, and data types of the dataset
2. Identifying and quantifying missing values across all columns
3. Resolving missing values in a way that preserved the dataset's integrity without introducing bias
4. Verifying that each cleaning operation produced the expected result before moving on

The project did not involve building models or producing visualizations. The sole focus was data quality.

---

## Action

### 1. Environment Setup
Imported the necessary libraries: `pandas`, `numpy`, `matplotlib`, `seaborn`, and `scikit-learn` pipeline utilities for potential downstream use.

### 2. Initial Data Exploration
Loaded the CSV file and ran a structured inspection of the dataset:
- `df.head()` and `df.info()` to understand shape and column types
- `df.describe()` and `df.describe(include="all")` to get statistical summaries across both numeric and categorical columns
- `df.columns` to review the full list of 111 survey questions
- `df.isnull().sum()` to produce a complete missing value count per column

This step confirmed that missing data was widespread, affecting the majority of columns to varying degrees.

### 3. Column Type Identification
Used `df.select_dtypes()` to separate float and object columns, identifying which columns carried numeric responses (e.g. coffee expertise ratings, tasting scores) versus categorical responses (e.g. age group, roast preference, employment status).

### 4. Missing Value Imputation
For each column with missing values, the **mode (most frequent value)** was used as the fill value. This approach was chosen over mean/median because the vast majority of columns are categorical or ordinal, and mean imputation would produce meaningless results on text-based survey responses.

The following columns were cleaned using this strategy:

| Column | Mode Applied |
|---|---|
| Age | 25-34 years old |
| Cups of coffee per day | 2 |
| Home brewing method | Pour over |
| Favorite coffee drink | Pourover |
| Coffee additions | No, just black |
| Roast level preference | Light |
| Coffee strength preference | Somewhat strong |
| Pre-tasting coffee type preference | Fruity |
| Coffee expertise rating | 7.0 |
| Coffee A/B/C preference | Coffee A |
| Coffee A vs D preference | Coffee D |
| Favorite overall coffee | Coffee D |
| On-the-go purchase location | Specialty coffee shop |
| Monthly coffee spend | $20-$40 |
| Most ever paid for a cup | $6-$8 |
| Max willing to pay for a cup | $8-$10 |
| Coffee equipment spend (past 5 yrs) | More than $1,000 |
| Good value at a cafe? | Yes |
| Good value for equipment? | Yes |
| Work type (home/in person) | I primarily work from home |
| Employment Status | Employed full-time |
| Education Level | Bachelor's degree |
| Ethnicity/Race | White/Caucasian |
| Political Affiliation | Democrat |

Each imputation was applied using `df[column].replace(np.nan, mode_value, inplace=True)` and immediately verified with `.info()` to confirm the non-null count reached 4,042.

Columns with 100% null values (the flavoring sub-question group) were identified but left as-is, since they contained no data to impute or recover.

---

## Result

By the end of the cleaning process:

- **All targeted columns were fully populated**: each verified column showed `4042 non-null` entries
- **No rows were dropped**: the dataset retained its original 4,042 observations
- **Missing data was resolved consistently** using a reproducible, documented approach
- **The dataset is analysis-ready** for downstream tasks such as segmentation, preference analysis, or visualization

The cleaned DataFrame provides a solid foundation for exploring questions like coffee habit trends by age group, spending behaviour by employment status, or flavour preference patterns across demographics, all without the distortion that incomplete data would introduce.

---

## Tools and Libraries

- **Python 3**
- **pandas**: data loading, inspection, and imputation
- **numpy**: null value handling
- **matplotlib / seaborn**: imported for potential visualization use
- **scikit-learn**: imported for potential pipeline and modelling use

---

## Dataset Source

Maven Analytics: [Great American Coffee Taste Test Challenge](https://mavenanalytics.io/challenges/maven-coffee-challenge/)
