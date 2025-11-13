# Ecommerce Web Performance & Purchase Behavior Analysis | SQL, BigQuery
![Image](https://github.com/user-attachments/assets/87a187d0-9f07-4941-ab65-62e06929886b)

 _Analyze the performance of an e-commerce website, focusing on traffic, engagement, and revenue over time | SQL_

**Author:** Nguyễn Duy Kiên  

**Tools Used:** SQL

---

## 📑 Table of Contents  
1. [📌 Background & Overview](#-background--overview)  
2. [📂 Dataset Description & Data Structure](#-dataset-description--data-structure)
4. [🔎 Final Conclusion & Recommendations](#-final-conclusion--recommendations)

---

## 📌 Background & Overview  

### Objective:
### 📖 What is this project about? What Business Question will it solve?

🎯 Main Business Question

How can the business optimize website performance and user conversion by analyzing visitor behavior, traffic sources, and product-level purchase patterns?

📘 Project Overview

- This project analyzes website traffic and e-commerce performance data over several months in 2017.
- It focuses on understanding user behavior, traffic source effectiveness, and conversion performance at multiple levels (session, user, and product).
- The project uses SQL queries to extract insights from web analytics data such as visits, pageviews, bounce rates, transactions, and revenue.

💡 Business Questions this project answers

- ✔️ How does website performance change across months (visits, pageviews, and transactions)?
- ✔️ Which traffic sources bring the most valuable visitors and have the lowest bounce rates?
- ✔️ How does revenue vary by traffic source and over time (weekly and monthly trends)?
- ✔️ How do purchasers behave differently from non-purchasers in terms of engagement (pageviews, transactions, and spending)?
- ✔️ What is the average value per session and per customer, helping to measure marketing ROI?
- ✔️ Which products are commonly purchased together, revealing potential cross-sell opportunities?
- ✔️ What is the conversion funnel from product view → add-to-cart → purchase, and how efficient is it for each product?

### 👤 Who is this project for?  

- ✔️ Digital Marketing Teams – to evaluate traffic sources, campaign performance, and bounce rate.
- ✔️ E-commerce Managers – to monitor key metrics like transactions, revenue, and conversion rates.
- ✔️ Product Managers – to identify top-performing and underperforming products and potential bundling opportunities.
- ✔️ Data Analysts – to build dashboards or performance reports using SQL and analytics data.
- ✔️ Business Decision Makers / Executives – to make data-driven decisions about marketing spend, website optimization, and sales strategy.

---

## 📂 Dataset Description & Data Structure  

### 📌 Data Source  

Data schema: [Click to view the dataset schema](https://support.google.com/analytics/answer/3437719?hl=en)

### 📊 How to access the data

1. Log in to your **Google Cloud Platform** account and create a new project.
2. Open the **BigQuery Console** and select your project.
3. Click on **"Add Data"** in the navigation panel, then choose **"Search a project"**.
4. In the search bar, enter the project ID: **bigquery-public-data.google_analytics_sample.ga_sessions and press Enter**.
5. Click on the **ga_sessions_** table to explore its structure and data.

---

## ⚒️ Data Processing
### 🔍 Task 1: Calculate total visit, pageview, transaction for Jan, Feb and March 2017 (order by month)

#### Purpose: 
The goal of this analysis is to calculate the total number of visits, pageviews, transactions, and revenue for each month (January, February, and March) in 2017, based on the data provided. The results will be ordered by month to observe the trends and variations in website performance over these three months.

#### Query:

```sql
SELECT
  FORMAT_DATE('%Y%m', PARSE_DATE('%Y%m%d', date)) AS month,
  SUM(totals.visits) AS total_visit,
  SUM(totals.pageviews) AS total_pageview,
  SUM(totals.transactions) AS total_transaction
FROM `bigquery-public-data.google_analytics_sample.ga_sessions_2017*`
WHERE _table_suffix BETWEEN '0101' AND '0331'
GROUP BY month
ORDER BY month;
```

#### Query Result:
| month	   | visits  	| pageviews   |	transactions |
|:---------|---------:|------------:|-------------:|
| 201701   |	   64694 |	      64691 |         	697 |
| 201702   |	   62192 |	      62185 |         	708 |
| 201703   |   	69931 |       69927	|          883 |

#### Insight:
There is a positive trend from February to March in all three metrics. February appears to be the weakest month in terms of site visits, which could be worth investigating for potential causes.

---

## 🔎 Final Conclusion & Recommendations  

👉🏻 Based on the insights and findings above, we would recommend the [stakeholder team] to consider the following:  

📍 Key Takeaways:  
✔️ Recommendation 1  
✔️ Recommendation 2  
✔️ Recommendation 3 There is a positive trend from February to March in all three metrics. February appears to be the weakest month in terms of site visits, which could be worth investigating for potential causes.

**_📌Remember to summarize the most core insights/ observations you extract from the entire projects. 
 Recap ONLY key actions/ recommendations. DO NOT copy paste everything above_**
