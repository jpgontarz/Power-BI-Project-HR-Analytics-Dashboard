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

The dataset includes six tables, capturing general employee data, employe survey data, manager survey data, and clocking in and out times, with nearly 22,100 rows and over 550 columns. However, I was working with three main tables: Employee_Survey, Employee_Data, and Manager_Survey.

![image](https://github.com/user-attachments/assets/1e2dd5d4-2380-4d79-96e1-85dd051b2ad7)

_Sample of Employee_Data table_

## ETL

Once I had the datasets and somewhat understood them, I could start the ETL (Extract, Transform, Load) process to clean the data.

I started organizing the three main data tables by loading each into Power BI's Power Query Editor, where I quickly changed the names of the tables to be slightly more succinct:

![image](https://github.com/user-attachments/assets/3081ef7b-f1ec-47ec-bbf1-09ca8192d284)

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

Then, I noticed the monthly income values in Employee_Data table display Indian Rupees. I wanted to change these values to show the corresponding dollar amounts from 2015, the year this data was recorded.

Finding an average exchange rate of 64.15 INR per USD, I made the following calculation:

```DAX
Monthly Income (USD) = Employee_Data[MonthlyIncome] / 64.15
```

![image](https://github.com/user-attachments/assets/b7a32983-2a61-4f91-bf06-0ed1729893ae)

_Monthly income (INR)_

![image](https://github.com/user-attachments/assets/30b0dede-327d-4c8e-997c-e806f5aa86e7)

_Monthly income (USD)_

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

![image](https://github.com/user-attachments/assets/5dc79726-5f50-4ad8-baa6-4f7b3e40f085)

_STAR Schema based on EmployeeID_

With this accomplished, I could start answering the necessary questions.

## Visualization and Insights

**1. What is the average Environment Satisfaction score across different departments?**

To find the average Environment Satisfaction score, I created a new measure, writing a simple DAX formula which utilized the AVERAGEX function.

```DAX
Average Environment Satisfaction = AVERAGEX(Employee_Survey, Employee_Survey[EnvironmentSatisfaction])
```

![image](https://github.com/user-attachments/assets/10fdea57-260b-486b-a444-de3a240ae4b5)

Marginally, The HR department boasts the highest average Environment Satisfaction scores among current employees with 2.78 out of 4, closely followed by Sales and Research & Development, both at 2.77.

**2. How does employees' work-life balance correlate with job satisfaction?**

![image](https://github.com/user-attachments/assets/6ec9a7ae-2f55-46c6-91ff-8cc538b0cc76)

Current employees with a work-life balance rating of two seem to be slightly more satisfied with their jobs than others.

**3. What is the distribution of employees based on how far they live from work?**

For this question, instead of listing out each distance, I decided to divide the distances by a five-mile group using some IF statements.

```DAX
DistanceFromHomeGroup = IF(Employee_Data[DistanceFromHome] <= 4, "< 5 miles",
    IF(AND(Employee_Data[DistanceFromHome] >= 5, Employee_Data[DistanceFromHome] <= 9), "5-10 miles",
        IF(AND(Employee_Data[DistanceFromHome] >= 10, Employee_Data[DistanceFromHome] <= 14), "10-15 miles",
            IF(AND(Employee_Data[DistanceFromHome] >= 15, Employee_Data[DistanceFromHome] <= 19), "15-20 miles",
                IF(AND(Employee_Data[DistanceFromHome] >= 20, Employee_Data[DistanceFromHome] <= 24), "20-25 miles",
                    IF(Employee_Data[DistanceFromHome] >= 25, "25+ miles"))))))
```

![image](https://github.com/user-attachments/assets/8a2d4350-d65d-4ef8-bce9-ddaad9e1cc88)    ![image](https://github.com/user-attachments/assets/99692917-13dc-424b-98da-8bd361a1b2e4)

There is nearly an exponential correlation to how far current employees live from work and how many in each category. Employees living within five miles of work make up nearly 39% of all employees at 1,428, at well over 25% is the 5-10 miles group with 1,951, followed by 435 in the 10-15 miles group at 11.76%, 7.7% in the 15-20 miles group at 285, over 8% for the 20-25 miles group at 306, and almost 8% for the 25+ miles group with 294 current employees.

**4. How many employees have been classified as high performers in each department?**

If a current employee received a job involvement rating and a performance rating both of 4, then I considered them as a high performer.

```DAX
HighPerformer? = IF(AND(Manager_Survey[JobInvolvement] = 4, Manager_Survey[PerformanceRating] = 4), "Yes", "No")
```

![image](https://github.com/user-attachments/assets/9d03a06e-3332-4470-bfb1-d95ad7feb25a)

Research & Development claim the most high performers at 33, second is Sales with 21, and last is HR with 3.

**5. Which job roles have had the most attrition in raw count?**

To answer this question, I made a simple bar graph displaying the count of those who no longer work for PeopleInsights by their former job role.

![image](https://github.com/user-attachments/assets/c4e3fd80-efaf-4dee-bd5b-d45897c9022f)

By far, the three job roles with the highest attrition by count are Sales Executive with 165 former employees, followed closely by Research Scientist with 159, and Laboratory Technician with 126.

**6. What percentage of employees travel for business versus those who do not?**

To find how many current employees travel at all versus those who do not, I decided to make a new column which designated as such.

```DAX
BusinessTravelAny? = IF(Employee_Data[BusinessTravel] <> "None", "Yes", "No")
```

Then, creating a simple table visual, I found the answer.

![image](https://github.com/user-attachments/assets/229ccb12-57fd-420a-8adc-80dc0eb6b5dd)

A vast majority of current employees traveled for the business to some degree, with nearly 89% traveling frequently or rarely, while only 11.19% never traveled.

**7. How does the attrition rate vary by age group and department?**

First, I created a new Age Group column to group ages.

```DAX
Age Group = IF(AND(Employee_Data[Age] >= 18, Employee_Data[Age] <= 24), "18-24",
    IF(AND(Employee_Data[Age] >= 25, Employee_Data[Age] <= 34), "25-34",
        IF(AND(Employee_Data[Age] >= 35, Employee_Data[Age] <= 44), "35-44",
            IF(AND(Employee_Data[Age] >= 45, Employee_Data[Age] <= 54), "45-54",
                IF(AND(Employee_Data[Age] >= 55, Employee_Data[Age] <= 64), "55-64")))))
```

Then, I created an Attrition Rate measure to display attrition rates.

```DAX
Attrition Rate = DIVIDE(CALCULATE(COUNT(Employee_Data[EmployeeID]), Employee_Data[Attrition] = "Yes"), COUNT(Employee_Data[EmployeeID]), 0)
```

I could then see the attrition rates by age group and department.

![image](https://github.com/user-attachments/assets/84d7880e-5f57-4819-87ac-d1bd60b93bc4)

The trend seemed to be that HR had the highest attrition rate by age group, and that usually the order of highest to lowest attrition rates went HR, Research & Development, then Sales, except for the 35-44 and 55-64 age groups where Sales had a higher rate than Research & Development.

**8. How does job involvement compare in employees who have left the company versus those who remain?**

For this question, I created a clustered column chart that shows both current and former employee counts by job involvement rating.

![image](https://github.com/user-attachments/assets/928834e2-f92c-46ed-a52f-2d15c2e14c54)

The job involvement rating distribution was actually fairly similarly between current and former employees.

**9. How many employees are in each education level?**

![image](https://github.com/user-attachments/assets/cc9ade5a-555c-448c-bbe7-03f958018a19)

The education level achieved by the most current employees was a Bachelor's degree. The vast majority of employees were at least partially college-educated.

**10. What are the average salaries based on job roles, and how do they compare to employee satisfaction?**

![image](https://github.com/user-attachments/assets/452ba4cc-c681-432f-85e8-6fade1ea5323)

Manufacturing Directors made the most at $1,065 per month, while HR employees made the least at $939 per month. There seemed to be no significant trend in how average job satisfaction compares with compensation, except for that HR employees, while earning the least, do boast the highest average job satisfaction.

**Dashboard**

Along with answering these questions for the HR department, I made an easy-to-use HR dashboard to provide some quick insights.

Included in the dashboard were the number of current employees, average monthly income, attrition rate, average environment satisfaction rating, employees by job role, income vs. education, employees by gender, work-life balance by job satisfaction, age distribution, and high performers by department.

![image](https://github.com/user-attachments/assets/58b20e0a-6229-4248-8a8a-57b16b9f496a)


## Recommendations



## Conclusion

