# **Customer Management UI Specification**

---

## 🎯 Purpose

Allow tenants to manage their customer base — add new customers, search and view existing ones, update info, and optionally link them to invoices for CRM and reporting.

---

## 🧱 Pages Overview

| Page                 | URL                   | Description                     |
| -------------------- | --------------------- | ------------------------------- |
| **Customer List**    | `/customers`          | Searchable list of customers    |
| **Add New Customer** | `/customers/new`      | Form to add a new customer      |
| **Edit Customer**    | `/customers/:id/edit` | Edit form with prefilled data   |
| **View Customer**    | `/customers/:id/view` | Full profile + purchase history |

---

## 🧩 Page-wise Component Breakdown

---

### 📄 `/customers` — **Customer List Page**

#### Components:

| Component      | Type       | Notes                                             |
| -------------- | ---------- | ------------------------------------------------- |
| Search Bar     | Input box  | Supports name, phone, or email search             |
| Filters        | Dropdowns  | Filter by: Active/Inactive                        |
| Customer Table | Table      | Shows name, phone, email, last invoice date, etc. |
| Add New Button | CTA button | Redirects to `/customers/new`                     |

#### Table Columns:

| Column       | Description                         |
| ------------ | ----------------------------------- |
| Name         | Full name of customer               |
| Phone        | If available                        |
| Email        | Optional                            |
| GST Number   | Optional                            |
| Total Spend  | Based on past invoices (optional)   |
| Last Invoice | Date of most recent invoice         |
| Status       | Active/Inactive                     |
| Actions      | View / Edit / Delete (soft) buttons |

---

### 📝 `/customers/new` — **Add New Customer Page**

| Field        | Label         | Type     | Validation                     | Optional? |
| ------------ | ------------- | -------- | ------------------------------ | --------- |
| `name`       | Customer Name | Text     | Required                       | ❌        |
| `phone`      | Phone Number  | Text     | Unique per tenant              | ✅        |
| `email`      | Email Address | Email    | Must be valid email            | ✅        |
| `gst_number` | GST Number    | Text     | Format validation (if entered) | ✅        |
| `address`    | Address       | Textarea | -                              | ✅        |
| `note`       | Notes         | Textarea | Internal use                   | ✅        |

#### Buttons:

- **Save Customer** → Creates customer (`POST /customers`)
- **Cancel / Back** → Returns to `/customers`

---

### 📝 `/customers/:id/edit` — **Edit Customer Page**

- Same as **Add New** layout
- Fields are **pre-filled** with current data
- Button: `Update Customer` (calls `PUT /customers/:id`)

---

### 👤 `/customers/:id/view` — **Customer Profile Page**

| Section       | Type       | Content                                           |
| ------------- | ---------- | ------------------------------------------------- |
| Header Info   | Labels     | Name, Phone, Email, GST, Address                  |
| Notes         | Text       | Custom notes or comments                          |
| Status        | Badge      | "Active" or "Inactive"                            |
| Total Spend   | Label      | Based on billing data                             |
| Last Invoice  | Label      | Invoice ID + Date                                 |
| Invoices List | Table/List | Past invoices with links                          |
| Quick Actions | Buttons    | “Create New Bill”, “Edit Customer”, “Soft Delete” |

---

## 🔐 Permissions & Access Control (UX Level)

| Role    | `/customers` | `new` / `edit` | `view` |
| ------- | ------------ | -------------- | ------ |
| Owner   | ✅ Full      | ✅             | ✅     |
| Admin   | ✅ Full      | ✅             | ✅     |
| Cashier | ✅ View/List | ✅ Create only | ✅     |
| Viewer  | ❌ No Access | ❌             | ❌     |

---
