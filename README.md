<img width="940" height="428" alt="image" src="https://github.com/user-attachments/assets/8a0bfd4e-9993-4ea7-94bd-815995178bd0" /># My-First-Project

# ShopNest Store Sales Analysis | Power BI

An interactive Power BI dashboard built on anonymized ShopNest marketplace data. It answers **eight business questions** covering sales, delivery performance, customer payments, product ratings, regional demand, seasonality and revenue growth.

**Tools & skills:** Power BI Desktop · Power Query · Data Modeling (star schema) · DAX measures · Slicers · Bookmarks · Drill-through · Map visuals

---

## Project at a glance

| | |
|---|---|
| **Orders** | 99,441 (96,478 delivered) |
| **Period covered** | Sep 2016 – Oct 2018 (2018 data is incomplete after Aug) |
| **States covered** | 27 |
| **Products / Sellers** | 32,951 / 3,095 |
| **Total sales revenue** | 16.01 M |

## Dataset

Nine CSV files describing the full order lifecycle:

| Table | What it contains |
|---|---|
| Orders | Status and timestamps: purchase, approval, carrier pickup, delivery, estimated delivery |
| Order Items | Product, seller, item price, freight value per order line |
| Order Payments | Payment type, installments, payment value |
| Order Reviews | Review score (1–5), comment title and message |
| Customers | Customer city and state |
| Sellers | Seller city and state |
| Products | Category name and physical attributes |
| Category Translation | Portuguese → English category names |
| Geolocation | Zip-prefix latitude / longitude (about 1 M rows) |

## Data preparation (applies to all questions)

1. **Loaded all nine CSVs** into Power BI and cleaned them in Power Query.
2. **Converted date columns** from text (`dd-mm-yyyy hh:mm`) to proper date/time types so that delivery comparisons and time-based grouping work correctly.
3. **Translated product categories** to English using a `Category Name` table, so visuals are readable.
4. **Built a `Calendar` table** with Year, Quarter and Year-Month fields to drive all time-based analysis.
5. **Modeled the tables** into a star schema around Orders, Order Items, Order Payments, Order Reviews, Products, Sellers and Customer Details, plus Geolocation for the map.
6. **Created a dedicated measures table** (`Measures`) holding the DAX measures: Total Sales, Total Price, Delayed Orders, On Time Orders, Delayed Orders by Category, Average Product Rating and the Top/Bottom product score measures.
7. **Added a `Delivery Status` field** (Delayed / On Time) by comparing actual delivery date with estimated delivery date.

---

## Q1. Which are the top 10 product categories by total sales?

<img width="940" height="442" alt="image" src="https://github.com/user-attachments/assets/f4a8ae7c-fab4-4389-b85e-55b873bdd2c0" />


**Data available:** Order Items (product, price), Products (category), Category Translation (English names).

**Data preparation:** Joined items to products and the translation table, summed item price by English category name, and applied a Top 10 filter ranked by that total.

**Result**
- **Health & Beauty** leads with about **1.26 M**, followed by **Watches & Gifts** (~1.21 M) and **Bed, Bath & Table** (~1.04 M).
- Top 5 categories each generate roughly 0.9 M or more.
- The top 10 categories account for about **62%** of total item sales, and the top 3 for about **26%**.

**Business implication:** Revenue is concentrated in a handful of categories. These should get priority in inventory planning, promotions and retention. Lower-ranked categories in the top 10 (Garden Tools, Auto, Housewares) are candidates for targeted marketing or assortment changes.

---

## Q2. How many delayed orders are there in each category?

<img width="940" height="443" alt="image" src="https://github.com/user-attachments/assets/189885f3-7a9f-45d9-9b8c-01a2d8fc6f63" />

**Data available:** Orders (delivered date, estimated delivery date), Order Items, Products, Category Translation.

**Data preparation:** An order is *delayed* when the actual delivery date is later than the estimated delivery date. Only orders with a recorded delivery date can be judged (about 3,000 orders have none). Delayed orders were then counted per category.

