# 🛒 E-Commerce Customer & Sales Analytics Platform

An end-to-end **Data Analytics project** designed to transform raw e-commerce data into reliable, business-ready insights using **Python, SQL, Power BI, and AI-assisted analytics**.

The project follows a realistic analytics workflow starting from raw and intentionally imperfect datasets, followed by data profiling, quality assessment, cleaning, validation, analysis, and business intelligence reporting.

---

## 📌 Project Overview

E-commerce businesses generate large volumes of data across customers, products, orders, marketing campaigns, and inventory.

The objective of this project is to build an analytics platform that answers important business questions such as:

* How are sales and profit performing over time?
* Which products and categories generate the highest revenue?
* Which customers are the most valuable?
* Which marketing channels generate the best return?
* Which products have inventory or stock-out risks?
* What factors are associated with changes in sales and customer behavior?

Unlike a basic dashboard project, this solution focuses on the **complete data analytics lifecycle**:

```text
Business Understanding
        ↓
Raw Data Collection
        ↓
Data Profiling
        ↓
Data Quality Assessment
        ↓
Data Cleaning
        ↓
Data Validation
        ↓
Exploratory Data Analysis
        ↓
SQL Analytics
        ↓
Power BI Dashboard
        ↓
AI-Assisted Insights
        ↓
Business Recommendations
```

---

# 🎯 Business Objectives

The project is designed to provide insights across four major business areas.

### 1. Sales Analytics

* Revenue trends
* Profit trends
* Order volume
* Average Order Value
* Category performance
* Product performance
* Regional performance
* Payment-method analysis

### 2. Customer Analytics

* Customer segmentation
* New vs returning customers
* Purchase frequency
* Customer monetary value
* RFM analysis
* Customer Lifetime Value
* Retention and potential churn analysis

### 3. Marketing Analytics

* Campaign performance
* Impressions
* Click-through rate
* Conversion rate
* Customer acquisition cost
* Return on Ad Spend
* Revenue by marketing channel

### 4. Inventory Analytics

* Current stock levels
* Fast-moving products
* Slow-moving products
* Stock-out risk
* Warehouse performance
* Inventory turnover
* Replenishment analysis

---

# 📊 Dataset

The project uses five separate raw datasets.

Each dataset contains approximately **50,000 records**, giving the project a total raw-data volume of approximately **250,000 records**.

| Dataset   | Records | Purpose                            |
| --------- | ------: | ---------------------------------- |
| Customers |  50,000 | Customer analysis                  |
| Products  |  50,000 | Product and profitability analysis |
| Orders    |  50,000 | Sales and transaction analysis     |
| Marketing |  50,000 | Campaign and marketing analysis    |
| Inventory |  50,000 | Inventory and stock analysis       |

---

# 📁 Dataset Structure

## Customers

Contains customer master information.

```text
customer_id
customer_name
age
gender
city
state
signup_date
customer_segment
preferred_device
```

### Business Use

Used for:

* Customer segmentation
* Demographic analysis
* Customer retention
* RFM analysis
* Customer Lifetime Value

---

## Products

Contains product master information.

```text
product_id
product_name
brand
category
sub_category
cost_price
selling_price
stock_quantity
supplier_rating
```

### Business Use

Used for:

* Product performance
* Category analysis
* Profitability
* Pricing analysis
* Inventory analysis

---

## Orders

Contains transactional sales information.

```text
order_id
customer_id
product_id
order_date
quantity
unit_price
discount_pct
revenue
shipping_cost
payment_method
order_status
customer_rating
estimated_cost
profit
```

### Business Use

This is the primary transactional dataset used for:

* Revenue analysis
* Profit analysis
* Order analysis
* Customer purchasing behavior
* Product performance
* Sales trends

---

## Marketing

Contains marketing campaign performance.

```text
campaign_id
campaign_date
campaign_name
campaign_type
channel
target_segment
impressions
clicks
conversions
spend
revenue_generated
ctr
conversion_rate
roas
```

### Business Use

Used for:

* Campaign performance
* Conversion analysis
* Marketing efficiency
* ROAS
* Customer acquisition analysis

---

## Inventory

Contains inventory snapshots.

```text
inventory_id
snapshot_date
product_id
warehouse
opening_stock
units_received
units_sold
damaged_units
closing_stock
stock_status
```

### Business Use

Used for:

* Stock monitoring
* Inventory analysis
* Stock-out detection
* Warehouse analysis
* Replenishment planning

---

# ⚠️ Intentional Data Quality Issues

To simulate a realistic business environment, the raw datasets intentionally contain data-quality problems.

Examples include:

* Missing values
* Duplicate records
* Negative quantities
* Invalid prices
* Missing customer IDs
* Missing product information
* Missing customer ratings
* Inconsistent categorical values
* Extreme/outlier values
* Invalid inventory values
* Potential referential-integrity issues

