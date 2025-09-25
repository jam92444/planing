**Items / Products / Services UI Specification**

---

## 🎯 Purpose

To allow businesses to manage their **product/service catalog**, used during billing. UI should support different configurations based on business type: **Retail**, **Services**, **Restaurant**, **Manufacturing**, etc.

---

## 🧱 Pages Overview

| Page             | URL               | Description                      |
| ---------------- | ----------------- | -------------------------------- |
| **Item List**    | `/items`          | View/search/filter all items     |
| **Add New Item** | `/items/new`      | Form to add a new item           |
| **Edit Item**    | `/items/:id/edit` | Edit existing item               |
| **View Item**    | `/items/:id/view` | Optional detailed read-only view |

---

## 🧩 Page-wise Component Breakdown

---

### 📄 `/items` — **Item List Page**

#### Components:

| Component        | Type      | Notes                                      |
| ---------------- | --------- | ------------------------------------------ |
| Search Bar       | Input     | Search by name or SKU                      |
| Filter Dropdowns | Dropdowns | Filter by Category, Type (Product/Service) |
| Add New Button   | Button    | Redirects to `/items/new`                  |
| Item Table       | Table     | List of all active items                   |

#### Table Columns:

| Column         | Description                         |
| -------------- | ----------------------------------- |
| Name           | Item name                           |
| Type           | Product / Service                   |
| Price          | ₹ + unit (e.g. ₹500 / hour)         |
| Tax Rate       | Shown as %                          |
| Category       | Optional                            |
| Stock Quantity | For Retail only                     |
| Status         | Active / Inactive (if soft-deleted) |
| Actions        | View / Edit / Delete                |

---

### 📝 `/items/new` — **Add New Item Page**

| Field            | Label                  | Type          | Required? | Notes                                               |
| ---------------- | ---------------------- | ------------- | --------- | --------------------------------------------------- |
| `name`           | Item Name              | Text          | ✅        | Unique per tenant                                   |
| `type`           | Type                   | Select        | ✅        | Product / Service                                   |
| `price`          | Price                  | Number        | ✅        | ≥ 0                                                 |
| `tax_rate`       | Tax Rate               | Select        | ✅        | Dropdown: 0%, 5%, 12%, 18%, 28%                     |
| `unit`           | Unit                   | Text          | ✅        | e.g., pcs, kg, hour                                 |
| `category`       | Category               | Dropdown/Text | ❌        | Optional (can be created inline)                    |
| `sku`            | SKU Code               | Text          | ❌        | For retail items only                               |
| `stock_quantity` | Opening Stock Quantity | Number        | ❌        | Only if inventory enabled or business type = retail |

#### Buttons:

- **Save Item** → Calls `POST /items`
- **Cancel** → Returns to item list

---

### 📝 `/items/:id/edit` — **Edit Item Page**

- Same layout as **Add New**, but:

  - Fields are pre-filled
  - Save button becomes **“Update Item”**
  - Calls `PUT /items/:id`

---

### 👁️ `/items/:id/view` — **(Optional) View Item Page**

| Section     | Content                 |
| ----------- | ----------------------- |
| Header      | Name + Type Badge       |
| Pricing     | Price + Tax Rate + Unit |
| Category    | Label                   |
| SKU         | If available            |
| Stock       | If applicable           |
| Status      | Active / Inactive       |
| Created At  | Timestamp               |
| Edit/Delete | Buttons                 |

---

## 🔐 Permissions Matrix

| Role    | `/items` | `new` / `edit` | `view` | `delete` |
| ------- | -------- | -------------- | ------ | -------- |
| Owner   | ✅       | ✅             | ✅     | ✅       |
| Admin   | ✅       | ✅             | ✅     | ✅       |
| Cashier | ✅       | ❌             | ✅     | ❌       |
| Viewer  | ✅       | ❌             | ✅     | ❌       |

---

## 📦 Database Flags (Backend Tip)

- Use `type` column to dynamically render UI
- Use `is_active = false` to **soft-delete**
- Consider adding `business_type` as part of tenant profile to auto-toggle form fields
