# EDA-TASK_8

# Exploratory Data Analysis (EDA) – Student Performance Dataset

## 1. Project Overview
This project performs Exploratory Data Analysis (EDA) on the **StudentsPerformance.csv** dataset. It examines students' math, reading, and writing scores, cleans categorical values, calculates statistical measures, creates total marks and percentage, detects outliers, and visualizes relationships between scores and student attributes.

## 2. Objective
- Inspect the dataset structure and quality.
- Check missing values and duplicate records.
- Clean categorical columns.
- Calculate descriptive statistics and quartiles.
- Create total marks and percentage features.
- Detect score outliers using the IQR method.
- Visualize score differences by parental education, lunch type, and test preparation.
- Explore relationships and correlations among subject scores.

## 3. Tools and Libraries
- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Jupyter Notebook / Google Colab

## 4. Dataset
**File:** `StudentsPerformance.csv`

The dataset has 1,000 student records and 8 original columns:

| Column | Description |
|---|---|
| gender | Student gender |
| race/ethnicity | Student group |
| parental level of education | Parent's education level |
| lunch | Lunch category |
| test preparation course | Test preparation status |
| math score | Math score |
| reading score | Reading score |
| writing score | Writing score |

## 5. Import Libraries

### Code
```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
```

### Description
Pandas is used for data handling, NumPy for numerical operations, Matplotlib and Seaborn for visualizations.

## 6. Load Dataset

### Code
```python
df = pd.read_csv("/content/StudentsPerformance.csv")
```

### Description
Reads the CSV file into a Pandas DataFrame named `df`. In a local environment, replace the path with the location of `StudentsPerformance.csv`.

## 7. Display First Five Rows

### Code
```python
df.head()
```

### Output
```text
   gender race/ethnicity parental level of education         lunch  \
0  female        group B           bachelor's degree      standard
1  female        group C                some college      standard
2  female        group B             master's degree      standard
3    male        group A          associate's degree  free/reduced
4    male        group C                some college      standard

  test preparation course  math score  reading score  writing score
0                    none          72             72             74
1               completed          69             90             88
2                    none          90             95             93
3                    none          47             57             44
4                    none          76             78             75
```

## 8. Dataset Shape

### Code
```python
df.shape
```

### Output
```text
(1000, 8)
```

**Result:** 1,000 rows and 8 columns.

## 9. Dataset Information

### Code
```python
df.info()
```

### Output
```text
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 1000 entries, 0 to 999
Data columns (total 8 columns):
 #   Column                       Non-Null Count  Dtype
---  ------                       --------------  -----
 0   gender                       1000 non-null  object
 1   race/ethnicity               1000 non-null  object
 2   parental level of education   1000 non-null  object
 3   lunch                        1000 non-null  object
 4   test preparation course       1000 non-null  object
 5   math score                  1000 non-null  int64
 6   reading score               1000 non-null  int64
 7   writing score               1000 non-null  int64
dtypes: int64(3), object(5)
memory usage: 62.6+ KB
```

## 10. Missing Values

### Code
```python
df.isnull().sum()
```

### Output
```text
gender                         0
race/ethnicity                 0
parental level of education    0
lunch                          0
test preparation course        0
math score                     0
reading score                  0
writing score                  0
dtype: int64
```

**Result:** No missing values were found.

## 11. Duplicate Records

### Code
```python
df.duplicated().sum()
```

### Output
```text
np.int64(0)
```

**Result:** No duplicate rows were found.

## 12. Descriptive Statistics

### Code
```python
df.describe()
```

### Output
```text
       math score  reading score  writing score
count  1000.00000    1000.000000    1000.000000
mean     66.08900      69.169000      68.054000
std      15.16308      14.600192      15.195657
min       0.00000      17.000000      10.000000
25%      57.00000      59.000000      57.750000
50%      66.00000      70.000000      69.000000
75%      77.00000      79.000000      79.000000
max     100.00000     100.000000     100.000000
```

## 13. Clean Categorical Columns

### Code
```python
categorical_cols = [
    'gender',
    'race/ethnicity',
    'parental level of education',
    'lunch',
    'test preparation course'
]

for col in categorical_cols:
    df[col] = (
        df[col]
        .astype('string')
        .str.strip()
        .str.lower()
    )

for col in categorical_cols:
    print(f"\n{col}:")
    print(df[col].unique())
```

