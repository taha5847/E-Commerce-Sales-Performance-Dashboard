# 📊 E-Commerce Sales & Order Performance Dashboard

> **Turning 1,200 raw orders into a clear story: where the money comes from, and where it leaks.**


*Data Analytics Internship · Project 4: Data Visualization · DecodeLabs (Batch 2026)*

---

## 📌 Project Overview

This project is a **4-page interactive Power BI dashboard** built on e-commerce order data (Jan 2023 to Jun 2025). It goes beyond "making charts": every page is designed around **one message**, an **action title**, and a closing **"So What?"** recommendation, following a data-storytelling approach.

> **Headline finding:** the business booked **$1.26M** in revenue, but **41% of orders were cancelled or returned**, which puts about **$520K** at risk. Like-for-like (Jan to Jun) revenue has also fallen about **10% each year**.

## 🎯 Business Questions Answered

1. How is the business performing overall, and is it growing?
2. Which products and order sizes drive revenue?
3. Why are orders being cancelled or returned, and where is the biggest loss?
4. Which marketing channels, coupons and payment methods actually work?

## 🖼️ Dashboard Preview

> 📷 *Add your screenshots to an `images/` folder and keep these names (or edit the paths).*

| Page 1: Executive Summary | Page 2: Product Performance |
|---|---|
| ![Page 1](images/page1_executive_summary.png) | ![Page 2](images/page2_product_performance.png) |

| Page 3: Fulfillment & Returns | Page 4: Marketing & Payments |
|---|---|
| ![Page 3](images/page3_fulfillment_returns.png) | ![Page 4](images/page4_marketing_payments.png) |

## 🧭 Dashboard Pages

| # | Page | Question it answers | Main visuals |
|---|---|---|---|
| 1 | **Executive Summary** | How are we doing overall? | 5 KPI cards, revenue trend (MoM %), orders by status, revenue by product and referral source |
| 2 | **Product Performance** | Which products make money? | KPI cards, revenue by product, monthly revenue by product, units sold, revenue by price segment |
| 3 | **Fulfillment & Returns** | Where are we losing orders? | Delivered / Cancelled / Returned %, Revenue at Risk, order status over time, status by payment method and referral source |
| 4 | **Marketing & Payments** | What drives sales? | Top referral source, coupon usage %, orders by payment method and quantity segment, coupon × referral matrix |

Every page includes a **Page Navigator** and **Year / Product / Order Status** slicers, and ends with a **"What we should do next"** box.

## 💡 Key Insights

- 💰 **$1.26M revenue** from **1,200 orders** and **1,189 customers**; average order value is about **$1,054**.
- ⚠️ **41% of orders are Cancelled or Returned**, which is roughly **$520K of revenue at risk**.
- 📉 **Jan to Jun revenue:** $287K (2023) → $257K (2024) → $232K (2025), about **-10% per year**.
- 🔺 The cancel + return rate rose from **39% (2024)** to **47% (2025)**.
- 🏆 **Chair** and **Printer** lead at about **$196K** each; **Phone** trails at about **$152K** despite the highest average unit price (~$375).
- 💎 **High-value orders (over $1,500) are 27% of orders but 56% of revenue.**
- 📱 **Instagram** is the best channel: most revenue (**$275K**) and the lowest failure rate (**37%**). Email and Facebook fail most (~43%).
- 💳 **Online** payments fail least (**35%**) versus **44%** for Credit Card and Gift Card.
- 🏷️ About **74% of orders use a coupon**, yet average order value barely changes ($1,036 to $1,070 versus $1,043 with no coupon).

> ℹ️ Differences between products, channels and coupons are only a few percentage points on about 150 to 300 orders each. Treat them as **hypotheses to test**, not proven causes. The strongest signals are the overall failure rate, the rise in 2025 and the yearly revenue decline. 2025 contains Jan to Jun only.

## ✅ Recommendations (the "So What?")

1. **Fix order failures first**: investigate the 2025 spike and review Credit Card / Gift Card payment flows.
2. **Scale Instagram** and review Email, which has high revenue but a high failure rate.
3. **Grow high-value orders** with bundles and upsells, and protect them because they carry most of the revenue at risk.
4. **Rethink coupons**: they do not lift basket size; keep SAVE10 (37% fail) and review WINTER15 (45% fail).
5. **Test Phone pricing or bundles**: highest price, lowest revenue.

## 🎨 Design Principles Applied

Built around the Project 4 blueprint (*Choose with Purpose, Edit with Precision, Tell a Story*):

