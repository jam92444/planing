### 🧮 Billing UX Rules

- **Tax Handling:**

  - _Inclusive:_ Tax is part of item price
  - _Exclusive:_ Tax is added to subtotal

- **Discount Handling:**

  - Flat: subtracted as fixed value
  - Percentage: applied on subtotal before tax

---

### ✳️ Validation Rules

- At least one item required to generate bill
- Quantity must be > 0
- Discount cannot exceed subtotal
- Payment method is required

---

### 🧾 Invoice Numbering UX

- Auto-generated based on tenant
- Format: `PREFIX-000001`
- Configurable from `/settings`

---

### 🧯 Error States

| Error                  | UI Feedback                     |
| ---------------------- | ------------------------------- |
| Empty item row         | “Please select an item”         |
| Discount > total       | “Discount cannot exceed amount” |
| Invalid payment method | “Select valid payment method”   |
| API error on save      | Toast: “Failed to save invoice” |

---

### ✅ Success States

- Toast: `Invoice generated (#ABC-00042)`
- Redirect: `/billing/ABC-00042/view`
- Download triggered if user clicks “Download PDF”

---

# 📦 Module Summary

| Feature                | Description                           |
| ---------------------- | ------------------------------------- |
| **Item-based billing** | Add multiple products/services        |
| **Tax + Discount**     | Per item and invoice-wide             |
| **Payment Tracking**   | Store how payment was made            |
| **Invoice Numbers**    | Auto-generated per tenant             |
| **Print-ready**        | Styled output with QR code (optional) |
| **Access Roles**       | Viewer 🚫, Others ✅                  |

---
