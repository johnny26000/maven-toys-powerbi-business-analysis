# Maven Toys — Answers to the Four Business Questions

**Source of findings:** Final submitted four-page Power BI PDF (`Maven.pdf`), with values read from its tables and visuals. Reports are shown with slicers set to **All**. The figures are outputs of the user's report, not independent recalculation of the cleaned Power BI model.

## Q1. Which product categories drive the biggest profits? Is this the same across store locations?

**Answer:** The **Toys** category contributes the most absolute gross profit (**$1,079,527**), followed by Electronics (**$1,001,437**). Electronics have the highest gross **margin percentage** at **44.57%**; Toys have a **21.20%** margin. Thus, the category that leads in total profit dollars does not have the best margin efficiency.

### Gross profit by category

| Category | Gross profit | Gross margin |
|---|---:|---:|
| Toys | **$1,079,527** | 21.20% |
| Electronics | $1,001,437 | **44.57%** |
| Art & Crafts | $753,354 | 27.85% |
| Games | $673,993 | 30.27% |
| Sports & Outdoors | $505,718 | 23.28% |
| **Total** | **$4,014,029** | **27.79% overall** |

The category profit amounts above sum the store-location/category values printed in the matrix on PDF page 1; the dashboard's main cards display **$14.44M revenue**, **$4.01M gross profit** and **1,090,565 units sold**.

### Category with the highest gross profit by store type

| Location type | Top category | Gross profit of top category |
|---|---|---:|
| Downtown | Toys | $630,029 |
| Commercial | Electronics | $287,574 |
| Residential | Toys | $136,214 |
| Airport | Electronics | $108,197 |

**Business meaning:** Preferences/profit contribution appear to differ across store formats. Review category assortment by type of location, while accounting for the number of stores in each group; a greater total is not necessarily higher profit **per store**.

**Recommendation:** Track gross profit, gross margin and gross profit per store. Prioritise investigation of product mix rather than selecting one category for every type of store.

**Caveat:** Gross profit is a product-cost measure, not net profit; operating expenses and historical transaction-specific discounts are not demonstrated in the report.

**Evidence:** `Maven.pdf`, page 1 / `screenshots/01-profitability.png`.

---

## Q2. Can we find seasonal trends or patterns in sales?

**Answer:** There are clear month-to-month fluctuations in both sales revenue and units sold over **January 2022–September 2023**. In the grouped month comparison, the 2023 bars appear above the 2022 bars for each comparable **January–September** month, suggesting improvement in matched months. The monthly line charts also show periods of stronger and weaker demand, including a prominent revenue peak in late 2022.

**What can be stated safely:**

- The observed monthly patterns are **descriptive**, not proof that the same changes recur every year.
- Compare **January–September 2022** with **January–September 2023** rather than comparing 12 months to 9 months.
- **YoY** compares the same period with the prior year; **MoM** compares consecutive months.
- The exported PDF has blank YoY/MoM cards for the all-period selection. Without a selected period or a separate matched-period calculation, a numerical growth percentage is not shown in the final export, and **none is claimed here**.
- Revenue and units sold both vary over time; further category-level analysis could test whether peaks are driven by increased volume or a changing product mix.

**Recommendation:** Use consistent year/month filters and category breakdowns, then monitor monthly changes. Collect more years before treating holiday seasonality or other recurring patterns as established facts.

**Caveat:** 21 months of data, with only one full calendar year, is insufficient to establish stable recurring annual seasonality. Do not assert a specific YoY % from an unfiltered card.

**Evidence:** `Maven.pdf`, page 2 / `screenshots/02-sales-trends.png`.

---

## Q3. Are we losing sales with products out of stock at certain locations?

**Answer supported by the data:** The inventory report finds **77 zero-stock product–store records**, or **4.83%** of the recorded inventory combinations. These stockouts affect **37 of 50 stores**, which is **74%** of the stores.

| Store location type | Out-of-stock combinations | Rate within recorded combinations |
|---|---:|---:|
| Residential | 14 | **~7.57%** |
| Downtown | 46 | ~4.97% |
| Commercial | 15 | ~3.85% |
| Airport | 2 | ~2.15% |
| **Total** | **77** | **4.83%** |

The dashboard includes an *Out of Stock Products* detail table and a historical-units-sold column, which can help rank potentially important shortages. For example, where a store previously sold a product frequently and now has zero stock, restocking deserves investigation.

**Direct answer to “are we losing sales?”** The report **cannot establish or quantify lost sales**. It indicates possible risk, because customer demand during the unavailable period and stockout duration are unknown. High historical sales do not by themselves prove missed demand.

**Recommendations:**

1. Examine zero-stock products at stores with high historical product sales.
2. Inspect why Residential store types have the highest relative stockout rate.
3. Record stockout dates, replenishment lead times, and unfulfilled demand to measure actual missed sales later.

**Caveats:** The denominator counts only existing inventory records; missing product–store rows must not automatically be considered zero stock. Rates are **inventory combination rates**, not revenue-loss rates.

**Evidence:** `Maven.pdf`, page 3 / `screenshots/03-stockout-risk.png`. The per-location counts were previously established from the original CSV analysis; the report's chart supports the rate ordering, not those exact counts in its labels.

---

## Q4. How much money is tied up in inventory at the toy stores?

**Answer:** The snapshot contains **29,742 units** across **35 products**, valued at approximately **$300.21K at listed product cost**.

**Formula:** `Inventory value = SUM over inventory records (stock on hand × product cost)`.

### Approximate inventory value by location

| Store-location type | Inventory value |
|---|---:|
| Downtown | **~$169K** |
| Commercial | ~$73K |
| Residential | ~$39K |
| Airport | ~$20K |

The rounded location figures above may not sum exactly to the dashboard total because the labels are shown in thousands. Toys carry the largest category-level inventory value (approximately $100K in the page 4 chart); they are not the only merchandise category in the chain.

The biggest individual product–store inventory line shown is **Lego Bricks at Maven Toys Puebla 3**: **117 units**, valued at **$4,093.83**. That figure is consistent with **117 × $34.99 = $4,093.83**.

**Recommendation:** Compare stock value by category and location with historical sales and gross profit to investigate capital allocation. Assess inventory turnover and stock coverage when reliable periods are available. High stock value alone does not indicate overstocking.

**Caveat:** Product cost multiplied by on-hand quantity is a cost-based **estimate** and is not necessarily the official accounting carrying value. The inventory file provides a snapshot, not historical monthly balances.

**Interpretation of the wording:** “At the toy stores” means **all locations and merchandise categories** belonging to Maven Toys, not only the `Toys` product category.

**Evidence:** `Maven.pdf`, page 4 / `screenshots/04-inventory-valuation.png`.

---

## Combined business insight

The four analyses suggest Maven Toys should assess **product mix, sales demand, inventory availability and capital allocation together**. High gross profit in Toys, high margins in Electronics, shortages across 37 stores, and ~$300K in recorded inventory capital give management a set of operational questions to investigate. They do **not** establish lost sales or prove that any store is overstocked.
