# Business-Analysis-Management-system
# Sales Analysis with SQL

A set of MySQL business-analysis queries that turn a raw sales table (`sales_analysis`) into executive KPIs, category/region performance, trend analysis, customer and product rankings, discount impact, and data-quality checks.

## What's inside

```
sales-analysis-sql/
├── README.md
├── REPORT.md                      # Analysis report (methodology + findings template)
└── queries/
    └── business_analysis.sql      # All analysis queries
```

## Expected table schema

The queries assume a table named `sales_analysis` with these columns:

| Column | Description |
|---|---|
| `OrderID`, `CustomerID`, `ProductID` | Identifiers |
| `CustomerName`, `ProductName` | Descriptive names |
| `Segment` | Customer segment |
| `Region` | Sales region |
| `Category`, `SubCategory` | Product hierarchy |
| `OrderDate`, `ShipDate` | Dates |
| `Quantity` | Units per order line |
| `Sales`, `Profit` | Revenue and profit per order line |
| `Discount` | Discount as a fraction (0.20 = 20%) |

## Queries

| # | Purpose | Techniques |
|---|---|---|
| 1 | Validate imported data (row count) | `COUNT` |
| 2 | Executive KPIs: orders, customers, units, sales, profit, margin | `COUNT(DISTINCT)`, `NULLIF` |
| 3 | Sales and profit by category | `GROUP BY` |
| 4 | Sales, profit and margin by region | `GROUP BY`, margin calc |
| 5 | Monthly sales and profit trend | `DATE_FORMAT` |
| 6 | Top 10 products by sales | `GROUP BY`, `LIMIT` |
| 7 | Top 10 customers by sales | `GROUP BY`, `LIMIT` |
| 8 | Sub-categories making a loss | `HAVING` |
| 9 | Discount band vs profitability | `CASE` bucketing |
| 10 | Customer segment analysis | `COUNT(DISTINCT)` |
| 13 | Top 3 products by profit within each category | CTEs, `DENSE_RANK() OVER (PARTITION BY ...)` |
| 14 | Data-quality checks (nulls, negatives, invalid quantities) | Boolean aggregation |

## Requirements

- MySQL 8.0+ (needed for window functions and CTEs in query 13)
- A `sales_analysis` table loaded with your data

## How to run

```bash
# 1. Load your data into MySQL as the sales_analysis table
# 2. Run the whole file
mysql -u <user> -p <database> < queries/business_analysis.sql

# Or run a single query from the MySQL shell / VS Code SQL extension
```

## Notes

- Revenue and profit figures are rounded to 2 decimals.
- Profit margin uses `NULLIF(SUM(Sales), 0)` to avoid divide-by-zero errors.
- Run query 14 first on new data. Missing dates or negative values will skew the trend and margin results.
- Queries 11 and 12 are placeholders in this version.

## License

MIT
