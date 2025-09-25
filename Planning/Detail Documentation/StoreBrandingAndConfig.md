# 📘 Module Documentation: Store Branding & Configuration

---

## 🧩 Purpose

This module handles **store-specific customization** that personalizes the billing app for each store (client), including:

- Logo and Store Name (displayed on UI and receipts)
- Currency symbol (₹, \$, etc.)
- Tax & default discount settings
- Optional theme colors (for UI consistency)

The goal is to build a **multi-tenant platform**, where each store sees their own identity and financial preferences — and the platform can scale to multiple clients easily.

---

## 🎯 Use Cases

| Use Case                  | Description                                                           |
| ------------------------- | --------------------------------------------------------------------- |
| White-label branding      | When you onboard a new store, just change their logo, name, tax, etc. |
| Store identity on receipt | Print receipts with correct logo, name, tax, currency                 |
| Dynamic UI setup          | On app load, UI adapts to store's branding                            |
| Financial configuration   | Each store can define tax %, discount type                            |

---

## 🏗️ Features in this Module

| Feature                    | Description                             |
| -------------------------- | --------------------------------------- |
| ✅ Store logo & name       | Show on all pages and receipt           |
| ✅ Currency                | Used in all prices, totals              |
| ✅ Tax %                   | Automatically applied to subtotal       |
| ✅ Default discount type   | "percent" or "fixed"                    |
| ✅ Theme colors (optional) | Set primary button/text color per store |

---

## 🛠️ Data Model (MongoDB)

```js
// models/Store.js
const StoreSchema = new mongoose.Schema({
  name: { type: String, required: true },
  logo_url: { type: String },
  currency: { type: String, default: "₹" },
  tax_percent: { type: Number, default: 0 },
  default_discount_type: {
    type: String,
    enum: ["percent", "fixed"],
    default: "percent",
  },
  theme: {
    primary_color: { type: String, default: "#2563eb" }, // Tailwind blue-600
    secondary_color: { type: String, default: "#f1f5f9" },
  },
  created_at: { type: Date, default: Date.now },
});
```

---

## 📡 API Endpoints

### `GET /api/store/:id`

**Purpose:** Load store configuration on app open

**Response:**

```json
{
  "_id": "store_xyz123",
  "name": "Alpha Fashions",
  "logo_url": "https://cdn.example.com/logo.png",
  "currency": "₹",
  "tax_percent": 5,
  "default_discount_type": "percent",
  "theme": {
    "primary_color": "#2563eb",
    "secondary_color": "#f1f5f9"
  }
}
```

---

### `PUT /api/store/:id`

**Purpose:** Update store settings (admin only)

**Request body:**

```json
{
  "name": "New Store Name",
  "logo_url": "https://cdn.example.com/logo-new.png",
  "currency": "$",
  "tax_percent": 10,
  "default_discount_type": "fixed",
  "theme": {
    "primary_color": "#16a34a",
    "secondary_color": "#fefce8"
  }
}
```

**Response:** Success message or updated store object

---

## 🧠 Frontend Behavior

### ✅ On App Load:

- Call `GET /api/store/:id`
- Store branding data in context or state
- Use it to:

  - Show logo & name in header
  - Use correct currency in cart/receipt
  - Apply tax % automatically
  - Style app with custom colors

---

### 🧾 Receipt Example

```
Alpha Fashions
Logo: [img]
Currency: ₹
Tax: 5%
Discount: 10% (default: percent)
```

---

### 🔧 Store Settings Page (`/settings`)

Only accessible to admins.

| Field                 | Input Type                  |
| --------------------- | --------------------------- |
| Store Name            | Text                        |
| Logo URL              | URL input or image uploader |
| Currency              | Dropdown (₹, \$, €, etc.)   |
| Tax %                 | Number input                |
| Default Discount Type | Dropdown (percent/fixed)    |
| Theme Colors          | Color picker                |

---

## 🔐 Security

- `GET /api/store/:id` is **public** (safe data for display)
- `PUT /api/store/:id` should be **protected** via JWT (admin role)

---

## 🧩 Integration Points

| Location               | Uses store config                                                                              |
| ---------------------- | ---------------------------------------------------------------------------------------------- |
| Header component       | Logo + Name                                                                                    |
| Receipt UI             | Logo, name, currency, tax                                                                      |
| Cart Total Calculation | Apply tax %                                                                                    |
| Discount logic         | Use default type if not specified                                                              |
| Global Theme           | Use primary_color/secondary_color in Tailwind classes (via CSS variables or custom classNames) |

---

## ✅ Success Criteria

| Criteria                          | Description            |
| --------------------------------- | ---------------------- |
| Branding shows on app load        | Based on store ID      |
| Receipt reflects store logo & tax | Accurate preview       |
| Admin can update branding         | Changes persist        |
| Tax is applied correctly          | Matches backend config |
| Currency is consistent            | In cart, bill, history |

---

## 📅 Implementation Order

### Step 1 – Backend:

- [x] MongoDB schema: `Store`
- [x] Routes: `GET`, `PUT` `/api/store/:id`
- [x] Seed one test store

### Step 2 – Frontend:

- [x] On app load, fetch store config
- [x] Show logo + name in `<Header />`
- [ ] Use currency, tax in billing logic
- [ ] Build `/settings` page for admin updates

---

## 🤔 Optional Future Enhancements

| Idea                         | Benefit                                                  |
| ---------------------------- | -------------------------------------------------------- |
| Logo upload (via Cloudinary) | Better UX for admins                                     |
| Theme preview live           | Admin sees how theme changes look                        |
| Multi-language support       | Global rollout                                           |
| Domain-based store loading   | Load branding by domain name (e.g. `store1.billgen.com`) |

---

# ✅ Summary

This module forms the **foundation** of your white-label platform.

It ensures that each store:

- Feels like their own app
- Uses their tax rules and branding
- Can run billing professionally and accurately

Once this is done, you can move to the **Product + Billing system**, using the store configuration as the base for all calculations.
