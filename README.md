# Ecommerce Web Performance & Purchase Behavior Analysis | SQL, BigQuery
![Image](https://github.com/user-attachments/assets/87a187d0-9f07-4941-ab65-62e06929886b)

 _Analyze the performance of an e-commerce website, focusing on traffic, engagement, and revenue over time | SQL_

**Author:** Nguyễn Duy Kiên  

**Date:** October 2025

**Tools Used:** SQL

---

## 📑 Table of Contents  
1. [📌 Background & Overview](#-background--overview)  
2. [📂 Dataset Description & Data Structure](#-dataset-description--data-structure)
3. [🔎 Final Conclusion & Recommendations](#-final-conclusion--recommendations)

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
| Row | month  | visits | pageviews        | transactions |
|-----|--------|--------|------------------|--------------|
| 1   | 201701 | 64694  | 64691            | 697          |
| 2   | 201702 | 62192  | 62185            | 708          |
| 3   | 201703 | 69931  | 69927            | 883          |

#### Insight:
There is a positive trend from February to March in all three metrics. February appears to be the weakest month in terms of site visits, which could be worth investigating for potential causes.

### 🔍 Task 2: Bounce rate per traffic source in July 2017 (Bounce_rate = num_bounce/total_visit) (order by total_visit DESC)

#### Purpose: 
The goal of this analysis is to calculate the bounce rate for different traffic sources in July 2017. By analyzing bounce rates across various traffic sources, we aim to identify which sources lead to higher user engagement and which may need optimization to improve user retention and website performance.

#### Query:

```sql
SELECT trafficSource.source AS source,
       COUNT(totals.visits) AS total_visits,
       COUNT(totals.bounces) AS total_no_of_bounces,
       100*COUNT(totals.bounces)/COUNT(totals.visits) AS bounce_rate
FROM `bigquery-public-data.google_analytics_sample.ga_sessions_201707*`
WHERE _table_suffix BETWEEN '01' AND '31'
GROUP BY source
ORDER BY total_visits DESC
```

#### Query Result:
|     | source               | total_visits | total_no_of_bounces | bounce_rate        |
|-----|----------------------|--------------|---------------------|--------------------|
| 1   | google               | 38400        | 19798               | 51.557291666666664 |
| 2   | (direct)             | 19891        | 8606                | 43.265798602382986 |
| 3   | youtube.com          | 6351         | 4238                | 66.729648874193046 |
| 4   | analytics.google.com | 1972         | 1064                | 53.9553752535497   |
| 5   | Partners             | 1788         | 936                 | 52.348993288590606 |
| ... | ...                  | ...          | ...                 | ...                |
| 93  | images.google.com.au | 1            | 1                   | 100.0              |
| 94  | it.pinterest.com     | 1            | 1                   | 100.0              |
| 95  | online.fullsail.edu  | 1            | 1                   | 100.0              |
| 96  | suche.t-online.de    | 1            | 1                   | 100.0              |
| 97  | google.com.br        | 1            | 0                   | 0.0                |

#### Insight:
The result presents a summary of website traffic from different sources and key metrics used to assess user engagement and behaviours. It focuses on four main elements: source, total_visits, total_no_bounces and bounce_rate.

Google received the highest website traffic, 38,400 visits in the first row, with 19,798 were single-page visits (bounces), leading to a bounce rate of approximately 51.56%. The second place is (direct) with 19,891 visits, of these 8,606 bounces, resulting to a bounce rate of 43.27%.

This suggests that the majority of users from this source left the website without navigating to other pages. A high bounce rate indicates that users from this source might not find the content they are looking for or the landing page lacks of engagement to encourage further exploration. Therefore, it is essential to deal with high bounce rate by analyzing the context of the source and user behaviors in order to find effective improvement strategies.

### 🔍 Task 3: Revenue by traffic source by week, by month in June 2017

#### Purpose: 
This analysis is to calculate the revenue generated by different traffic sources for June 2017, broken down by week and month. This analysis will help identify the most profitable traffic sources over time, allowing for targeted marketing and optimization strategies to maximize revenue growth.

#### Query:

```sql
SELECT 'Month' AS time_type,
       FORMAT_DATE('%Y%m',PARSE_DATE('%Y%m%d',date)) AS time,
       trafficSource.source AS source,
       SUM(product.productrevenue)/1000000 AS revenue
FROM `bigquery-public-data.google_analytics_sample.ga_sessions_201706*`,
UNNEST (hits) AS hits,
UNNEST (hits.product) AS product
WHERE _table_suffix BETWEEN '01' AND '31' AND product.productrevenue IS NOT NULL
GROUP BY time_type, time, source
UNION ALL
SELECT 'Week' AS time_type,
       FORMAT_DATE('%Y%V',PARSE_DATE('%Y%m%d',date)) AS time,
       trafficSource.source AS source,
       SUM(product.productrevenue)/1000000 AS revenue
FROM `bigquery-public-data.google_analytics_sample.ga_sessions_201706*`,
UNNEST (hits) AS hits,
UNNEST (hits.product) AS product
WHERE _table_suffix BETWEEN '01' AND '31' AND product.productrevenue IS NOT NULL
GROUP BY time_type, time, source
ORDER BY revenue DESC
```

#### Query Result:
| Row | time_type | time   | source           | revenue      |
|-----|-----------|--------|------------------|--------------|
| 1   | Month     | 201706 | (direct)         | 97333.619695 |
| 2   | Week      | 201724 | (direct)         | 30908.909927 |
| 3   | Week      | 201725 | (direct)         | 27295.319924 |
| 4   | Month     | 201706 | google           | 18757.17992  |
| 5   | Week      | 201723 | (direct)         | 17325.679919 |
| ... | ...       | ...    | ...              | ...          |
| 42  | Month     | 201706 | bing             | 13.98        |
| 43  | Week      | 201722 | sites.google.com | 13.98        |
| 44  | Week      | 201724 | bing             | 13.98        |
| 45  | Week      | 201724 | l.facebook.com   | 12.48        |
| 46  | Month     | 201706 | l.facebook.com   | 12.48        |

#### Insight:
This table shows revenue data with various attributes, including time type, time period, souce and the amount of revenue. Revenue comes from different sources, including direct website visits, search traffic and referrals from other websites.

### 🔍 Task 04: Average number of pageviews by purchaser type (purchasers vs non-purchasers) in June, July 2017.

#### Purpose: 
Calculate the average number of pageviews by purchaser type to understand how user behavior differs between purchasers and non-purchasers, providing insights into user engagement and purchase behavior.

#### Query:

```sql
WITH 
purchaser_data AS (
  SELECT
      FORMAT_DATE("%Y%m", PARSE_DATE("%Y%m%d", date)) AS month,
      SUM(totals.pageviews) / COUNT(DISTINCT fullvisitorid) AS avg_pageviews_purchase
  FROM `bigquery-public-data.google_analytics_sample.ga_sessions_2017*`,
    UNNEST(hits) hits,
    UNNEST(product) product
  WHERE _table_suffix BETWEEN '0601' AND '0731'
    AND totals.transactions >= 1
    AND product.productRevenue IS NOT NULL
  GROUP BY month
),
non_purchaser_data AS (
  SELECT
      FORMAT_DATE("%Y%m", PARSE_DATE("%Y%m%d", date)) AS month,
      SUM(totals.pageviews) / COUNT(DISTINCT fullvisitorid) AS avg_pageviews_non_purchase
  FROM `bigquery-public-data.google_analytics_sample.ga_sessions_2017*`,
    UNNEST(hits) hits,
    UNNEST(product) product
  WHERE _table_suffix BETWEEN '0601' AND '0731'
    AND totals.transactions IS NULL
    AND product.productRevenue IS NULL
  GROUP BY month
)
SELECT
    pd.*,
    avg_pageviews_non_purchase
FROM purchaser_data pd
FULL JOIN non_purchaser_data USING(month)
ORDER BY pd.month;
```

#### Query Result:
| Row | month  | avg_pageviews_purchase | avg_pageviews_non_purchase |
|-----|--------|------------------------|----------------------------|
| 1   | 201706 | 94.02050113895217      | 316.86558846341671         | 
| 2   | 201707 | 124.23755186721992     | 334.05655979568053         | 


#### Insight:
The data reveals a significant difference in average page views between users who made a purchase and those who did not. On average, users who did not purchase tend to view more pages on the website than those who made a purchase. For instance, in June 2017, the average pageviews for non-purchasers (316.87) were sgnificantly higher compared to purchasers (94.02).

While user engagement is high, there might be barriers preventing conversions. It is important to find effective strategies in order to encourage these high-pageview users to make purchase, such as imporving website navigation, enhancing product page, reducing checkout process, etc.

### 🔍 Task 5: Average number of transactions per user that made a purchase in July 2017

#### Purpose: 
Calculate the average number of transactions per user who made a purchase in July 2017 to explore the purchasing patterns and frequency of repeat buyers. This will provide a clearer understanding of customer loyalty and help identify opportunities to enhance retention strategies.

#### Query:

```sql
SELECT FORMAT_DATE('%Y%m',PARSE_DATE('%Y%m%d',date)) AS month,
       SUM(totals.transactions)/COUNT(DISTINCT fullVisitorID) AS Avg_total_transactions_per_user
FROM `bigquery-public-data.google_analytics_sample.ga_sessions_201707*`,
UNNEST (hits) AS hits,
UNNEST (hits.product) AS product
WHERE _table_suffix BETWEEN '01' AND '31'
      AND totals.transactions >=1
      AND product.productRevenue IS NOT NULL
GROUP BY month
```

#### Query Result:
| Row | month  | Avg_total_transactions_per_user |
|-----|--------|---------------------------------|
| 1   | 201707 | 4.16390041493776                |

#### Insight:
The table presents the average total transactions per user in July, indicating that the typical user made approximately 4.16 transactions during July 2017. This data could help analyze user behavior, track user engagement with the platform, and evaluate the impact of marketing campaigns or promotions during that time.

### 🔍 Task 6: Revenue by traffic source by week, by month in June 2017

#### Purpose: 
Identify the other products purchased by customers who bought the "YouTube Men's Vintage Henley" in July 2017. The output should list the product name and the quantity ordered, providing insights into cross-selling opportunities and customer preferences.

#### Query:

```sql
SELECT FORMAT_DATE('%Y%m',PARSE_DATE('%Y%m%d',date)) AS Month,
       SUM(product.productRevenue)/COUNT(totals.visits)/1000000 AS avg_revenue_by_user_per_visit
FROM `bigquery-public-data.google_analytics_sample.ga_sessions_201707*`,
UNNEST (hits) AS hits,
UNNEST (hits.product) AS product
WHERE _table_suffix BETWEEN '01' AND '31'
      AND totals.transactions IS NOT NULL
      AND product.productRevenue IS NOT NULL
GROUP BY month
```

#### Query Result:
| Row | Month  | avg_revenue_by_user_per_visit |
|-----|--------|-------------------------------|
| 1   | 201707 | 43.856598348051243            | 

#### Insight:
The average revenue per user per visit for July 2017 is 43.86. This suggests that, on average, each user spent approximately 44 during that month. This could be an important metric for businesses to measure user engagement and activity.

### 🔍 Task 7: Other products purchased by customers who purchased product "YouTube Men's Vintage Henley" in July 2017. Output should show product name and the quantity was ordered.

#### Purpose: 
Identify the other products purchased by customers who bought the "YouTube Men's Vintage Henley" in July 2017. The output should list the product name and the quantity ordered, providing insights into cross-selling opportunities and customer preferences.

#### Query:

```sql
WITH buyer_list AS (
    SELECT
        distinct fullVisitorId  
    FROM `bigquery-public-data.google_analytics_sample.ga_sessions_201707*`
    , UNNEST(hits) AS hits
    , UNNEST(hits.product) as product
    WHERE product.v2ProductName = "YouTube Men's Vintage Henley"
    AND totals.transactions>=1
    AND product.productRevenue is not null
)

SELECT
  product.v2ProductName AS other_purchased_products,
  SUM(product.productQuantity) AS quantity
FROM `bigquery-public-data.google_analytics_sample.ga_sessions_201707*`
, UNNEST(hits) AS hits
, UNNEST(hits.product) as product
JOIN buyer_list using(fullVisitorId)
WHERE product.v2ProductName != "YouTube Men's Vintage Henley"
 and product.productRevenue is not null
 AND totals.transactions>=1
GROUP BY other_purchased_products
ORDER BY quantity DESC;
```

#### Query Result:
| Row | other_purchased_products                                  | quantity |
|-----|-----------------------------------------------------------|----------|
| 1   | Google Sunglasses                                         | 20       |
| 2   | Google Women's Vintage Hero Tee Black                     | 7        |
| 3   | SPF-15 Slim & Slender Lip Balm                            | 6        |
| 4   | Google Women's Short Sleeve Hero Tee Red Heather          | 4        |
| 5   | YouTube Men's Fleece Hoodie Black                         | 3        |
| ... | ...                                                       | ...      |
| 46  | YouTube Women's Short Sleeve Tri-blend Badge Tee Charcoal | 1        |
| 47  | YouTube Hard Cover Journal                                | 1        |
| 48  | Android BTTF Moonshot Graphic Tee                         | 1        |
| 49  | Google Men's Airflow 1/4 Zip Pullover Black               | 1        |
| 50  | YouTube Men's Long & Lean Tee Charcoal                    | 1        |

#### Insight:
Overall, the data offers valuable insights into customer preferences, product popularity, and opportunities for marketing and merchandising strategies. A deeper analysis of historical data, combined with customer demographics, could provide a more comprehensive understanding of these trends.

### 🔍 Task 8: Calculate cohort map from pageview to addtocart to purchase in last 3 month. For example, 100% pageview then 40% add_to_cart and 10% purchase.

#### Purpose: 
Generate a cohort map to track the customer journey from product view to add-to-cart and finally to purchase in January, February, and March 2017. This will help analyze conversion rates at each stage of the purchase funnel, providing insights into where customers drop off and identifying opportunities to optimize the sales process.

#### Query:

```sql
WITH num_action_type AS
(SELECT FORMAT_DATE('%Y%m',PARSE_DATE('%Y%m%d',date)) AS month,
       COUNTIF(eCommerceAction.action_type = '2') AS num_product_view,  
       COUNTIF(eCommerceAction.action_type = '3') AS num_addtocart,
       COUNTIF(eCommerceAction.action_type = '6' AND product.productRevenue IS NOT NULL) AS num_purchase,
FROM `bigquery-public-data.google_analytics_sample.ga_sessions_2017*`,
UNNEST (hits) AS hits,
UNNEST (hits.product) AS product
WHERE _table_suffix BETWEEN '0101' AND '0331'
GROUP BY month)


SELECT month,
       num_product_view,
       num_addtocart,
       num_purchase,
       100*num_addtocart/num_product_view AS add_to_cart_rate,
       100*num_purchase/num_product_view AS purchase_rate
FROM num_action_type
ORDER BY month
```

#### Query Result:
| Row | month  | num_product_view | num_addtocart | num_purchase | add_to_cart_rate   | purchase_rate      |
|-----|--------|------------------|---------------|--------------|--------------------|--------------------|
| 1   | 201701 | 25787            | 7342          | 2143         | 28.471710551828441 | 8.31038895567534   |
| 2   | 201702 | 21489            | 7360          | 2060         | 34.250081437014288 | 9.5862999674251945 |
| 3   | 201703 | 23549            | 8782          | 2977         | 37.292454032018348 | 12.641725763302052 |

#### Insight:
The table shows five different metrics and rates related to user behaviours from January to March 2017.

In general, the number of product views increases gradually during this period. Similarly, the add-to-cart rate and purchase rate showed an upward trend, indicating enhanced user engagement and conversion.

---

## 🔎 Final Conclusion & Recommendations  

### 📌 Insights:

- Traffic and conversion metrics improved steadily from January to March, showing stronger user engagement and better purchase behaviour over time.

- Google drives the highest traffic but also has the highest bounce rate, indicating misalignment between user search intent and landing page relevance.

- Non-purchasers view significantly more pages than purchasers, suggesting high interest but friction in the checkout or decision-making process.

- July shows unusually high transaction frequency and revenue per visit, implying strong seasonal or campaign-driven performance.

- Revenue contribution varies across channels, highlighting differences in traffic quality and marketing effectiveness.

### 📍 Recommendations:

1. Optimize SEO landing pages and conduct A/B tests to reduce bounce rate from Google traffic.

2. Improve product pages and streamline the checkout flow to convert high-engagement users who currently do not purchase.

3. Reallocate marketing budget toward channels with better conversion quality and lower bounce rates.

4. Analyze July’s successful campaigns or seasonal factors and replicate similar strategies in weaker months.

5. Implement remarketing for high-intent users and enhance product recommendations to increase conversion and revenue.
