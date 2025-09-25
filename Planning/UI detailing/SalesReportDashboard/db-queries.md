# 🧠 `db-queries.md` — Backend Aggregation Summary

---

### 🔸 `/dashboard-metrics`

**Tables Involved:** `invoices`, `invoice_items`

```sql
SELECT
  SUM(grand_total) AS total_sales,
  COUNT(*) AS total_bills,
  SUM(CASE WHEN DATE(created_at) = CURRENT_DATE THEN grand_total ELSE 0 END) AS daily_sales
FROM invoices
WHERE tenant_id = ? AND is_deleted = false;
```

---

### 🔸 `/reports/tax`

```sql
SELECT
  tax_rate,
  SUM(tax_amount) AS tax_collected,
  SUM(taxable_amount) AS taxable_amount
FROM invoice_items
WHERE tenant_id = ? AND is_deleted = false
GROUP BY tax_rate;
```

---

### 🔸 `/reports/top-selling-items`

```sql
SELECT
  item_name,
  SUM(quantity) AS total_sold,
  SUM(quantity * unit_price) AS revenue
FROM invoice_items
WHERE tenant_id = ? AND created_at BETWEEN ? AND ?
GROUP BY item_name
ORDER BY total_sold DESC
LIMIT 10;
```

---

### 🔸 `/reports/category-sales`

Join `invoice_items` with `items` and group by `category`.

```sql
SELECT
  items.category,
  SUM(invoice_items.quantity * invoice_items.unit_price) AS revenue
FROM invoice_items
JOIN items ON items.id = invoice_items.item_id
WHERE invoice_items.tenant_id = ? AND items.is_active = true
GROUP BY items.category;
```

---
