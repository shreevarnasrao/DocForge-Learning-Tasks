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
After plotting Insights:
- More student in the cluster bettween 60 to 80.Total Score looks more symmtrical while maths score looks slightly skewed.3 of them have clustered around 60 to 80 while few in exterme ends.
- Group E consistenly overperforms over other groups.Similar can be said about standard vs free lunch but the difference is that huge.Females outperform males on reading and writing but the gap narrows on math.
- Using the correlation heatmap Reading and writing are very strongly correlated at 0.95 while 0.80 with math instead of writing.so maybe reading and writing could almost be compressed into one feature


## Task 1.3 – Feature Engineering & Preprocessing
```
total_score = math + reading + writing
``` 
This make sense for this model as we dont have study time or other data.combine 3 scores give the model the students overall performance.rather than trying to look at 3 columns worth of data this ones combines everything into one.

### Encode Categorical Columns
```
df['gender'] = df['gender'].map({'female': 1, 'male': 0})
df['lunch'] = df['lunch'].map({'standard': 1, 'free/reduced': 0})
df['test_preparation_course'] = df['test_preparation_course'].map({'completed': 1, 'none': 0})
```
we use binary encoding 0/1 for when column has 2 values.we do this as Ml models are math and cant handle categories,so have to convert to numbers.
```
df = pd.get_dummies(df, columns=['race/ethnicity', 'parental_level_of_education'])
```

we use one-hot encoding when columns have more than 2 values.this treats all the different category same no bias.Each row gets a 1 in its group's column, 0 everywhere else.
for example 
BEFORE:
| race/ethnicity |
|----------------|
| group C        |
| group A        |
| group E        |
| group B        |
| group D        |

AFTER:
| group_A | group_B | group_C | group_D | group_E |
|---------|---------|---------|---------|---------|
|    0    |    0    |    1    |    0    |    0    |
|    1    |    0    |    0    |    0    |    0    |
|    0    |    0    |    0    |    0    |    1    |

One column becomes five. Each row has exactly one 1 and four 0s.They're assigned based on which category the row actually belongs to.

### Scale Numeric Features
ML model require feature scaling to ensure that all features contribute equally to the model's performance, preventing features with larger numerical ranges from dominating the learning process.
For example,iq is between 100 to 130 and total score is between 0 to 600 then total score dominates more when not used feature scaling.
-  StandardScaler: convert the values of column that has mean 0 and standard deviation of 1.
   ```
   scaler = StandardScaler()
   df[['math_score', 'reading_score', 'writing_score']] = scaler.fit_transform(df[['math_score', 'reading_score', 'writing_score']])
   ```
-  MinMaxScaler - keeps everything between 0 and 1.
    ```
    scaler = MinMaxScaler()
    df[['math_score', 'reading_score', 'writing_score']] = scaler.fit_transform(df[['math_score', 'reading_score', 'writing_score']])
    ```







