## 🏢 Module Name: **Tenant Settings / Preferences**

---

## ✅ Goal

Allow each tenant (business) to customize their account, billing behavior, branding, and other preferences to tailor the app to their unique needs.

---

## 🧭 Functional Scope

| Feature                  | Description                                      |
| ------------------------ | ------------------------------------------------ |
| Business Info            | Name, address, contact details, GSTIN            |
| Billing Settings         | Invoice numbering format, tax defaults, currency |
| Branding                 | Upload logo, color themes                        |
| User Management          | Manage roles, invite users                       |
| Payment Options          | Configure accepted payment methods               |
| Notification Preferences | Email/SMS alerts, report scheduling              |
| Data Retention           | Archiving, backup settings                       |
| Multi-Currency           | If enabled, set base and secondary currencies    |
| Time Zone                | Set tenant-specific timezone                     |

---

## 🧱 Pages (Frontend)

| URL                       | Purpose                          |
| ------------------------- | -------------------------------- |
| `/settings/business-info` | Business details form            |
| `/settings/billing`       | Invoice numbering, tax, currency |
| `/settings/branding`      | Upload logo, color scheme        |
| `/settings/users`         | Add/edit users and roles         |
| `/settings/payments`      | Configure payment methods        |
| `/settings/notifications` | Email/SMS preferences            |

---

## 🔐 Permissions

| Role    | Access                                   |
| ------- | ---------------------------------------- |
| Owner   | ✅ Full                                  |
| Admin   | ✅ Limited (maybe no ownership transfer) |
| Cashier | ❌ No                                    |
| Viewer  | ❌ No                                    |

---

## 📄 API Endpoints

### 1. `GET /settings/business-info`

Fetch current tenant business info.

---

### 2. `PUT /settings/business-info`

Update tenant business info.

---

### 3. `GET /settings/billing`

Fetch billing-related settings.

---

### 4. `PUT /settings/billing`

Update billing settings like:

- Invoice prefix/suffix
- Starting invoice number
- Default tax rate
- Currency code (₹, \$, etc.)

---

### 5. `GET /settings/branding`

Fetch branding info.

---

### 6. `PUT /settings/branding`

Update logo, colors.

---

### 7. `GET /settings/users`

List tenant users.

---

### 8. `POST /settings/users`

Invite/add user.

---

### 9. `PUT /settings/users/:id`

Update user role or deactivate user.

---

### 10. `DELETE /settings/users/:id`

Remove user from tenant.

---

## 🗃️ Database Schema Highlights

| Table             | Description                                |
| ----------------- | ------------------------------------------ |
| `tenants`         | Stores tenant info & preferences           |
| `tenant_settings` | Key-value store for customizable settings  |
| `users`           | User accounts linked to tenants with roles |

---

## 🔧 Business Logic / Rules

- Invoice numbers must be **unique per tenant**
- User invites send email with signup link
- Only one Owner per tenant (optional transfer feature)
- Settings changes audit logged (optional)
- Timezone affects reports and timestamps

---

## 🎨 UI/UX Notes

- Use wizard or multi-step form for initial setup
- Preview invoice numbering format live
- Show role permissions clearly on user management page
- Upload logo preview before save

---

## ✅ Summary

| Module Area      | API                       | Description                      |
| ---------------- | ------------------------- | -------------------------------- |
| Business Info    | `/settings/business-info` | Name, address, GSTIN             |
| Billing Settings | `/settings/billing`       | Invoice numbering, tax, currency |
| Branding         | `/settings/branding`      | Logo, colors                     |
| User Management  | `/settings/users`         | Manage tenant users & roles      |

---
