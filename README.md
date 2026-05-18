# 🍕 Mr. Pizza — Sales Forecasting & Inventory Management
### A Business Data Management Case Study

![Python](https://img.shields.io/badge/Python-3.x-blue?logo=python)
![Excel](https://img.shields.io/badge/Microsoft_Excel-217346?logo=microsoft-excel&logoColor=white)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen)
![Data](https://img.shields.io/badge/Data-Primary%20%7C%20Handwritten%20Notebook-orange)

---

## 📌 What This Project Is About

Mr. Pizza is a real, single-operator food shop in **Kathgodam, Uttarakhand**, run entirely by one person — the owner, Mr. Prakash Singh Danu. The shop sells pizzas, burgers, shakes, and beverages through walk-in customers and Zomato.

I visited the shop in person, collected **5 months of handwritten sales data** from the owner's notebook, digitized it, and used data analysis to solve three real business problems the owner was facing.

---

## 🏪 The Business

| Detail | Info |
|---|---|
| Shop Name | Mr. Pizza |
| Location | Kathgodam, Uttarakhand |
| Established | 2023 |
| Owner | Mr. Prakash Singh Danu |
| Channels | Walk-in + Zomato delivery |
| Menu | 54 items across 9 categories |

---

## ❓ The 3 Problems I Solved

**Problem 1 — No way to predict demand**
The owner had no system to know how much to buy each week. He over-purchased in winter and ran out of stock on busy days. Ingredients were going to waste.

**Problem 2 — No inventory management**
He managed all 47 items by guesswork — buying a little of everything, with no idea which items actually drove most of his revenue.

**Problem 3 — No understanding of customer patterns**
He had no insight into which days were busy or slow, and no basis for running any kind of promotion.

---

## 📊 The Dataset

| Metric | Value |
|---|---|
| Data Source | Owner's handwritten sales notebook (primary data) |
| Time Period | September 2023 – January 2024 |
| Total Records | 997 line-item transactions |
| Trading Days | 139 days |
| Unique Items | 54 |
| Total Revenue | ₹1,52,778 |

**Columns in the dataset:**

| Column | Description |
|---|---|
| Date | Transaction date |
| Day | Day of week (auto-filled using TEXT formula) |
| Category | Pizza / Burger / Shake etc (auto via VLOOKUP) |
| Item Name | Name of the item sold |
| Size | Small / Medium / Large |
| Qty | Units sold |
| Unit Price | Price per unit (auto via VLOOKUP from Menu sheet) |
| Total Price | Qty × Unit Price |
| Daily Total | Total revenue for that day (via SUMIF) |

---

## 🔧 Methods Used

### Method 1 — 4-Week Moving Average (MA4) for Forecasting
To solve Problem 1, I used a rolling 4-week average to predict next week's demand.

```
MA4(t) = [ R(t) + R(t-1) + R(t-2) + R(t-3) ] ÷ 4
```

I also derived seasonal adjustment factors from the data:
- **November–December:** reduce forecast by 15% (winter slowdown)
- **January:** increase forecast by 20% (peak month)

**Why MA4 and not ARIMA?** Only 20 weekly data points available — ARIMA requires 2+ years of data. MA4 is also simple enough for the owner to calculate by hand every Friday.

---

### Method 2 — ABC Inventory Classification
To solve Problem 2, I ranked all 47 items by cumulative revenue contribution (Pareto analysis).

```
Share(i) = Revenue(i) ÷ Total Revenue × 100%
```

| Class | Threshold | Items | Revenue | Rule |
|---|---|---|---|---|
| A | Cumulative ≤ 70% | 12 | ₹1,05,854 (69.3%) | Always in stock |
| B | 70% – 90% | 9 | — | Weekly review |
| C | > 90% | 26 | — | Order on demand only |

**Key finding:** All 12 Class A items share just 7 core ingredients — Cheese, Pizza Base, Tomato Sauce, Capsicum, Corn, Onion, Paneer. The owner only needs to monitor 7 ingredients to protect 69% of his revenue.

---

### Method 3 — Day-of-Week Pattern Analysis
To solve Problem 3, I aggregated revenue by day of week across all 139 trading days.

| Day | Revenue | % of Week |
|---|---|---|
| Sunday | ₹36,600 | 24.0% |
| Saturday | ₹26,971 | 17.7% |
| Wednesday | ₹20,729 | 13.6% |
| Friday | ₹19,642 | 12.9% |
| Thursday | ₹19,029 | 12.5% |
| Monday | ₹16,703 | 10.9% |
| Tuesday | ₹13,104 | 8.6% |

Sunday generates **2.8× more revenue than Tuesday.** Weekend (Sat+Sun) = 41.6% of all weekly revenue.

---

## 📈 Key Results

| Finding | Value |
|---|---|
| Total Revenue (5 months) | ₹1,52,778 |
| Pizza's share of revenue | 79.8% |
| Winter revenue decline (Sep→Dec) | 25.4% |
| January recovery | +36.1% from December |
| Top 2 items' revenue share | 22.5% |
| Sunday vs Tuesday revenue | 2.8× difference |
| Class A items | 12 items = 69.3% of revenue |

---

## ✅ Recommendations

**For forecasting (P1):**
Every Friday → average last 4 weeks of revenue → that's next week's procurement budget. Apply -15% in Nov-Dec and +20% in January.
- Expected impact: Save ₹3,000–5,000/month in winter wastage

**For inventory (P2):**
Maintain 2-day buffer of 7 core ingredients for Class A items. Stop pre-buying for 37 Class C items.
- Expected impact: Protect ₹1,05,854 of Class A revenue from stockout risk

**For customer retention (P3):**
Launch Tuesday combo — Kulhad Pizza + Cold Drink at ₹120 (~10% off). Never stock out of Kulhad Pizza (highest volume item: 165 units sold).
- Expected impact: +20% Tuesday revenue within 8 weeks (~₹2,600/month)

**Combined projected impact: ~₹20,000–22,000 over 5 months (≈13–14% of current revenue)**

---

## 🗂️ Project Structure

```
mr-pizza-analysis/
│
├── data/
│   └── Mr_Pizza_Analysis.xlsx     # Full dataset + analysis (5 sheets)
│
├── notebooks/
│   └── analysis.py                # Python charts (Matplotlib + NumPy)
│
├── report/
│   └── Final_Report.pdf           # Full written report
│
└── README.md
```

**Excel file has 5 sheets:**
- `Menu` — Item master list with prices
- `Sales Data` — All 997 transactions with auto-filled formulas
- `Purchase Data` — Ingredient purchase records
- `Summary Analysis` — All 6 analysis tables (monthly, category, ABC, day-of-week, descriptive stats, MA4)
- `Charts` — Data for visualizations

---

## 🛠️ Tools Used

| Tool | Purpose |
|---|---|
| Microsoft Excel / Google Sheets | Data entry, cleaning, VLOOKUP, SUMIF, SUMPRODUCT, Pivot Tables, MA4 calculation |
| Python (Matplotlib, NumPy) | Generating publication-quality charts |
| Primary data collection | In-person visits to the shop, owner interviews |

---

## ⚠️ Limitations

- Only 5 months of data — one seasonal cycle is not enough for full seasonal decomposition
- No customer identification data — anonymous walk-ins, no POS system
- Incomplete purchase records — COGS and gross margins cannot be calculated
- ABC classification is revenue-based, not profit-based
- MA4 was not compared against MA7

---

## 🚀 Future Scope

- ABC-XYZ analysis (demand variability) with 12+ months of data
- Holt-Winters exponential smoothing for better seasonal forecasting
- Free POS app (Vyapar / OkCredit) for automated daily tracking
- WhatsApp CRM for targeted Tuesday promotions
- Full profit-based ABC using COGS data

---

## 👤 Author

**Lakshika Dev**
IIT Madras — Online BS Degree Program
Business Data Management Capstone Project
Roll No: 22F3000223

---

*This project used real primary data collected from an actual small business. All analysis was done on the actual sales records of Mr. Pizza shop, Kathgodam.*
