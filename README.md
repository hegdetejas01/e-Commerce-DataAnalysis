# Comprehensive E-Commerce Exploratory Data Analysis & Modeling Report

* **Author:** Tejas S Hegde  
* **Date:** October 2026  
* **Project Phase:** IT Transition & Data Science Preparation (July–October 2026)  

---

## 1. Project Overview

This repository houses a comprehensive end-to-end data analysis, cleaning, univariate exploration, and data hygiene pipeline built around a synthetic e-commerce dataset containing **customers**, **products**, and **sales transactions**. 

The primary objective of this project is to demonstrate professional-grade data manipulation, memory optimization, exploratory data analysis (EDA), and structured documentation using Python, Pandas, NumPy, Matplotlib, and Seaborn.

---

## 2. Dataset Architecture & File Summary

The project ingests three primary datasets located in the `datasets/` directory:

| Dataset | File Name | Shape | Description |
| :--- | :--- | :--- | :--- |
| **Customers** | `customers.csv` | 40,000 rows $\times$ 15 columns | Customer demographic data, location, loyalty tier, total orders, and lifetime spend. |
| **Products** | `products.csv` | 2,000 rows $\times$ 12 columns | Product catalog details, pricing structures, discounts, stock quantities, weights, and ratings. |
| **Sales Transactions** | `sales.csv` | 250,000 rows $\times$ 21 columns | Granular order-level records mapping customers to products, order dates, payment methods, and statuses. |

---

## 3. Data Cleaning & Memory Optimization Pipeline

To ensure computational efficiency and handle missing data gracefully, the following preprocessing steps were applied across the tables:

### A. Column Standardization
* Converted all dataframe column names to lowercase to maintain consistent snake_case schema naming conventions.

### B. Customers Table Optimization
* **Dropped Redundant Columns:** `age_group`, `date_of_birth`, and `phone` were removed as age is maintained numerically and communication channels are not required for aggregate analysis.
* **Datatype Downcasting:** Converted `age` and `total_orders` from standard integer types to downcasted memory-efficient types (`int8`).

### C. Sales Table Optimization
* **Missing Value Imputation:**
  * `coupon_code`: Null entries filled with `"NOT APPLICABLE"`.
  * `review_text`: Null entries filled with `"NOT PROVIDED"`.
  * `rating`: Imputed missing customer feedback scores using the global average rating ($\approx 4.40$).
* **Dropped Redundant Columns:** `city`, `state`, `customer_age`, and `customer_age_group` were dropped since they already exist in the normalized `customers` table.
* **Datatype Downcasting:** Downcasted `quantity` and `rating` to `int8` and `float16` respectively.

### D. Products Table Optimization
* **Datatype Downcasting:** Optimized numerical columns by converting `avg_rating` (`float16`), `total_reviews` (`int16`), `weight_kg` (`float16`), and `stock_quantity` (`int32`).

---

## 4. Key Insights from Univariate Analysis

### Customer Demographics & Purchasing Behavior
* **Loyalty Dominance:** Platinum tier members represent roughly two-thirds of the total customer base, indicating a highly skewed loyalty distribution.
* **Gender Parity:** The customer base shows an equal distribution between male and female cohorts.
* **Spending Distribution:** The majority of customer lifetime spend is concentrated in lower-value brackets ($< \text{INR } 100,000$).
* **Age Demographics:** A high density of active customers falls within the core working-age bracket of **[20–35] years**.

### Sales & Transaction Dynamics
* **Shipping Cost:** Virtually all transactions reflect **Zero Shipping Cost** ($0.00$), pointing toward widespread free shipping incentives.
* **Payment Gateways:** **UPI** is the dominant payment mode (51.4%), followed closely by Cash on Delivery (**COD** at 32.9%), with credit/debit cards comprising the remaining share.
* **Order Status:** Approximately **80%** of total orders are successfully delivered, while cancellations and returns account for roughly **10%**.
* **Review Sentiment:** An overwhelmingly low volume of products received negative reviews, showing a heavily left-skewed satisfaction profile.
* **Seasonality & Time:** 
  * Monthly order volumes remain steady throughout the year with a noticeable peak observed in **June**.
  * Hourly demand is remarkably uniform across all 24 hours of the day ($\approx 10,400\text{--}10,600$ orders per hour), showing no sharp peak or off-peak constraints.

---

## 5. Repository Structure

```text
├── datasets/
│   ├── customers.csv
│   ├── products.csv
│   └── sales.csv
├── notebooks/
│   └── ecommerce_eda.ipynb
├── README.md