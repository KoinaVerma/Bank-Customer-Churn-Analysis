# Bank Customer Churn Analysis

Customer churn is a significant challenge for financial institutions, directly affecting revenue and profitability. This project explores the customer churn problem for a fictional bank using Power BI. By analyzing factors that contribute to customers leaving the bank, we aim to identify trends and potential interventions to reduce churn.

## Table of Content

## Project Overview

Objective

The goal of this analysis is to identify key drivers of customer churn and to develop actionable insights that help the bank improve customer retention. By understanding the profile and behavior of customers who are more likely to churn, the bank can focus its efforts on retaining high-value customers and reducing churn.

Problem Statement

This project addresses the following key questions:

What is the overall churn rate of the bank?
Which customer segments (age, gender, geography) are more likely to churn?
What role do credit scores, tenure, and salary play in customer churn?
How can the bank better predict and prevent customer churn?
Data Source
The dataset used for this analysis is downloaded from Kaggle, which provides various fictitious datasets for data projects. The dataset includes the following features:

CustomerID
Geography
Gender
Age
Tenure
CreditScore
Balance
Salary
Active/Inactive status
Credit Card holding status
Churn status (Exit/Retained)

## Data Cleaning

Before starting the analysis, the following data cleaning steps were applied:

Handling Missing Values: Checked for any missing or null values in the dataset and replaced them appropriately.
Formatting Data Types: Ensured all columns had correct data types for analysis (e.g., numerical, categorical).
Creation of Date Table: Created a DateMaster table to manage time-based analyses.
Feature Engineering: Added calculated columns like ChurnPercentage to enhance analysis.

## Analysis

The analysis was conducted using Power BI, and various visualizations were created to explore customer churn patterns.

### KPI Cards

Total Customers: Measures the total number of customers.
M code: CALCULATE(COUNTROWS(Customer))

Active Customers: Measures customers who are still active.
M code: CALCULATE(COUNTROWS(Customer), Customer[Active] = "Yes")

Inactive Customers: Measures customers who have become inactive.
M code: CALCULATE(COUNTROWS(Customer), Customer[Active] = "No")

Credit Card Holders: Measures the number of customers with a credit card.
M code: CALCULATE(COUNTROWS(Customer), Customer[CreditCard] = "Yes")

Non-Credit Card Holders: Measures customers without a credit card.
M code: CALCULATE(COUNTROWS(Customer), Customer[CreditCard] = "No")

Exit Customers: Measures customers who have exited.
M code: CALCULATE(COUNTROWS(Customer), Customer[ChurnStatus] = "Exited")

Retained Customers: Measures customers who have not churned.
M code: CALCULATE(COUNTROWS(Customer), Customer[ChurnStatus] = "Retained")

### Visualizations

Clustered Column Chart (Total Customers by Year and Active Category): Displays customer distribution across years, comparing active vs inactive customers.

Line Chart (Exit Customers by Month): Shows the monthly trend of customers exiting the bank.

Clustered Bar Chart (Exit Customers by Credit Type): Compares churn rates between credit card holders and non-holders.

Donut Chart (Exit Customers by Gender): Displays the proportion of male and female customers who have exited the bank.

Pie Chart (Customer Categories): Breaks down customers into different categories (active, inactive, churned, etc.).

Matrix (Churn Percentage by Year and Month): Shows churn rates over time, allowing for a detailed view of churn trends by specific periods.

## Insights

Total Customers: The bank has a large customer base, but a significant portion is inactive.

Active vs Inactive Customers: There is a notable number of inactive customers, indicating potential issues with customer engagement.

Credit Card Holders: Credit card holders are less likely to churn, suggesting the importance of offering credit products to retain customers.

Churn Trend: The line chart reveals that customer churn spikes at certain times of the year, indicating possible seasonal effects or external factors.

Gender Analysis: Male and female customers churn at different rates, with males having a slightly higher churn rate in this dataset.

Churn by Credit Type: Non-credit card holders are more likely to churn, possibly due to fewer benefits or engagement with the bank’s services.

## Recommendations

Enhance Customer Engagement: The bank should focus on re-engaging inactive customers through personalized offers, rewards, or additional services, such as credit cards or loans, to increase their interaction with the bank.

Credit Card Incentives: Given that credit card holders are less likely to churn, offering targeted incentives for customers without credit cards may help improve retention.

Targeted Retention Programs: Create specific retention programs for high-risk segments such as non-credit card holders and inactive customers, particularly during periods where churn spikes are observed.

Improve Churn Prediction: Implement machine learning models to predict high-risk customers based on credit score, tenure, and salary to take proactive measures.

Address Gender Disparity: Further analysis is needed to understand why churn rates differ between genders, and the bank should explore tailored engagement strategies for each group.

## Conclusion

This project provides an in-depth analysis of customer churn within a bank and highlights key areas where improvements can be made to increase customer retention. By leveraging data insights and implementing targeted interventions, the bank can reduce churn and improve overall profitability.
