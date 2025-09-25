## 🎯 Purpose

Allow authorized users to create, view, and print invoices with item-level details, discounts, tax handling, and payment tracking.

---

## 🧾 Pages

| Page                | URL                  | Access (Role)         |
| ------------------- | -------------------- | --------------------- |
| **New Invoice**     | `/billing/new`       | Owner, Admin, Cashier |
| **Invoice Details** | `/billing/:id/view`  | Owner, Admin, Cashier |
| **Print Invoice**   | `/billing/:id/print` | Owner, Admin, Cashier |
| **Billing History** | `/billing/history`   | Owner, Admin, Cashier |

---

## 🧩 Components Per Page

### ✅ `/billing/new` — **New Invoice Page**

| Component             | Type             | Notes                                        |
| --------------------- | ---------------- | -------------------------------------------- |
| **Customer Selector** | Search dropdown  | Optional (Customer name or ID)               |
| **Item Selector**     | Multi-row input  | Select item → add qty → auto-calc price, tax |
| **Discount Field**    | Input + dropdown | % or Flat; applies to total                  |
| **Tax Type Selector** | Radio            | Inclusive / Exclusive                        |
| **Subtotal / Total**  | Auto-calc fields | Real-time updates                            |
| **Payment Method**    | Dropdown         | Cash, Card, UPI, Credit                      |
| **Notes Field**       | Textarea         | Optional message on invoice                  |
| **Generate Bill**     | Primary Button   | Triggers `POST /billing`                     |

---

### 📄 `/billing/:id/view` — **Invoice Details Page**

| Component                  | Type    | Notes                            |
| -------------------------- | ------- | -------------------------------- |
| **Invoice Metadata**       | Labels  | Invoice #, Date, Customer, etc.  |
| **Item Table**             | Table   | Item Name, Qty, Rate, Tax, Total |
| **Discount & Tax Summary** | Inline  | Show breakdown                   |
| **Payment Method**         | Label   | e.g. "Paid via UPI"              |
| **Download / Print**       | Buttons | Link to `/print` or download PDF |

---

### 🖨 `/billing/:id/print` — **Printable View**

| Section           | Details                              |
| ----------------- | ------------------------------------ |
| **Business Info** | Logo, Name, Address (from settings)  |
| **Invoice Block** | Number, Date/Time, Customer          |
| **Item Table**    | Condensed + clean layout             |
| **Summary**       | Subtotal, Tax, Discount, Grand Total |
| **QR Code**       | Optional (e.g. UPI, invoice link)    |

---

### 📚 `/billing/history` — **Invoice List**

| Column             | Notes                       |
| ------------------ | --------------------------- |
| **Invoice No.**    | Clickable link to view page |
| **Date**           | Timestamp of invoice        |
| **Customer**       | Name or ID                  |
| **Amount**         | Grand Total                 |
| **Payment Method** | Cash, Card, etc.            |

Filters:

- Date range
- Payment method
- Customer name

---

# 🔧 Coming Soon / Optional Enhancements

- Invoice template customization
- Barcode scanner integration
- Split payments and refunds
- Internationalization (multi-currency)
