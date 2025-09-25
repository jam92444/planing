## 👤 Module Name: **Customer Management**

---

## ✅ Goal

Allow tenants (businesses) to **create, view, update, and manage customers**, so that bills/invoices can be optionally associated with specific customers for tracking, reporting, and CRM purposes.

This is optional — walk-in customers may not need entry, but regular customers should be saved.

---

## 🧭 Functional Scope

| Function           | Description                                |
| ------------------ | ------------------------------------------ |
| Add Customer       | Create a new customer entry                |
| Edit Customer      | Update customer details                    |
| View Customer      | See purchase history, contact info         |
| Delete Customer    | Soft delete customer (hide from dropdowns) |
| Attach to Invoices | Link customers during bill generation      |
| Search/Filter      | Easily find by name, phone, GST, etc.      |

---

## 🧱 Pages (Frontend)

| URL                   | Purpose                              |
| --------------------- | ------------------------------------ |
| `/customers`          | View/search all customers            |
| `/customers/new`      | Add new customer                     |
| `/customers/:id/edit` | Edit existing customer               |
| `/customers/:id/view` | View full profile & purchase history |

---

## 🔐 Permissions

| Role    | Access              |
| ------- | ------------------- |
| Owner   | ✅ Full             |
| Admin   | ✅ Full             |
| Cashier | ✅ Create/View only |
| Viewer  | ❌ No access        |

---

## 🧪 Customer Use in Billing

During invoice creation:

- You can select an **existing customer**
- Or create a **new one inline**
- Or skip customer (anonymous/walk-in)

If linked, invoice will store:

- Customer name, ID
- Optional: phone/email
- Useful for loyalty, tracking, returns, etc.

---

## 📄 API Endpoints

### 1. `GET /customers`

Optional query params:

- `search=nameOrPhone`
- `limit`, `page`

---

### 2. `POST /customers`

```json
{
  "name": "Rahul Sharma",
  "phone": "9876543210",
  "email": "rahul@example.com",
  "gst_number": "29ABCDE1234F2Z5", // Optional
  "address": "Bangalore, India",
  "note": "VIP customer"
}
```

---

### 3. `GET /customers/:id`

Returns full customer profile and summary.

---

### 4. `PUT /customers/:id`

Update customer details.

---

### 5. `DELETE /customers/:id`

Soft delete (set `is_active = false`).

---

## 🗃️ Database Schema

### Table: `customers`

| Field      | Type      | Description         |
| ---------- | --------- | ------------------- |
| id         | UUID      | PK                  |
| tenant_id  | FK        | Linked to business  |
| name       | string    | Required            |
| phone      | string    | Optional but unique |
| email      | string    | Optional            |
| gst_number | string    | Optional            |
| address    | string    | Optional            |
| note       | string    | Optional            |
| is_active  | boolean   | For soft delete     |
| created_at | timestamp |                     |
| updated_at | timestamp |                     |

---

### (Optional) Table: `customer_invoices`

Auto-linked via billing module.

| Field       | Type |
| ----------- | ---- |
| invoice_id  | FK   |
| customer_id | FK   |

Used for reverse lookup: all purchases by a customer.

---

## 📊 UI Suggestions

- Display total spend by customer
- Show last invoice date
- Filter by high-value or repeat customers
- Quick action: "Create Bill for Customer"

---

## 🔒 Validation Rules

| Field | Rule                                   |
| ----- | -------------------------------------- |
| Name  | Required                               |
| Phone | Optional but must be unique per tenant |
| GST   | Optional, validate format if entered   |
| Email | Optional, must be valid format         |

---

## 🧩 Optional Features for Future

| Feature               | Benefit                                 |
| --------------------- | --------------------------------------- |
| Loyalty points        | Track and reward purchases              |
| Tags/Labels           | Mark customers as VIP, Wholesaler, etc. |
| Bulk import           | Upload CSV of customers                 |
| Birthday reminders    | CRM-style engagement                    |
| Email/SMS integration | Notify about promotions or bills        |

---

## ✅ API Summary

| Method   | Endpoint         | Purpose               |
| -------- | ---------------- | --------------------- |
| `GET`    | `/customers`     | List all customers    |
| `POST`   | `/customers`     | Create new customer   |
| `GET`    | `/customers/:id` | View customer profile |
| `PUT`    | `/customers/:id` | Edit customer         |
| `DELETE` | `/customers/:id` | Soft delete           |

---