### Output
```text
gender:
['female', 'male']

race/ethnicity:
['group b', 'group c', 'group a', 'group d', 'group e']

parental level of education:
["bachelor's degree", 'some college', "master's degree",
 "associate's degree", 'high school', 'some high school']

lunch:
['standard', 'free/reduced']

test preparation course:
['none', 'completed']
```

**Result:** Categorical values were converted to string type, trimmed, and changed to lowercase.

## 14. Mean, Median, Standard Deviation, and Quartiles

### Code
```python
score_cols = [
    'math score',
    'reading score',
    'writing score'
]

stats = df[score_cols].agg(
    ['mean', 'median', 'std']
).T

stats['Q1'] = df[score_cols].quantile(0.25)
stats['Q2'] = df[score_cols].quantile(0.50)
stats['Q3'] = df[score_cols].quantile(0.75)

print(stats.round(2))
```

### Output
```text
                mean  median    std     Q1    Q2    Q3
math score     66.09    66.0  15.16  57.00  66.0  77.0
reading score  69.17    70.0  14.60  59.00  70.0  79.0
writing score  68.05    69.0  15.20  57.75  69.0  79.0
```

## 15. Create Total Marks and Percentage

### Code
```python
df['total marks'] = (
    df['math score']
    + df['reading score']
    + df['writing score']
)

df['percentage'] = (
    df['total marks'] / 300
) * 100

print(df[
    ['math score', 'reading score',
     'writing score', 'total marks', 'percentage']
].head())

print(df[['total marks', 'percentage']].describe())
```

### Output – First Five Records
```text
   math score  reading score  writing score  total marks  percentage
0          72             72             74          218   72.666667
1          69             90             88          247   82.333333
2          90             95             93          278   92.666667
3          47             57             44          148   49.333333
4          76             78             75          229   76.333333
```

### Output – Summary
```text
       total marks   percentage
count  1000.000000  1000.000000
mean    203.312000    67.770667
std      42.771978    14.257326
min      27.000000     9.000000
25%     175.000000    58.333333
50%     205.000000    68.333333
75%     233.000000    77.666667
max     300.000000   100.000000
```

## 16. Subject Summary and Median

### Code
```python
subjects = ['math score', 'reading score', 'writing score']

print(df[subjects].describe())

print("Median:")
print(df[subjects].median())
```

### Output – Median
```text
math score       66.0
reading score    70.0
writing score    69.0
dtype: float64
```

## 17. Outlier Detection Using IQR

### Code
```python
for col in subjects:
    Q1 = df[col].quantile(0.25)
    Q3 = df[col].quantile(0.75)

    IQR = Q3 - Q1

    lower = Q1 - 1.5 * IQR
    upper = Q3 + 1.5 * IQR

    outliers = df[(df[col] < lower) | (df[col] > upper)]

    print(col, "Outliers:", len(outliers))

df.to_csv("Student_Performance_Cleaned.csv", index=False)

print("Task completed!")
```

### Output
```text
math score Outliers: 8
reading score Outliers: 6
writing score Outliers: 5
Task completed!
```

**Result:** The cleaned and feature-enriched dataset is exported as `Student_Performance_Cleaned.csv`.

## 18. Average Scores by Parental Education

### Code
```python
education_avg = df.groupby(
    'parental level of education'
)[subjects].mean()

education_avg.plot(kind='bar', figsize=(10, 5))
plt.title('Average Scores by Parental Education')
plt.xlabel('Parental Education Level')
plt.ylabel('Average Score')
plt.xticks(rotation=45, ha='right')
plt.legend(title='Subject')
plt.tight_layout()
plt.show()
```

### Output
A bar chart comparing average math, reading, and writing scores across parental education levels.

<img width="989" height="490" alt="image" src="https://github.com/user-attachments/assets/d566d8cc-9019-4b39-b0ea-44dbc4eef5ba" />

## 19. Average Scores by Lunch Type

### Code
```python
lunch_avg = df.groupby('lunch')[subjects].mean()

lunch_avg.plot(kind='bar', figsize=(8, 5))

plt.title('Average Scores by Lunch Type')
plt.xlabel('Lunch Type')
plt.ylabel('Average Score')
plt.xticks(rotation=0)
plt.legend(title='Subject')
plt.tight_layout()
plt.show()
```

