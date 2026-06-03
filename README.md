# NexPharma Sales Dashboard — Power BI Portfolio Project

A multi-page Power BI sales dashboard built for the senior leadership team of a pharmaceutical distributor in Malaysia. The dashboard provides a single source of truth across sales, product, and field activity data — replacing manual reporting with a self-serve, refreshable reporting system.

> **Note:** All data in this repository is anonymised sample data. No real customer, product, or transaction data is included.

---

## Project Overview

**Client:** Pharmaceutical distributor (confidential)
**Role:** Web & Application Consultant
**Tools:** Power BI Desktop · Power Query · Zoho CRM · Google Drive · Excel

The client was operating without a centralised reporting system. Sales data lived in Zoho CRM, inventory in Zoho Inventory, and invoices were tracked manually. This project consolidated all data sources into a single refreshable dashboard.

---

## Dashboard Pages

| Page | Description |
|------|-------------|
| Sales Health | Overall revenue, order volume, and invoice status trends |
| Product Performance | Sales by SKU, brand, and product category |
| Sales Rep Activity | Rep-level visit frequency, order conversion, and coverage by region |

---

## Data Architecture

Data flows through three layers before reaching the dashboard:

```
Zoho CRM (source)
    ↓ CSV export
01_raw/          ← raw exports, untouched
    ↓ Power Query
02_staging/      ← cleaned, typed, renamed
    ↓ Power Query
03_facts/        ← dimensional model (fact + dimension tables)
    ↓
Power BI visuals
```

This dimensional model separates facts (transactions, visits) from dimensions (customers, products, sales reps), making measures and filters reliable across all pages.

---

## Data Sources

All source data is exported from Zoho CRM using saved export templates. Five core tables feed the dashboard:

| File | Description |
|------|-------------|
| `Sales_Order.csv` | Sales orders with line-item detail |
| `Invoice_YYYY_MM.csv` | Monthly invoice files (appended, not replaced) |
| `Credit_Note.csv` | Credit notes linked to invoices |
| `Item.csv` | Product master — SKU, brand, price, stock |
| `Contacts.csv` | Customer master — type, region, assigned rep |
| `Meetings.csv` | Field visit log — check-in location, purpose, brands discussed |

Sample data matching the real schema is included in `Data_Input/01_raw/`.

---

## Folder Structure

```
powerbi-nexpharma-dashboard/
├── Data_Input/
│   └── 01_raw/
│       ├── Invoices/
│       │   └── Invoice_2026_01.csv
│       ├── Sales_Order.csv
│       ├── Credit_Note.csv
│       ├── Item.csv
│       ├── Contacts.csv
│       └── Meetings.csv
├── Data_Update_Guide.txt
├── Zoho_Export_Guide.txt
├── PowerBI_Installation_Guide.txt
├── Nexpharma Google Drive Shared Folder Setup Guide.pdf
└── README.md
```

---

## Documentation

Full operational documentation is included in this repo:

- **Data_Update_Guide.txt** — monthly refresh process (export → replace → refresh → save)
- **Zoho_Export_Guide.txt** — exact fields and templates for each Zoho export
- **PowerBI_Installation_Guide.txt** — Power BI Desktop setup for end users
- **Google Drive Setup Guide** — shared folder structure and sync configuration for multi-user access

---

## Key Technical Decisions

- **Local PBIX + shared Google Drive data** — keeps the dashboard fast while allowing one data owner to update files for all users
- **Append-only invoice files** — invoice history is preserved by adding new monthly files rather than overwriting, maintaining a full audit trail
- **Strict file naming conventions** — Power Query paths are fixed; consistent naming prevents refresh failures across all user machines
- **Dimensional model in Power Query** — separates transformation logic from visuals, making the dashboard easier to maintain and extend