**Result**
- **Bed, Bath & Table** has the most delayed orders (**800+**), followed by **Health & Beauty** and **Sports & Leisure**.
- Furniture Décor, Computers Accessories and Watches & Gifts also show significant delays.
- Stationery and Fashion Bags & Accessories have the fewest.
- Overall, about **8% of delivered orders** arrived late.

**Business implication:** Delays are highest in the highest-volume categories, which points to logistics and warehouse capacity rather than one-off failures. Fixing fulfillment for the top three categories would improve reviews and retention the most. Tracking the delay *rate* per category, not just the count, would show which categories are truly worst.

---

## Q3. How do delayed and on-time orders compare month by month? (with drill-through)

<img width="940" height="415" alt="image" src="https://github.com/user-attachments/assets/a0a0baf5-d1fc-43b3-b067-cfd77d9862d4" />

**Data available:** Orders (dates, status), Customers (state), Order Items and Payments (sales amount), Calendar.

**Data preparation:** Created On Time and Delayed order measures, plotted them by Year-Month in a stacked column chart (green = on time, red = delayed) and added **Year** and **Quarter** slicers. A **drill-through page** was built showing Order ID, Customer State, Sales Amount, Estimated Delivery Date, Actual Delivery Date and Delivery Status.

**Result**
- On-time deliveries exceed delayed ones in every month.
- Order volume grew from early 2017 and peaked in late 2017 (November 2017 was the busiest month).
- Delays rise with volume, but stay a small share of deliveries.
- A source-data cross-check shows a delay spike in **Feb–Mar 2018**, where roughly 16–21% of orders arrived late, well above the average.

**Business implication:** Delivery operations held up well during peak demand, but the early-2018 spike is worth investigating (specific sellers, regions or carriers). The drill-through page lets a user go from a bad month straight to the individual late orders.

---

## Q4. Which payment methods do customers use most?

<img width="940" height="406" alt="image" src="https://github.com/user-attachments/assets/5b60e226-2d7a-4581-a891-756677bceb0e" />

**Data available:** Order Payments (payment type, value), plus Year / Quarter / State / Category slicers through the model.

**Data preparation:** Summed payment value by payment type and displayed it as a donut chart with slicers for Year, Quarter, Customer State and Product Category.

**Result**
- **Credit card:** ~**78.3%** of payment value.
- **Boleto** (bank slip): ~**17.9%**.
- **Voucher** and **debit card** together make up only about 4%.

**Business implication:** The business depends heavily on card payments, so a smooth, secure card checkout (including installments) is critical. Boleto is a sizeable secondary channel and still needs to be supported.

---

## Q5. Which 10 products are rated highest and lowest?

**Top 10 highest rated**

<img width="940" height="213" alt="image" src="https://github.com/user-attachments/assets/f16d5226-a4ba-4341-972b-dd6fa32d8185" />

<img width="940" height="223" alt="image" src="https://github.com/user-attachments/assets/3ea54f9f-c3fd-4a4c-a79c-abd3d621e6bf" />


**Bottom 10 lowest rated**

<img width="940" height="433" alt="image" src="https://github.com/user-attachments/assets/4dd5b57e-0646-41e9-b4d8-da4ba390495b" />


**Data available:** Order Reviews (score 1–5), Order Items (to link reviews to products), Products.

**Data preparation:** Linked reviews to products through the order, calculated an average rating per product, and applied Top 10 / Bottom 10 filters in two bar charts.

**Result**
- All top 10 products have an average rating of **5.0**, and all bottom 10 have **1.0**.

**Business implication:** Low-rated products should be checked for quality, listing accuracy, packaging or delivery problems, while high-rated ones set the benchmark. **Caveat:** thousands of products tie at 5.0 (about 14,000) and 1.0 (about 2,800), and most of them have only *one* review, so which 10 appear is essentially arbitrary. A minimum review count (for example 10+) would make this ranking much more reliable.

---

## Q6. Which states have high and low sales?