For example:

```text
payment_method

UPI
upi
Credit Card
credit card
COD
```

These values may represent the same business category but require standardization.

Another example:

```text
quantity

2
3
1
-5
4
```

A negative quantity cannot automatically be treated as an error because it could potentially represent a return or adjustment. Therefore, business context must be considered before modifying the record.

---

# 🔍 Data Profiling

Before modifying any raw data, the first step is to understand its structure and quality.

Python and Pandas are used to perform initial profiling.

### Basic structure

```python
df.shape
df.head()
df.tail()
df.sample(10)
```

### Data types and completeness

```python
df.info()
df.dtypes
```

### Statistical summary

```python
df.describe()
```

### Missing-value analysis

```python
df.isnull().sum()
```

### Duplicate analysis

```python
df.duplicated().sum()
```

This allows us to establish a baseline before cleaning.

---

# 🧪 Data Quality Assessment

The raw datasets are evaluated using four major categories.

## 1. Completeness

Checks whether required information is missing.

Examples:

```text
Missing customer_id
Missing product_id
Missing dates
Missing ratings
Missing category
```

---

## 2. Validity

Checks whether values follow acceptable business rules.

Examples:

```text
Quantity <= 0
Rating outside 1–5
Negative price
Negative inventory
Discount outside expected range
```

---

## 3. Consistency

Checks whether the same business concept is represented consistently.

Example:

```text
UPI
upi
UPI 
```

These values should be standardized.

---

## 4. Integrity

Checks relationships between datasets.

For example:

```text
Customers
    │
customer_id
    ↓
Orders
    │
product_id
    ↓
Products
```

Every valid order should reference a valid customer and product wherever the business process requires it.

---

# 🧹 Data Cleaning Strategy

The project follows a **business-rule-based cleaning approach**.

We do not automatically replace every missing value or delete every unusual record.

The process is:

```text
Detect
  ↓
Investigate
  ↓
Apply Business Rule
  ↓
Transform / Flag / Exclude
  ↓
Validate
```

---

## Missing Values

Treatment depends on the business meaning of the column.

### Example

**Customer age**

Could be:

```text
Median imputation
OR
Unknown category
```

depending on the analytical requirement.

**Customer rating**

A missing rating does not mean a rating of zero.

Therefore:

```text
Missing rating → NULL / Unknown
```

rather than:

```text
Missing rating → 0
```

---

## Duplicate Records

Exact duplicate rows can be removed after verification.

For business keys such as:

```text
order_id
customer_id
product_id
```

duplicates require investigation before removal.

The objective is to prevent:

```text
Duplicate transaction
        ↓
Inflated revenue
        ↓
Incorrect KPI
        ↓
Incorrect business decision
```

---

## Negative Values

Negative values are investigated according to business context.

For example:

```text
quantity = -5
```

may represent:

* Data-entry error
* Return
* Refund
* Inventory adjustment

Therefore, the value is **not blindly converted to +5**.

---

## Outliers

Outliers are identified using statistical techniques such as the IQR method.

However:

> An outlier is not automatically an error.

A very high-value order may be a legitimate transaction.

Therefore, extreme values are investigated before removal.

---

# ✅ Data Validation

After cleaning, the datasets are validated again.

Validation includes:

### Record count

```python
len(df)
```

### Missing values

```python
df.isnull().sum()
```

### Duplicate records

```python
df.duplicated().sum()
```

### Business rules

```python
(df["quantity"] <= 0).sum()
```

### Referential integrity

```python
orders["customer_id"].isin(
    customers["customer_id"]
).all()
```

### Date validation

```python
pd.to_datetime(
    df["order_date"],
    errors="coerce"
)
```

The cleaned data should pass the defined validation rules before moving to downstream analytics.

---

# 🧮 Key Business Metrics

The project will calculate metrics such as:

### Revenue

```text
Revenue = Quantity × Unit Price × (1 − Discount)
```

### Profit

```text
Profit =
Revenue − Product Cost − Shipping Cost
```

### Average Order Value

```text
AOV = Total Revenue / Total Orders
```

### Conversion Rate

```text
Conversion Rate =
Conversions / Clicks
```

### CTR

```text
CTR =
Clicks / Impressions
```

### ROAS

```text
ROAS =
Revenue Generated / Marketing Spend
```

### Customer Acquisition Cost

```text
CAC =
Marketing Acquisition Cost / New Customers
```

---

# 🧠 RFM Customer Segmentation

Customer behavior will be analyzed using:

### Recency

How recently did the customer purchase?

### Frequency

How frequently did the customer purchase?

### Monetary

How much did the customer spend?

```text
Customer
   ↓
Recency
Frequency
Monetary
   ↓
RFM Score
   ↓
Customer Segment
```

