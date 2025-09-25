## 📊 Module Name: **Reports & Dashboard**

---

## ✅ Goal

Provide business owners and admins with actionable insights from billing, tax, and product sales. The module must show key metrics and trends to help track performance and make decisions.

---

## 🧭 Functional Scope

| Area                 | Description                                 |
| -------------------- | ------------------------------------------- |
| 📆 Sales Reports     | Daily, weekly, monthly totals with filters  |
| 🧾 Tax Reports       | Tax collected per rate (e.g. 5%, 12%, etc.) |
| 🛒 Top Items         | Best-selling items by revenue or quantity   |
| 📈 Dashboard Metrics | Quick stats: total sales, revenue, payments |
| 📂 Category Reports  | Revenue per category                        |
| 🧍 Customer Reports  | Who spends the most, etc. (optional)        |

---

## 🧱 Pages

| URL                          | Description                           |
| ---------------------------- | ------------------------------------- |
| `/dashboard`                 | Summary view for daily/weekly metrics |
| `/reports/sales`             | Detailed sales breakdown with filters |
| `/reports/tax`               | Tax summary grouped by rate           |
| `/reports/top-selling-items` | View top 10 or top N items            |
| `/reports/category-sales`    | Sales grouped by item category        |

---

## 🔐 Permissions

| Role    | Access       |
| ------- | ------------ |
| Owner   | ✅ Full      |
| Admin   | ✅ Full      |
| Cashier | ❌ None      |
| Viewer  | ✅ Read-only |

---

## 📄 API Endpoints

### 1. `GET /dashboard-metrics`

Returns quick dashboard stats:

```json
{
  "total_sales": 325000,
  "total_bills": 154,
  "daily_sales": 22000,
  "pending_payments": 2,
  "top_items": [{ "name": "Shampoo", "quantity": 40, "revenue": 6000 }]
}
```

---

### 2. `GET /reports/sales?from=2025-09-01&to=2025-09-30`

Returns all invoice totals between two dates.

```json
[
  {
    "date": "2025-09-01",
    "total_sales": 10500,
    "invoice_count": 12
  },
  ...
]
```

---

### 3. `GET /reports/top-selling-items?limit=5`

Returns top N items based on revenue or quantity.

```json
[
  {
    "item_name": "AC Repair",
    "total_sold": 50,
    "revenue": 25000
  }
]
```

---

### 4. `GET /reports/revenue-summary?from=...&to=...`

```json
{
  "total_sales": 82000,
  "discount_given": 5000,
  "tax_collected": 12000,
  "net_revenue": 75000
}
```

---

### 5. `GET /reports/tax`

Group invoices by tax rates:

```json
[
  {
    "tax_rate": 18,
    "tax_collected": 4500,
    "taxable_amount": 25000
  },
  {
    "tax_rate": 5,
    "tax_collected": 800,
    "taxable_amount": 16000
  }
]
```

---

## 📊 Dashboard Widgets (UI Elements)

| Widget                       | Description                             |
| ---------------------------- | --------------------------------------- |
| **Total Sales**              | Total revenue for today/week/month      |
| **Invoices Today**           | # of invoices created today             |
| **Outstanding Payments**     | Bills marked unpaid (if credit allowed) |
| **Top 5 Items**              | Quick view of best-sellers              |
| **Sales Chart**              | Line or bar chart over time             |
| **Category Sales Pie Chart** | Revenue split by item categories        |
| **Avg. Invoice Value**       | Total / # of invoices                   |

---

## 🧮 Business Logic

- **Time zone aware**: All reports must respect tenant's local timezone
- **Filtered by tenant_id**
- **Exclude deleted invoices**
- **Data must refresh in real time or on-demand**

---

## 🗃️ Supporting Database Queries

You’ll use aggregated queries on:

- `invoices`
- `invoice_items`
- `items`
- `categories`
- `customers` (optional)

---

## 📌 Report Exporting (optional)

Allow CSV/PDF export of:

- Sales reports
- Tax summary
- Top-selling items

APIs:

- `GET /reports/sales/export`
- `GET /reports/tax/export`

---

## 🧩 Optional Future Features

| Feature            | Description                                     |
| ------------------ | ----------------------------------------------- |
| Compare Periods    | Compare sales week-over-week or year-over-year  |
| Custom Charts      | Let users configure charts/widgets              |
| Customer Loyalty   | Top customers by spend                          |
| Forecasting        | Use past data to predict future sales           |
| Real-time Alerting | Notify on unusual trends (e.g. 0 sales by noon) |

---

## ✅ API Summary

| Method | Route                        | Description            |
| ------ | ---------------------------- | ---------------------- |
| `GET`  | `/dashboard-metrics`         | Dashboard numbers      |
| `GET`  | `/reports/sales`             | Sales by date range    |
| `GET`  | `/reports/top-selling-items` | Top items              |
| `GET`  | `/reports/revenue-summary`   | Overall totals         |
| `GET`  | `/reports/tax`               | Tax collected per rate |
| `GET`  | `/reports/category-sales`    | Revenue per category   |
