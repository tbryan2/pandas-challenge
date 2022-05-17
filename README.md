# pandas-challenge
# Pandas Challenge
## School & Student Data

### Obervations:

- The analysis of this dataset revealed a few unexpected truths:
    - If you look at the _Scores by School Spending_ table you'll see that there's a negative correlation between budget per student and students' scores. It seems the less money per student, the better their test scores and vice versa.
    - In a similar vein, Charter schools appear to have noticeably better scores than District schools. The top 5 schools are all Charter and the bottom 5 are all District.

## Analysis


```python
# Dependencies and Setup
import pandas as pd

# File to Load (Remember to Change These)
school_data_to_load = "Resources/schools_complete.csv"
student_data_to_load = "Resources/students_complete.csv"

# Read School and Student Data File and store into Pandas DataFrames
school_data = pd.read_csv(school_data_to_load)
student_data = pd.read_csv(student_data_to_load)

# Combine the data into a single dataset.  
df = pd.merge(student_data, school_data, how="left", on=["school_name", "school_name"])
```


```python
# Checking df column datatypes
df.dtypes
```




    Student ID        int64
    student_name     object
    gender           object
    grade            object
    school_name      object
    reading_score     int64
    math_score        int64
    School ID         int64
    type             object
    size              int64
    budget            int64
    dtype: object




```python
# Check for null values
df.isna().sum()
```




    Student ID       0
    student_name     0
    gender           0
    grade            0
    school_name      0
    reading_score    0
    math_score       0
    School ID        0
    type             0
    size             0
    budget           0
    dtype: int64




```python
# Gettting a look at the data structure
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Student ID</th>
      <th>student_name</th>
      <th>gender</th>
      <th>grade</th>
      <th>school_name</th>
      <th>reading_score</th>
      <th>math_score</th>
      <th>School ID</th>
      <th>type</th>
      <th>size</th>
      <th>budget</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>Paul Bradley</td>
      <td>M</td>
      <td>9th</td>
      <td>Huang High School</td>
      <td>66</td>
      <td>79</td>
      <td>0</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>Victor Smith</td>
      <td>M</td>
      <td>12th</td>
      <td>Huang High School</td>
      <td>94</td>
      <td>61</td>
      <td>0</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>Kevin Rodriguez</td>
      <td>M</td>
      <td>12th</td>
      <td>Huang High School</td>
      <td>90</td>
      <td>60</td>
      <td>0</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>Dr. Richard Scott</td>
      <td>M</td>
      <td>12th</td>
      <td>Huang High School</td>
      <td>67</td>
      <td>58</td>
      <td>0</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>Bonnie Ray</td>
      <td>F</td>
      <td>9th</td>
      <td>Huang High School</td>
      <td>97</td>
      <td>84</td>
      <td>0</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Calculate total number of schools
total_schools = school_data['school_name'].nunique()
```


```python
# Calculate total number of students
total_students = school_data['size'].sum()
```


```python
# Calculate total budget
total_budget = school_data['budget'].sum()
```


```python
# Calculate average math score
average_math_score = df['math_score'].mean()
```


```python
# Calculate average reading score
average_reading_score = df['reading_score'].mean()
```


```python
# Calculate percentage of students that have a passing math score
percent_pass_math = (df['math_score'][df.math_score >= 70].count() / len(df['math_score'])) * 100
```


```python
# Calculate percentage of students that have a passing reading score
percent_pass_reading = (df['reading_score'][df.reading_score >= 70].count() / len(df['reading_score'])) * 100
```


```python
# Calculate percentage of students that passed overall
percent_pass_overall = (df['Student ID'][(df.reading_score >= 70) & (df.math_score >= 70)].count() / len(df['Student ID']) * 100)
```


```python
# Create a dataframe to hold the results
district_summary = pd.DataFrame({
    "Total Schools" : total_schools,
    "Total Students": total_students,
    "Total Budget": total_budget,
    "Average Math Score": average_math_score,
    "Average Reading Score": average_reading_score,
    "% Passing Math": percent_pass_math,
    "% Passing Reading": percent_pass_reading,
    "% Overall Passing": percent_pass_overall
}, index = [0])
```

