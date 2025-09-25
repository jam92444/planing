# 📤 `export-config.md` — CSV / PDF Export Structure

---

## ✅ CSV Columns for Sales Report

| Column          | Notes                    |
| --------------- | ------------------------ |
| Date            | Billing date             |
| Invoice Count   | Number of bills that day |
| Gross Revenue   | Pre-discount, pre-tax    |
| Discounts Given | Discount applied         |
| Tax Collected   | GST included             |
| Net Revenue     | Final ₹ collected        |

---

## ✅ CSV Columns for Tax Report

| Column        | Notes                 |
| ------------- | --------------------- |
| GST Rate      | 0%, 5%, 12%, 18%, 28% |
| Taxable Amt   | Amount before tax     |
| Tax Collected | Total GST collected   |

---

## ✅ PDF Export:

- Header: Business logo, address
- Title: Report type + date range
- Table format as above
- Footer: Page number, printed on {date}

Export triggers:

- Button: “📤 Export CSV”
- Button: “🧾 Export PDF”

---
