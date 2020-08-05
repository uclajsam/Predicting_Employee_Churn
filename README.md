# Predicting Employee Churn

## Problem Statement
Employee turnover is a costly problem that many companies face today.  The actual cost for replacing an employe can be very significant.  According to a study conducted by the Center for American Progress, companies spend about 20% of an employees' salary to replace that individual.  The percentage can be significantly higher if a "key" employee or C-level executive needs to be replaced.  In addition to the monetary cost, substantial time is needed to replace that employee (i.e. interviewing potential candidates, onboarding, training, etc.).  Companies can spend a few weeks to a few months replacing employees, which can reduce a company's profitability and production over time.  

In this study, we will attempt to answer the following questions these companies face: <br>
**1) What are the key factors that cause employees to leave a company?** <br>
**2) What strategies/action plan can a company implement to minimize the effect of employees leaving?**

## The Data
The dataset contains information from 15,000 employees for a particular company.  The target variable is labeled as 'turnover' and it is a binary type (0 = still with company, 1 = left company).  The remainig features are defined in the dictionary below:

| Variable | Description |
| -------- | ----------- |
| satisfaction | Level of satisfaction for the employee, ranging from 0 (lowest) to 1 (highest) |
| evaluation | Rate of evaluation, defined for the range 0 (lowest) to 1 (highest) |
| project_count | Number of projects employee is working on |
| average_monthly_hours | Number of hours an employee works on a monthly basis |
| years_at_company| Number of years employed at the company |
| work_accident | Number of accidents employee has been involved in at work |
| turnover | Binary variable: 0 = employee still with the company, 1 = employee left the company |
| promotion | Binary variable: 0 = not promoted, 1 = promoted |
| department | Name of the department employee works in |
| salary | Description of salary amount - (low/medium/high) | 

## Exploratory Data Analysis (EDA)
Below is a countpolot of the target variable.  There is a clear imbalance between employees who stayed (74%) and those who left (26%).  We will address this imbalance when we start modeling the data.

<p align = "center">
<img src = "https://user-images.githubusercontent.com/60159655/89428538-57663580-d6f1-11ea-80f6-4421d64051fb.png" />
</p>

### Numerical Variable Distribution
<p align = "center">
<img src = "https://user-images.githubusercontent.com/60159655/89429158-102c7480-d6f2-11ea-96f9-ace32e94b6bb.png" />
</p>

From the histograms, a few observations can be made:
- For the Satisfaction Distribution, there is a significant number of employees who have low satisfaction (less than 0.2).
- For the Evaluation Distribution, there are clearly a group of employees who are being evaluated more often than other problems.  This could potentially be a problem.
- For the Project Count Distribution, most employees work on 3-4 projects at one given time.
- Average Monthly Hours shows a bimodal distribution.  There are two distinct peaks, 150 hours and 250 hours. 

We will perform other visualizations on these variables to see if other information can be deduced.

## KDE (Kernel Density Estimator) Plots
<p align = "center">
<img src = "https://user-images.githubusercontent.com/60159655/89431711-1f60f180-d6f5-11ea-9e0e-da8b1dd52c72.png" />
</p>
