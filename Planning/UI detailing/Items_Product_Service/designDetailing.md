# UX Logic & Behavior

---

## 🧠 Dynamic Form Behavior (Business Type Based)

| Business Type     | Field Overrides or Conditions                                 |
| ----------------- | ------------------------------------------------------------- |
| **Retail**        | Show `SKU`, `Stock Quantity`                                  |
| **Services**      | Hide `SKU`, `Stock` — Show `Duration` (optional future field) |
| **Restaurant**    | Add toggle: **Tax Inclusive** checkbox                        |
| **Manufacturing** | Future: Cost Price, BOM (hidden for now)                      |

---

## 🔍 Search & Filter Logic

- Search matches:

  - Item name (partial, case-insensitive)
  - SKU (exact or partial)

- Filter:

  - `type` → Dropdown (Product / Service)
  - `category` → Dropdown
  - `status` → Active / Inactive

---

## ✅ Field Validation Rules

| Field            | Rule                                        |
| ---------------- | ------------------------------------------- |
| `name`           | Required, unique per tenant                 |
| `price`          | Required, must be ≥ 0                       |
| `tax_rate`       | Must be from allowed list: 0, 5, 12, 18, 28 |
| `unit`           | Required                                    |
| `stock_quantity` | Must be ≥ 0 (if applicable)                 |
| `sku`            | Optional, but must be unique if present     |

---

## ⚠️ Error States & Feedback

| Scenario                    | UI Feedback                                                   |
| --------------------------- | ------------------------------------------------------------- |
| Missing required fields     | "This field is required" below input                          |
| Duplicate name              | "Item name already exists"                                    |
| Invalid price or tax        | "Price must be ≥ 0", "Invalid tax rate"                       |
| Delete item used in invoice | Warn user: “Item is in use in past invoices” (optional logic) |

---

## 🧩 Smart Features

- **Soft Delete:**

  - Changes `is_active = false`
  - Item disappears from billing dropdown
  - Can optionally be restored via filter in `/items`

- **Category Management:**

  - Inline dropdown + add-new (modal or small input)
  - Optionally manage via `/settings/items/categories`

- **Tax Rate Dropdown:**

  - Pre-filled: 0%, 5%, 12%, 18%, 28%
  - Can be customized via settings in future

---

## 🔁 Optional Enhancements

| Feature          | UX Detail                                                     |
| ---------------- | ------------------------------------------------------------- |
| Barcode Scan     | Input SKU by scanning (auto-fills item)                       |
| Bulk Import      | Upload CSV to bulk add/edit items                             |
| Low-Stock Alerts | Show red badge in list view when `stock_quantity < threshold` |
| Multi-pricing    | Allow different prices for B2B/B2C customers (future)         |
| Inventory Sync   | Deduct stock on billing (if stock tracking enabled)           |

---
