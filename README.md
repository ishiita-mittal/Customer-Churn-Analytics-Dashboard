# 📊 Customer Churn Analytics Dashboard

## 📌 Project Overview

The **Customer Churn Analytics Dashboard** is an interactive Power BI
business intelligence project designed to analyze customer churn,
retention, revenue performance, and customer value.

The dashboard transforms customer, subscription, transaction, plan, and
date data into actionable insights that can help stakeholders identify
churn patterns and prioritize retention activities.

------------------------------------------------------------------------

## 🎯 Business Problem

Customer churn can directly affect recurring revenue and long-term
customer value. Businesses need a way to understand:

-   How many customers have churned?
-   What is the overall churn rate?
-   Which plans and cities have higher churn?
-   How does customer retention change across cohorts?
-   Which customers are valuable, loyal, or at risk?
-   How is revenue performing over time?
-   How can retention targets be compared with current performance?

This dashboard was created to answer these questions through interactive
Power BI analysis.

------------------------------------------------------------------------

## 🎯 Project Objectives

1.  Measure overall customer churn.
2.  Analyze churn across subscription plans, cities, and gender.
3.  Track revenue trends over time.
4.  Analyze cohort-based customer retention.
5.  Segment customers using RFM analysis.
6.  Implement time-intelligence KPIs.
7.  Provide customer-level drill-through analysis.
8.  Compare retention targets using a What-if parameter.
9.  Translate analytical findings into retention recommendations.

------------------------------------------------------------------------

## 🛠️ Tech Stack

  Technology         Purpose
  ------------------ --------------------------------------------------------------
  Power BI Desktop   Dashboard development and visualization
  DAX                Measures, KPIs, retention and time-intelligence calculations
  Power Query        Data loading and transformation
  Excel              Source dataset
  Git / GitHub       Version control and project documentation
  VS Code            Project and Git management

------------------------------------------------------------------------

## 🏗️ Data Model

The project uses a **star-schema-oriented data model** with fact and
dimension tables.

### Dimension Tables

-   `DimCustomer` --- customer attributes such as CustomerID, name,
    gender, city and signup date.
-   `DimPlan` --- subscription plan information such as PlanID, plan
    name and price.
-   `DimDate` --- calendar/date table used for time-based analysis.

### Fact Tables

-   `FactSubscription` --- subscription status, start date, end date,
    customer and plan information.
-   `FactTransaction` --- customer transactions, transaction dates and
    revenue amounts.

### Model Concept

``` text
                 DimCustomer
                      |
                      |
DimPlan ---- FactTransaction ---- DimDate
                      |
                      |
              FactSubscription
```

The model separates descriptive dimensions from business-event fact
data, making the report easier to filter and analyze.

------------------------------------------------------------------------

## 📋 Dataset

The project uses a synthetic customer analytics dataset.

  Table                Approx. Records Purpose
  ------------------ ----------------- -----------------------------------
  DimCustomer                      500 Customer master data
  DimPlan                            4 Subscription plan information
  DimDate                          731 Calendar dates from 2024--2025
  FactSubscription                 500 Customer subscription information
  FactTransaction                6,933 Transaction and revenue data

### Main Business Fields

**Customer** - CustomerID - CustomerName - Gender - City - SignupDate

**Subscription** - CustomerID - PlanID - StartDate - EndDate - Status

**Transaction** - TransactionID - CustomerID - PlanID -
TransactionDate - Amount

------------------------------------------------------------------------

## 🔄 Data Preparation

The source data was loaded into Power BI and organized into separate
customer, plan, date, subscription, and transaction tables.

Additional analytical fields were created for:

-   Cohort month
-   Transaction month
-   Cohort period
-   Last purchase date
-   Recency
-   Frequency
-   Monetary value
-   RFM scores
-   RFM customer segment

The `DimDate` table was configured as the model's date table for
time-intelligence calculations.

------------------------------------------------------------------------

## 📐 Key DAX Measures

### Total Customers

``` dax
Total Customers =
DISTINCTCOUNT(DimCustomer[CustomerID])
```

### Churned Customers

``` dax
Churned Customers =
CALCULATE(
    DISTINCTCOUNT(FactSubscription[CustomerID]),
    FactSubscription[Status] = "Churned"
)
```

### Churn Rate

``` dax
Churn Rate =
DIVIDE(
    [Churned Customers],
    [Total Customers],
    0
)
```

### Total Revenue

``` dax
Total Revenue =
SUM(FactTransaction[Amount])
```

### Retention Rate

``` dax
Retention Rate =
DIVIDE([Cohort Customers], [Cohort Size], 0)
```

### Rolling 12-Month Churn Rate

``` dax
Rolling 12M Churn Rate =
VAR RollingChurn =
    CALCULATE(
        [Churned Customers],
        USERELATIONSHIP(
            DimDate[Date],
            FactSubscription[EndDate]
        ),
        DATESINPERIOD(
            DimDate[Date],
            MAX(DimDate[Date]),
            -12,
            MONTH
        )
    )
RETURN
    DIVIDE(
        RollingChurn,
        [Total Customers],
        0
    )
```

