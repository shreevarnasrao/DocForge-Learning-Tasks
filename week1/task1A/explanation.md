# Explanation for Task 1A
```
df = pd.read_csv('/kaggle/input/datasets/spscientist/students-performance-in-exams/StudentsPerformance.csv')
print(df.head())
print(df.columns.tolist())
print(df.shape)
print(df.dtypes)
```
In this code block we have used `df = pd.read_csv('/kaggle/input/datasets/spscientist/students-performance-in-exams/StudentsPerformance.csv')`  to used to read the dataset and assigned a variable called df.

- `print(df.head())` - used to print the first 5 rows
- `print(df.columns.tolist())` - column names
- `print(df.shape)` - data set shape
- `print(df.dtypes)` - print the data types

### 2) Handle Missing values
```
print(df.isnull().mean()*100)
print(df.isnull().sum())
```
This code is used to check how many columns are empty and percentage of it.If its too less we can drop like less than 5 percent as its doesnt effect the entire dataset.If its too much we can use median and mode.Median is used when its in number like math_score etc and mode is used in like race/gender

Another strategy is too delete the row with the maximum missing values and fill the ones with least missing values.

However here we dont have any missing values so we dont have to worry about it.
```
race/ethnicity                 0.0
parental level of education    0.0
lunch                          0.0
test preparation course        0.0
math score                     0.0
reading score                  0.0
writing score                  0.0
dtype: float64
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

### 3) Standardize Column Names and Clean Categorical Data

- `df.columns = df.columns.str.lower().str.replace(' ', '_')` - here we have lowered the column name and replaced every space (' ') with '_'
- `df['gender'] = df['gender'].str.lower()` - here we have taken into account the column gender if it have Female and female
- `df['lunch'] = df['lunch'].str.lower()` - same thing here too.

### 4) Basic Analysis
```
df['total_score'] = df['math_score']+df['reading_score']+df['writing_score']
top10=df.nlargest(10, 'total_score')
```
In this we have created anoter coloumn called total score.we have assigned a variable called top 10 and used .nlargest(10, 'total_score) to find the top 10 total score.
```
by_education=df.groupby('parental_level_of_education')['total_score'].mean().sort_values
by_race=df.groupby('race/ethnicity')['total_score'].mean().sort_values
by_lunch=df.groupby('lunch')['total_score'].mean()
by_gender =df.groupby('gender')['total_score'].mean()

print(by_education)
print("---")
print(by_race)
print("---")
print(by_lunch)
print("---")
print(by_gender)
```
`.mean()` is used find the average values of the required ones.
`.sort` values makes them from lowest to highest. df.groupby first groups all of them like in race/ethnicity group a,groupb etc,male,female etc and averages them out.

``` Observation/inferance 
From this i can infer that more the parent education better score of the childer,female has higher average than male,students having in standard have higher probability of scoring higher marks than free/reduced as it coreallted to financial condition.The gap between groups is significant
```


## Task 1.2 – Visualization & Insights







