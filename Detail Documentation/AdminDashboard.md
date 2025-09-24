
## 📘 Module Documentation: **F. Admin Dashboard**

---

## 🧩 1. Purpose

The **Admin Dashboard** is a secure area for **store administrators** to:

* Manage products and inventory
* Configure store-level settings (logo, receipt size, currency, etc.)
* (Optionally) Manage cashier/admin users
* Access sales analytics and top product insights

---

## 🧑‍💼 2. Who is an Admin?

An **Admin** is a privileged user with full access to:

* Product CRUD (Create, Read, Update, Delete)
* Store branding & settings
* Billing history & analytics
* User management (if multi-user enabled)

Other users (e.g., cashiers) will have **limited access**: only billing and bill history.

---

## 🧱 3. Admin Dashboard Overview

| Section             | Features                                                    |
| ------------------- | ----------------------------------------------------------- |
| 🔹 Products         | Add/edit/delete products and variants                       |
| 🔹 Store Settings   | Change logo, currency, tax %, theme color, etc.             |
| 🔹 Users (optional) | Admin/cashier role management                               |
| 🔹 Analytics        | Daily/weekly/monthly sales, top items, inventory low alerts |

---

## 💼 4. Features in Detail

---

### 🔹 A. Product Management

| Feature           | Description                                    |
| ----------------- | ---------------------------------------------- |
| View all products | Paginated table with label\_id, name, variants |
| Add product       | Enter label\_id, name, add variants            |
| Edit product      | Update name, price, or stock per variant       |
| Delete product    | Soft delete or full delete                     |
| Stock adjustment  | Manual stock increase/decrease per variant     |