### District Summary


```python
district_summary
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Total Schools</th>
      <th>Total Students</th>
      <th>Total Budget</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>15</td>
      <td>39170</td>
      <td>24649428</td>
      <td>78.985371</td>
      <td>81.87784</td>
      <td>74.980853</td>
      <td>85.805463</td>
      <td>65.172326</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Create a DataFrame to be joined on by calculations
school_summary = pd.DataFrame({
    "School Name": school_data['school_name'],
    "School Type": school_data['type'],
    "Size": school_data['size'],
    "Total Budget": school_data['budget']
})
```


```python
# Calculate budget per student
for school in school_data['school_name']:
    budget_per_student = pd.DataFrame({"School Name": school_data['school_name'],
                                       "Budget Per Student" : school_data['budget'] / school_data['size']})
```


```python
# Create DataFrame of just school names and reading scores
school_reading_average_score = df[['school_name','reading_score']]

# Group this DataFrame by school and aggregate by reading score average
school_reading_average_score = school_reading_average_score.groupby(['school_name']).mean()

# Reset index to default
school_reading_average_score = school_reading_average_score.reset_index()

# Rename column
school_reading_average_score = school_reading_average_score.rename(columns={"school_name": "School Name"})
```


```python
# Create DataFrame of just school names and math scores
school_math_average_score = df[['school_name','math_score']]

# Group this DataFrame by school and aggregate by math score average
school_math_average_score = school_math_average_score.groupby(['school_name']).mean()

# Reset index to default
school_math_average_score = school_math_average_score.reset_index()

# Rename column
school_math_average_score = school_math_average_score.rename(columns={"school_name": "School Name"})
```


