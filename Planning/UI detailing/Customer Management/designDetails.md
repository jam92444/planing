# UX Rules, Edge Cases, Validations

---

## 🧾 Field Validation Rules

| Field        | Rule                                                     |
| ------------ | -------------------------------------------------------- |
| `name`       | Required, min 2 chars, trimmed                           |
| `phone`      | Optional, but must be unique per tenant (server checked) |
| `email`      | Valid email format (if entered)                          |
| `gst_number` | Optional — if entered, must match GST format (India)     |
| `note`       | Max 1000 chars                                           |

---

## 🔍 Search Behavior (on `/customers`)

- Debounced search on:

  - `name` (partial match)
  - `phone`
  - `email`

- Results update via `GET /customers?search=rahul&page=1`

---

## ⚠️ Error States

| Scenario                        | UI Feedback                                 |
| ------------------------------- | ------------------------------------------- |
| Missing required fields         | “This field is required” (below field)      |
| Phone/email already exists      | “Phone number already exists”               |
| Server/API failure              | Toast: “Could not save customer. Try again” |
| Trying to view deleted customer | Redirect + message: “Customer not found”    |

---

## ✅ Success UX

- After creating:

  - Toast: “Customer added”
  - Redirect to: `/customers/:id/view`

- After editing:

  - Toast: “Customer updated”

- After soft delete:

  - Confirmation modal
  - Status set to **inactive**
  - Customer removed from dropdowns in billing

---

## 💡 Inline Customer Creation (Billing Integration)

- If customer not found in billing dropdown:

  - Show "➕ Add New Customer"
  - Opens a small inline modal with:

    - Name (required)
    - Phone / Email (optional)
    - Save → Adds customer + links to invoice

---

## 🎨 UI Suggestions (Enhancements)

- Customer Tags:

  - Add chips like “VIP”, “Wholesaler”

- Total Spend Badge:

  - Visual tier (Silver/Gold/Platinum) based on spend

- Last Interaction:

  - “Last seen: 5 days ago” (based on last bill)

---

## 🧩 Optional Add-ons (for future versions)

| Feature            | Description                          |
| ------------------ | ------------------------------------ |
| Tags/Labels        | Add colored tags like VIP, B2B, etc. |
| Loyalty Points     | Track and reward regular customers   |
| CSV Import/Export  | Bulk manage customers                |
| Reminders          | Birthdays, inactivity, follow-ups    |
| Communication Logs | See past emails/SMS sent (via CRM)   |

---
