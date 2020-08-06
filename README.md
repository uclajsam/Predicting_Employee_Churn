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

### KDE (Kernel Density Estimator) Plots
<p align = "center">
<img src = "https://user-images.githubusercontent.com/60159655/89431711-1f60f180-d6f5-11ea-9e0e-da8b1dd52c72.png" />
</p>

These plots provide some **_key observations_**
- For employees who left, a trimodal disribution can be seen for the Satisfaction KDE Plot (peaks at 0.1, 0.4, and 0.8).
- For employees who left, a bimodal distribution can be seen for the Evaluation KDE Plot (peaks at 0.5 and 0.9).
- For employees who left, a bimodal distribution can be seen for the Average Monthly Hours KDE Plot (peaks at 150 hours and 250 hours, which mirror the historgram shape above).

### Plotting Cluster Groups

**Average Monthly Hours Cluster**
<p align = "center">
<img src = "https://user-images.githubusercontent.com/60159655/89436808-98634780-d6fb-11ea-9cb1-86a3bde752b7.png" />
</p>

**Evaluation Cluster**
<p align = "center">
<img src = "https://user-images.githubusercontent.com/60159655/89436879-b0d36200-d6fb-11ea-911f-4bd47e607182.png" />
</p>

From these clusters, there are three distinct groups of employees who left the company

**First Cluster - Low Satisfaction, High Evaluation:** These employees have very low satisfaction levels but are evaluated many times and work around 250 hours per month, on average.  These employees could be micromanaged by their superiors far more often than other employees.  In addition, they are clearly burned out from working too much.

**Second Cluster - Low/Mid Satisfaction, Low Evaluation:** These employees are evaluated at an average level, but they work 150 hours per month (which is below the company average).  The general feeling from management is that these employees have underperformed.  Therefore, management chooses not to train or develop them into better employees.  As a result, employees feel a lack of support and might want to leave.

**Third Cluster - High Satisfaction, High Evaluation:** These employees have evaluation ratings between 0.7 to 0.9 and work, on average, 250 hours.  Management could classify these group of employees as "superstars" since they are happy, work hard, and produce results.  However, employees might perform in such a manner in order to prepare for a better opportunity.  A promotion could be given within the company or they can look for a better opportunity at another company.

### Distribution of Turnover for Each Department
For each department, the percentage of employees who left versus those who stayed are relatively the same.  This shows that not one department experiences a greater percentage of turnover compared to other departments.

<p align = "center">
<img src = "https://user-images.githubusercontent.com/60159655/89440825-1544f000-d701-11ea-88d8-89eaa21d2825.png" />
</p>

### Correlation Matix
From the correlation matrix, `evaluation`, `project_count`, and `average_mothly_hours` are the only variables significantly correlated with one another.  From our analysis above, these are the variables that show clear distincton with `turnover`.  In addition, `satisfaction` is the most correlated variable with `turnover`.  

<p align = "center">
<img src = "https://user-images.githubusercontent.com/60159655/89443925-d5ccd280-d705-11ea-9268-a3d4a9ec63a7.png" />
</p>

## Modeling

### Data Preprocessing
Before we can create a representative model to predict turnover, we need to handle the imbalance in the target variable.  To create a balanced target variable, we utilize the following strategies:

1. Upsample the minority class
2. Upsample using SMOTE (Synthetic Minority Over-Sampling Techniques)
3. Downsample the majority class

Evaluating the F1 score using these three techniques with a 80/20 train-test split, **_upsampling using SMOTE_** provided the best score.  We will use the SMOTE sample for predicting how these models work on the test data. 
| Method | F1 Score |
| -------- | ----------- |
| Original Sample | 0.4451 |
| Upsample | 0.7843 |
| SMOTE | 0.8103
| Downsample | 0.7799 | 


### Model Evaluation
We decided to use a Logistic Regression, Random Forest Classifier, and Gradient Boosting Classifier to train our models.  These are typical models used for classification problems.  We used 10-fold cross validation to ensure reliable results.  Below is a table of our metrics:
<p align = "center">
<img src = "https://user-images.githubusercontent.com/60159655/89477068-257cbf80-d741-11ea-9857-0564b4fc81e4.png" />
</p>

**Observation:** Cross validation of the training set shows that Random Forest and Gradient Boosting achieve the best metric scores.  We will use these models for predicting turnover on the test data. 

### Model Evaluation for Test Data
The ROC/AUC curve for the three models using the test data is shown below:
<p align = "center">
<img src = "https://user-images.githubusercontent.com/60159655/89488736-9d58e300-d75d-11ea-8cdc-f458f3c2a9fd.png" />
</p>


## Recommendations for Employee Churn
To create a proper retention plan, the following suggestions should be implemented:

- **Monthly Income:** There was a good portion of employees paid in the low end of the spectrum.  Some employees might leave because they are not paid adequately compared to other positions.  The company should at least pay employees the industry average for their specific position.
- **Overtime:** There was a good portion of employees working at least 60+ hours per week (240+ hours per month).  Our analysis showed that employees who worked to much were not satisfied with their current job.  The company should do what they can do limit extra work by providing support and setting realistic expectations for what duties are required for each position.

Each "cluster" discovered in our analysis should have a retention strategy tailored to that group.  By taking a proactive approach, companies could potentially save millions of dollars, as well as numerous hours, to replace and train new employees.

## Conclusion
The goal of this project was to identify key factors that cause employees for a company.  For our particular dataset, employees typically left if they were not satisfied, not paid enough, or were not managed in an effective manner.  The company should do what they can to reduce employee turnover as much as possible in order to minimize costs.
