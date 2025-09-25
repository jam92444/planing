## 📦 Module Name: **Items / Products / Services Management**

---

## ✅ Goal

Enable businesses to **create, view, edit, and delete items** (products or services) they sell — which can later be used for billing/invoicing. This module must adapt based on the **business type** selected during onboarding.

---

## 🧭 Functional Scope

| Function         | Description                                      |
| ---------------- | ------------------------------------------------ |
| List Items       | View all items/services added by tenant          |
| Add Item         | Create a new item with name, price, tax, etc.    |
| Edit Item        | Modify item details                              |
| Delete Item      | Soft delete item from tenant catalog             |
| Categorize Items | Optional: organize items into categories         |
| Search/Filter    | Quickly find items by name, code, category, etc. |

---

## 🧱 Pages (Frontend)

| URL               | Purpose                                        |
| ----------------- | ---------------------------------------------- |
| `/items`          | List all items with search/filter              |
| `/items/new`      | Form to add a new item                         |
| `/items/:id/edit` | Edit existing item                             |
| `/items/:id/view` | (Optional) View item details in read-only mode |

---

## 🔐 Permissions

| Role    | Access                      |
| ------- | --------------------------- |
| Owner   | Full access                 |
| Admin   | Full access                 |
| Cashier | Can view only (for billing) |
| Viewer  | View-only access            |

All data is **tenant-isolated** — users can only manage items within their tenant.

---

## 🧪 Business Type Customization

| Business Type     | Special Fields                              |
| ----------------- | ------------------------------------------- |
| **Retail**        | SKU, Category, Stock                        |
| **Services**      | Duration (optional), Service Category       |
| **Restaurant**    | Item Type (Food/Drink), Tax Inclusive       |
| **Manufacturing** | Cost Price, Units, BOM (Future enhancement) |

---

## 📄 API Endpoints

### 1. `GET /items`

**Fetch all items belonging to tenant**

**Query Parameters:**

- `search` (string)
- `category`
- `limit`, `page`

**Response:**

```json
[
  {
    "id": "item_123",
    "name": "Shampoo 200ml",
    "price": 150,
    "tax_rate": 18,
    "unit": "pcs",
    "category": "Personal Care",
    "type": "product",
    "stock_quantity": 50
  }
]
```

---

### 2. `POST /items`

**Create a new item**

```json
{
  "name": "AC Repair",
  "type": "service",
  "price": 500,
  "tax_rate": 18,
  "unit": "hour",
  "category": "Maintenance"
}
```

📌 Validation:

- Name must be unique per tenant
- Price must be ≥ 0
- Tax must be from allowed rates

---

### 3. `PUT /items/:id`

**Update item details**

```json
{
  "price": 600,
  "category": "Electrical"
}
```

---

### 4. `DELETE /items/:id`

Soft delete an item (set `is_active = false`)

---

### 5. `GET /items/:id`

Fetch full item details for edit/view

---

## 🗃️ Database Schema

### Table: `items`

| Field            | Type                        | Notes                 |
| ---------------- | --------------------------- | --------------------- |
| `id`             | UUID                        | PK                    |
| `tenant_id`      | FK                          | Ensures multi-tenancy |
| `name`           | string                      | Required              |
| `type`           | enum (`product`, `service`) | Required              |
| `price`          | decimal                     | Required              |
| `tax_rate`       | decimal                     | Optional              |
| `unit`           | string                      | e.g. pcs, hour, kg    |
| `category`       | string                      | Optional              |
| `sku`            | string                      | Optional (for retail) |
| `stock_quantity` | integer                     | Optional (for retail) |
| `is_active`      | boolean                     | Soft delete           |
| `created_at`     | timestamp                   |                       |
| `updated_at`     | timestamp                   |                       |

---

## 🧠 Business Logic

- Each tenant has its **own item catalog**.
- You may allow **duplicate item names across tenants**, but not within the same tenant.
- Items marked inactive are **excluded from billing and dropdowns**.
- System should enforce price >= 0, valid tax rates (0, 5, 12, 18, 28%).

---

## 🔍 UI Elements (Frontend Notes)

- Search bar with name, SKU
- Filter by category or item type
- Add/Edit form:

  - Name (required)
  - Price (required)
  - Tax rate (dropdown)
  - Type (product/service)
  - Unit
  - Category
  - Stock (if enabled in settings)

- Toggle visibility (soft delete / archive)

---

## 🧩 Optional Future Features

| Feature          | Description                                 |
| ---------------- | ------------------------------------------- |
| Barcode Support  | Generate or scan barcodes for SKU           |
| Stock Tracking   | Auto deduct stock on billing                |
| Inventory Alerts | Notify low-stock items                      |
| Bulk Import      | CSV import/export items                     |
| Multi-pricing    | Different price levels for wholesale/retail |

---

## ✅ Summary of API Routes

| Method   | Route        | Purpose          |
| -------- | ------------ | ---------------- |
| `GET`    | `/items`     | List items       |
| `POST`   | `/items`     | Add new item     |
| `GET`    | `/items/:id` | View item        |
| `PUT`    | `/items/:id` | Edit item        |
| `DELETE` | `/items/:id` | Soft delete item |

---
