

## 📘 Module: **Authentication & Role-Based Access Control (RBAC)**

---

### 🧩 1. Purpose

To **secure the app** by:

* Requiring users to **log in** to access billing and admin features
* Restricting access based on **roles** (e.g., cashier vs admin)
* Ensuring users only manage **data from their store**

---

### 👥 2. User Roles

| Role      | Permissions                                       |
| --------- | ------------------------------------------------- |
| `owner`   | Full control: products, users, billing, analytics |
| `admin`   | Manage products, billing, history, analytics      |
| `cashier` | Only billing and billing history                  |

> You can start with `admin` and `cashier`, and add `owner` later if needed.

---

## 🧾 3. Features Overview

| Feature               | Description                        |
| --------------------- | ---------------------------------- |
| 🔐 Login for staff    | Email/password authentication      |
| 🔒 JWT-based sessions | Secure API access                  |
| ⚙️ Role-based access  | Permissions vary per role          |
| 🏪 Store scoping      | Users see only their store's data  |
| 🚫 Block unauthorized | Prevent access to protected routes |

---

## 🗃️ 4. Data Model

### `User` Collection (MongoDB)

```js
{
  _id: ObjectId,
  name: "Amit Kumar",
  email: "amit@store.com",
  password_hash: "...",       // hashed with bcrypt
  role: "admin",              // "admin", "cashier", "owner"
  store_id: ObjectId,         // references Store
  active: true,
  created_at: Date
}
```

---

## 📡 5. Backend API

| Method | Endpoint           | Description                   |
| ------ | ------------------ | ----------------------------- |
| `POST` | `/api/auth/login`  | Login with email/password     |
| `GET`  | `/api/auth/me`     | Get current logged-in user    |
| `POST` | `/api/auth/logout` | (Optional) client-side logout |
| `POST` | `/api/users`       | Admin creates new user        |
| `PUT`  | `/api/users/:id`   | Update user (role, active)    |
| `GET`  | `/api/users`       | List users (admin only)       |

---

### 🔐 `POST /api/auth/login`

**Request:**

```json
{
  "email": "amit@store.com",
  "password": "securePassword"
}
```

**Response:**

```json
{
  "token": "jwt-token",
  "user": {
    "_id": "...",
    "name": "Amit",
    "role": "admin",
    "store_id": "storeId123"
  }
}
```

---

## 🔑 6. Authentication with JWT

### JWT Payload Example

```json
{
  "user_id": "userId123",
  "store_id": "storeId123",
  "role": "admin",
  "exp": 1698500000
}
```

> JWT is stored in localStorage or HttpOnly cookie (secure option).

---

## 🔒 7. Middleware: Role + Store Access

### ✅ Protect Backend Routes

```js
// Example: Protect route
function authRequired(req, res, next) {
  const token = req.headers.authorization?.split(" ")[1];
  const decoded = jwt.verify(token, JWT_SECRET);
  req.user = decoded; // attach to request
  next();
}

function roleRequired(role) {
  return (req, res, next) => {
    if (req.user.role !== role) return res.status(403).json({ error: "Forbidden" });
    next();
  }
}
```

---

## 💻 8. Frontend Auth Flow (React)

| Component          | Purpose                              |
| ------------------ | ------------------------------------ |
| `<LoginPage />`    | Staff login form                     |
| `<PrivateRoute />` | Wrapper for protected pages          |
| `<AuthContext />`  | Global state to store logged-in user |
| `useAuth()` hook   | Custom hook for access & logout      |

---

### 🌐 `LoginPage.jsx`

* POST to `/api/auth/login`
* Store token in localStorage or cookie
* Redirect to dashboard (if admin) or billing (if cashier)

---

## 🧩 9. Route Access Control

### On Frontend

```jsx
// Example: Admin-only route
if (user?.role !== 'admin') {
  return <Navigate to="/unauthorized" />
}
```

### On Backend

```js
app.get('/api/products', authRequired, roleRequired('admin'), (req, res) => {
  // Only admin can get products
});
```

---

## 🏪 10. Store Scoping

All queries (bills, products, etc.) must **filter by the user's `store_id`**, like:

```js
Product.find({ store_id: req.user.store_id })
```

This ensures **multi-tenant security**, so one store can’t access another’s data.

---

## ✅ 11. Success Criteria

| ✅                                              | Requirement |
| ---------------------------------------------- | ----------- |
| Secure login (email + password)                |             |
| JWT token with role + store\_id                |             |
| Frontend route protection                      |             |
| Backend middleware for role/store scope        |             |
| Only authorized users access protected routes  |             |
| Billing, exchange, and admin panels are scoped |             |
| Easy to plug in or disable (optional module)   |             |

---

## 🔐 12. Optional Enhancements

| Feature           | Benefit                     |
| ----------------- | --------------------------- |
| Forgot password   | Email reset link            |
| OTP login (later) | Easier mobile usage         |
| Two-factor auth   | For owners/admins           |
| Login logs        | Security auditing           |
| Deactivate user   | Temporarily block staff     |
| Change password   | Self-service or admin-reset |

---

## 🛡️ 13. Security Checklist

* ✅ Passwords hashed with `bcrypt`
* ✅ JWT expires in 24 hours
* ✅ API endpoints protected by middleware
* ✅ Role-based access enforced
* ✅ Frontend avoids exposing admin-only UI
* ✅ Store scoping on all data reads/writes

---

## ✅ Summary

The **Authentication System** adds proper **user access control**, enabling:

* Secure logins
* Role-based permissions (admin vs cashier)
* Scoped access to their store’s data
* Foundation for future multi-store, team-based scaling

It’s optional, but **strongly recommended** for any production-grade system.

---

