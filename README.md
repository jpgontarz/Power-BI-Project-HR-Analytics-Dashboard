# Power BI Project: HR Analytics Dashboard

![image](https://github.com/user-attachments/assets/6224a7bf-a12f-4a9a-bb08-061230b6333b)

## Project Structure

- [Overview](#overview)
- [Task](#task)
- [About the Data](#about-the-data)
- [ETL](#etl)
- [Data Modeling and Insights](#data-modeling-and-insights)

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

#### Null Values

The first step in transforming the data was make sure the row headers are correct. When I loaded the data into Power Query, some of the tables' column headers were pushed down into Row 2, and I easily fixed this with the Use First Row as Headers tool.

![image](https://github.com/user-attachments/assets/21d502bc-a392-4ab3-bd14-917242509076)
_Dictionary table before row header change_


![image](https://github.com/user-attachments/assets/79384294-f917-4455-bd09-50c1817be7ec)
_Dictionary table after row header change_

Next, I looked for any null/not available values and promptly changed them. For the Dictionary table I could simply use the Replace Values tool to change the null values to blank.

![image](https://github.com/user-attachments/assets/82ebc2b4-9bf0-4f18-b671-08de4a1e1f2b)

_Null values..._

![image](https://github.com/user-attachments/assets/a172f9e7-43fc-49e5-b15d-1f98354da042)

_... changed to blank_

![image](https://github.com/user-attachments/assets/40af0a5d-5864-4e72-ba16-b62e947d70d6)

_Not available values..._

![image](https://github.com/user-attachments/assets/a65b4c17-5708-4a29-b19a-3242fb2680c3)

_... changed to N/A_

#### Duplicates

Next, I checked for and removed any duplicate rows by using the Remove Duplicates tool. There were no duplicates found.

#### Standardization

I then looked for any inconsistent formatting, incorrect values, or any data that could be changed to be easier to read. For example, in the Employee_Data table BusinessTravel column, I was not satisfied with the grammar of the entries.

![image](https://github.com/user-attachments/assets/41d3753e-f159-461a-b5dc-596cfaa41c7e)

_Employee_Data, BusinessTravel before change_

![image](https://github.com/user-attachments/assets/17460c9b-7eb5-49c9-9ba3-2d4e23d32d1c)

_Employee_Data, BusinessTravel after change_

This was the only standardization change I needed to make. With the data cleaning finished, I then applied the changes and loading the data.

## Data Modeling and Insights



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


### Conclusion
