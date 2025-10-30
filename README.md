# store-revenue-segmentation-sql
SQL project categorizing stores by size and revenue, analyzing their contribution in 2023
# Store Revenue Segmentation (SQL Project)

## 🧩 Problem Statement
Classify stores based on squaremeters and net revenue to analyze their contribution in 2023.

**Store Categorization:**
1. Small Store - Low Revenue (<1000 sqm, revenue < 100,000)  
2. Small Store - High Revenue (<1000 sqm, revenue ≥ 100,000)  
3. Medium Store - Low Revenue (1000–2000 sqm, revenue < 300,000)  
4. Medium Store - High Revenue (1000–2000 sqm, revenue ≥ 300,000)  
5. Large Store - Low Revenue (>2000 sqm, revenue < 500,000)  
6. Large Store - High Revenue (>2000 sqm, revenue ≥ 500,000)  
7. Online Store (squaremeters IS NULL)

**Analysis Goals:**
- Compute total revenue per store category  
- Determine percentage contribution of each category to overall revenue  
- Use a Common Table Expression (CTE) to simplify calculations

---

## 💡 SQL Solution
```sql
WITH store_revenue AS (
    SELECT
        st.storekey,
        st.squaremeters,
        SUM(s.quantity * s.netprice * s.exchangerate) AS revenue
    FROM sales s
    JOIN store st ON s.storekey = st.storekey
    WHERE s.orderdate BETWEEN '2023-01-01' AND '2023-12-31'
    GROUP BY st.storekey, st.squaremeters
)
SELECT
    CASE
        WHEN squaremeters < 1000 AND revenue < 100000 THEN '1 - Small Store - Low Revenue'
        WHEN squaremeters < 1000 AND revenue >= 100000 THEN '2 - Small Store - High Revenue'
        WHEN squaremeters BETWEEN 1000 AND 2000 AND revenue < 300000 THEN '3 - Medium Store - Low Revenue'
        WHEN squaremeters BETWEEN 1000 AND 2000 AND revenue >= 300000 THEN '4 - Medium Store - High Revenue'
        WHEN squaremeters > 2000 AND revenue < 500000 THEN '5 - Large Store - Low Revenue'
        WHEN squaremeters > 2000 AND revenue >= 500000 THEN '6 - Large Store - High Revenue'
        WHEN squaremeters IS NULL THEN '7 - Online Store'
    END AS store_category,
    SUM(revenue) AS total_net_revenue,
    (SUM(revenue) / (SELECT SUM(revenue) FROM store_revenue) * 100.0) AS percentage_contribution
FROM store_revenue
GROUP BY store_category 
ORDER BY store_category;

 Explanation;

1.First, we calculate how much money each store made in 2023.

2.We use information from both the sales and store tables to get the numbers we need.

3.Then, we divide stores into categories based on their size (small, medium, large) and revenue (low or high). Online stores are grouped separately.

4.For each category, we add up all the revenue and figure out what percentage of the total revenue it represents.

5.Finally, we show the results grouped by store category so it’s easy to compare.

Tools I Used — Store Revenue Segmentation

## 🧰 Tools I Used
 **SQL** – For performing advanced data segmentation and revenue analysis  
 **PostgreSQL** – To run and validate complex queries  
 **Common Table Expression (CTE)** – For calculating total revenue per store before categorization  
 **CASE WHEN Statements** – To classify stores based on size and revenue level  
 **Aggregate Functions (SUM, Percentage Calculations)** – To find total and percentage contributions  
 **GROUP BY** – To summarize data by store category  
- **Data Filtering (WHERE Clause)** – To include only 2023 sales  
 **Data Visualization (Optional)** – To visualize revenue contribution by store categories

## 📊 Revenue by Store Category
A table showing the store category, total net revenue, and percentage contribution for each store category in 2023.
![Category_Revenue png](https://github.com/user-attachments/assets/78016712-1685-4f50-b93c-cd97fbb593ac)

