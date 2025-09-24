# 🧭 Full Planning Documentation

## (Feature List, Modules, Hierarchy, API, Data Models, and Milestones)

---

## 🧠 What You're Building (Brief Recap)

A secure, scalable, multi-store **POS + Bill Generator Web App** with:

- Store-specific branding, products, and settings
- Fast billing flow with variants, discounts, and print
- Exchange system
- Inventory tracking
- Admin dashboard
- Multi-tenant backend with API
- Optional authentication

---

# 🗃️ 1. Master Feature List

Here’s the full list of **features** you’re planning to implement, grouped by **functional module**.

---

## ✅ A. Store Branding & Configuration

| Feature                         | Description                         |
| ------------------------------- | ----------------------------------- |
| Store logo/name/currency        | Custom per store                    |
| Load store settings on app open | From backend                        |
| Theme settings (optional)       | Primary color, tax %, discount type |

---

## ✅ B. Product Management

| Feature                    | Description                         |
| -------------------------- | ----------------------------------- |
| Add/edit/delete product    | With `label_id`, name, variants     |
| Variants per product       | Size, price, stock for each variant |
| Search product by label ID | On billing page                     |
| Track stock                | Update on bill/exchange             |

---

## ✅ C. Billing System

| Feature                 | Description                  |
| ----------------------- | ---------------------------- |
| Add to cart by label ID | Auto shows variants & sizes  |
| Choose size, quantity   | From available stock         |
| Add discount (fixed/%)  | Per item or whole bill       |
| Auto subtotal + total   | With discount & tax          |
| Submit bill             | Save to DB and print receipt |
| Receipt preview         | Before submit                |
| Print/export receipt    | PDF or print directly        |

---

## ✅ D. Exchange Flow

| Feature               | Description         |
| --------------------- | ------------------- |
| Select old bill       | Fetch from DB       |
| Choose returned items | Reduce stock        |
| Add new items         | Like normal bill    |
| Adjust totals         | Refund or pay extra |
| Save exchange bill    | Type = `exchange`   |

---

## ✅ E. Billing History

| Feature                | Description                      |
| ---------------------- | -------------------------------- |
| View all bills         | Paginated, filter by date/amount |
| View bill details      | Item list, discounts, totals     |
| Reprint previous bills | PDF or print                     |
| Export reports         | Daily, monthly CSV               |

---

## ✅ F. Admin Dashboard

| Feature                  | Description                           |
| ------------------------ | ------------------------------------- |
| Add/edit/delete products | Inventory control                     |
| Update store settings    | Logo, currency, etc.                  |
| (Optional) Manage users  | Admin/cashier roles                   |
| View analytics           | Total sales, items sold, top products |

---

## ✅ G. Authentication (Optional for now)

| Feature               | Description                             |
| --------------------- | --------------------------------------- |
| Login for store staff | JWT-based login                         |
| Role-based access     | Owner, Admin, cashier                   |
| Store-scoped data     | Only see their store’s products & bills |

---

# 🧱 2. System Modules & Hierarchy

Breakdown of **app modules and structure**.

---

## 🔹 A. Frontend (React + Tailwind)

### Pages:

| Page                | Purpose                  |
| ------------------- | ------------------------ |
| `/`                 | Billing page             |
| `/exchange`         | Start exchange           |
| `/products`         | Admin product management |
| `/bills`            | View bills               |
| `/settings`         | Store branding settings  |
| `/login` (optional) | Auth login               |

---

### Components:

| Component            | Purpose                    |
| -------------------- | -------------------------- |
| `<Header />`         | Logo, store name, branding |
| `<ProductSearch />`  | Label input + size/qty     |
| `<Cart />`           | Live cart with total       |
| `<ReceiptPreview />` | Print preview UI           |
| `<ExchangeForm />`   | Return + replace items     |
| `<BillList />`       | Billing history            |
| `<ProductForm />`    | Admin add/edit product     |
| `<StoreSettings />`  | Logo/currency/etc.         |
| `<Login />`          | Optional login page        |

---

## 🔹 B. Backend (Node.js + Express + MongoDB)

### Controllers:

| Controller                     | Purpose                          |
| ------------------------------ | -------------------------------- |
| `storeController.js`           | Get/update store settings        |
| `productController.js`         | CRUD for products                |
| `billController.js`            | Create bill/exchange, list bills |
| `authController.js` (optional) | Login, register                  |

---

### Models (MongoDB Schema):

| Model             | Fields                                       |
| ----------------- | -------------------------------------------- |
| `Store`           | name, logo_url, currency, tax, discount type |
| `Product`         | store_id, label_id, name, variants\[]        |
| `Bill`            | store_id, items\[], total, type, date        |
| `User` (optional) | email, password, store_id, role              |

---

## 🔌 API Endpoints (Planned)

---

### 🏪 Store API

```
GET    /api/store/:id         → Get store settings
PUT    /api/store/:id         → Update store settings
```

---

### 📦 Product API

```
GET    /api/products?store_id=xxx       → Get all products
GET    /api/products/:label_id?store_id=xxx  → Find product by label
POST   /api/products                    → Add new product
PUT    /api/products/:id                → Edit product
DELETE /api/products/:id                → Delete product
```

---

### 🧾 Billing API

```
POST   /api/bills                       → Create bill or exchange
GET    /api/bills?store_id=xxx         → List bills
GET    /api/bills/:id                  → Get bill details
```

---

### 🔐 Auth API (Optional)

```
POST   /api/auth/login                  → Login
GET    /api/auth/me                    → Get current user
```

---

# 🚀 3. Development Roadmap (Milestone-Based)

This is your **step-by-step execution plan**.

---

### ✅ M1: Branding UI + Store Config

- [x] Setup React + Tailwind
- [x] Display store name/logo from backend
- [ ] Receipt preview with branding
- [ ] Connect frontend to backend `/api/store/:id`

---

### 🚧 M2: Backend API Setup

- [ ] Setup Express server
- [ ] Connect MongoDB (Atlas/local)
- [ ] Create `store`, `product`, and `bill` models
- [ ] Build routes & controllers

---

### 🏷️ M3: Product Search + Cart Flow

- [ ] Search by label ID
- [ ] Show size, price, stock
- [ ] Add to cart
- [ ] Set quantity, discount, live total
- [ ] Frontend cart state + validation

---

### 🧾 M4: Billing System

- [ ] Submit bill → save to DB
- [ ] Update product stock
- [ ] Generate receipt preview
- [ ] Print / Export

---

### 🔄 M5: Exchange System

- [ ] Fetch old bill
- [ ] Select returned items
- [ ] Add replacement items
- [ ] Auto total adjustment
- [ ] Update stock + save exchange

---

### 🧾 M6: Billing History

- [ ] List bills
- [ ] Filter/search
- [ ] View bill details
- [ ] Reprint/export

---

### ⚙️ M7: Admin Dashboard

- [ ] Add/edit/delete product UI
- [ ] Store setting form
- [ ] Analytics (sales total etc.)

---

### 🔐 M8: Auth & Security (Optional)

- [ ] Add login page
- [ ] JWT + role-based access
- [ ] Protect API routes
- [ ] Add user model