### Output
A bar chart comparing average subject scores for standard and free/reduced lunch categories.

<img width="790" height="490" alt="image" src="https://github.com/user-attachments/assets/0cc33f80-ed51-4fb7-9103-8bb7d3f3c75d" />


## 20. Subject Scores by Test Preparation

### Code
```python
for subject in subjects:
    plt.figure(figsize=(7, 5))

    sns.boxplot(
        data=df,
        x='test preparation course',
        y=subject
    )

    plt.title(f'{subject.title()} by Test Preparation')
    plt.xlabel('Test Preparation Course')
    plt.ylabel('Score')
    plt.tight_layout()
    plt.show()
```

### Output
Three boxplots are produced:
- Math score by test preparation
- Reading score by test preparation
- Writing score by test preparation

<img width="690" height="490" alt="image" src="https://github.com/user-attachments/assets/3cc9e3bc-a50d-4103-8ed4-cf03c26f0c00" />

<img width="690" height="490" alt="image" src="https://github.com/user-attachments/assets/c45cd638-c3ca-4d4a-b60a-436887fdabbe" />

## 21. Relationship Between Subject Scores

### Code
```python
sns.scatterplot(data=df, x='math score', y='reading score')
plt.title('Math vs Reading Scores')
plt.show()

sns.scatterplot(data=df, x='math score', y='writing score')
plt.title('Math vs Writing Scores')
plt.show()

sns.scatterplot(data=df, x='reading score', y='writing score')
plt.title('Reading vs Writing Scores')
plt.show()
```

### Output
Three scatter plots are generated:
- Math vs Reading
- Math vs Writing
- Reading vs Writing

<img width="571" height="455" alt="image" src="https://github.com/user-attachments/assets/ca0adf7e-ee0c-4204-b059-0f29edc94f71" />

<img width="571" height="455" alt="image" src="https://github.com/user-attachments/assets/330bb48f-4bcd-4b89-8da7-21e4d7cefb54" />

<img width="571" height="455" alt="image" src="https://github.com/user-attachments/assets/618a77a9-05f0-47b0-bd59-c1c648e0e4fe" />



## 22. Correlation Heatmap

### Code
```python
correlation = df[subjects].corr()

sns.heatmap(
    correlation,
    annot=True,
    cmap='coolwarm',
    vmin=-1,
    vmax=1
)

plt.title('Correlation Between Subject Scores')
plt.tight_layout()
plt.show()
```

### Output
A heatmap showing correlation values among math, reading, and writing scores

<img width="614" height="470" alt="image" src="https://github.com/user-attachments/assets/c9c14d54-1b1c-4729-95e3-6d37a3054cd5" />


## 23. Key Findings
- The dataset has **1,000 records** and **8 original columns**.
- No missing values were found.
- No duplicate rows were found.
- Mean scores: Math **66.09**, Reading **69.17**, Writing **68.05**.
- Mean total marks: **203.312 out of 300**.
- Mean percentage: **67.7707%**.
- IQR outlier counts: Math **8**, Reading **6**, Writing **5**.
- The notebook compares average scores across parental education and lunch categories, displays subject scores by test preparation, and visualizes relationships between subjects using scatter plots and a correlation heatmap.

## 24. Output File
The notebook saves the processed dataset as:

```text
Student_Performance_Cleaned.csv
```

## 25. Project Structure
```text
EDA-Student-Performance/
├── EDA_TASK_7 (1).ipynb
├── StudentsPerformance.csv
├── Student_Performance_Cleaned.csv
├── README.md
└── images/
    ├── parental_education_scores.png
    ├── lunch_scores.png
    ├── math_test_prep.png
    ├── reading_test_prep.png
    ├── writing_test_prep.png
    ├── math_reading_scatter.png
    ├── math_writing_scatter.png
    ├── reading_writing_scatter.png
    └── score_correlation_heatmap.png
```

## 26. Conclusion
This EDA explored student performance in math, reading, and writing. The dataset was inspected for missing and duplicate values, categorical columns were standardized, summary statistics were calculated, total marks and percentages were added, and outliers were identified with the IQR method. Graphs were used to compare scores across student categories and examine relationships between subjects.

---

**Author:** RAJADURAI V
**Task:** Exploratory Data Analysis (EDA)  
**Dataset:** StudentsPerformance.csv
