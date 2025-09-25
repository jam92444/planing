## 🔐 Page 1: **Signup Page**

**📍URL:** `/signup`

### 🎯 Purpose:

Allow the first user to create an account and establish a new tenant (business/workspace).

---

### 🧩 Components:

| Element                   | Type             | Notes                                             |
| ------------------------- | ---------------- | ------------------------------------------------- |
| **Title**                 | Header           | “Create Account” or “Start Your Business”         |
| **Name**                  | Input (text)     | Placeholder: “Sanjay”                             |
| **Email**                 | Input (email)    | Must be unique                                    |
| **Password**              | Input (password) | Add strength meter, min 8 chars, show/hide toggle |
| **Submit Button**         | Button (primary) | “Sign Up”                                         |
| **Existing Account Link** | Text + Link      | “Already have an account? Log in”                 |

---

## 🔑 Page 2: **Login Page**

**📍URL:** `/login`

### 🎯 Purpose:

Authenticate existing users and start a session.

---

### 🧩 Components:

| Element                     | Type             | Notes                            |
| --------------------------- | ---------------- | -------------------------------- |
| **Email**                   | Input (email)    | Placeholder: `you@example.com`   |
| **Password**                | Input (password) | Includes “show/hide” toggle      |
| **Login Button**            | Button (primary) | “Log In”                         |
| **Forgot Password**         | Link             | Opens password recovery flow     |
| **Signup Redirect Link**    | Text + Link      | “Don’t have an account? Sign up” |
| **Social Login (Optional)** | Google OAuth     | If implemented                   |

---

## ✉️ Page 3: **Invite User**

**📍URL:** `/users/invite` (Accessible only to Admins/Owners)

### 🎯 Purpose:

Allow Admins/Owners to add users to their tenant.

---

### 🧩 Components:

| Element         | Type             | Notes                           |
| --------------- | ---------------- | ------------------------------- |
| **Email**       | Input (email)    | Invitee’s email                 |
| **Role**        | Dropdown         | cashier, admin, viewer, etc.    |
| **Send Invite** | Button (primary) | Triggers email with invite link |
| **Status Box**  | Inline Message   | Success or error feedback       |

---

## 👥 Page 4: **User Management**

**📍URL:** `/users` (Admin/Owner only)

### 🎯 Purpose:

List, view, and manage users within the current tenant.

---

### 🧩 Components:

| Element              | Type          | Notes                                       |
| -------------------- | ------------- | ------------------------------------------- |
| **User List**        | Table         | Columns: Name, Email, Role, Status, Actions |
| **Edit Role**        | Dropdown      | Inline or modal-based role change           |
| **Remove User**      | Icon / Button | Confirm before delete                       |
| **Status Indicator** | Tag           | active, invited, suspended                  |

---
