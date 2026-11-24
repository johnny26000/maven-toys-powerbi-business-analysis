# DAX Measures, Modelling Approach and Interpretation

These definitions document the calculation logic used during development of the Maven Toys project. They are intended for learning and reproducibility; the included `.pbix` is the source of truth for the exact saved measure names and final model state.

## Relationship model

- `products[Product_ID]` (one) → `sales[Product_ID]` (many)
- `stores[Store_ID]` (one) → `sales[Store_ID]` (many)
- `products[Product_ID]` (one) → `Inventory[Product_ID]` (many)
- `stores[Store_ID]` (one) → `Inventory[Store_ID]` (many)
- `Date[Date]` (one) → `sales[Date]` (many)

These relationships allow Product and Store filters to affect both sales and inventory. The Date table filters **sales**, not historical inventory balances; inventory records are a snapshot.

## Profitability

```dax
Total_Revenue =
SUMX(
    'sales',
    'sales'[Units] * RELATED('products'[Product_Price])
)
```

Each sales record's `Units` is multiplied by the related product's listed selling price, then summed.

```dax
Gross_Profit =
SUMX(
    'sales',
    'sales'[Units] *
    (RELATED('products'[Product_Price]) - RELATED('products'[Product_Cost]))
)
```

Gross margin is profit divided by revenue, usually using `DIVIDE`:

```dax
Gross_Margin =
DIVIDE([Gross_Profit], [Total_Revenue])
```

**Assumption:** Catalogue prices and product costs are used as proxy values for transaction dates; the report does not establish transaction-specific price changes or discount history.

## Sales trends and growth

Example monthly and yearly comparisons:

```dax
Revenue_Previous_Month =
CALCULATE(
    [Total_Revenue],
    DATEADD('Date'[Date], -1, MONTH)
)

Revenue_MoM_Pct =
DIVIDE(
    [Total_Revenue] - [Revenue_Previous_Month],
    [Revenue_Previous_Month]
)

Revenue_Previous_Year =
CALCULATE(
    [Total_Revenue],
    SAMEPERIODLASTYEAR('Date'[Date])
)

Revenue_YoY_Pct =
DIVIDE(
    [Total_Revenue] - [Revenue_Previous_Year],
    [Revenue_Previous_Year]
)
```

These are **illustrative DAX definitions**, not a byte-for-byte export of the model's DAX. Format growth as Percentage in Power BI; a result of `0.31` means `31%`. To prevent misleading all-period cards, evaluate a clearly selected year or month, and use matched calendar periods because 2023 ends in September.

## Stockouts

```dax
StockOut =
CALCULATE(
    COUNTROWS('Inventory'),
    'Inventory'[Stock_On_Hand] = 0
)

StockOut_Rate =
DIVIDE(
    [StockOut],
    COUNTROWS('Inventory')
)

Stores_with_Stockouts =
CALCULATE(
    DISTINCTCOUNT('Inventory'[Store_ID]),
    'Inventory'[Stock_On_Hand] = 0
)
```

- `StockOut` counts **zero-stock inventory records** (product–store pairs if the pair key is unique).
- `StockOut_Rate` divides these rows by **all records in current filter context**, so a Stock Status slicer may change the denominator to only out-of-stock records and make it show 100%.
- `Stores_with_Stockouts` counts **unique** affected stores, not individual zero-stock items.
- The out-of-stock result is not a calculation of missed orders or lost revenue.

Optional descriptive category column:

```dax
Stock Status =
SWITCH(
    TRUE(),
    ISBLANK('Inventory'[Stock_On_Hand]), "Unknown",
    'Inventory'[Stock_On_Hand] = 0, "Out of Stock",
    'Inventory'[Stock_On_Hand] <= 5, "Low Stock",
    "In Stock"
)
```

The low-stock cutoff of five units is an **illustrative rule**, not a validated policy of the business.

## Inventory valuation

```dax
Inventory_Value_At_Cost =
SUMX(
    'Inventory',
    'Inventory'[Stock_On_Hand] * RELATED('products'[Product_Cost])
)
```

For each inventory row, `RELATED` fetches the unit product cost; `SUMX` multiplies it by recorded stock and adds the results. **Create this as a measure** with *New measure*, not as a calculated column if you want it to summarise dynamically by slicers.

Inventory valuation should be shown in **USD**, consistent with the dataset's product prices and costs. It is a cost-based snapshot estimate, not the expected selling value or an inventory holding-cost expense.

## Recommended validation checks

1. The selected Product and Store dimensions should correctly filter `sales` and `Inventory`.
2. On the unfiltered snapshot: `StockOut = 77`, `StockOut_Rate ≈ 4.83%`, `Stores_with_Stockouts = 37`.
3. The inventory total displays `Stock on Hand = 29,742`, `Inventory Cost Value ≈ $300.21K`.
4. The profitability matrix category gross-profit cells sum to `$4,014,029`.
5. YoY and MoM use a **specific date period** and never confuse the whole of 2022 with the first nine months of 2023.
6. A visual-level filter for `Out of Stock` on the detailed product table should not inadvertently change the overall KPI denominator.
