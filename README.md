# Retail Sales & Customer Segmentation Analysis

## Business Question
An online UK-based retailer (B2B gift/home décor wholesaler) wants to understand:
- How is revenue trending over time, and when are peak seasons?
- Which products and customers drive the most revenue?
- Can we segment customers into actionable groups to improve retention and marketing?

---

## Dataset
**Source:** [Online Retail II — UCI Machine Learning Repository](https://archive.ics.uci.edu/dataset/502/online+retail+ii)  
**Period:** December 2009 – December 2011  
**Raw rows:** 1,067,371 | **After cleaning:** 779,425 (73% retained)

---

## Tech Stack
| Tool | Purpose |
|---|---|
| Python (pandas, scikit-learn) | Data cleaning, RFM analysis, KMeans clustering |
| PostgreSQL | Normalized relational database (4-table schema) |
| SQLAlchemy + psycopg2 | Python ↔ PostgreSQL connection |
| matplotlib / seaborn | Visualizations |
| Excel (CSV export) | Stakeholder-facing summary dashboard |

---

## Project Structure
```
retail_project/
│
├── retail.ipynb               # Main analysis notebook
├── retail_db.session.sql      # SQL queries (revenue trend, top products, LTV, MoM growth)
├── customer_segments_summary.csv   # Segment-level summary (for Excel dashboard)
├── rfm_customers_full.csv     # Per-customer RFM scores and segments
├── insights.md                # Running notes during analysis
└── README.md                  # This file
```

---

## Approach

### 1. Data Cleaning
Removed rows with missing Customer IDs (~243K rows), cancelled orders (Invoice starting with "C"), zero/negative quantities and prices, and duplicates. Retained 73% of raw data.

### 2. PostgreSQL Schema (Normalized)
Designed a 4-table normalized schema:
- `customers` (customer_id, country)
- `products` (stock_code, description)
- `invoices` (invoice, invoice_date, customer_id)
- `order_items` (order_item_id, invoice, stock_code, quantity, price)

Price is stored in `order_items` (not `products`) to preserve historical transaction pricing.

### 3. SQL Analysis
Wrote 4 queries to answer core business questions:
- Monthly revenue trend (seasonal pattern identified)
- Top 10 products by revenue
- Top 10 customers by lifetime value
- Month-over-month revenue growth %

### 4. RFM + KMeans Customer Segmentation
Computed Recency, Frequency, and Monetary metrics per customer. Scaled with StandardScaler, used the elbow method to select K=4 clusters, and ran KMeans to produce 4 segments.

---

## Key Insights & Recommendations

### 📌 Insight 1: Revenue is Strongly Seasonal — Plan Inventory for October
October and November consistently exceed £1M in monthly revenue across both years (Oct 2010: £1.03M, Nov 2010: £1.16M; Oct 2011: £1.03M, Nov 2011: £1.15M). December drops sharply, indicating customers place holiday orders in Oct/Nov.

**Recommendation:** Begin inventory build-up and marketing campaigns by September. Launch early-access promotions in October to capture peak demand before competitors.

---

### 📌 Insight 2: 33% of Customers Are Lost — But They're Not All Equal
2,037 customers (33%) haven't purchased in over a year with an average spend of only £619. However, the Loyal segment (3,859 customers) shows moderate spend (avg £2,811) and recent activity — they are at risk of becoming Lost if not engaged.

**Recommendation:** Do not invest in win-back campaigns for the Lost segment — their low lifetime value makes the ROI poor. Instead, invest in retaining Loyal customers before they churn: targeted re-engagement emails at the 60-day inactivity mark.

---

### 📌 Insight 3: 46 Customers Generate Disproportionate Revenue — Protect Them
Champions (42 customers, avg £67K spend) and VIP Wholesale (4 customers, avg £422K spend) together represent under 1% of the customer base but are responsible for a significant share of total revenue.

**Recommendation:** Assign a dedicated account manager to all VIP Wholesale and Champion customers. Offer them early product access, volume discounts, and direct contact channels. The cost of losing even one VIP Wholesale client far exceeds the cost of a retention program.

---

## Files to Reproduce This Analysis
1. Download `online_retail_II.xlsx` from UCI ML Repository
2. Create a virtual environment: `python -m venv retail_env`
3. Install dependencies: `pip install -r requirements.txt`
4. Run `retail.ipynb` top to bottom
5. Set up PostgreSQL, create `retail_db`, and run schema SQL before loading data
