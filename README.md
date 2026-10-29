# Retail E-Commerce-DataAnalysis & Preprocessing

## Project Overview
This project performs end-to-end data cleaning, memory optimization, and exploratory data analysis (EDA) across three relational retail e-commerce datasets: **Sales**, **Customers**, and **Products**. The primary objective is to streamline the relational schema, optimize memory footprints for large-scale transaction records, and uncover actionable insights across customer demographics, purchasing behavior, catalog pricing, fulfillment performance, and temporal demand patterns.

---

## Dataset Architecture

The analysis is built on three core relational tables representing different facets of the retail ecosystem:

| Dataset | Initial Records | Initial Columns | Description |
| :--- | :--- | :--- | :--- |
| **Sales** | $250,000$ | $21$ | Granular transaction logs including order quantities, pricing, discounts, shipping costs, payment modes, delivery statuses, and customer feedback. |
| **Customers** | $40,000$ | $15$ | Customer profiles containing demographic attributes, loyalty tier memberships, cumulative order counts, and lifetime spending metrics. |
| **Products** | $2,000$ | $12$ | Product catalog details including category hierarchies, base prices, discount percentages, stock levels, weights, and aggregate review scores. |

---

## Data Cleaning & Preprocessing Pipeline

To prepare the relational datasets for efficient analysis and downstream modeling, a structured preprocessing workflow was executed:

### 1. Schema Standardization & Feature Pruning
* **Column Standardization:** All column names across the `Sales`, `Customers`, and `Products` tables were standardized to lowercase snake_case to ensure consistency during table joins and querying.
* **Redundant Attribute Removal:**
  * **Customers Table:** Dropped `age_group`, `date_of_birth`, and `phone` to eliminate redundant demographic encodings and personally identifiable information (PII) not required for aggregate analysis.
  * **Sales Table:** Removed duplicated demographic columns (`city`, `state`, `customer_age`, and `customer_age_group`) that were already normalized within the `Customers` table, enforcing relational integrity.

### 2. Missing Value Imputation
Missing data was concentrated in the `Sales` table and handled based on domain logic rather than row deletion to preserve the full $250,000$-record transaction history:
* **`coupon_code` ($199,815$ missing values):** Imputed with `"NOT APPLICABLE"`, reflecting transactions completed without promotional codes.
* **`review_text` ($129,970$ missing values):** Imputed with `"NOT APPLICABLE"`, representing orders where customers chose not to leave written feedback.
* **`rating` ($129,970$ missing values):** Imputed using the global mean rating of $\approx 4.40$ to maintain the central tendency of the rating distribution without introducing extreme variance.

### 3. Memory Optimization (Data Type Downcasting)
To reduce RAM consumption and accelerate computation, numerical columns across all three tables were downcast from default 64-bit types (`int64`, `float64`) to their smallest viable subtypes (`int8`, `int16`, `int32`, and `float16`):
* **Downcasted Attributes:** `age`, `total_orders`, `quantity`, `rating`, `avg_rating`, `total_reviews`, `weight_kg`, and `stock_quantity`.

---

## Key Exploratory Data Analysis (EDA) Insights

### 1. Customer Demographics & Loyalty (`Customers`)
* **Gender Distribution:** The platform exhibits a balanced gender split, with male and female shoppers represented equally.
* **Age Profile:** Customer ages span from $18$ to $75$ years with a mean age of **$35.31$ years**. The primary demographic concentration lies within the **$20\text{--}35$ age bracket**.
* **Loyalty Tier Skew:** Membership is heavily concentrated at the highest tier, with **Platinum members** accounting for roughly two-thirds ($\approx 66.7\%$) of the total user base.
* **Order Frequency & Lifetime Spend:**
  * Customers place an average of **$5$ orders** (ranging from $0$ to $17$ orders).
  * Cumulative spending averages **Rs. $118,539.16$** with a median of **Rs. $90,059.66$**. The spending distribution is positively (right) skewed, with the majority of shoppers spending under **Rs. $100,000$**.

### 2. Catalog Pricing & Inventory (`Products`)
* **Pricing & Discounts:** Catalog selling prices average **Rs. $13,180.34$** after applying an average promotional discount of **$27.47\%$**.
* **Product Ratings:** Catalog-wide product ratings consistently fall between **$3.0$ and $5.0$**, indicating strong baseline product quality across categories.

### 3. Transactional, Payment & Fulfillment Metrics (`Sales`)
* **Order Value & Volume:** Actual checkout transaction values average **Rs. $23,722.77$** with a mean order quantity of **$1.25$ units** per transaction.
* **Free Shipping Adoption:** Out of $250,000$ total orders, **$232,459$ transactions ($92.98\%$)** incurred **zero shipping cost**, acting as a major driver of checkout conversion.
* **Payment Preferences:** Digital payments dominate the checkout experience:
  * **UPI:** $51.4\%$ of all transactions
  * **Cash on Delivery (COD):** $32.9\%$ of all transactions
  * **Other Modes (Cards/Net Banking/Wallets):** $15.7\%$ of all transactions
* **Order Fulfillment:** **$80\%$** of all orders reach successful delivery, while returned and cancelled orders combined represent **$10\%$** of total volume.
* **Customer Sentiment:** Qualitative review categorization confirms that negative and lukewarm reviews represent the smallest fraction of submitted customer feedback.

### 4. Temporal Demand Patterns
* **Monthly Seasonality:** Order intake remains highly uniform across eleven months of the year, experiencing a pronounced mid-year surge that peaks in **June**.
* **24-Hour Purchasing Cycle:** Hourly order flow remains remarkably steady around the clock at **$10,400$ to $10,600$ orders per hour** ($\approx 4.17\%$ of daily volume per hour), indicating continuous 24/7 platform utilization without severe peak-hour bottlenecks.

---

## Tech Stack & Libraries
* **Programming Language:** Python 3.x
* **Data Manipulation & Optimization:** `pandas`, `numpy`
* **Data Visualization:** `matplotlib`, `seaborn`
* **Environment:** Jupyter Notebook

---

## Repository Structure

```text
├── datasets/
│   ├── customers.csv
│   ├── products.csv
│   └── sales.csv
├── notebooks/
│   └── ecommerce_eda.ipynb
├── README.md
```

---

## Getting Started

1. **Clone the repository:**
   ```bash
   git clone https://github.com/your-username/retail-ecommerce-analysis.git
   cd retail-ecommerce-analysis
   ```
2. **Install dependencies:**
   ```bash
   pip install pandas numpy matplotlib seaborn
   ```
3. **Run the analysis:**
   Launch Jupyter Notebook and open the analysis notebook:
   ```bash
   jupyter notebook notebooks/eda_and_cleaning.ipynb
   ```

---

## Strategic Business Recommendations
1. **Capitalize on the June Peak:** Align major inventory restocks, server capacity scaling, and flagship marketing campaigns ahead of the June demand surge.
2. **Convert COD to Digital Payments:** With COD still representing $32.9\%$ of volume, introducing small instant discounts or cashback incentives for UPI ($51.4\%$) can reduce return-to-origin (RTO) risks and cash-handling overhead.
3. **Re-engage Mid-Tier Spenders:** Since cumulative spend is right-skewed with most users spending under Rs. $100,000$, targeted cross-selling and bundled promotions (increasing average basket size beyond $1.25$ units) can help move median spenders closer to the mean.
4. **Sustain Free Shipping Thresholds:** Given that $92.98\%$ of orders utilize free shipping, maintaining this benefit—or tying it to minimum cart values—remains essential for preserving conversion rates.
