---
layout: post
title: Data Scientist Salary?
subtitle: Looking for a job?
cover-img: /assets/img/data-science-career-oppotunities.jpg
thumbnail-img: /assets/img/salary distribution.png
share-img: /assets/img/data-science-career-oppotunities.jpg
tags: [data, scientist, science, jobs]
comments: true
---
I will be graduating soon. I wanted to know what I have to look forward to. The dataset I chose is a job listing for Data Scientists positions gathered from Glassdoor.

## Objectives

What kind of jobs get higher salaries? (Job Title, Salary Estimate)
What kind of companies pays more? (Company, Size, Industry, Revenue)
Does the job location matter to salaries?


The limit and assumption is
 

*   the salary estimates come from Glassdoor, which may not reflect the actual salaries.
*   The dataset only reflet the outcome at the time the dataset was published, which is supposed to be July 2020. 

### Hypothesis:

If people lost their jobs in the middle of the pandemic, there are job openings.

I started with importing my data from a local file, uploaded libraries, and clean the dataset. I removed unnecessary columns, by using .drop(). Luckily there weren't any missing values. 

```
#Remove Rating values from Company Name. 
ds['Company Name'],_=ds['Company Name'].str.split('\n', 1).str
# 1st column after split, 2nd column after split (delete when '_')
# string.split(separator, maxsplit) maxsplit default -1, which means all occurrances

# Split salary into two columns min salary and max salary.
ds['Salary Estimate'],_=ds['Salary Estimate'].str.split('(', 1).str


#exclude hourly rating salaries
ds=ds[(ds['Salary Estimate'].str.contains(' Per Hour'))==False].reset_index(drop=True)

# Split salary into two columns min salary and max salary.
# lstrip is for removing leading characters; rstrip is for removing rear characters
ds['Min_Salary'],ds['Max_Salary']=ds['Salary Estimate'].str.split('-').str
ds['Min_Salary']=ds['Min_Salary'].str.strip(' ').str.lstrip('$').str.rstrip('K').fillna(0).astype('int')
ds['Max_Salary']=ds['Max_Salary'].str.strip(' ').str.lstrip('$').str.rstrip('K').fillna(0).astype('int')


# To estimate the salary with regression and other analysis, better come up with one number: Est_Salary = (Min_Salary+Max_Salary)/2
ds['Est_Salary']=(ds['Min_Salary']+ds['Max_Salary'])/2

# To estimate the size with regression and other analysis, better come up with one number: Est_Salary = (Min_Salary+Max_Salary)/2
#ds['Est_Size']=(ds['Min_Size']+ds['Max_Size'])/2

# Separate 'City' & 'State' from job 'Location'
ds['City'],ds['State'] = ds['Location'].str.split(', ',1).str


# Clean up duplicated city names in State's name
ds['State']=ds['State'].replace('Arapahoe, CO','CO')
ds['State']=ds['State'].replace('Los Angeles, CA','CA')
ds['State']=ds['State'].replace('NY (US), NY','NY')
```
I looked at a few aspects to see my possibilities for a good job by using the bar chart and explanatory visualizations, for example, the current openings, Top 20 cities with their minimum and maximum salaries and Size of Employees Vs No of Companies, etc.
