# Bank Customer Churn Analysis

Customer churn is a significant challenge for financial institutions, directly affecting revenue and profitability. This project explores the customer churn problem for a fictional bank using **Power BI**. By analyzing factors that contribute to customers leaving the bank, I aim to identify trends and potential interventions to **reduce churn**.

<br>

## TABLE OF CONTENT

- [Project Overview](#project_overview)
- [Data Source](#data_source)
- [Data Cleaning](#data_cleaning)
- [Data Preparation](#data_preparation)
- [Analysis](#analysis)
- [Insights](#insights)
- [Recommendations](#recommendations)
- [Conclusion](#conclusion)

<br>
  
## PROJECT OVERVIEW

The goal of this analysis is to **identify key drivers of customer churn** and to **develop actionable insights** that help the bank improve customer retention. By understanding the profile and behavior of customers who are more likely to churn, the bank can focus its efforts on **retaining high-value customers** and **reducing churn**.

<br>

## DATA SOURCE

The data was sourced from **Kaggle.com**, which offers a range of fictitious datasets for data projects. The dataset used for this analysis is in **CSV format** and consists of **14 columns** and **10,000 rows**. It contains the following columns:

<br>

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

## DATA CLEANING

After importing the data into Power BI, I first verified that all **data types** were correct. Using the column distribution and column quality features in the Power Query Editor, I checked for **duplicates** and **empty** values and found none. Additionally, I removed the 'RowNumber' column, as it was a sequential identifier not relevant to the analysis.


<br>

## DATA PREPARATION

As part of getting the dataset ready for analysis, I took several steps to ensure that I could capture important trends and insights effectively. To do this, I created a Calendar Table and added custom columns to improve the segmentation and organization of the data.

<br>
 
### 1. **Creating a Calendar Table** :
   
- First I created a calendar table to manage time-based filtering, which is crucial for tracking customer churn patterns over different periods. By incorporating fields like **year, month,** and **month name**, I ensured that I could slice and dice the data more flexibly and easily identify trends.

- The formula used:

``` Calendar Table
  Calendar_Table = CALENDAR(FIRSTDATE(Bank_Churn[Bank DOJ]),LASTDATE(Bank_Churn[Bank DOJ]))
```

<br>

- The formula generated a range of dates based on the earliest and latest dates found in the "car_data" table under the "Date" column in a new table.

- Then I extracted the month, year and week from the designated column, using the following formulas:

  --- for year
  Year = YEAR(Calendar_Table[Date])

  --- for month
  Month = MONTH(Calendar_Table[Date])

  --- for month name
  Month Name = FORMAT(Calendar_Table[Date],"MMM")

<br>

   **A quick look at the Calendar Table:**

<br>

- To establish a relationship between the car_data table and the newly created Calendar_table, I connected the tables through the common field which is Date In the calendar table, each date appeared only once, denoted by the symbol '1', while in the car_data table some dates might have appeared multiple times, indicated by the symbol '*'. Which signified a **One-to-Many** relationship between the two tables.

<br>

### 2. **Adding the 'Credit Type' Column** :

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

### 3. **Adding the 'Age Group' Column** :

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

## ANALYSIS

The analysis was conducted using Power BI, and various KPI cards and visualizations were created to explore customer churn patterns.

<br>

### DASHBOARD 1

#### 1. KPI Cards

I used **DAX measures** in Power BI to create these KPI cards, provides a clear picture of the bank's customer status and areas that need attention, such as re-engaging inactive customers and reducing churn.

<br>

- **Total Customers** : (10,000)

   Measures the total number of customers. The bank has a broad customer base, offering a large opportunity for retention strategies.

``` dax
  Total Customers = COUNT(Bank_Churn[CustomerId])
```

<br>

- **Active Customers** : (5,151)

   Measures customers who are still active. Around half of the total customers are actively engaged with the bank, indicating room for improvement in customer engagement.

``` dax
  Active Customers = CALCULATE(COUNT(Bank_Churn[CustomerId]),ActiveCustomer[ActiveCategory]="Active Member")
```

<br>

- **Inactive Customers** : (4,849)

    Measures customers who have become inactive. Nearly half of the customer base is inactive, which poses a risk for churn if not re-engaged.

``` dax
  Inactive Customers = CALCULATE(COUNT(Bank_Churn[CustomerId]),ActiveCustomer[ActiveCategory]="Inactive Member")
```

<br>

- **Credit Card Holders** : (7,055)

   Shows the number of customers who hold a credit card with the bank. A significant portion of customers use the bank’s credit card services, which could be leveraged to improve retention.

``` dax
  Credit Card Holders = CALCULATE(COUNT(Bank_Churn[CustomerId]),CreditCard[Category]="credit card holder")
```

<br>

- **Non-Credit Card Holders** : (2,945)

   Displays the number of customers who do not have a credit card. There is an opportunity to target non-credit card holders with incentives to boost engagement and potentially reduce churn.
  
``` dax
  Non-Credit Card Holders = CALCULATE(COUNT(Bank_Churn[CustomerId]),CreditCard[Category]="non credit card holder")
```

<br>

- **Exit Customers** : (2,037)

   This card shows the number of customers who have exited or churned. The churn rate is substantial, with over 20% of the customer base leaving, highlighting the need for retention-focused strategies.

``` dax
  Exit Customers = CALCULATE([Total Customers],ExitCustomer[ExitCategory]="Exit")
```

<br>

- **Retained Customers** : (7,963)

   Displays the number of customers who have stayed with the bank and not churned. The majority of customers are retained, which is a positive sign, but continued efforts are necessary to prevent future churn.
  
``` dax
  Retained Customers = CALCULATE([Total Customers],ExitCustomer[ExitCategory]="Retain")
```

<br>

**2. Annual Distribution of Active and Inactive Customers**: A **column chart** that displays the yearly breakdown of active vs. inactive customers, highlighting engagement trends over time.

**3. Monthly Exit Customers and Prior Month Comparison**: A **line chart** comparing monthly churn with the previous month’s values, allowing identification of seasonal or monthly churn spikes.

**4. Exit Customers by Gender**: A **donut chart** showing churn rates for males and females, revealing gender-based churn differences.

**5. Exit Customers by Credit Type**: A **bar chart** detailing churn by credit score categories, illustrating churn likelihood across different creditworthiness levels.

**6. Exit Customers by Credit Card Status**: A **pie chart** showing churn rates for credit card holders vs. non credit card holders, indicating a higher churn rate among card holders.

#### 7. Slicers

- **Year** : Filters the data to display information for a selected year or across multiple years.
  
- **Month Name** : Allows you to view data for a specific month, analyzing trends or churn patterns on a monthly basis.
  
- **Geography Location** : Filters the data based on the geographic region of the customers, helping analyze regional churn behavior.
  
- **Active Category** : Filters by customer activity status (active or inactive) to focus on specific groups of customers.
  
- **Exit Category** : Filters the data to show only the customers who have exited (churned) or retained.
  
- **Gender Category** : Enables analysis of churn trends by filtering the data based on customer gender (male or female).

  <br>

### DASHBOARD 2

**1. Monthly Churn Rates Across Years**: A **heatmap** providing a month-by-month view of churn rates across years, highlighting periods of high churn.

**2. Churn % by Geography Location**: A **map** visual displaying churn percentages across different regions, identifying geographic areas with higher churn rates.

**3. Credit Score, Churn Rate, and Tenure Correlation**: A **scatter plot** that shows the relationship between credit score, churn rate, and tenure, revealing insights on how creditworthiness and tenure impact churn.

**4. Churn % by Age Group**: A **pie chart** categorizing churn by age group, indicating which age groups are more likely to churn.

**5. Churn % by Tenure (years)**: A **line chart** showing the churn rate over different tenure lengths, with specific tenure ranges experiencing higher churn.

**6. Churned and Retained Customers by Age Group**: A **bar chart** comparing churned and retained customers across different age groups, helping visualize which groups have higher retention.

<br>

## INSIGHTS

**1. Customer Engagement**: Customer Engagement: Nearly half of the customer base **(4,849 out of 10,000)** is **inactive**, as shown in the KPI cards. This presents a risk for churn if engagement strategies aren’t implemented to re-engage inactive customers.

**2. Credit Card Impact**: The Exit Customers by Credit Card Status pie chart indicates that **credit card holders** have a higher churn rate (**69.91%** or 1,424 customers) compared to non-credit card holders (30.09% or 613 customers). This suggests that current credit card offerings may not provide sufficient value to retain customers.

**3. Churn Trends**: The Monthly Exit Customers and Prior Month Comparison line chart shows seasonal churn patterns, with significant spikes in **November (307 exits)** and **December (283 exits)**. These peaks could indicate external or seasonal factors affecting churn rates.

**4. Age-Based Churn**: The Churn % by Age Group pie chart highlights that "Senior Citizens" (ages 56-92) have the highest churn rate at 50.5%, followed by "Adults" (ages 36-55) at 38.01%, while "Young Adults" (ages 18-35) show the lowest churn rate at 11.48%. This insight suggests a need for targeted retention efforts aimed at **Senior Citizen** customers.

**5. Geographic Variations**: The Churn % by Geography Location map shows that certain regions have higher churn rates, with **Germany (32.44%)** being the highest. This suggests that regional factors, possibly economic or competitive pressures, may influence churn.

**6. Creditworthiness Influence**: As seen in the Exit Customers by Credit Type bar chart, customers with lower credit scores (classified as **"Fair" and "Poor"**) exhibit higher churn rates, with **685** and **520** exits, respectively. This underscores the importance of providing tailored support or services for lower-score customers to improve retention.

<br>

## RECOMMENDATIONS

**1. Increase Engagement for Inactive Customers**: Design targeted campaigns to re-engage the **4,849 inactive customers**, such as personalized offers, rewards, or exclusive services.

**2. Enhance Credit Card Value Proposition**: Given that credit card holders have a higher churn rate, the bank should review its credit card offerings and consider **adding more benefits** or **incentives** to encourage loyalty.

**3. Implement Seasonal Retention Campaigns**: To address seasonal churn peaks observed in **November** and **December**, consider implementing proactive retention campaigns in these months, such as **limited-time offers** or **enhanced customer support**.

**4. Focus on Retaining Senior Citizens**: With Senior Citizens showing the highest churn rate at **50.5%**, develop specific retention strategies tailored to this age group. Consider offering services that cater to their needs, such as **enhanced customer support, financial planning resources,** or **loyalty programs** specifically designed for long-term customers.

**5. Regional Focus on High-Churn Areas**: For regions with higher churn, such as **Germany**, conduct a deeper analysis to **understand regional issues** and **implement localized retention strategies**.

**6. Support for Low Credit Score Customers**: Provide **financial wellness programs** or **credit-building services** targeted at customers with lower credit scores (those classified as **"Fair"** or **"Poor"**) to improve their financial health and foster loyalty.

<br>

## CONCLUSION

This Bank Customer Churn Analysis provides valuable insights into customer behavior, identifying key factors influencing churn such as **credit card ownership, regional variations, age group,** and **credit score**. By leveraging these insights, the bank can implement targeted retention strategies to improve customer engagement, particularly among inactive customers, senior citizens, and high-churn regions like Germany. Additionally, enhancing the value proposition for credit card holders and offering tailored support for lower credit score customers can further reduce churn. Through data-driven interventions, the bank can effectively improve customer retention, reduce revenue loss, and increase long-term profitability.