<img width="940" height="434" alt="image" src="https://github.com/user-attachments/assets/540e23f3-a2b4-4d2a-afa9-48af6b86ea1b" />

**Data available:** Customers (state), Orders, Payments, Geolocation (coordinates for the map).

**Data preparation:** Aggregated total sales by customer state and displayed it as a bubble map (geographic spread) next to a bar chart (exact comparison).

**Result**
- **São Paulo (SP)** leads by a wide margin at about **6.0 M** (~37% of all sales).
- **Rio de Janeiro (RJ)** (~2.1 M) and **Minas Gerais (MG)** (~1.9 M) follow.
- SP, RJ and MG together generate roughly **63%** of revenue.
- Northern states such as Roraima, Amapá and Acre are each under 0.02 M.

**Business implication:** Demand is concentrated in the economically developed south-east. That is both a dependency risk and a growth opportunity: targeted marketing and better logistics could lift sales in under-served northern and north-eastern states.

---

## Q7. Is there a seasonal or quarterly sales pattern?

<img width="940" height="428" alt="image" src="https://github.com/user-attachments/assets/1e7b09ad-a9b4-421c-9b1d-5cfb46206234" />

**Data available:** Orders (purchase date), Payments (sales value), Calendar (Year, Quarter).

**Data preparation:** Grouped total sales by Year and Quarter and plotted the trend as a line chart.

**Result**
- Sales grew steadily from late 2016 through 2017 and peaked in **Q2 2018 at about 3.34 M**.
- **Q4 2017** jumped about 43% over Q3 2017, driven by a record month in November 2017 (about 1.19 M).
- **Q4 2018 is near zero** because the dataset has almost no data for that quarter.
- **Q3 2018 is also partial** (July–August only), so its lower total is not a real decline. Monthly sales in July and August were only modestly below the Q2 monthly average.

**Business implication:** Late-year demand (likely holiday and Black Friday) is a peak worth planning inventory, staffing and campaigns around. With only about two years of data, this should be treated as an indication of seasonality and confirmed with more history.

---

## Q8. What is the total revenue, and how has it changed over time?

<img width="940" height="428" alt="image" src="https://github.com/user-attachments/assets/ac041878-2460-4e0b-a2e7-c74288b31dbb" />


**Data available:** Order Payments (payment value), Orders (purchase date), Calendar.

**Data preparation:** A KPI card shows total sales, and a line chart shows monthly revenue with a **Year slicer** for filtering.

**Result**
- **Total revenue: about 16.01 M.**
- Yearly revenue: 2016 ≈ 0.06 M, 2017 ≈ 7.25 M, 2018 ≈ 8.70 M.
- Revenue grew steadily through 2017 and stayed above 1 M per month for most of 2018.
- **2018 covers only about eight months** and already exceeds full-year 2017 by roughly 20%. The drop in the last months is incomplete data, not a business decline.

**Business implication:** The business is growing strongly year over year. Forecasting and budgeting should use complete periods only, so that partial months do not distort the trend.

---

## Data notes & limitations

- **Two revenue definitions:** Q1 uses **item price** (excluding freight), while Q7 and Q8 use **payment value** (which includes freight). Both are labelled sales in the dashboard, so compare them with care.
- **Incomplete end of data:** September–October 2018 contains almost no orders, and Q3 2018 is only two months.
- **Missing values:** about 610 products have no category, and about 3% of orders have no delivery date (mostly not-yet-delivered or canceled).
- **Reviews:** some orders have multiple reviews, and most review comments are blank.

## Repository contents

- `README.md`: this summary
- `Power BI- Assessment-Geet Kumar.pbip`: Power BI project file (open this in Power BI Desktop)
- `Power BI- Assessment-Geet Kumar.Report/` and `.SemanticModel/`: report and data model folders that the .pbip file needs
- `Power BI Assessment - Geet Kumar.docx`: written assessment

- ## How to open the dashboard

1. Download or clone this repository.
2. Keep the `.pbip` file and its `.Report` and `.SemanticModel` folders together in one folder.
3. Open the `.pbip` file in Power BI Desktop.
