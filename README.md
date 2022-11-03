
Import required dependencies
In[1]:
``import pandas as pd
import os``


##Deliverable 1: Collect the Data:
To collect the data that you’ll need, complete the following steps:
 1. Using the Pandas `read_csv` function and the `os` module, import the data from the `new_full_student_data.csv` file, and create a DataFrame called student_df. 
 2. Use the head function to confirm that Pandas properly imported the data.

In[2]:
Create the path and import the data
`full_student_data = os.path.join('../Resources/new_full_student_data.csv')
student_df = pd.read_csv(full_student_data)`

In[3]:
Verify that the data was properly imported
`student_df.head()`

##Deliverable 2: Prepare the Data
To prepare and clean your data for analysis, complete the following steps:
 1. Check for and remove all rows with `NaN`, or missing, values in the student DataFrame. 
 2. Check for and remove all duplicate rows in the student DataFrame.
 3. Use the `str.replace` function to remove the "th" from the grade levels in the grade column.
 4. Check data types using the `dtypes` property.
 5. Remove the "th" suffix from every value in the grade column using `str` and `replace`.
 6. Change the grade colum to the `int` type and verify column types.
 7. Use the head (and/or the tail) function to preview the DataFrame.

In[4]:
Check for null values
`student_df.isnull()
student_df.isnull().sum()`


In[5]:
Drop rows with null values and verify removal
`student_df = student_df.dropna()
student_df.isnull().sum()`

In[6]:
Check for duplicated rows
`student_df.duplicated().sum()`

In[7]:
Drop duplicated rows and verify removal
`student_df = student_df.drop_duplicates()
student_df`

In[8]:
Check data types
`student_df.dtypes`

In[9]:
Examine the grade column to understand why it is not an int
`student_df.head`

In[10]:
Remove the non-numeric characters and verify the contents of the column
`student_df.loc[:,"grade"] = student_df.loc[:,"grade"].str.replace("th","")
student_df`

In[11]:
Change the grade column to the int type and verify column types
`student_df.loc[:,"grade"] = student_df.loc[:,"grade"].astype("int64")
student_df.dtypes`

##Deliverable 3: Summarize the Data
Describe the data using summary statistics on the data as a whole and on individual columns.
 1. Generate the summary statistics for each DataFrame by using the `describe` function.
 2. Display the mean math score using the `mean` function. 
 3. Store the minimum reading score as `min_reading_score`.

In[12]:
Display summary statistics for the DataFrame
`student_df.describe()`

In[13]:
Display the mean math score using the mean function
`student_df["math_score"].mean()`

In[14]:
Store the minimum reading score as min_reading_score
`min_reading_score = student_df["reading_score"].min()
print(min_reading_score)`

##Deliverable 4: Drill Down into the Data
Drill down to specific rows, columns, and subsets of the data.To drill down into the data, complete the following steps:
 1. Use `loc` to display the grade column.
 2. Use `iloc` to display the first 3 rows and columns 3, 4, and 5.
 3. Show the rows for grade nine using `loc`.
 4. Store the row with the minimum overall reading score as `min_reading_row` using `loc` and the `min_reading_score` found in Deliverable 3.
 5. Find the reading scores for the school and grade from the output of step three using `loc` with multiple conditional statements.
 6. Using conditional statements and `loc` or `iloc`, find the mean reading score for all students in grades 11 and 12 combined.

In[15]:
Use loc to display the grade column
`student_df.loc[:,"grade"]`

In[16]:
Use `iloc` to display the first 3 rows and columns 3, 4, and 5.
`student_df.iloc[0:3,[3,4,5]]`

In[17]:
Select the rows for grade nine and display their summary statistics using `loc` and `describe`.
`student_df.loc[student_df["grade"] == 9].describe()`

In[18]:
Store the row with the minimum overall reading score as `min_reading_row using `loc` and the `min_reading_score` found in Deliverable 3.

`min_reading_score = student_df["reading_score"].min()
min_reading_row = student_df.loc[student_df["reading_score"] == min_reading_score]
min_reading_row`

In[19]:
Use loc with conditionals to select all reading scores from 10th graders at Dixon High School.
`dhs_10_reading_scores = student_df.loc[(student_df["grade"] == 10) & (student_df["school_name"] == "Dixon High School"), ["school_name", "reading_score"]]
dhs_10_reading_scores`

In[20]:

Find the mean reading score for all students in grades 11 and 12 combined.
`grade = student_df.loc[(student_df["grade"] == 11) | (student_df["grade"] == 12), ["reading_score"]].mean()
grade`

##Deliverable 5: Make Comparisons Between District and Charter Schools
Compare district vs charter schools for budget, size, and scores.
Make comparisons within your data by completing the following steps:
 1. Using the `groupby` and `mean` functions, look at the average reading and math scores per school type.
 2. Using the `groupby` and `count` functions, find the total number of students at each school.
 3. Using the `groupby` and `mean` functions, find the average budget per grade for each school type.

In[21]:
Display the average budget for each school type by using the groupby and mean functions, as the following image shows:
`avg_school_budget = student_df.loc[:,["school_type","school_budget"]]
avg_school_budget.groupby("school_type").mean()`

In[22]:
Use groupby and mean to find the average reading and math scores for each school type.
`avg_student_scores_by_grade = student_df.loc[:,["school_type","reading_score","math_score"]]
avg_student_scores_by_grade.groupby("school_type").mean`

In[23]:
Use the `groupby`, `count`, and `sort_values` functions to find the total number of students at each school and sort from most students to least students.
`student_df.rename(columns = {"student_name":"student_count"}, inplace = True)
list(student_df)
avg_student_scores_by_grade = student_df.loc[:,["school_name","student_count"]]
avg_student_scores_by_grade.groupby("school_name").count().sort_values(by = "student_count", ascending = False)`

In[24]:
Find the average math score and budget by grade for each school type by using the groupby and mean functions, as the following image shows:
`avg_math_score_by_school =student_df.loc[:,["school_type", "grade", "math_score","school_budget"]]
avg_math_score_by_school.groupby(["school_type","grade"]).mean(["math_score","school_budget"]).round(decimals=0)`

##Deliverable 6: Summarize Your Findings
Based on the current analysis, the only mentionable discovery is that Charter schools have better math scores than Public schools. However, it would be worthwhile to investigate correlations between a school's budget and its math and reading scores if we had more information to aggregate regarding the school's demographics, such as average income.
