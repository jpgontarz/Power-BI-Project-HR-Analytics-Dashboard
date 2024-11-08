# Power BI Project: HR Analytics Dashboard

![image](https://github.com/user-attachments/assets/6224a7bf-a12f-4a9a-bb08-061230b6333b)

## Project Structure

- [Overview](#overview)
- [Task](#task)
- [About the Data](#about-the-data)
- [ETL](#etl)
- [Data Modeling](#data-modeling)
- [Visualization and Insights](#visualization-and-insights)
- [Recommendations](#recommendations)
- [Conclusion](#conclusion)

## Overview

_About 15% of the PeopleInsights workforce leaves each year. Company management have noticed a recurring theme of delayed projects and unkept deadlines among former employees, resulting in the need for a sizeable recruiting department and many resources allocated to new training._

## Task

As an HR analyst, PeopleInsights have contacted me to understand various factors that could be correlated to the high attrition rate and to see what potential changes they could make to the workplace to better retain employees.

Questions that PeopleInsights would like answered are:

1. What is the average Environment Satisfaction score across different departments?

2. How does Work-Life Balance correlate with Job Satisfaction in the Employee Survey Data?

3. What is the distribution of employees based on their Distance from Home?

4. How many employees have been classified as high performers (Performance Rating) in each department?

5. What is the trend in Employee Count by Department over time?

6. What percentage of employees travel for business versus those who do not?

7. How does Attrition rate vary by Age group and Department?

8. What is the average Job Involvement score among employees who have left the company versus those who remain?

9. How many employees fall into each Education Level category?

10. What are the average salaries based on Job Roles, and how do they compare to Employee Satisfaction levels?

## About the Data

Original data, along with an explanation of each column, can be found [here](https://www.kaggle.com/datasets/vjchoudhary7/hr-analytics-case-study/data).

The dataset includes six tables, capturing general employee data, employe survey data, manager survey data, and clocking in and out times, with nearly 22,100 rows and over 550 columns.

![image](https://github.com/user-attachments/assets/1e2dd5d4-2380-4d79-96e1-85dd051b2ad7)

_Sample of Employee_Data table_

## ETL

Once I had the datasets and somewhat understood them, I could start the ETL (Extract, Transform, Load) process to clean the data.

I started organizing the five main data tables by loading each into Power BI's Power Query Editor, where I quickly changed the names of the tables to be slightly more succinct:

![image](https://github.com/user-attachments/assets/72ec42ef-1664-43b7-b474-0db70270b1cd)

Next, I made sure the row headers were correct. When I loaded the data into Power Query, some of the tables' column headers were pushed down into Row 2, and I easily fixed this with the Use First Row as Headers tool.

![image](https://github.com/user-attachments/assets/21d502bc-a392-4ab3-bd14-917242509076)
_Dictionary table before row header change_


![image](https://github.com/user-attachments/assets/79384294-f917-4455-bd09-50c1817be7ec)
_Dictionary table after row header change_

#### Data Types

First, I checked to determine if any column data types needed fixing. For instance, in the Environment Satisfaction column of the Employee_Survey table, the data type was text, with null values displaying "NA". I wanted these values to be whole numbers where possible.

![image](https://github.com/user-attachments/assets/c1c75682-be58-489c-a7bb-b4b01c97ccd8)
_Employee_Survey, Environment Satisfaction before data type change_

I changed the "NA" values to blank, and then changed the data types to whole number.

![image](https://github.com/user-attachments/assets/ef6cf7c3-a92a-4eb3-b4d9-c297a75dc07e)

_Employee_Survey, Environment Satisfaction after data type change_

I applied this change to all similar columns.

#### Duplicates

Next, I checked for and removed any duplicate rows by using the Remove Duplicates tool. There were no duplicates found.

#### Standardization

I then looked for any inconsistent formatting, incorrect values, or any data that could be changed to be easier to read. For example, in the Employee_Data table BusinessTravel column, I was not satisfied with the grammar of the entries.

![image](https://github.com/user-attachments/assets/41d3753e-f159-461a-b5dc-596cfaa41c7e)

_Employee_Data, BusinessTravel before change_

![image](https://github.com/user-attachments/assets/17460c9b-7eb5-49c9-9ba3-2d4e23d32d1c)

_Employee_Data, BusinessTravel after change_

This was the only standardization change I needed to make. With the data cleaning finished, I then applied the changes and loading the data.

## Data Modeling

#### STAR Schema

To model the data in order to visualize correctly, I needed to use a STAR Schema. Specifically, with Employee_Data as my central table, I established relationships with the other tables via EmployeeID.

![image](https://github.com/user-attachments/assets/88c14644-9a7b-44ef-b0d5-c1793e9f7dee)

_STAR Schema based on EmployeeID_

With this accomplished, I could start answering the necessary questions.

## Visualization and Insights

**1. What is the average Environment Satisfaction score across different departments?**

To find the average Environment Satisfaction score, I created a new measure, writing a simple DAX formula which utilized the AVERAGEX function.

```DAX
Average Environment Satisfaction = AVERAGEX(Employee_Survey, Employee_Survey[EnvironmentSatisfaction])
```

![image](https://github.com/user-attachments/assets/5b84119b-4aa6-44a6-b8f2-e9a3b926b0aa)


The HR department boasts the highest average Environment Satisfaction scores from employees with 2.83 out of 4, followed by Sales at 2.73 and Research & Development closely behind Sales at 2.72.

**2. How does employees' work-life balance correlate with job satisfaction?**

![image](https://github.com/user-attachments/assets/30f3eb57-061b-495c-a139-a147c39cfea1)

There does not appear to be any significant correlaion between the work life balance ratings and the average job satisfaction ratings.

**3. What is the distribution of employees based on how far they live from work?**

For this question, instead of listing out each distance, I decided to divide the distances by a five-mile group using some IF statements.

```DAX
DistanceFromHomeGroup = 
IF(Employee_Data[DistanceFromHome] <= 4, "< 5 miles",
    IF(AND(Employee_Data[DistanceFromHome] >= 5, Employee_Data[DistanceFromHome] <= 9), "5-10 miles",
        IF(AND(Employee_Data[DistanceFromHome] >= 10, Employee_Data[DistanceFromHome] <= 14), "10-15 miles",
            IF(AND(Employee_Data[DistanceFromHome] >= 15, Employee_Data[DistanceFromHome] <= 19), "15-20 miles",
                IF(AND(Employee_Data[DistanceFromHome] >= 20, Employee_Data[DistanceFromHome] <= 24), "20-25 miles",
                    IF(Employee_Data[DistanceFromHome] >= 25, "25+ miles"))))))
```

![image](https://github.com/user-attachments/assets/b4d1b246-1a22-4eaa-a7e7-2f72b9d3ed43)

There is an exponential correlation to how far employees live from work and how many in each category. Employees living within five miles of work make up nearly 39% of all employees at 1,701, next at over 25% is the 5-10 miles group with 1,119, followed by 525 in the 10-15 miles group at 378, about 9% in the 15-20 miles group at 378, almost 8% for the 20-25 miles group at 351, and lastly 7.62% for the 25+ miles group with 336 employees.

**4. How many employees have been classified as high performers in each department?**

If an employee received a job involvement rating and a performance rating both of 4, then I considered them as a high performer.

```DAX
HighPerformer? = IF(AND(Manager_Survey[JobInvolvement] = 4, Manager_Survey[PerformanceRating] = 4), "Yes", "No")
```

![image](https://github.com/user-attachments/assets/5c18ccbe-9326-4237-bfff-f7de605007ad)

Research & Development claim the most high performers at 39, second is Sales with 24, and last is HR with 3.

**5. What is the trend (if any) in employee count by department over time?**





**6. What percentage of employees travel for business versus those who do not?**

**7. How does the attrition rate vary by age group and department?**

**8. How does job involvement compare in employees who have left the company versus those who remain?**

**9. How many employees are in each education level?**

**10. What are the average salaries based on job roles, and how do they compare to employee satisfaction?**


## Recommendations



## Conclusion