> Variants include size, price, stock (same structure you've used).

**Backend Collection:** `products`

---

### 🔹 B. Store Settings

| Setting          | Description                        |
| ---------------- | ---------------------------------- |
| Store Name       | Displayed in header and receipts   |
| Logo URL         | Uploaded or pasted image URL       |
| Receipt Size     | Default: 55mm, A5, A4              |
| Currency         | ₹ / \$ / € etc.                    |
| Tax %            | Applied globally to bills          |
| Discount Mode    | Allow fixed or % (or both)         |
| Theme (optional) | Primary color for UI customization |

**Backend Collection:** `stores`

```js
{
  _id: "storeId123",
  name: "Fashion Mart",
  logo_url: "...",
  currency: "₹",
  default_receipt_size: "55mm",
  tax_percent: 5,
  discount_mode: ["percent", "fixed"],
  primary_color: "#1d4ed8"
}
```

---

### 🔹 C. User Management (Optional)

If you want multi-user login (admin/cashier):

| Feature             | Description                    |
| ------------------- | ------------------------------ |
| View users          | List users by role             |
| Add user            | Create new login with role     |
| Change password     | Reset user password            |
| Role assignment     | Admin or cashier               |
| Disable/delete user | Soft-disable or delete account |

**Backend Collection:** `users`

```js
{
  _id: "userId123",
  name: "Ravi",
  email: "ravi@store.com",
  password_hash: "...",
  role: "admin" | "cashier",
  store_id: "storeId123",
  active: true
}
```

> Authentication can use JWT or sessions.

---

### 🔹 D. Sales Analytics

| Metric                 | Description             |
| ---------------------- | ----------------------- |
| Daily/Monthly Sales    | Line or bar chart       |
| Total Sales            | ₹ total this week/month |
| Total Bills            | Number of sale/exchange |
| Top Selling Products   | Based on quantity sold  |
| Low Stock Alerts       | Items below threshold   |
| Return Rate (Optional) | Based on exchanges      |

> This uses the `bills` collection to calculate totals and insights.

---

## 📡 5. Backend APIs

### 🔐 Auth Routes (Optional)

| Method | Endpoint           | Description         |
| ------ | ------------------ | ------------------- |
| `POST` | `/api/auth/login`  | Admin/cashier login |
| `POST` | `/api/auth/logout` | End session         |
| `GET`  | `/api/me`          | Get logged-in user  |

---

### 📦 Product Routes

| Method   | Endpoint                  | Description             |
| -------- | ------------------------- | ----------------------- |
| `GET`    | `/api/products`           | List products           |
| `POST`   | `/api/products`           | Add new product         |
| `PUT`    | `/api/products/:id`       | Update product/variants |
| `DELETE` | `/api/products/:id`       | Delete product          |
| `PATCH`  | `/api/products/:id/stock` | Update stock            |

---

### ⚙️ Store Settings

| Method | Endpoint     | Description                             |
| ------ | ------------ | --------------------------------------- |
| `GET`  | `/api/store` | Get current store settings              |
| `PUT`  | `/api/store` | Update settings (name, logo, tax, etc.) |

---

### 👥 User Management (Optional)

| Method   | Endpoint         | Description             |
| -------- | ---------------- | ----------------------- |
| `GET`    | `/api/users`     | List all users          |
| `POST`   | `/api/users`     | Create new user         |
| `PUT`    | `/api/users/:id` | Update user role/status |
| `DELETE` | `/api/users/:id` | Delete user             |

---

### 📊 Analytics

| Method | Endpoint                      | Description                |
| ------ | ----------------------------- | -------------------------- |
| `GET`  | `/api/analytics/summary`      | Total sales, top products  |
| `GET`  | `/api/analytics/daily`        | Sales by day               |
| `GET`  | `/api/analytics/top-products` | Top-selling items          |
| `GET`  | `/api/analytics/low-stock`    | Products below stock limit |

---

## 💻 6. Frontend Components

| Component                | Description               |
| ------------------------ | ------------------------- |
| `<AdminDashboard />`     | Main dashboard container  |
| `<ProductTable />`       | View + manage products    |
| `<ProductForm />`        | Add/edit form             |
| `<StoreSettings />`      | Logo, currency, tax, etc. |
| `<UserList />`           | (optional) manage users   |
| `<AnalyticsDashboard />` | Charts and stats          |

---

## 🔐 7. Role-Based Access Control (RBAC)

| Page               | Role Required      |
| ------------------ | ------------------ |
| Admin Dashboard    | `admin`            |
| Billing Page       | `admin`, `cashier` |
| Billing History    | `admin`, `cashier` |
| Product Management | `admin` only       |
| Store Settings     | `admin` only       |
| Analytics          | `admin` only       |
| User Management    | `admin` only       |

---

## 📊 8. Analytics UI Suggestions

Use chart libraries like:

* [`recharts`](https://recharts.org/)
* [`chart.js`](https://www.chartjs.org/)
* [`nivo`](https://nivo.rocks/)

Example widgets:

* 📈 Sales Over Time (Line Chart)
* 💰 Total Revenue (Card)
* 👕 Top Products (Bar Chart)
* ⚠️ Low Stock Items (List or Table)

---

## ✅ 9. Success Criteria

| ✅                                                               | Feature |
| --------------------------------------------------------------- | ------- |
| Product CRUD with variants                                      |         |
| Logo, tax, and store settings update                            |         |
| Receipt size and currency are configurable                      |         |
| Admin-only access to dashboard                                  |         |
| Analytics visible for sales, items, inventory                   |         |
| (Optional) User management system                               |         |
| RBAC properly enforced                                          |         |
| All updates persist and reflect in app (billing, receipt, etc.) |         |

---

## 🔮 10. Future Enhancements

| Feature                  | Description                        |
| ------------------------ | ---------------------------------- |
| Product categories       | For better filtering and reporting |
| Multi-store support      | One admin, multiple stores         |
| Import products from CSV | For fast onboarding                |
| Staff activity logs      | Track changes and access           |
| Mobile admin panel       | View insights on mobile            |

---

# ✅ Summary

The **Admin Dashboard** provides the full set of tools for running and managing a store:

* Inventory control
* Store personalization (branding, tax, receipts)
* Optional user management for scaling
* Sales insights & low stock alerts

It connects with all major parts of the system — **billing**, **product management**, **exchange**, **history**, and more.

---
