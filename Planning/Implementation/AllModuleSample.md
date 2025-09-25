## 🔧 Modules, Pages, APIs, and Flow (Detailed)

### 🌐 1. **Onboarding / Business Setup**

**Scenario**: A new business signs up and selects its business type (e.g., retail, service, food).

#### **Pages**

- `/signup`
- `/onboarding/business-type`
- `/onboarding/setup`

#### **Steps**

1. User signs up (name, email, password)
2. Chooses business type
3. Fills business details (GST, logo, currency, etc.)

#### **APIs**

- `POST /auth/signup`
- `POST /tenant/create` → `business_type`, `name`, `tax_settings`
- `GET /tenant/settings`
- `PUT /tenant/settings/update`

#### **Data Fields**

```json
{
  "business_name": "ABC Retail",
  "business_type": "retail",
  "currency": "INR",
  "tax_enabled": true,
  "tax_percentage": 18,
  "logo_url": "...",
  "industry_specific_fields": { ... }
}
```

---

### 🧑‍💼 2. **User Auth + Role Management**

**Scenario**: Each business (tenant) can have multiple users (admin, cashier, manager).

#### **Pages**

- `/login`
- `/users` (admin only)
- `/users/invite`

#### **APIs**

- `POST /auth/login`
- `GET /users`
- `POST /users/invite`
- `PUT /users/:id/role`
- `DELETE /users/:id`

#### **Roles**

- **Admin**: All access
- **Cashier**: Create bills only
- **Manager**: Can see reports
- **Owner**: Superadmin

---

### 📦 3. **Items/Products/Services Module**

**Scenario**: Businesses add items they sell or services they provide.

#### **Pages**

- `/items`
- `/items/new`
- `/items/:id/edit`

#### **APIs**

- `GET /items`
- `POST /items`
- `PUT /items/:id`
- `DELETE /items/:id`

#### **Fields**

```json
{
  "name": "Shampoo",
  "price": 150,
  "tax_rate": 18,
  "unit": "pcs",
  "category": "Personal Care"
}
```

Retail: SKU, stock
Services: Hourly rate, service category

---

### 🚗 4. **Bill Generation Module**

**Scenario**: Create a new bill, add items, scan customer (if any), print/download.

#### **Pages**

- `/billing/new`
- `/billing/:id/view`
- `/billing/:id/print`

#### **Steps**

1. Select customer (optional)
2. Add line items
3. Auto-calculate total, tax
4. Mark payment (cash/card/UPI)
5. Save and download/print invoice

#### **APIs**

- `GET /billing`
- `POST /billing`
- `GET /billing/:id`
- `PUT /billing/:id`
- `DELETE /billing/:id`

#### **Fields**

```json
{
  "customer_id": "optional",
  "items": [{ "item_id": "123", "quantity": 2, "price": 100 }],
  "tax_total": 36,
  "discount": 10,
  "grand_total": 226,
  "payment_status": "paid"
}
```

---

### 📊 5. **Reports & Dashboard**

**Scenario**: Admins and managers check sales reports and analytics.

#### **Pages**

- `/reports/sales`
- `/reports/tax`
- `/dashboard`

#### **APIs**

- `GET /reports/sales?from=...&to=...`
- `GET /reports/top-selling-items`
- `GET /reports/revenue-summary`
- `GET /dashboard-metrics`

#### **Widgets**

- Total Sales
- Daily Revenue
- Outstanding Payments
- Top Items
- Sales by Category

---

### 👥 6. **Customer Management (Optional)**

**Scenario**: Businesses can associate invoices with customers.

#### **Pages**

- `/customers`
- `/customers/new`

#### **APIs**

- `GET /customers`
- `POST /customers`
- `PUT /customers/:id`
- `DELETE /customers/:id`

#### **Fields**

```json
{
  "name": "Rahul Sharma",
  "phone": "9876543210",
  "email": "rahul@gmail.com",
  "gst_number": "optional"
}
```

---

### 📥 7. **PDF Export / Print Module**

**Scenario**: After generating bill, user can print or download as PDF.

#### **Pages**

- `/billing/:id/print` → printable layout

#### **APIs**

- `GET /billing/:id/pdf`

Can use tools like:

- `pdfmake`
- `puppeteer`
- `jspdf`

---

### ⚙️ 8. **Tenant & Settings Module**

**Scenario**: Each tenant can update their settings.

#### **Pages**

- `/settings/general`
- `/settings/tax`
- `/settings/billing-template`

#### **APIs**

- `GET /tenant/settings`
- `PUT /tenant/settings`

---

## Example Use Flow: **Retail Shop**

> **Goal**: Park vehicle → Scan code → Add items → Generate bill → Print → Report

### Steps:

1. User logs in as cashier
2. Goes to `/billing/new`
3. Adds items (e.g., Water Bottle, Snacks)
4. System auto-calculates totals + tax
5. Clicks “Generate Bill”
6. System calls `POST /billing`
7. Redirects to `/billing/:id/view`
8. User clicks “Print” → opens `/billing/:id/print`
9. Report gets updated automatically
10. Manager views sales from `/reports/sales`

---

## ✅ Final Summary: Pages + APIs

| Page                 | API                        | Role           |
| -------------------- | -------------------------- | -------------- |
| `/signup`, `/login`  | `POST /auth/*`             | Public         |
| `/dashboard`         | `GET /dashboard-metrics`   | All users      |
| `/items`             | `GET, POST, PUT /items`    | Admin, Manager |
| `/billing`           | `POST, GET /billing`       | Cashier, Admin |
| `/billing/:id/print` | `GET /billing/:id/pdf`     | All            |
| `/reports/sales`     | `GET /reports/sales`       | Manager, Admin |
| `/settings`          | `GET/PUT /tenant/settings` | Owner only     |
