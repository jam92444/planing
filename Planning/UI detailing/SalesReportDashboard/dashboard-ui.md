# ✅ `dashboard-ui.md` — Detailed UI Layout & Interaction

---

## 🔝 Page: `/dashboard`

### 📌 Purpose:

Show at-a-glance business health for today / this week / this month.

---

### 🧩 Widgets Layout (Top to Bottom)

| Widget Name           | Type       | Description                             |
| --------------------- | ---------- | --------------------------------------- |
| **Date Range Filter** | Dropdown   | Today / This Week / This Month / Custom |
| Total Sales           | Metric Box | ₹ amount of all billed sales            |
| Invoices Today        | Metric Box | # of invoices for selected date range   |
| Avg. Invoice Value    | Metric Box | Total sales / invoice count             |
| Outstanding Payments  | Metric Box | # and ₹ of unpaid invoices              |
| Sales Chart           | Line/Bar   | Date-wise sales trend                   |
| Category-wise Revenue | Pie Chart  | Revenue distribution by item categories |
| Top 5 Selling Items   | Table      | Sorted by quantity or revenue           |

---

### 🖱️ Widget Interactions

| Action                        | Behavior                                                  |
| ----------------------------- | --------------------------------------------------------- |
| Click on "Top Items" row      | Opens `/reports/top-selling-items` with pre-applied range |
| Click on "Category Pie Slice" | Opens `/reports/category-sales` for that category         |
| Change date range             | Reloads all widgets dynamically                           |

---

### 🧰 Frontend Components

- `MetricCard` – for Total Sales, Invoice Count, etc.
- `ChartCard` – re-usable chart widget (line, bar, pie)
- `TableCard` – tabular data (top items)

s