This can help identify groups such as:

* High-value customers
* Loyal customers
* Recent customers
* Potentially inactive customers

---

# 🛠️ Technology Stack

### Programming & Data Processing

* Python
* Pandas
* NumPy
* Matplotlib

### Database & Analytics

* SQL
* PostgreSQL / MySQL

### Business Intelligence

* Power BI
* DAX
* Power Query

### Development

* VS Code / Jupyter Notebook
* Git
* GitHub

### AI Layer

AI-assisted analytics will be explored for:

* Natural-language business questions
* Automated insight generation
* Trend explanations
* Business insight summarization

AI-generated insights will be treated as an analytical aid and validated against the underlying data.

---

# 📂 Project Structure

```text
ecommerce-analytics/
│
├── data/
│   ├── raw/
│   │   ├── customers_raw.csv
│   │   ├── products_raw.csv
│   │   ├── orders_raw.csv
│   │   ├── marketing_raw.csv
│   │   └── inventory_raw.csv
│   │
│   └── cleaned/
│
├── notebooks/
│   ├── 01_data_quality_audit.ipynb
│   ├── 02_data_cleaning.ipynb
│   └── 03_exploratory_analysis.ipynb
│
├── sql/
│   ├── schema.sql
│   ├── data_quality_checks.sql
│   ├── business_analysis.sql
│   └── customer_analysis.sql
│
├── powerbi/
│   └── ecommerce_analytics.pbix
│
├── reports/
│   ├── data_quality_report.xlsx
│   └── business_insights.pdf
│
├── README.md
└── requirements.txt
```

---

# 📈 Planned Power BI Dashboard

## Executive Overview

Key KPIs:

```text
Total Revenue
Total Profit
Total Orders
Total Customers
Average Order Value
Profit Margin
```

Visualizations:

* Revenue trend
* Profit trend
* Sales by category
* Sales by region
* Top products
* Order status

---

## Customer Analytics

* Customer segments
* RFM analysis
* New vs returning customers
* Customer revenue
* Purchase frequency
* Customer Lifetime Value

---

## Marketing Analytics

* Campaign spend
* Impressions
* Clicks
* Conversions
* CTR
* Conversion rate
* CAC
* ROAS

---

## Inventory Analytics

* Stock status
* Closing stock
* Fast-moving products
* Slow-moving products
* Stock-out risk
* Warehouse performance

---

# 🔄 End-to-End Workflow

```text
                RAW DATA
                    │
                    ▼
             DATA PROFILING
                    │
                    ▼
          QUALITY ASSESSMENT
                    │
       ┌────────────┼────────────┐
       ▼            ▼            ▼
   Missing       Invalid      Duplicate
    Values        Values        Data
       └────────────┼────────────┘
                    ▼
           BUSINESS RULES
                    │
                    ▼
             DATA CLEANING
                    │
                    ▼
             DATA VALIDATION
                    │
                    ▼
          EXPLORATORY ANALYSIS
                    │
                    ▼
              SQL ANALYTICS
                    │
                    ▼
              POWER BI MODEL
                    │
                    ▼
             INTERACTIVE BI
                    │
                    ▼
            AI-ASSISTED INSIGHTS
                    │
                    ▼
          BUSINESS RECOMMENDATIONS
```

---

# 🎯 Expected Project Outcome

The final solution will transform raw and imperfect e-commerce data into a validated analytical dataset and interactive business intelligence platform.

The completed project will demonstrate the ability to:

* Understand business requirements
* Work with large datasets
* Identify data-quality problems
* Apply business rules
* Clean and validate data
* Perform exploratory analysis
* Write analytical SQL
* Build business metrics
* Develop Power BI dashboards
* Communicate insights to stakeholders

---

# 🚧 Project Status

| Phase                     | Status         |
| ------------------------- | -------------- |
| Business Requirements     | ✅ Completed    |
| Raw Dataset Creation      | ✅ Completed    |
| Intentional Data Issues   | ✅ Completed    |
| Data Profiling            | 🔄 In Progress |
| Data Quality Assessment   | 🔄 In Progress |
| Data Cleaning             | ⏳ Next         |
| Data Validation           | ⏳ Pending      |
| Exploratory Data Analysis | ⏳ Pending      |
| SQL Analytics             | ⏳ Pending      |
| Power BI Dashboard        | ⏳ Pending      |
| AI Insight Layer          | ⏳ Pending      |
| Final Documentation       | ⏳ Pending      |

---

# 👨‍💻 Author

**Ranjan Kumar**

Data Analytics | Python | SQL | Excel | Power BI

---

## 📌 Project Philosophy

> **Reliable analytics starts with reliable data.**

This project therefore treats data cleaning and validation as core analytical processes rather than preliminary tasks.
