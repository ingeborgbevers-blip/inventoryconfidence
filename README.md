# Inventory Confidence Diagnostic

![Power BI Dashboard](Executive%20summary.png)

## Overview

This project explores inventory confidence, stock movement reconciliation, and operational reporting clarity.

Using the public PwC inventory case study dataset, the project was built as a PostgreSQL and Power BI reporting workflow. The analysis focused on whether opening stock, closing stock, purchases, and sales connected clearly enough for management to trust the stock position.

The project demonstrates a realistic business intelligence workflow:

CSV files → PostgreSQL staging schema → SQL reporting views → Power BI dashboards → management case study

The aim was not to prove that the warehouse team had failed. The aim was to show where stock records reconciled, where review was needed, and which assumptions had to be made visible before the report could support confident decisions.

---

## The Problem

Stock reports can look acceptable at first glance while still leaving management unsure whether the numbers are safe to rely on.

In this case study, opening stock, closing stock, purchases, and sales were available as separate reports.

At headline level, the movement almost reconciled:

* 4.22M opening stock quantity
* 4.89M closing stock quantity
* 4.90M expected closing stock
* -2.88K difference to review

The summary difference was relatively small, but the detailed review still identified:

* 2.76K product/location checks that matched expected movement
* 385 product/location checks needing review
* 330 large variance records
* 29 records where movement was recorded without an opening/closing stock comparison
* 26 small variance records

The management question was not:

> Who got this wrong?

It was:

> Which product/location checks already reconcile, which need review, and what assumptions are missing before the report can be trusted?

---

## The Approach

The analysis used PostgreSQL to connect and test the data before presenting it in Power BI.

The core reconciliation logic was:

```text
Opening stock
+ purchases received
- sales recorded
= expected closing stock
```

This was then compared with the actual closing stock count.

The project also separated matched records from review items, because not every stock record needed investigation. This made the reporting more practical for management and warehouse teams.

---

## PostgreSQL Layer

The PostgreSQL database was structured with:

* a `staging` schema for imported CSV tables
* a `reporting` schema for cleaned reporting views
* raw tables preserved for traceability
* cleaned tables/views for typed fields and analysis
* reporting views for stock movement reconciliation
* location-level views for store and city review

Key tables included:

* `beginning_inventory`
* `ending_inventory`
* `purchase_data`
* `purchase_invoices`
* `purchase_price`
* `sales_data`

Key reporting views included:

* opening stock summary
* closing stock summary
* purchases received in 2016
* sales recorded in 2016
* net recorded stock movement
* stock count difference
* inventory confidence diagnostic
* large variance by store/city
* movement recorded without stock snapshot

---

## Power BI Layer

The Power BI report was split into two main views:

### 1. Executive Inventory Confidence View

The executive page answered:

> Do stock counts and recorded movements broadly agree?

It showed:

* opening stock quantity
* closing stock quantity
* expected closing stock
* difference to review
* product/location checks needing review
* confidence flags by review category

This page was designed to give management a clear summary without forcing them into transaction-level detail.

### 2. Warehouse / Location Review View

The warehouse page answered:

> Where should the review start?

It linked exceptions back to location and showed that city totals were useful for scanning, but store-level detail was needed for action.

This was important because some cities contained more than one store. A city-level total could show the pattern, but the store-level view was needed to support a practical review conversation.

---

## Key Findings

## 1. The Summary Position Almost Reconciled

At headline level, the expected closing stock was close to the actual closing stock.

This suggested that the overall reporting picture was not broken beyond use.

However, the detail still mattered.

## 2. 385 Product/Location Checks Needed Review

The diagnostic separated records that reconciled from those that needed attention.

This helped avoid treating all stock data as equally problematic.

## 3. Large Variances Were Concentrated Enough to Review

The review identified 330 large variance records.

This gave management a clearer starting point for investigation.

## 4. Store-Level Detail Was Needed for Action

Location review showed that some city totals covered more than one store.

For operational follow-up, store was the action grain. City was only the grouping layer.

## 5. The Issue Was Better-Connected Evidence, Not Blame

The review did not lead to a single conclusion that the data was wrong.

It showed where the business needed clearer connections between stock counts, purchase movements, sales movements, adjustments, transfers, and reporting definitions.

---

## Recommendations

Recommendations would depend on maturity, budget, and existing systems.

## 1. Low-Cost Process Control

For smaller SMEs without budget for new systems:

* regular stock confidence checks
* agreed cut-off dates
* shared adjustment logs
* clearer stock movement ownership
* recurring exception review

## 2. Connected Reporting Layer

For businesses already using spreadsheets, accounts software, or partial warehouse systems:

* link stock counts to purchases and sales
* create one reconciliation view
* separate matched records from review items
* use SQL, Power Query, or Power BI to connect existing reports

## 3. System Improvement

For businesses ready to improve systems:

* ERP/WMS integration
* consistent master data
* controlled transaction types
* clearer reporting definitions
* role-specific reporting for management, warehouse, purchasing, sales, and finance

---

## Data Limitations

This case study uses a public dataset and should not be treated as a live stock audit.

The analysis does not prove:

* stock loss
* warehouse error
* failed process ownership
* incorrect accounting
* or system failure

It shows where the available reporting data does not fully explain the movement between opening and closing stock.

The dashboard identifies review areas. A real business would still need process knowledge, adjustment records, transfer logs, and system context to explain the causes.

---

## Suggested Business Use

This case study is relevant for SMEs where management needs to understand whether stock counts, purchase records, and sales activity tell the same story.

It is especially relevant for:

* manufacturing
* food production
* wholesale
* warehousing
* distribution
* SAP Business One / ERP environments
* operational finance teams
* purchasing and stock control teams

The project demonstrates how reporting can move from a static stock report to a practical confidence review.

The value is not only in showing the numbers.

The value is in showing which numbers deserve trust, which need review, and what the business can do next.

---

## Tools Used

* PostgreSQL
* SQL
* Power BI Desktop
* Power Query
* Public PwC inventory case study dataset
* GitHub
* PDF case study export

---

## Project Status

Completed as a portfolio case study.

Potential future improvements include:

* adding stock adjustment data
* adding transfer records
* adding returns and write-off data
* testing transaction cut-off rules
* building a monthly stock confidence workflow
* creating an ERP/WMS integration version using real business data where permitted
