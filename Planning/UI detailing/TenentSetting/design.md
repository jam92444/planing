### 📁 **2. `tenant-settings-ui.md`**

> 🎨 Page layout and field structure for `/settings` section

---

#### 📄 `/settings/business-info`

| Field         | Type     | Notes                         |
| ------------- | -------- | ----------------------------- |
| Business Name | Text     | Required                      |
| Address       | Textarea | Optional                      |
| Phone Number  | Tel      | Optional (used for receipts)  |
| Email Address | Email    | For account communication     |
| GSTIN         | Text     | Optional, validated format    |
| Timezone      | Select   | Dropdown (auto detects local) |

---

#### 📄 `/settings/billing`

| Field                  | Type     | Notes                       |
| ---------------------- | -------- | --------------------------- |
| Invoice Prefix         | Text     | e.g., FCS-                  |
| Invoice Suffix         | Text     | e.g., /2025                 |
| Starting Invoice No.   | Number   | Must be ≥ last used number  |
| Default Tax Rate (%)   | Select   | Dropdown: 0%, 5%, 12%, etc. |
| Currency Code          | Select   | ₹ INR, $ USD, etc.          |
| Invoice Footer Message | Textarea | Optional thank-you note     |

---

#### 📄 `/settings/branding`

| Field           | Type      | Notes                                |
| --------------- | --------- | ------------------------------------ |
| Logo Upload     | File      | Accepts JPG/PNG, 2MB max             |
| Primary Color   | Color     | For UI highlights and invoices       |
| Secondary Color | Color     | Used in charts/buttons               |
| Preview Area    | Component | Shows invoice + UI sample w/ changes |

---

#### 📄 `/settings/users`

| Section         | Type       | Notes                           |
| --------------- | ---------- | ------------------------------- |
| Invite User     | Modal/Form | Email + Role selector           |
| Roles Available | Fixed List | Owner, Admin, Viewer            |
| User Table      | Table      | Name, Email, Role, Status, Edit |

---

#### 📄 `/settings/payments`

| Field                  | Type      | Notes                       |
| ---------------------- | --------- | --------------------------- |
| Accepted Methods       | Checklist | Cash, UPI, Card, Netbanking |
| Default Payment Method | Select    | Must be one of accepted     |

---

#### 📄 `/settings/notifications`

| Field             | Type   | Notes                                |
| ----------------- | ------ | ------------------------------------ |
| Email Alerts      | Toggle | Invoice confirmation, reports, etc.  |
| SMS Notifications | Toggle | Invoice confirmation (if integrated) |
| Daily Report Time | Time   | When to email daily sales summary    |

---

---

### 📁 **3. `tenant-settings-design-details.md`**

> 🧠 UX/UI design intent + behaviors + validations

---

### 🎯 General UI Principles

- Use a **left tab menu** for all `/settings/*` pages
- Use **card-based sections** to split settings cleanly
- Save actions must be sticky or fixed at bottom
- All forms must support **validation** and **auto-save alerts**

---

### 🛠️ Field Validations

| Field          | Rule                              |
| -------------- | --------------------------------- |
| Business Name  | Required                          |
| GSTIN          | Optional, must match valid format |
| Invoice Number | Must be number, ≥ current max     |
| Email          | Must be valid email format        |
| Logo           | Max 2MB, PNG/JPEG only            |

---

### 🖼️ Branding Preview UX

- Live preview of invoice with:

  - Uploaded logo
  - Primary color in headers/titles
  - Footer message rendered

- Preview shown **as static card**, not editable

---

### 🔐 Role-Based Access Behavior

| Role    | Can Modify Settings?     | Can Invite Users? | Can Change Ownership? |
| ------- | ------------------------ | ----------------- | --------------------- |
| Owner   | ✅ All                   | ✅ Yes            | ✅ Yes                |
| Admin   | ✅ Limited (no transfer) | ✅ Yes            | ❌ No                 |
| Viewer  | ❌ No                    | ❌ No             | ❌ No                 |
| Cashier | ❌ No                    | ❌ No             | ❌ No                 |

---

### 📩 User Invitation Flow

1. Admin/Owner opens Invite Modal
2. Enters **Email + Role**
3. App sends invite email with **activation link**
4. User signs up → automatically added to tenant

---

### 🧠 Smart Behavior

- If `timezone` changes, auto-refresh report timestamps
- If `currency` changes, all new invoices adopt new symbol
- Changing `starting invoice number` shows warning:

  > "Invoices must continue sequentially. Are you sure?"

---

### 🧪 Save Feedback

- On save: toast message “✅ Settings updated successfully”
- If error: red alert “❌ Something went wrong. Please retry.”

---

### 💬 Multi-language/Locale (Future-ready)

- All settings labels should use translation keys
- Example: `settings.business_name.label = "Business Name"`

---

Let me know if you also need:

- Wireframe mockups (low fidelity)
- Figma-style component mapping
- Export format JSON for branding themes
- Settings backup/import/export module

I'm ready with those next if you want them.
