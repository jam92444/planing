## 🔐 **Module Name**: User Authentication + Role & Permission Management

---

## ✅ **Goal**

1. Allow users to **sign up**, **log in**, and **manage sessions** securely.
2. Support **multi-role access** per tenant: `owner`, `admin`, `cashier`, etc.
3. Ensure strict **data isolation**: users can only access their tenant’s data.
4. Enable tenant admins to **invite/manage users** within their business.

---

## 🧭 **User Flow Overview**

| Action       | Pages             | Description                                    |
| ------------ | ----------------- | ---------------------------------------------- |
| Signup       | `/signup`         | Creates first user + tenant                    |
| Login        | `/login`          | Authenticates user, returns session/token      |
| Invite Users | `/users/invite`   | Owner/Admin invites more users to their tenant |
| Manage Roles | `/users/:id/role` | Owner/Admin assigns/revokes roles              |
| View Users   | `/users`          | List users under current tenant                |
| Logout       | -                 | Ends session/token                             |

---

## 🧱 **Backend API Design**

### 1. `POST /auth/signup`

Creates user + tenant

```json
{
  "name": "Sanjay",
  "email": "sanjay@example.com",
  "password": "securePass123"
}
```

### 2. `POST /auth/login`

Login with email/password

```json
{
  "email": "sanjay@example.com",
  "password": "securePass123"
}
```

**Response:**

- Access token (JWT or session cookie)
- User info
- Tenant ID

---

### 3. `POST /users/invite` (Admin/Owner only)

Invite another user to your tenant.

```json
{
  "email": "cashier@abc.com",
  "role": "cashier"
}
```

**Backend:**

- Creates user (inactive)
- Sends invite email
- On accept, user sets password

---

### 4. `PUT /users/:id/role` (Admin/Owner only)

```json
{
  "role": "admin"
}
```

---

### 5. `GET /users` (Admin/Owner only)

List all users in tenant.

```json
[
  {
    "id": "u1",
    "name": "Sanjay",
    "email": "sanjay@abc.com",
    "role": "owner"
  },
  ...
]
```

---

### 6. `DELETE /users/:id`

Remove a user from tenant.

---

## 👥 **User Roles & Permissions**

| Role        | Description                          | Permissions                       |
| ----------- | ------------------------------------ | --------------------------------- |
| **Owner**   | Main creator of tenant               | Full access to everything         |
| **Admin**   | Can manage billing, items, users     | Everything except deleting tenant |
| **Cashier** | Only allowed to create/view invoices | No access to settings or reports  |
| **Viewer**  | (Optional) Read-only for reports     | View-only                         |

Implement using:

- Role field in user table
- Middleware to check `req.user.role` and `req.user.tenant_id`

---

## 🔒 **Authentication System**

You can use:

| Feature                       | Tech                                        |
| ----------------------------- | ------------------------------------------- |
| Password Hashing              | `bcrypt`                                    |
| Token Auth                    | `JWT` (access + refresh tokens) or sessions |
| Email verification (optional) | Send grid, nodemailer, etc.                 |
| Social Login (optional)       | Google OAuth                                |

---

## 🗃️ **Database Schema**

### `users` table

| Field         | Type                         |
| ------------- | ---------------------------- |
| id            | UUID                         |
| tenant_id     | FK                           |
| name          | string                       |
| email         | string (unique)              |
| password_hash | string                       |
| role          | enum (owner, admin, etc.)    |
| status        | active / invited / suspended |
| created_at    | timestamp                    |

---

## 🔐 **Security Rules**

- 🚫 User cannot access data of other tenants.
- ✅ Middleware must validate token → extract `user_id`, `tenant_id`, `role`.
- 🔐 Sensitive routes (`/users`, `/settings`, `/reports`) must check for role.

---

## 📌 Example Middleware

```js
function checkRole(requiredRole) {
  return function (req, res, next) {
    if (req.user.role !== requiredRole)
      return res.status(403).json({ message: "Forbidden" });
    next();
  };
}
```

---

## ✅ Summary

| Feature     | API                   | Role Needed |
| ----------- | --------------------- | ----------- |
| Sign up     | `POST /auth/signup`   | Public      |
| Login       | `POST /auth/login`    | Public      |
| Invite user | `POST /users/invite`  | Admin/Owner |
| Change role | `PUT /users/:id/role` | Owner       |
| List users  | `GET /users`          | Admin/Owner |
| Delete user | `DELETE /users/:id`   | Admin/Owner |

---
