# 🍔 Swiggy Food Delivery Analytics Dashboard

**An end-to-end data analytics project** — from a raw, messy 420K+ row food-delivery dataset to a fully interactive Power BI dashboard, uncovering a critical order-fulfillment issue hidden behind aggregate order-volume metrics.

![Dashboard](https://github.com/akashkum121/Swiggy_Analysis_Project/blob/main/Screenshots/Dashboard.png)

---

---

## Project Overview

This project simulates a real-world Swiggy-style food-delivery analytics workflow — starting from **raw, intentionally messy transactional data** (customers, restaurants, menu items, orders, order items, delivery partners, and reviews) and ending with a **single-page executive Power BI dashboard**.

The goal was to move past surface-level "orders and revenue" reporting and surface an **operational reliability problem** that raw order counts alone don't reveal.

---

## Problem Statement

> Orders and revenue look healthy on the surface — so why does deeper analysis of order status reveal a serious fulfillment problem?

This project answers that question by breaking down order outcomes (not just order counts) across cuisine, category, restaurant, and geography.

---

## Dataset

Seven relational tables:

| Table | Description |
|---|---|
| `customers` | Customer details & signup info |
| `restaurants` | Restaurant details, cuisine, rating |
| `menu_items` | Menu catalog per restaurant |
| `delivery_partners` | Delivery partner details & ratings |
| `orders` | Order-level transaction data |
| `order_items` | Line-item level detail |
| `reviews` | Customer reviews & ratings per order |

Raw data included realistic messiness: mixed date formats, inconsistent currency symbols (₹ / Rs. / INR), typos in cuisine/city names, mixed boolean formats (`is_veg`: Yes/Y/1/TRUE), duplicate records, and missing values — all cleaned programmatically.

---

## Tech Stack

| Tool | Purpose |
|---|---|
| **Python** (Pandas, NumPy, dateutil) | Data cleaning, null handling, deduplication |
| **Power Query** | Data shaping & transformation inside Power BI |
| **DAX** | KPI measures, calculated columns |
| **Power BI** | Data modeling & interactive dashboard |

---

## Data Cleaning Summary

- **Mixed date/datetime formats** → parsed and standardized to `YYYY-MM-DD`
- **Currency inconsistency** (`₹499`, `Rs.499`, `INR 499`) → cleaned to plain floats
- **Cuisine/city typos** (`North Indian`/`north indian`/`N.Indian`, `Bangalore`/`Bengaluru`/`Banglore`) → standardized via lookup mapping
- **Mixed boolean formats** (`is_veg`: Yes/Y/1/TRUE vs No/N/0/FALSE) → standardized to a single Yes/No format
- **Duplicate rows & duplicate order IDs** → identified and removed
- **Missing delivery partner IDs** → explicitly labeled `"Not Assigned"` instead of fabricated
- **Missing ratings/review text** → left as genuine nulls where appropriate, flagged for analysis

The CSVs in this repo are the **final cleaned outputs**, ready to load directly into Power BI.

---

## Data Model

```
customers ──1:N── orders ──1:N── order_items ──N:1── menu_items ──N:1── restaurants
              │        └──1:1── reviews
              └──N:1── delivery_partners
```

![Model Relationships](https://github.com/akashkum121/Swiggy_Analysis_Project/blob/main/Screenshots/Model_Relationship.png)

---

## Dashboard Features

**KPI Strip:** Total Sale · Average Rating · Total Customers · Total Orders · Order Items

**Visuals:**
- **Cuisine Rating** — total rating trend across cuisines (Fast Food, Desert, Italian, South Indian, North Indian, Chinese, Bakery, Biryani)
- **Monthly Sales & Orders** — combo chart tracking order volume and item sales by quarter/month
- **Category Performance** — total items by category (Deserts, Breads, Starters, Main Course, Beverages, Rice & Biryani)
- **Order Status** — donut chart breaking down Delivered / Cancelled / Delivered Late / Food Not Delivered
- **Top 10 Restaurants** — leaderboard by total order volume
- **Orders by City** — geographic map (Mumbai, Pune, Bangalore, Hyderabad, Chennai, Kolkata)

**Interactive filters:** Year, Month, Cuisine, Order Status, Food Type, City

---

## Key Insights

1. **Only ~25% of orders are successfully "Delivered."** The Order Status breakdown shows Delivered, Cancelled, Delivered Late, and Food Not Delivered all sitting at roughly equal ~25% shares — meaning **75% of all orders have some fulfillment problem**. This is invisible if you only look at total order count or revenue, which both look healthy.
2. **Order volume nearly doubled starting mid-year** — a clear jump from ~50K orders/month (Jan–May) to ~90K+ orders/month (June onward), suggesting either a seasonal demand shift, a marketing push, or new city/restaurant launches worth investigating.
3. **Cuisine ratings decline steadily from Fast Food to Biryani** — Fast Food leads in total rating (~162K), with a consistent downward trend through Desert, Italian, South Indian, North Indian, Chinese, Bakery, ending at Biryani (~158K). This is a relatively narrow band, suggesting cuisine type alone isn't a major satisfaction driver — reinforcing that the **fulfillment problem (point 1) is likely the bigger lever** on customer experience than cuisine quality.
4. **Category performance is tightly clustered** (163.5M–166.3M total items across Deserts, Breads, Starters, Main Course, Beverages, Rice & Biryani) — no single category is significantly under- or over-performing, so category mix isn't driving the fulfillment issue either.
5. **Average rating sits at a modest 3.00/5** — consistent with the high non-delivery rate; ratings are likely being dragged down by fulfillment failures rather than food/menu quality.

---

## Recommendations

- **Prioritize fulfillment reliability over acquisition** — with 75% of orders facing some delivery issue, fixing this will likely move average rating and repeat-order rate more than any menu or pricing change.
- **Investigate the mid-year order-volume jump** — determine if delivery infrastructure (partners, restaurants onboarded) scaled proportionally, since a demand spike without matching capacity is a common cause of rising cancellations/late deliveries.
- **Segment "Cancelled" vs "Delivered Late" vs "Food Not Delivered"** by city and restaurant to isolate whether the issue is restaurant-side (prep delays) or logistics-side (delivery partner capacity).
- **Correlate review ratings with order status** — confirm that low ratings cluster on non-Delivered orders, which would validate fulfillment as the primary lever for the 3.00 average rating.

---

## How to Run

1. Clone this repo
2. Open Power BI Desktop → **Get Data** → **Text/CSV** and load each file from `data/`
3. Set up relationships between tables as shown in `model_relationships.png`

---

## Project Structure

```
├── data/
│   ├── customers.csv
│   ├── delivery_partners.csv
│   ├── menu_items.csv
│   ├── order_items.csv
│   ├── orders.csv
│   ├── restaurants.csv
│   └── reviews.csv
├── dashboard_screenshot.png
├── model_relationships.png
└── README.md
```

---

*This project uses a synthetically generated dataset built specifically for practicing real-world data cleaning and BI dashboard workflows.*