The rolling churn calculation uses the trailing 12-month date window and
the subscription end-date relationship.

------------------------------------------------------------------------

## 📊 Customer Segmentation --- RFM

RFM stands for:

-   **Recency** --- how recently the customer purchased.
-   **Frequency** --- how often the customer purchased.
-   **Monetary** --- how much the customer spent.

Customers are scored on these dimensions and grouped into segments such
as:

-   **Champions**
-   **Loyal Customers**
-   **Potential Loyal**
-   **At Risk / Lost**

This helps prioritize customers for retention and re-engagement
strategies.

------------------------------------------------------------------------

## 📈 Cohort Retention Analysis

Customers are grouped according to their signup month.

The dashboard then tracks how many customers from each cohort remain
active across subsequent periods.

This helps identify:

-   Strong and weak customer cohorts
-   Retention drop-off periods
-   Changes in customer engagement over time

------------------------------------------------------------------------

## 🎛️ What-if Analysis

A **Target Retention** What-if parameter allows stakeholders to select a
desired retention target.

The dashboard dynamically calculates the corresponding target churn
rate:

``` dax
Target Churn Rate =
1 - DIVIDE([Target Retention Value], 100)
```

For example:

> If the target retention is 92%, the corresponding target churn rate is
> 8%.

------------------------------------------------------------------------

## 🔎 Drill-through Analysis

The report includes a customer-level drill-through page.

Users can select a customer from the main dashboard and navigate to
detailed information including:

-   Customer ID
-   Customer name
-   Gender
-   City
-   Cohort month
-   Last purchase date
-   Purchase frequency
-   Monetary value
-   RFM segment

This allows stakeholders to move from high-level analysis to individual
customer investigation.

------------------------------------------------------------------------

## 📊 Dashboard Pages

### Page 1 --- Overview

Contains:

-   Total Customers
-   Churned Customers
-   Churn Rate
-   Total Revenue
-   Monthly Revenue Trend
-   Churn by Plan
-   Churn by City
-   Churn by Gender
-   Interactive slicers

### Page 2 --- Retention & Segmentation

Contains:

-   Cohort Retention Matrix
-   RFM Customer Segmentation
-   Target Retention What-if parameter
-   Target Churn Rate
-   MTD Revenue
-   YTD Revenue
-   Rolling 12-Month Revenue
-   Rolling 12-Month Churn Rate Trend

### Page 3 --- Customer Detail

Contains:

-   Customer-level drill-through table
-   RFM information
-   Purchase behavior
-   Retention recommendations

------------------------------------------------------------------------

## 📌 Current Dashboard KPIs

Based on the current synthetic dataset:

  KPI                                  Value
  ------------------------------- ----------
  Total Customers                        500
  Churned Customers                      134
  Churn Rate                           26.8%
  Latest YTD Revenue                \~₹4.20M
  Latest Rolling 12M Churn Rate        21.4%

------------------------------------------------------------------------

## 💡 Retention Recommendations

The dashboard translates analysis into practical actions:

-   Prioritize **At Risk / Lost** customers for re-engagement.
-   Focus retention campaigns on higher-churn subscription plans.
-   Use cohort trends to identify periods of significant customer
    drop-off.
-   Provide targeted onboarding, benefits, or incentives to customers
    showing disengagement.
-   Compare actual churn performance against the selected retention
    target.

------------------------------------------------------------------------

## 📁 Project Structure

``` text
Customer-Churn-Analytics-Dashboard/
│
├── Customer Churn Analytics Dashboard.pbip
├── Customer Churn Analytics Dashboard.Report/
├── Customer Churn Analytics Dashboard.SemanticModel/
├── Customer Churn Analytics Dashboard.pbix
├── .gitignore
└── README.md
```

------------------------------------------------------------------------

## 🚀 How to Run

1.  Clone or download this repository.
2.  Install **Power BI Desktop**.
3.  Open the `.pbip` project in Power BI Desktop.
4.  Ensure the source Excel dataset is available if the model requires a
    refresh.
5.  Refresh the data if required.
6.  Explore the three dashboard pages and interact with the slicers,
    What-if parameter, and drill-through functionality.

> The `.pbix` file is retained as a backup copy of the report.

------------------------------------------------------------------------

## 🔍 Key Skills Demonstrated

-   Power BI dashboard development
-   Star schema data modeling
-   Power Query data preparation
-   DAX measures
-   Filter context and CALCULATE
-   Time intelligence
-   Cohort retention analysis
-   RFM customer segmentation
-   What-if parameters
-   Drill-through reports
-   KPI development
-   Business insight generation
-   Git and GitHub project management

------------------------------------------------------------------------

## 👩‍💻 Author

**Ishita Mittal**

MCA --- Data Analytics / Technology

GitHub: `ishiita-mittal`

------------------------------------------------------------------------

## 📌 Project Status

**Portfolio / Academic Project**

The project is designed as an end-to-end Power BI analytics prototype
using a synthetic customer dataset.
