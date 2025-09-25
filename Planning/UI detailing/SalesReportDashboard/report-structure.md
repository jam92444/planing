# 📄 `report-structure.md` — Detailed Report Views & Filters

---

## 📆 `/reports/sales`

### Purpose: View sales trends by date

| Element        | Type                               | Notes                         |
| -------------- | ---------------------------------- | ----------------------------- |
| Date Range     | Filter                             | Required (from/to)            |
| Invoice Status | Filter                             | All / Paid / Unpaid (if used) |
| Table Columns  | Date, Invoice Count, Total Sales ₹ |                               |
| Chart          | Line/Bar                           | Sales over time               |

---

## 🧾 `/reports/tax`

### Purpose: Tax collected grouped by GST rate

| Column         | Description                  |
| -------------- | ---------------------------- |
| Tax Rate (%)   | GST slab (e.g. 5%, 12%, 18%) |
| Taxable Amount | Amount before tax            |
| Tax Collected  | GST amount collected         |

---

## 🛒 `/reports/top-selling-items`

| Filter        | Notes                     |
| ------------- | ------------------------- |
| Sort by       | Quantity Sold / Revenue ₹ |
| Limit (Top N) | Default 10, customizable  |
| Date Range    | Required                  |

| Table Columns | Item Name, Qty Sold, Revenue ₹ |
| Optional Chart | Bar chart |

---

## 📂 `/reports/category-sales`

| Purpose       | Analyze revenue by item category |
| ------------- | -------------------------------- |
| Table Columns | Category, Revenue, % of total    |
| Pie Chart     | Visual category comparison       |

---
