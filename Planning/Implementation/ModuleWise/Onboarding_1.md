## 🧭 **Module Name**: Onboarding / Business Setup

---

## ✅ **Goal**

Allow a **new business** to register on the platform, choose their **business type**, and configure their **initial settings** such as branding, tax details, and invoice preferences.

This process results in the creation of:

- A **Tenant**
- An **Owner User**
- A **Business Profile**
- Initial configurations based on business type

---

## 📌 **Flow Overview**

### Step-by-Step User Journey:

1. **Visit Signup Page (`/signup`)**
2. **Submit:**

   - Name
   - Email
   - Password
   - (Optional) Mobile

3. **Verify Email (optional OTP/email verification step)**
4. **Select Business Type**

   - Retail
   - Services
   - Restaurant
   - Manufacturing
   - Others (custom)

5. **Fill Business Details**

   - Business name
   - GST/VAT ID (optional)
   - Currency
   - Country/Region
   - Time zone
   - Upload logo

6. **Configure Preferences**

   - Tax: Inclusive/Exclusive
   - Default invoice prefix
   - Enable stock tracking?

7. **Create Default Settings**

   - Create default categories
   - Set tax rates

8. **Redirect to Dashboard or Items Setup**

---

## 🛠️ **Pages**

| URL                         | Purpose                             |
| --------------------------- | ----------------------------------- |
| `/signup`                   | Collects initial user info          |
| `/onboarding/business-type` | Business type selection             |
| `/onboarding/setup`         | Full business profile & preferences |
| `/onboarding/success`       | Confirmation & redirect             |

---

## 🔐 Roles Created

| Role           | Description                                |
| -------------- | ------------------------------------------ |
| Owner          | First user – full admin                    |
| Admin (future) | Can manage everything but tenant settings  |
| Staff (future) | Restricted to bill/invoice generation only |

---

## 🧱 **Backend API Design**

### 1. `POST /auth/signup`

```json
{
  "name": "Sanjay",
  "email": "sanjay@abc.com",
  "password": "supersecure123"
}
```

📌 Creates:

- `User` record (status: pending or active)
- Triggers email verification (optional)

---

### 2. `POST /tenant/create`

```json
{
  "user_id": "abc123",
  "business_name": "ABC Supermart",
  "business_type": "retail",
  "country": "IN",
  "currency": "INR"
}
```

📌 Creates:

- `Tenant` record
- Assigns user as `Owner`
- Sets default preferences based on business type

---

### 3. `POST /tenant/settings/init`

```json
{
  "tax_enabled": true,
  "default_tax_rate": 18,
  "invoice_prefix": "ABC",
  "stock_tracking": true
}
```

📌 Creates:

- Settings profile
- Optionally pre-populates tax rates, invoice templates

---

### 4. `GET /tenant/onboarding-status`

Used to check if onboarding is complete (for conditional routing).

---

## 🗃️ Data Models

### 🏢 Tenant

| Field         | Type                              |
| ------------- | --------------------------------- |
| id            | UUID                              |
| name          | string                            |
| business_type | enum (`retail`, `services`, etc.) |
| country       | string                            |
| currency      | string                            |
| logo_url      | string                            |
| created_at    | timestamp                         |

---

### 👤 User

| Field         | Type                             |
| ------------- | -------------------------------- |
| id            | UUID                             |
| tenant_id     | FK                               |
| name          | string                           |
| email         | string                           |
| password_hash | string                           |
| role          | enum (`owner`, `admin`, `staff`) |
| status        | active/pending                   |
| verified      | boolean                          |

---

### ⚙️ TenantSettings

| Field            | Type    |
| ---------------- | ------- |
| tenant_id        | FK      |
| tax_enabled      | boolean |
| default_tax_rate | number  |
| invoice_prefix   | string  |
| stock_tracking   | boolean |
| invoice_template | string  |
| timezone         | string  |

---

### 📂 Business Type Presets

You may use a static config or DB table for these presets:

| business_type | default_categories         | default_tax | notes                  |
| ------------- | -------------------------- | ----------- | ---------------------- |
| retail        | Electronics, FMCG, Apparel | 18%         | Stock enabled          |
| services      | Consulting, Maintenance    | 0% or 18%   | No stock tracking      |
| restaurant    | Food, Drinks               | 5% or 12%   | Tax inclusive pricing  |
| manufacturing | Raw Material, Labor        | Varies      | May need cost tracking |

---

## 🧪 Validation & Edge Cases

| Case                           | Handling                             |
| ------------------------------ | ------------------------------------ |
| Duplicate email                | Reject signup                        |
| No business type selected      | Cannot proceed                       |
| GST ID invalid                 | Show warning (not required to block) |
| Same user in multiple tenants? | Optional multi-tenant user mode      |

---

## 🧭 Final Outcome After Onboarding

Once onboarding completes:

✅ Tenant is created
✅ First user (owner) is created
✅ Business type is stored
✅ Default settings and tax rates are initialized
✅ Redirect to Dashboard or Items page

---

## 🎯 Future Enhancements (Optional)

- AI prompt to auto-detect business type from name
- Business category suggestions
- Clone presets from successful tenants
- Auto-configure invoice format (B2B/B2C templates)
