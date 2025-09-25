Perfect — here's a **detailed design brief for your designer** covering all 4 onboarding pages. It includes **form fields, UI elements, user interactions**, and **UX notes** for each page, so your designer can build accurate and user-friendly screens.

---

# 🎨 **Onboarding UI Design Specification Document**

---

## ✅ Page 1: **Signup Page**

**📍URL:** `/signup`

### 🎯 Purpose:

Capture initial user details to create an account and start onboarding.

---

### 🧩 Components:

| Element                      | Type             | Notes                                                      |
| ---------------------------- | ---------------- | ---------------------------------------------------------- |
| **Page Title**               | Text/Header      | e.g., “Create Your Account” or “Get Started”               |
| **Full Name**                | Input (text)     | Placeholder: "John Doe"                                    |
| **Email**                    | Input (email)    | Validation: Must be valid email format                     |
| **Password**                 | Input (password) | Add show/hide toggle, min 8 chars, with strength indicator |
| **Mobile (optional)**        | Input (number)   | Optional, but validated for digits                         |
| **Terms & Privacy Checkbox** | Checkbox         | “I agree to the Terms and Privacy Policy”                  |
| **Signup Button**            | Button (primary) | Label: "Create Account"                                    |
| **Already have an account?** | Link             | Link to `/login`                                           |
| **Social Signup (Optional)** | Buttons          | Google / Apple buttons if supported                        |

---

### 🧪 Validation & Error States:

- Show inline errors (e.g., "Invalid email", "Password too short")
- On duplicate email: “This email is already registered”
- Disable “Signup” until all required fields are valid

---

### ✨ UX Notes:

- Add loader or spinner after clicking signup
- Smooth transition to next page (`/onboarding/business-type`)
- Consider animated background or branding on the right side

---

## 🏢 Page 2: **Business Type Selection**

**📍URL:** `/onboarding/business-type`

### 🎯 Purpose:

Let user choose their business category to personalize setup.

---

### 🧩 Components:

| Element                 | Type                  | Notes                                                        |
| ----------------------- | --------------------- | ------------------------------------------------------------ |
| **Page Title**          | Header                | e.g., “What type of business are you setting up?”            |
| **Description**         | Subtext               | Short paragraph on why you’re asking                         |
| **Business Type Cards** | Selectable Cards/Grid | Options: Retail, Services, Restaurant, Manufacturing, Others |
| **Next Button**         | Button (primary)      | Disabled until selection is made                             |

---

### 🖼️ UI Design Ideas:

- Use icons or illustrations on business type cards
- Highlight selected card with a border or tick mark
- Tooltip or hover notes for each type (optional)

---

### ✅ Business Types:

| Type          | Icon Idea          | Tooltip Example                 |
| ------------- | ------------------ | ------------------------------- |
| Retail        | 🛒 Shopping Cart   | Selling physical goods or items |
| Services      | 🛠️ Wrench          | Offering consulting or labor    |
| Restaurant    | 🍽️ Plate & Cutlery | Serving food or beverages       |
| Manufacturing | 🏭 Factory         | Producing/manufacturing items   |
| Others        | ❓ Question mark   | Custom or niche business        |

---

## 🧾 Page 3: **Business Setup & Preferences**

**📍URL:** `/onboarding/setup`

### 🎯 Purpose:

Collect full business details and initial configuration preferences.

---

### 🔄 Consider breaking into **sections/tabs** if the form is long.

---

### 🧩 Section 1: Business Info

| Field              | Type         | Notes                                       |
| ------------------ | ------------ | ------------------------------------------- |
| **Business Name**  | Input (text) | Required                                    |
| **GST/VAT ID**     | Input (text) | Optional, validate format per country       |
| **Country/Region** | Dropdown     | Auto-select based on IP (optional)          |
| **Currency**       | Dropdown     | Autofill based on country                   |
| **Time Zone**      | Dropdown     | Based on country                            |
| **Logo Upload**    | File upload  | Preview thumbnail, allow image formats only |

---

### 🧩 Section 2: Preferences

| Field                     | Type            | Notes                                                |
| ------------------------- | --------------- | ---------------------------------------------------- |
| **Tax Enabled**           | Toggle (yes/no) | Label: “Is tax applicable for your pricing?”         |
| **Tax Type**              | Radio group     | Inclusive / Exclusive (only shown if tax is enabled) |
| **Default Tax Rate (%)**  | Input (number)  | Default to suggested rate based on business type     |
| **Invoice Prefix**        | Input (text)    | Example: “ABC”                                       |
| **Enable Stock Tracking** | Toggle          | Only shown if business type supports inventory       |

---

### 🧩 Section 3: (Optional Default Setup)

| Field                         | Type                              | Notes                  |
| ----------------------------- | --------------------------------- | ---------------------- |
| **Create Default Categories** | Toggle                            | Based on business type |
| **Set Default Tax Rates**     | Auto-applied or customizable list |                        |

---

### 🧪 Validation:

- Required fields must be filled before proceeding
- GST/VAT validation warning, not blocking
- Allow saving as draft (optional)

---

### 🧭 Buttons:

- **Back** (goes to business type page)
- **Complete Setup / Continue** (submits settings and moves to success)

---

## ✅ Page 4: **Success Page**

**📍URL:** `/onboarding/success`

### 🎯 Purpose:

Confirm successful setup and guide user to next step.

---

### 🧩 Components:

| Element           | Type         | Notes                                                               |
| ----------------- | ------------ | ------------------------------------------------------------------- |
| **Success Icon**  | Illustration | e.g., checkmark, confetti, or animated success badge                |
| **Heading**       | Text         | e.g., “Your business is ready!”                                     |
| **Subtext**       | Text         | “Welcome aboard, \[Business Name] is now live on \[Platform Name].” |
| **Primary CTA**   | Button       | “Go to Dashboard” or “Set Up Your First Item”                       |
| **Secondary CTA** | Link/Button  | Optional: “Invite team”, “Watch onboarding video”                   |

---

### ✨ UX Ideas:

- Confetti animation or subtle animation for feedback
- Pull in business name from setup
- Optionally show a short “what’s next” checklist

---

## ✅ Summary of Fields Across All Pages

| Page              | Key Fields/Inputs                                                                                      |
| ----------------- | ------------------------------------------------------------------------------------------------------ |
| **Signup**        | Name, Email, Password, Mobile (optional)                                                               |
| **Business Type** | Business Type (radio or cards)                                                                         |
| **Setup**         | Business name, GST ID, Country, Currency, Timezone, Logo, Tax settings, Invoice prefix, Stock tracking |
| **Success**       | No inputs — display only                                                                               |

---