```python
# Create DataFrame of just school names and reading scores
school_reading_pass_rate = df[['school_name', 'reading_score']]

# Rename school_name to better formatting
school_reading_pass_rate = school_reading_pass_rate.rename(columns={'school_name': 'School Name'})

# Create column counting students
school_reading_pass_rate['student_count'] = 1

# Create column pass/fail for conditional >/< 70
school_reading_pass_rate['pass_count'] = [1 if score > 70 else 0 for score in df['reading_score']]

# Group by school
school_reading_pass_rate = school_reading_pass_rate.groupby('School Name').sum()

# Create pass fail percentage column
school_reading_pass_rate['% Passing Reading'] = (
    school_reading_pass_rate['pass_count'] / school_reading_pass_rate['student_count'] * 100)

# Drop unnecessary columns
school_reading_pass_rate = school_reading_pass_rate.drop(
    ['reading_score','student_count','pass_count'], 1)

# Reset index
school_reading_pass_rate = school_reading_pass_rate.reset_index()
```

    C:\Users\bryantim\AppData\Local\Temp\ipykernel_16624\1119901948.py:21: FutureWarning: In a future version of pandas all arguments of DataFrame.drop except for the argument 'labels' will be keyword-only.
      school_reading_pass_rate = school_reading_pass_rate.drop(
    


```python
# Create DataFrame of just school names and math scores
school_math_pass_rate = df[['school_name', 'math_score']]

# Rename school_name to better formatting
school_math_pass_rate = school_math_pass_rate.rename(columns={'school_name': 'School Name'})

# Create column counting students
school_math_pass_rate['student_count'] = 1

# Create column pass/fail for conditional >/< 70
school_math_pass_rate['pass_count'] = [1 if score > 70 else 0 for score in df['math_score']]

# Group by school
school_math_pass_rate = school_math_pass_rate.groupby('School Name').sum()

# Create pass fail percentage column
school_math_pass_rate['% Passing Math'] = (
    school_math_pass_rate['pass_count'] / school_math_pass_rate['student_count'] * 100)

# Drop unnecessary columns
school_math_pass_rate = school_math_pass_rate.drop(
    ['math_score','student_count','pass_count'], 1)
```

    C:\Users\bryantim\AppData\Local\Temp\ipykernel_16624\3977246586.py:21: FutureWarning: In a future version of pandas all arguments of DataFrame.drop except for the argument 'labels' will be keyword-only.
      school_math_pass_rate = school_math_pass_rate.drop(
    


```python
# Create DataFrame of just school names and reading scores
school_overall_pass_rate = df[['school_name', 'reading_score', 'math_score']]

# Rename school_name to better formatting
school_overall_pass_rate = school_overall_pass_rate.rename(columns={'school_name': 'School Name'})

# Create column counting students
school_overall_pass_rate['student_count'] = 1

# Create reading column pass/fail for conditional >/< 70
school_overall_pass_rate['reading_pass_count'] = [1 if score >= 70 else 0 for score in df['reading_score']]

# Create reading column pass/fail for conditional >/< 70
school_overall_pass_rate['math_pass_count'] = [1 if score >= 70 else 0 for score in df['math_score']]

# Sum up reading and math columns
school_overall_pass_rate['sum_pass_count'] = school_overall_pass_rate['reading_pass_count'] + school_overall_pass_rate['math_pass_count']

# Create overall column pass/fail for conditional = 2
school_overall_pass_rate['overall_pass_count'] = [1 if score == 2 else 0 
                                                  for score in school_overall_pass_rate['sum_pass_count']]

# Group by school
school_overall_pass_rate = school_overall_pass_rate.groupby('School Name').sum()

# Create pass fail percentage column
school_overall_pass_rate['% Passing Overall'] = (
    school_overall_pass_rate['overall_pass_count'] / school_overall_pass_rate['student_count'] * 100)

# Drop unnecessary columns
school_overall_pass_rate = school_overall_pass_rate.drop(
    ['reading_score', 'math_score', 'student_count', 'reading_pass_count', 'math_pass_count',
    'sum_pass_count', 'overall_pass_count'], 1)
```

    C:\Users\bryantim\AppData\Local\Temp\ipykernel_16624\3521200253.py:31: FutureWarning: In a future version of pandas all arguments of DataFrame.drop except for the argument 'labels' will be keyword-only.
      school_overall_pass_rate = school_overall_pass_rate.drop(
    


```python
# Merge dataframes clean up extra columns
school_summary = pd.merge(school_summary, budget_per_student, how='outer', on=["School Name", "School Name"])
```


```python
# Merge w/ average reading score
school_summary = pd.merge(school_summary, school_reading_average_score, how='outer', on=["School Name", "School Name"])
school_summary = school_summary.rename(columns={"reading_score": "Average Reading Score"})
```


```python
# Merge w/ average math score
school_summary = pd.merge(school_summary, school_math_average_score, how='outer', on=["School Name", "School Name"])
school_summary = school_summary.rename(columns={"math_score": "Average Math Score"})
```


```python
# Merge w/ % passing reading
school_summary = pd.merge(school_summary, school_reading_pass_rate, how='outer', on=["School Name", "School Name"])
```


```python
# Merge w/ % passing math
school_summary = pd.merge(school_summary, school_math_pass_rate, how='outer', on=["School Name", "School Name"])
```


```python
# Merge w/ % passing overall
school_summary = pd.merge(school_summary, school_overall_pass_rate, how='outer', on=["School Name", "School Name"])
```

### School Summary


```python
school_summary
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>School Name</th>
      <th>School Type</th>
      <th>Size</th>
      <th>Total Budget</th>
      <th>Budget Per Student</th>
      <th>Average Reading Score</th>
      <th>Average Math Score</th>
      <th>% Passing Reading</th>
      <th>% Passing Math</th>
      <th>% Passing Overall</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Huang High School</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>655.0</td>
      <td>81.182722</td>
      <td>76.629414</td>
      <td>78.813850</td>
      <td>63.318478</td>
      <td>53.513884</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Figueroa High School</td>
      <td>District</td>
      <td>2949</td>
      <td>1884411</td>
      <td>639.0</td>
      <td>81.158020</td>
      <td>76.711767</td>
      <td>78.433367</td>
      <td>63.750424</td>
      <td>53.204476</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Shelton High School</td>
      <td>Charter</td>
      <td>1761</td>
      <td>1056600</td>
      <td>600.0</td>
      <td>83.725724</td>
      <td>83.359455</td>
      <td>92.617831</td>
      <td>89.892107</td>
      <td>89.892107</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Hernandez High School</td>
      <td>District</td>
      <td>4635</td>
      <td>3022020</td>
      <td>652.0</td>
      <td>80.934412</td>
      <td>77.289752</td>
      <td>78.187702</td>
      <td>64.746494</td>
      <td>53.527508</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Griffin High School</td>
      <td>Charter</td>
      <td>1468</td>
      <td>917500</td>
      <td>625.0</td>
      <td>83.816757</td>
      <td>83.351499</td>
      <td>93.392371</td>
      <td>89.713896</td>
      <td>90.599455</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Wilson High School</td>
      <td>Charter</td>
      <td>2283</td>
      <td>1319574</td>
      <td>578.0</td>
      <td>83.989488</td>
      <td>83.274201</td>
      <td>93.254490</td>
      <td>90.932983</td>
      <td>90.582567</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Cabrera High School</td>
      <td>Charter</td>
      <td>1858</td>
      <td>1081356</td>
      <td>582.0</td>
      <td>83.975780</td>
      <td>83.061895</td>
      <td>93.864370</td>
      <td>89.558665</td>
      <td>91.334769</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Bailey High School</td>
      <td>District</td>
      <td>4976</td>
      <td>3124928</td>
      <td>628.0</td>
      <td>81.033963</td>
      <td>77.048432</td>
      <td>79.300643</td>
      <td>64.630225</td>
      <td>54.642283</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Holden High School</td>
      <td>Charter</td>
      <td>427</td>
      <td>248087</td>
      <td>581.0</td>
      <td>83.814988</td>
      <td>83.803279</td>
      <td>92.740047</td>
      <td>90.632319</td>
      <td>89.227166</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Pena High School</td>
      <td>Charter</td>
      <td>962</td>
      <td>585858</td>
      <td>609.0</td>
      <td>84.044699</td>
      <td>83.839917</td>
      <td>92.203742</td>
      <td>91.683992</td>
      <td>90.540541</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Wright High School</td>
      <td>Charter</td>
      <td>1800</td>
      <td>1049400</td>
      <td>583.0</td>
      <td>83.955000</td>
      <td>83.682222</td>
      <td>93.444444</td>
      <td>90.277778</td>
      <td>90.333333</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Rodriguez High School</td>
      <td>District</td>
      <td>3999</td>
      <td>2547363</td>
      <td>637.0</td>
      <td>80.744686</td>
      <td>76.842711</td>
      <td>77.744436</td>
      <td>64.066017</td>
      <td>52.988247</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Johnson High School</td>
      <td>District</td>
      <td>4761</td>
      <td>3094650</td>
      <td>650.0</td>
      <td>80.966394</td>
      <td>77.072464</td>
      <td>78.281874</td>
      <td>63.852132</td>
      <td>53.539172</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Ford High School</td>
      <td>District</td>
      <td>2739</td>
      <td>1763916</td>
      <td>644.0</td>
      <td>80.746258</td>
      <td>77.102592</td>
      <td>77.510040</td>
      <td>65.753925</td>
      <td>54.289887</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Thomas High School</td>
      <td>Charter</td>
      <td>1635</td>
      <td>1043130</td>
      <td>638.0</td>
      <td>83.848930</td>
      <td>83.418349</td>
      <td>92.905199</td>
      <td>90.214067</td>
      <td>90.948012</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Create a DataFrame of the top 5 rows by % Passing Overall
top_5_overall_passing = school_summary.nlargest(5,'% Passing Overall')
top_5_overall_passing.sort_values(by=['% Passing Overall'], inplace=True, ascending=False)
```

### Top Performing Schools (By % Overall Passing)


```python
top_5_overall_passing
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>School Name</th>
      <th>School Type</th>
      <th>Size</th>
      <th>Total Budget</th>
      <th>Budget Per Student</th>
      <th>Average Reading Score</th>
      <th>Average Math Score</th>
      <th>% Passing Reading</th>
      <th>% Passing Math</th>
      <th>% Passing Overall</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>6</th>
      <td>Cabrera High School</td>
      <td>Charter</td>
      <td>1858</td>
      <td>1081356</td>
      <td>582.0</td>
      <td>83.975780</td>
      <td>83.061895</td>
      <td>93.864370</td>
      <td>89.558665</td>
      <td>91.334769</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Thomas High School</td>
      <td>Charter</td>
      <td>1635</td>
      <td>1043130</td>
      <td>638.0</td>
      <td>83.848930</td>
      <td>83.418349</td>
      <td>92.905199</td>
      <td>90.214067</td>
      <td>90.948012</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Griffin High School</td>
      <td>Charter</td>
      <td>1468</td>
      <td>917500</td>
      <td>625.0</td>
      <td>83.816757</td>
      <td>83.351499</td>
      <td>93.392371</td>
      <td>89.713896</td>
      <td>90.599455</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Wilson High School</td>
      <td>Charter</td>
      <td>2283</td>
      <td>1319574</td>
      <td>578.0</td>
      <td>83.989488</td>
      <td>83.274201</td>
      <td>93.254490</td>
      <td>90.932983</td>
      <td>90.582567</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Pena High School</td>
      <td>Charter</td>
      <td>962</td>
      <td>585858</td>
      <td>609.0</td>
      <td>84.044699</td>
      <td>83.839917</td>
      <td>92.203742</td>
      <td>91.683992</td>
      <td>90.540541</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Create a DataFrame of the bottom 5 rows by % Passing Overall
bottom_5_overall_passing = school_summary.nsmallest(5,'% Passing Overall')
bottom_5_overall_passing.sort_values(by=['% Passing Overall'], inplace=True)
```

### Bottom Performing Schools (By % Overall Passing)


```python
bottom_5_overall_passing
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>School Name</th>
      <th>School Type</th>
      <th>Size</th>
      <th>Total Budget</th>
      <th>Budget Per Student</th>
      <th>Average Reading Score</th>
      <th>Average Math Score</th>
      <th>% Passing Reading</th>
      <th>% Passing Math</th>
      <th>% Passing Overall</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>11</th>
      <td>Rodriguez High School</td>
      <td>District</td>
      <td>3999</td>
      <td>2547363</td>
      <td>637.0</td>
      <td>80.744686</td>
      <td>76.842711</td>
      <td>77.744436</td>
      <td>64.066017</td>
      <td>52.988247</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Figueroa High School</td>
      <td>District</td>
      <td>2949</td>
      <td>1884411</td>
      <td>639.0</td>
      <td>81.158020</td>
      <td>76.711767</td>
      <td>78.433367</td>
      <td>63.750424</td>
      <td>53.204476</td>
    </tr>
    <tr>
      <th>0</th>
      <td>Huang High School</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>655.0</td>
      <td>81.182722</td>
      <td>76.629414</td>
      <td>78.813850</td>
      <td>63.318478</td>
      <td>53.513884</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Hernandez High School</td>
      <td>District</td>
      <td>4635</td>
      <td>3022020</td>
      <td>652.0</td>
      <td>80.934412</td>
      <td>77.289752</td>
      <td>78.187702</td>
      <td>64.746494</td>
      <td>53.527508</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Johnson High School</td>
      <td>District</td>
      <td>4761</td>
      <td>3094650</td>
      <td>650.0</td>
      <td>80.966394</td>
      <td>77.072464</td>
      <td>78.281874</td>
      <td>63.852132</td>
      <td>53.539172</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Create DataFrame w/ just school, math scores, and grade
math_scores_by_grade = df[['school_name', 'math_score', 'grade']]

# Rename columns
math_scores_by_grade = math_scores_by_grade.rename(columns=
                                                   {'school_name':'School Name', 'math_score': 'Math Score', 'grade': 'Grade'})

# Create a pivot table of math scores by school and grade using math scores as values
math_scores_by_grade = pd.pivot_table(
    math_scores_by_grade, values = 'Math Score', index=['School Name'], columns = 'Grade')

# Reorder the columns so they are chronological
math_scores_by_grade = math_scores_by_grade[['9th', '10th', '11th', '12th']]
```

### Math Scores by Grade


```python
math_scores_by_grade
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>Grade</th>
      <th>9th</th>
      <th>10th</th>
      <th>11th</th>
      <th>12th</th>
    </tr>
    <tr>
      <th>School Name</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Bailey High School</th>
      <td>77.083676</td>
      <td>76.996772</td>
      <td>77.515588</td>
      <td>76.492218</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>83.094697</td>
      <td>83.154506</td>
      <td>82.765560</td>
      <td>83.277487</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>76.403037</td>
      <td>76.539974</td>
      <td>76.884344</td>
      <td>77.151369</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>77.361345</td>
      <td>77.672316</td>
      <td>76.918058</td>
      <td>76.179963</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>82.044010</td>
      <td>84.229064</td>
      <td>83.842105</td>
      <td>83.356164</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>77.438495</td>
      <td>77.337408</td>
      <td>77.136029</td>
      <td>77.186567</td>
    </tr>
    <tr>
      <th>Holden High School</th>
      <td>83.787402</td>
      <td>83.429825</td>
      <td>85.000000</td>
      <td>82.855422</td>
    </tr>
    <tr>
      <th>Huang High School</th>
      <td>77.027251</td>
      <td>75.908735</td>
      <td>76.446602</td>
      <td>77.225641</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>77.187857</td>
      <td>76.691117</td>
      <td>77.491653</td>
      <td>76.863248</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>83.625455</td>
      <td>83.372000</td>
      <td>84.328125</td>
      <td>84.121547</td>
    </tr>
    <tr>
      <th>Rodriguez High School</th>
      <td>76.859966</td>
      <td>76.612500</td>
      <td>76.395626</td>
      <td>77.690748</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <td>83.420755</td>
      <td>82.917411</td>
      <td>83.383495</td>
      <td>83.778976</td>
    </tr>
    <tr>
      <th>Thomas High School</th>
      <td>83.590022</td>
      <td>83.087886</td>
      <td>83.498795</td>
      <td>83.497041</td>
    </tr>
    <tr>
      <th>Wilson High School</th>
      <td>83.085578</td>
      <td>83.724422</td>
      <td>83.195326</td>
      <td>83.035794</td>
    </tr>
    <tr>
      <th>Wright High School</th>
      <td>83.264706</td>
      <td>84.010288</td>
      <td>83.836782</td>
      <td>83.644986</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Create DataFrame w/ just school, reading scores, and grade
reading_scores_by_grade = df[['school_name', 'reading_score', 'grade']]

# Rename columns
reading_scores_by_grade = reading_scores_by_grade.rename(columns=
                                                   {'school_name':'School Name', 'reading_score': 'Reading Score', 'grade': 'Grade'})

# Create a pivot table of math scores by school and grade using math scores as values
reading_scores_by_grade = pd.pivot_table(
    reading_scores_by_grade, values = 'Reading Score', index=['School Name'], columns = 'Grade')

# Reorder the columns so they are chronological
reading_scores_by_grade = reading_scores_by_grade[['9th', '10th', '11th', '12th']]
```

### Reading Scores by Grade


```python
reading_scores_by_grade
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>Grade</th>
      <th>9th</th>
      <th>10th</th>
      <th>11th</th>
      <th>12th</th>
    </tr>
    <tr>
      <th>School Name</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Bailey High School</th>
      <td>81.303155</td>
      <td>80.907183</td>
      <td>80.945643</td>
      <td>80.912451</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>83.676136</td>
      <td>84.253219</td>
      <td>83.788382</td>
      <td>84.287958</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>81.198598</td>
      <td>81.408912</td>
      <td>80.640339</td>
      <td>81.384863</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>80.632653</td>
      <td>81.262712</td>
      <td>80.403642</td>
      <td>80.662338</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>83.369193</td>
      <td>83.706897</td>
      <td>84.288089</td>
      <td>84.013699</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>80.866860</td>
      <td>80.660147</td>
      <td>81.396140</td>
      <td>80.857143</td>
    </tr>
    <tr>
      <th>Holden High School</th>
      <td>83.677165</td>
      <td>83.324561</td>
      <td>83.815534</td>
      <td>84.698795</td>
    </tr>
    <tr>
      <th>Huang High School</th>
      <td>81.290284</td>
      <td>81.512386</td>
      <td>81.417476</td>
      <td>80.305983</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>81.260714</td>
      <td>80.773431</td>
      <td>80.616027</td>
      <td>81.227564</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>83.807273</td>
      <td>83.612000</td>
      <td>84.335938</td>
      <td>84.591160</td>
    </tr>
    <tr>
      <th>Rodriguez High School</th>
      <td>80.993127</td>
      <td>80.629808</td>
      <td>80.864811</td>
      <td>80.376426</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <td>84.122642</td>
      <td>83.441964</td>
      <td>84.373786</td>
      <td>82.781671</td>
    </tr>
    <tr>
      <th>Thomas High School</th>
      <td>83.728850</td>
      <td>84.254157</td>
      <td>83.585542</td>
      <td>83.831361</td>
    </tr>
    <tr>
      <th>Wilson High School</th>
      <td>83.939778</td>
      <td>84.021452</td>
      <td>83.764608</td>
      <td>84.317673</td>
    </tr>
    <tr>
      <th>Wright High School</th>
      <td>83.833333</td>
      <td>83.812757</td>
      <td>84.156322</td>
      <td>84.073171</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Create DataFrame of just necessary columns from school_summary
scores_by_school_spending = school_summary[
    ['Budget Per Student','Average Math Score', 'Average Reading Score', '% Passing Math', 
     '% Passing Reading', '% Passing Overall' ]]
```


```python
# Create bins for spending per student ranges
bins = [0, 584, 629, 644, 680]

group_labels = ['<$585', '$585-630', '$630-645', '$645-680']
```


```python
# Slice the data and place it into bins
pd.cut(scores_by_school_spending['Budget Per Student'], bins, labels=group_labels).head()
```




    0    $645-680
    1    $630-645
    2    $585-630
    3    $645-680
    4    $585-630
    Name: Budget Per Student, dtype: category
    Categories (4, object): ['<$585' < '$585-630' < '$630-645' < '$645-680']




```python
 # Place the data series into a new column inside of the DataFrame
scores_by_school_spending["Spending Per Student Group"] = pd.cut(scores_by_school_spending['Budget Per Student'], bins, labels=group_labels)
```

    C:\Users\bryantim\AppData\Local\Temp\ipykernel_16624\3414563277.py:2: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      scores_by_school_spending["Spending Per Student Group"] = pd.cut(scores_by_school_spending['Budget Per Student'], bins, labels=group_labels)
    


```python
# Group by spending per student group
scores_by_school_spending = scores_by_school_spending.groupby('Spending Per Student Group').mean()

# Drop budget per student column
scores_by_school_spending = scores_by_school_spending[
    ['Average Math Score', 'Average Reading Score', '% Passing Math', '% Passing Reading', '% Passing Overall']]
```

###  Scores by School Spending


```python
scores_by_school_spending
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Passing Overall</th>
    </tr>
    <tr>
      <th>Spending Per Student Group</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;$585</th>
      <td>83.455399</td>
      <td>83.933814</td>
      <td>90.350436</td>
      <td>93.325838</td>
      <td>90.369459</td>
    </tr>
    <tr>
      <th>$585-630</th>
      <td>81.899826</td>
      <td>83.155286</td>
      <td>83.980055</td>
      <td>89.378647</td>
      <td>81.418596</td>
    </tr>
    <tr>
      <th>$630-645</th>
      <td>78.518855</td>
      <td>81.624473</td>
      <td>70.946108</td>
      <td>81.648261</td>
      <td>62.857656</td>
    </tr>
    <tr>
      <th>$645-680</th>
      <td>76.997210</td>
      <td>81.027843</td>
      <td>63.972368</td>
      <td>78.427809</td>
      <td>53.526855</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Create DataFrame of just necessary columns from school_summary
scores_by_school_size = school_summary[
    ['Size','Average Math Score', 'Average Reading Score', '% Passing Math', 
     '% Passing Reading', '% Passing Overall' ]]
```


```python
# Create bins for school sizes 
bins = [0, 999, 1999, 5000]

group_labels = ['Small (< 1000)', 'Medium (1000-2000)', 'Large (2000-5000)']
```


```python
# Slice the data and place it into bins
pd.cut(scores_by_school_size['Size'], bins, labels=group_labels)
```




    0      Large (2000-5000)
    1      Large (2000-5000)
    2     Medium (1000-2000)
    3      Large (2000-5000)
    4     Medium (1000-2000)
    5      Large (2000-5000)
    6     Medium (1000-2000)
    7      Large (2000-5000)
    8         Small (< 1000)
    9         Small (< 1000)
    10    Medium (1000-2000)
    11     Large (2000-5000)
    12     Large (2000-5000)
    13     Large (2000-5000)
    14    Medium (1000-2000)
    Name: Size, dtype: category
    Categories (3, object): ['Small (< 1000)' < 'Medium (1000-2000)' < 'Large (2000-5000)']




```python
 # Place the data series into a new column inside of the DataFrame
scores_by_school_size["School Size"] = pd.cut(scores_by_school_size['Size'], bins, labels=group_labels)
```

    C:\Users\bryantim\AppData\Local\Temp\ipykernel_16624\2356858920.py:2: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      scores_by_school_size["School Size"] = pd.cut(scores_by_school_size['Size'], bins, labels=group_labels)
    


```python
# Group by spending per student group
scores_by_school_size = scores_by_school_size.groupby('School Size').mean()

# Drop budget per student column
scores_by_school_size = scores_by_school_size[
    ['Average Math Score', '% Passing Math', '% Passing Reading', '% Passing Overall']]
```

### Scores by School Size


```python
scores_by_school_size
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Average Math Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Passing Overall</th>
    </tr>
    <tr>
      <th>School Size</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Small (&lt; 1000)</th>
      <td>83.821598</td>
      <td>91.158155</td>
      <td>92.471895</td>
      <td>89.883853</td>
    </tr>
    <tr>
      <th>Medium (1000-2000)</th>
      <td>83.374684</td>
      <td>89.931303</td>
      <td>93.244843</td>
      <td>90.621535</td>
    </tr>
    <tr>
      <th>Large (2000-5000)</th>
      <td>77.746417</td>
      <td>67.631335</td>
      <td>80.190800</td>
      <td>58.286003</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Create a DataFrame from school_summary of just the columns needed
scores_by_school_type = school_summary[[
    'School Type', 'Average Math Score', '% Passing Math', '% Passing Reading', '% Passing Overall']]
```


```python
# Group by school type
scores_by_school_type = scores_by_school_type.groupby('School Type').mean()
```

### Scores by School Type


```python
scores_by_school_type
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Average Math Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Passing Overall</th>
    </tr>
    <tr>
      <th>School Type</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Charter</th>
      <td>83.473852</td>
      <td>90.363226</td>
      <td>93.052812</td>
      <td>90.432244</td>
    </tr>
    <tr>
      <th>District</th>
      <td>76.956733</td>
      <td>64.302528</td>
      <td>78.324559</td>
      <td>53.672208</td>
    </tr>
  </tbody>
</table>
</div>


