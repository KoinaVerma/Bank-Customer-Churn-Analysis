# Bank Customer Churn Analysis

Customer churn is a significant challenge for financial institutions, directly affecting revenue and profitability. This project explores the customer churn problem for a fictional bank using Power BI. By analyzing factors that contribute to customers leaving the bank, I aim to identify trends and potential interventions to reduce churn.

## Table of Content

## Project Overview

### Objective

The goal of this analysis is to identify key drivers of customer churn and to develop actionable insights that help the bank improve customer retention. By understanding the profile and behavior of customers who are more likely to churn, the bank can focus its efforts on retaining high-value customers and reducing churn.

## Data Source

The dataset used for this analysis is downloaded from Kaggle.com, which provides various fictitious datasets for data projects. The dataset includes the following columns:

**CustomerId** : Unique identifier for each customer.

**CreditScore** : Credit score of the customer, indicating creditworthiness.

**GeographyID** : Geographic region the customer belongs to, represented by a numeric code.

**GenderID** : Gender of the customer, represented as a numeric code (1 for male, 2 for female).

**Age** : Age of the customer.

**Tenure** : Number of years the customer has been with the bank.

**Balance** : Account balance of the customer.

**NumOfProducts** : Number of products the customer holds with the bank (e.g., loans, savings accounts).

**HasCrCard** : Whether the customer owns a credit card (1 for yes, 0 for no).

**IsActiveMember** : Whether the customer is an active member of the bank (1 for active, 0 for inactive).

**EstimatedSalary** : Estimated annual salary of the customer.

**Exited** : Whether the customer has churned (1 for yes, 0 for no).

**Bank DOJ** : The date the customer joined the bank.

<br>

## Data Cleaning

Before starting the analysis, the following data cleaning steps were applied:

Handling Missing Values: Checked for any missing or null values in the dataset and replaced them appropriately.
Formatting Data Types: Ensured all columns had correct data types for analysis (e.g., numerical, categorical).
Creation of Date Table: Created a DateMaster table to manage time-based analyses.
Feature Engineering: Added calculated columns like ChurnPercentage to enhance analysis.

## Analysis

The analysis was conducted using Power BI, and various KPI cards and visualizations were created to explore customer churn patterns.

<br>

### KPI Cards

I used **DAX measures** in Power BI to create these KPI cards, provides a clear picture of the bank's customer status and areas that need attention, such as re-engaging inactive customers and reducing churn.

<br>

1. **Total Customers** : (10,000)

- Measures the total number of customers. The bank has a broad customer base, offering a large opportunity for retention strategies.

``` dax
  Total Customers = CALCULATE(COUNTROWS(Customer))
```

<br>

2. **Active Customers** : (5,151)

- Measures customers who are still active. Around half of the total customers are actively engaged with the bank, indicating room for improvement in customer engagement.

``` dax
Active Customers = CALCULATE(COUNTROWS(Customer), Customer[Active] = "Yes")
```

<br>

3. **Inactive Customers** : (4,849)

    Measures customers who have become inactive. Nearly half of the customer base is inactive, which poses a risk for churn if not re-engaged.

``` dax
Inactive Customers = CALCULATE(COUNTROWS(Customer), Customer[Active] = "No")
```

<br>

4. **Credit Card Holders** : (7,055)

   Shows the number of customers who hold a credit card with the bank. A significant portion of customers use the bank’s credit card services, which could be leveraged to improve retention.

``` dax
Credit Card Holders = CALCULATE(COUNTROWS(Customer), Customer[CreditCard] = "Yes")
```

<br>

5. **Non-Credit Card Holders** : (2,945)

   Displays the number of customers who do not have a credit card. There is an opportunity to target non-credit card holders with incentives to boost engagement and potentially reduce churn.
  
``` dax
Non-Credit Card Holders = CALCULATE(COUNTROWS(Customer), Customer[CreditCard] = "No")
```

<br>

6. **Exit Customers** : (2,037)

   This card shows the number of customers who have exited or churned. The churn rate is substantial, with over 20% of the customer base leaving, highlighting the need for retention-focused strategies.

``` dax
Exit Customers = CALCULATE(COUNTROWS(Customer), Customer[ChurnStatus] = "Exited")
```

<br>

7. **Retained Customers** : (7,963)

   Displays the number of customers who have stayed with the bank and not churned. The majority of customers are retained, which is a positive sign, but continued efforts are necessary to prevent future churn.
  
``` dax
Retained Customers = CALCULATE(COUNTROWS(Customer), Customer[ChurnStatus] = "Retained")
```

<br>

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
