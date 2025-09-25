## 🧾 Module Name: **Billing / Invoice Generation**

---

## ✅ Goal

Allow users (typically cashiers or admins) to create detailed invoices by selecting items/services, applying tax/discounts, and generating bills for printing or digital sharing.

---

## 🧭 Functional Scope

| Function        | Description                                     |
| --------------- | ----------------------------------------------- |
| Create Bill     | Select items, calculate total, save invoice     |
| View Bill       | View saved bill and payment status              |
| Print/Download  | Generate printable invoice or PDF               |
| Apply Discounts | Flat or percentage discount per invoice         |
| Handle Tax      | Inclusive or exclusive tax calculation          |
| Payment Methods | Track how payment was made (cash/card/UPI/etc.) |
| Auto Numbering  | Maintain sequential invoice numbers per tenant  |

---

## 🧱 Pages (Frontend)

| URL                  | Purpose                   |
| -------------------- | ------------------------- |
| `/billing/new`       | Create a new invoice      |
| `/billing/:id/view`  | View invoice details      |
| `/billing/:id/print` | Print/download invoice    |
| `/billing/history`   | List of all past invoices |

---

## 🔐 Permissions

| Role    | Access         |
| ------- | -------------- |
| Owner   | ✅ Full        |
| Admin   | ✅ Full        |
| Cashier | ✅ Create/View |
| Viewer  | ❌ No access   |

---

## 🧪 Workflow (Billing Flow)

1. User goes to `/billing/new`
2. (Optional) Selects a customer
3. Adds one or more items/services
4. System auto-calculates:

   - Subtotal
   - Tax (per item or overall)
   - Discounts
   - Grand Total

5. Selects payment method
6. Clicks “Generate Bill”
7. Bill is saved via `POST /billing`
8. Redirects to `/billing/:id/view`
9. User can print or download the invoice

---

## 📄 API Endpoints

### 1. `POST /billing`

Creates a new invoice.

```json
{
  "customer_id": "cust_123",
  "items": [
    { "item_id": "item_001", "quantity": 2, "price": 150 },
    { "item_id": "item_002", "quantity": 1, "price": 300 }
  ],
  "tax_type": "inclusive", // or "exclusive"
  "discount": {
    "type": "percentage",
    "value": 10
  },
  "payment_method": "cash", // cash / card / upi / credit
  "notes": "Thank you!"
}
```

---

### 2. `GET /billing`

Returns all invoices with optional filters:

- `from`, `to` (date)
- `payment_status`
- `customer_id`

---

### 3. `GET /billing/:id`

Returns full details of a single invoice.

---

### 4. `GET /billing/:id/print` or `/billing/:id/pdf`

Returns a **printable version** or downloadable PDF.

---

### 5. `DELETE /billing/:id`

Soft delete invoice (optional feature).

---

## 🗃️ Database Schema

### Table: `invoices`

| Field            | Type                     | Notes                             |
| ---------------- | ------------------------ | --------------------------------- |
| `id`             | UUID                     | PK                                |
| `tenant_id`      | FK                       |                                   |
| `invoice_number` | string                   | Auto-generated, e.g. `ABC-000123` |
| `customer_id`    | FK                       | Optional                          |
| `subtotal`       | decimal                  |                                   |
| `tax_total`      | decimal                  |                                   |
| `discount_type`  | string (flat/percentage) |                                   |
| `discount_value` | decimal                  |                                   |
| `grand_total`    | decimal                  |                                   |
| `payment_method` | enum                     |                                   |
| `notes`          | text                     |                                   |
| `created_by`     | user_id                  |                                   |
| `created_at`     | timestamp                |                                   |

---

### Table: `invoice_items`

| Field         | Type              |
| ------------- | ----------------- |
| `id`          | UUID              |
| `invoice_id`  | FK                |
| `item_id`     | FK                |
| `item_name`   | string (snapshot) |
| `unit_price`  | decimal           |
| `quantity`    | int               |
| `total_price` | decimal           |
| `tax_rate`    | decimal           |

---

## 🔢 Invoice Numbering System

- Automatically incremented per tenant
- Configurable prefix (from `/settings`)
- Example: `ABC-00001`, `ABC-00002`, etc.

---

## 📦 Sample Bill Calculation (Logic)

| Item    | Qty | Unit Price | Tax (18%) | Line Total |
| ------- | --- | ---------- | --------- | ---------- |
| Shampoo | 2   | ₹150       | ₹54       | ₹354       |
| Soap    | 1   | ₹50        | ₹9        | ₹59        |

- Subtotal: ₹300
- Tax: ₹63
- Discount (10%): ₹36.3
- Grand Total: ₹326.7 (rounded to ₹327)

---

## 📤 PDF / Print Format Includes:

- Business Name + Logo
- Bill Number
- Date & Time
- Items Table
- Tax Summary
- Discount
- Payment Method
- Total Amount
- QR Code (optional)

---

## 🧩 Optional Add-ons

| Feature           | Description                            |
| ----------------- | -------------------------------------- |
| Multi-currency    | Use settings to show ₹, \$, etc.       |
| QR Code           | Link to online invoice or UPI          |
| Split Payments    | Partial payments support               |
| Invoice Templates | Choose layout/style from `/settings`   |
| Return/Refund     | Future enhancement                     |
| Barcode Scan      | Add item via barcode at billing screen |

---

## ✅ API Summary

| Method   | Route                | Description           |
| -------- | -------------------- | --------------------- |
| `POST`   | `/billing`           | Create invoice        |
| `GET`    | `/billing`           | List invoices         |
| `GET`    | `/billing/:id`       | View invoice          |
| `GET`    | `/billing/:id/print` | Printable view        |
| `DELETE` | `/billing/:id`       | Optional: soft delete |

---
