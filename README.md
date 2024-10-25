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

<br>

## Data Preparation

As part of getting the dataset ready for analysis, I took several steps to ensure that I could capture important trends and insights effectively. To do this, I created a Calendar Table and added custom columns to improve the segmentation and organization of the data.

<br>
 
1. **Creating a Calendar Table** :
   
- I created a calendar table to manage time-based filtering, which is crucial for tracking customer churn patterns over different periods. By incorporating fields like **year, month,** and **month name**, I ensured that I could slice and dice the data more flexibly and easily identify trends.

- The formula used:

``` Calendar Table
Calendar_Table = CALENDAR(FIRSTDATE(Bank_Churn[Bank DOJ]),LASTDATE(Bank_Churn[Bank DOJ]))
```

<br>

2. **Adding the 'Credit Type' Column** :

- Understanding customer creditworthiness is key to this analysis. I added a column to categorize customers based on their credit score into meaningful ranges: **"Excellent," "Very Good," "Good," "Fair,"** and **"Poor."** This allows me to examine how customers from different credit score categories behave when it comes to churn.

- The **DAX** code used:

``` Credit Type
CreditType = SWITCH(TRUE(),
   Bank_Churn[CreditScore] >= 800 && Bank_Churn[CreditScore] <= 850, "Excellent",
   Bank_Churn[CreditScore] >= 740 && Bank_Churn[CreditScore] <= 799, "Very Good",
   Bank_Churn[CreditScore] >= 670 && Bank_Churn[CreditScore] <= 739, "Good",
   Bank_Churn[CreditScore] >= 580 && Bank_Churn[CreditScore] <= 669, "Fair",
   Bank_Churn[CreditScore] >= 300 && Bank_Churn[CreditScore] <= 579, "Poor")
```  

<br>

3. **Adding the 'Age Group' Column** :

- Age plays an important role in customer behavior, so I created an additional column to group customers into three distinct categories: **"Young Adults," "Adults,"** and **"Senior Citizens."** This will help me analyze churn trends across different life stages, which could be valuable for targeted customer retention strategies.

- The **DAX** code used:

``` Age Group
AgeGroup = SWITCH(TRUE(),
   Bank_Churn[Age] >= 18 && Bank_Churn[Age] <= 35, "Young Adults",
   Bank_Churn[Age] >= 36 && Bank_Churn[Age] <= 55, "Adults",
   Bank_Churn[Age] >= 56 && Bank_Churn[Age] <= 92, "Senior Citizens",
   "Unknown")
```

<br>

## Analysis

### Calendar Table

The first thing I did while starting to analyze the data was creating a calendar table. The calendar table helped me in performing accurate and flexible time-based analysis. It allowed me to easily perform date-related calculations such as Year-to-Date (YTD), Month-to-Date (MTD), and Year-on-year (YOY), as well as compare data across different time periods.

First I created a table named Calendar table;

Calendar_Table = CALENDAR(FIRSTDATE(Bank_Churn[Bank DOJ]),LASTDATE(Bank_Churn[Bank DOJ]))

The formula generated a range of dates based on the earliest and latest dates found in the "car_data" table under the "Date" column in a new table.

Then I extracted the month, year and week from the designated column, using the following formulas:

--- for year
Year = YEAR(Calendar_Table[Date])

--- for month
Month = MONTH(Calendar_Table[Date])

--- for month name
Month Name = FORMAT(Calendar_Table[Date],"MMM")

<br>

A quick look at the Calendar Table:



To establish a relationship between the car_data table and the newly created Calendar_table, I connected the tables through the common field which is Date In the calendar table, each date appeared only once, denoted by the symbol '1', while in the car_data table some dates might have appeared multiple times, indicated by the symbol '*'. Which signified a **One-to-Many** relationship between the two tables.

### Added Column





The analysis was conducted using Power BI, and various KPI cards and visualizations were created to explore customer churn patterns.

<br>

### KPI Cards

I used **DAX measures** in Power BI to create these KPI cards, provides a clear picture of the bank's customer status and areas that need attention, such as re-engaging inactive customers and reducing churn.

<br>

1. **Total Customers** : (10,000)

   Measures the total number of customers. The bank has a broad customer base, offering a large opportunity for retention strategies.

``` dax
  Total Customers = COUNT(Bank_Churn[CustomerId])
```

<br>

2. **Active Customers** : (5,151)

   Measures customers who are still active. Around half of the total customers are actively engaged with the bank, indicating room for improvement in customer engagement.

``` dax
  Active Customers = CALCULATE(COUNT(Bank_Churn[CustomerId]),ActiveCustomer[ActiveCategory]="Active Member")
```

<br>

3. **Inactive Customers** : (4,849)

    Measures customers who have become inactive. Nearly half of the customer base is inactive, which poses a risk for churn if not re-engaged.

``` dax
  Inactive Customers = CALCULATE(COUNT(Bank_Churn[CustomerId]),ActiveCustomer[ActiveCategory]="Inactive Member")
```

<br>

4. **Credit Card Holders** : (7,055)

   Shows the number of customers who hold a credit card with the bank. A significant portion of customers use the bank’s credit card services, which could be leveraged to improve retention.

``` dax
  Credit Card Holders = CALCULATE(COUNT(Bank_Churn[CustomerId]),CreditCard[Category]="credit card holder")
```

<br>

5. **Non-Credit Card Holders** : (2,945)

   Displays the number of customers who do not have a credit card. There is an opportunity to target non-credit card holders with incentives to boost engagement and potentially reduce churn.
  
``` dax
  Non-Credit Card Holders = CALCULATE(COUNT(Bank_Churn[CustomerId]),CreditCard[Category]="non credit card holder")
```

<br>

6. **Exit Customers** : (2,037)

   This card shows the number of customers who have exited or churned. The churn rate is substantial, with over 20% of the customer base leaving, highlighting the need for retention-focused strategies.

``` dax
  Exit Customers = CALCULATE([Total Customers],ExitCustomer[ExitCategory]="Exit")
```

<br>

7. **Retained Customers** : (7,963)

   Displays the number of customers who have stayed with the bank and not churned. The majority of customers are retained, which is a positive sign, but continued efforts are necessary to prevent future churn.
  
``` dax
  Retained Customers = CALCULATE([Total Customers],ExitCustomer[ExitCategory]="Retain")
```

<br>

### Visualizations

Clustered Column Chart (Total Customers by Year and Active Category): Displays customer distribution across years, comparing active vs inactive customers.

Line Chart (Exit Customers by Month): Shows the monthly trend of customers exiting the bank.

Clustered Bar Chart (Exit Customers by Credit Type): Compares churn rates between credit card holders and non-holders.

Donut Chart (Exit Customers by Gender): Displays the proportion of male and female customers who have exited the bank.

Pie Chart (Customer Categories): Breaks down customers into different categories (active, inactive, churned, etc.).

Matrix (Churn Percentage by Year and Month): Shows churn rates over time, allowing for a detailed view of churn trends by specific periods.

<br>

### Slicers

- **Year** : Filters the data to display information for a selected year or across multiple years.
  
- **Month Name** : Allows you to view data for a specific month, analyzing trends or churn patterns on a monthly basis.
  
- **Geography Location** : Filters the data based on the geographic region of the customers, helping analyze regional churn behavior.
  
- **Active Category** : Filters by customer activity status (active or inactive) to focus on specific groups of customers.
  
- **Exit Category** : Filters the data to show only the customers who have exited (churned) or retained.
  
- **Gender Category** : Enables analysis of churn trends by filtering the data based on customer gender (male or female).

  <br>

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