- **5-second rule:** KPIs sit in the top-left; each page has one clear message.
- **Action titles and the SCR framework** (Situation, Complication, Resolution) on every page.
- **Right chart for the question:** bars for comparison, lines for trends, 100% stacked bars for status mix, a matrix for coupon × channel. No pie charts and no 3D.
- **Axis integrity:** bar and column axes start at zero.
- **High data-ink ratio:** light gridlines, direct labels, transparent visuals on custom backgrounds.
- **Color as a spotlight:** grey for context, one blue accent (`#2563EB`) for the insight.
- **Custom theme and backgrounds** for a consistent, professional look (navy `#14213D`, grey `#6B7280`, blue `#2563EB`).

## 🛠️ Tools & Skills

- **Power BI Desktop**: data modelling, DAX, report design, themes, Page Navigator, slicers
- **Power Query**: data cleaning and type fixes
- **DAX**: KPI and time-intelligence measures
- **Data storytelling and dashboard design**

## 🗂️ Dataset

| Item | Detail |
|---|---|
| File | `dataset_cleaned.csv` |
| Rows | 1,200 orders (1,189 unique customers) |
| Period | Jan 2023 to Jun 2025 |
| Columns | OrderID, CustomerID, Date, Product, Quantity, UnitPrice, TotalPrice, PaymentMethod, OrderStatus, CouponCode, ReferralSource, ItemsInCart, ShippingAddress, PriceSegment, QuantitySegment |
| Products | Chair, Printer, Laptop, Tablet, Monitor, Desk, Phone |
| Order statuses | Delivered, Shipped, Pending, Cancelled, Returned |

**Derived fields**
- `PriceSegment`: Low (under $500), Medium ($500 to $1,500), High (over $1,500) based on order total
- `QuantitySegment`: Low (1 to 2 items), Medium (3 to 4), High (5)

## 🧹 Data Preparation

1. Converted `Date` to a proper Date type and set numeric types (Whole number / Fixed decimal).
2. Replaced the corrupted `Year` column with a **Date Table** (`CALENDAR`) marked as the date table, related to the orders on `Date`.
3. Sorted `Month` by `Month Number` and used `Year-Month` for clean time axes.
4. Added price and quantity segments for deeper analysis.

## 🧮 Key DAX Measures

```DAX
Revenue = SUM(dataset_cleaned[TotalPrice])

MoM % =
VAR Curr = SUM(dataset_cleaned[TotalPrice])
VAR Prev = CALCULATE(SUM(dataset_cleaned[TotalPrice]), PREVIOUSMONTH('Date Table'[Date]))
RETURN DIVIDE(Curr - Prev, Prev)

Cancel & Return Rate =
DIVIDE(
    CALCULATE(COUNTROWS(dataset_cleaned), dataset_cleaned[OrderStatus] IN {"Cancelled", "Returned"}),
    COUNTROWS(dataset_cleaned)
)

Revenue At Risk =
CALCULATE(SUM(dataset_cleaned[TotalPrice]), dataset_cleaned[OrderStatus] IN {"Cancelled", "Returned"})

Coupon Usage % =
DIVIDE(
    CALCULATE(COUNTROWS(dataset_cleaned), dataset_cleaned[CouponCode] <> "No Coupon"),
    COUNTROWS(dataset_cleaned)
)
```

Other measures: `Total Orders`, `Avg Order Value`, `Average Items in Cart`, `Unit Sold`, `Products Sold`, `Avg Unit Price`, `Top Product Revenue`, `Delivered %`, `Cancelled %`, `Returned %`, `Top Referral Source`, `Unique Customers`.

## 📁 Repository Structure

```
├── Ecommerce_Sales.pbix        # Power BI dashboard
├── data/
│   └── dataset_cleaned.csv     # Cleaned dataset
├── images/                     # Dashboard screenshots
├── theme/
│   └── E-Commerce_Sales_Dashboard_Theme.json
└── README.md
```

## ▶️ How to Use

1. Download or clone this repository.
2. Open `Ecommerce_Sales.pbix` in **Power BI Desktop**.
3. If prompted, update the data source path: **Transform data → Data source settings → Change source** to `data/dataset_cleaned.csv`.
4. Click **Refresh** and explore using the Page Navigator and slicers.

## 📚 What I Learned

- Designing dashboards around a **single message per page** instead of showing every chart possible.
- Writing **action titles** and "So What?" recommendations that lead to a decision.
- Building a proper **Date Table** and time-intelligence measures such as MoM %.
- Using **color and layout** to guide attention and keep visuals honest.
- Being clear about **data limits**: partial-year data and small sample sizes.

## 👤 Author

**Taha Parvaiz** · Data Analytics Intern at DecodeLabs

🔗 [LinkedIn](www.linkedin.com/in/taha-parvaiz-939705321) · 💻 [GitHub](https://github.com/taha5847/E-Commerce-Sales-Performance-Dashboard/edit/main/)

## 🙏 Acknowledgements

Thanks to **DecodeLabs** for the Data Analytics internship and the Project 4 (Data Visualization) brief.

---

⭐ If you found this project useful, please give the repo a star!
