# 📥 `dummydata.json` — Sample Payloads for Widgets & Reports

---

### 🔹 `/dashboard-metrics`

```json
{
  "total_sales": 525000,
  "total_bills": 230,
  "daily_sales": 21000,
  "pending_payments": 3,
  "top_items": [
    { "name": "AC Repair", "quantity": 42, "revenue": 25200 },
    { "name": "Haircut", "quantity": 30, "revenue": 9000 }
  ]
}
```

---

### 🔹 `/reports/sales`

```json
[
  { "date": "2025-09-01", "total_sales": 10800, "invoice_count": 13 },
  { "date": "2025-09-02", "total_sales": 14500, "invoice_count": 15 },
  ...
]
```

---

### 🔹 `/reports/top-selling-items`

```json
[
  { "item_name": "Facial", "total_sold": 40, "revenue": 28000 },
  { "item_name": "Hair Spa", "total_sold": 22, "revenue": 19800 }
]
```

---

### 🔹 `/reports/revenue-summary`

```json
{
  "total_sales": 105000,
  "discount_given": 7000,
  "tax_collected": 12800,
  "net_revenue": 98000
}
```

---

### 🔹 `/reports/tax`

```json
[
  { "tax_rate": 18, "tax_collected": 8500, "taxable_amount": 47222 },
  { "tax_rate": 5, "tax_collected": 1500, "taxable_amount": 30000 }
]
```

---
