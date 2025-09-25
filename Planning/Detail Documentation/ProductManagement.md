This doc will cover:

1. ✅ Why this module is needed
2. ✅ What it will do
3. ✅ Data model (MongoDB schema)
4. ✅ Backend (API routes + controller responsibilities)
5. ✅ Frontend components
6. ✅ Behavior (Add, Edit, Search, Stock tracking)
7. ✅ UX examples
8. ✅ Security & validations
9. ✅ Success checklist

---

# 📘 Module Documentation: **Product Management**

---

## 🧩 1. Purpose

The **Product Management module** enables each store to:

- Add new products with sizes/variants
- Edit product info (price, stock, name)
- Remove outdated products
- Search products by their label code (during billing)
- Auto-track stock when bills are created or items exchanged

This module ensures that the system has an up-to-date, searchable inventory with real-time stock control.

---

## 🎯 2. Functional Goals

| Feature               | Description                                        |
| --------------------- | -------------------------------------------------- |
| ✅ Add product        | With label ID, name, variants (size, price, stock) |
| ✅ Edit product       | Update price, stock, name, variants                |
| ✅ Delete product     | If no longer needed                                |
| ✅ Variant system     | Each product has multiple sizes or versions        |
| ✅ Label-based search | Fast scan/entry in billing page                    |
| ✅ Stock sync         | Auto-update on sale/exchange                       |
| ✅ Store-scoped       | Each store sees/manages their own inventory        |

---

## 🛠️ 3. Data Model: MongoDB (Mongoose Schema)

```js
// models/Product.js

const VariantSchema = new mongoose.Schema({
  size: { type: String, required: true },
  price: { type: Number, required: true },
  stock: { type: Number, required: true },
});

const ProductSchema = new mongoose.Schema({
  store_id: {
    type: mongoose.Schema.Types.ObjectId,
    ref: "Store",
    required: true,
  },
  label_id: { type: String, required: true, unique: true },
  name: { type: String, required: true },
  description: { type: String },
  variants: [VariantSchema],
  created_at: { type: Date, default: Date.now },
});

module.exports = mongoose.model("Product", ProductSchema);
```

---

## 📡 4. Backend API

### 🔁 All product routes are **scoped to a store_id**.

| Method   | Endpoint                               | Description                  |
| -------- | -------------------------------------- | ---------------------------- |
| `GET`    | `/api/products?store_id=xxx`           | Get all products for a store |
| `GET`    | `/api/products/:label_id?store_id=xxx` | Get product by label ID      |
| `POST`   | `/api/products`                        | Create new product           |
| `PUT`    | `/api/products/:id`                    | Update product               |
| `DELETE` | `/api/products/:id`                    | Delete product               |

---

### 🔧 Example: Create Product (POST)

**Endpoint:** `/api/products`

**Body:**

```json
{
  "store_id": "store123",
  "label_id": "AC70002",
  "name": "Kids Blue Jeans",
  "variants": [
    { "size": "22", "price": 500, "stock": 4 },
    { "size": "24", "price": 500, "stock": 2 },
    { "size": "28", "price": 500, "stock": 3 }
  ]
}
```

**Response:**

```json
{
  "message": "Product created successfully",
  "product_id": "64e2d..."
}
```

---

### 🧮 Stock Update Logic

In `billController.js`, when a bill is submitted:

- For each item:

  - Find the product by `label_id`
  - Find the matching `variant.size`
  - Reduce the stock based on quantity

- For exchange:

  - Returned item: Increase stock
  - New item: Decrease stock

---

## 🧑‍💻 5. Frontend Components

### 🧱 Admin Pages

| Component           | Role                                         |
| ------------------- | -------------------------------------------- |
| `<ProductForm />`   | Create or edit product                       |
| `<ProductList />`   | Show all products for current store          |
| `<VariantEditor />` | Add/edit/remove variants inside product form |
| `<ConfirmDelete />` | Prompt before deleting a product             |

---

### 🔎 Billing Page (`/`)

| Component             | Role                                                   |
| --------------------- | ------------------------------------------------------ |
| `<ProductSearch />`   | Input for `label_id`, fetch and show matching product  |
| `<VariantSelector />` | Dropdown to choose size from available stock           |
| `<Cart />`            | Add item with quantity, discount, subtotal calculation |

---

## 🔍 6. Product Search Flow

### 🔹 By Label ID

- Cashier types or scans label (e.g. `AC70002`)
- Call: `GET /api/products/AC70002?store_id=xxx`
- Show result:

  - Name
  - Variant sizes + prices + stock

- Allow selection of size and quantity
- Auto-calculate price and push to cart

---

## 🖼️ 7. UX Example (Admin)

### Add New Product:

1. Click "Add Product"
2. Form:

   - Label ID: `AC70002`
   - Name: `Kids Blue Jeans`
   - Add Variant:

     - Size: 22 → Price: 500 → Stock: 4
     - Size: 24 → Price: 500 → Stock: 2

   - Submit

3. Toast: ✅ “Product added”

---

### Billing Page:

1. Type or scan `AC70002`
2. Variants dropdown:

   - 22 — ₹500 (4 in stock)
   - 24 — ₹500 (2 in stock)

3. Choose size `22`, enter quantity `2`
4. Add to cart

---

## ✅ 8. Validations & Rules

| Rule                        | Check                              |
| --------------------------- | ---------------------------------- |
| Unique `label_id`           | Per store                          |
| At least one variant        | Required                           |
| No negative stock           | After billing or exchange          |
| Store isolation             | Can’t fetch other store’s products |
| Price must be ≥ 0           | Prevent mistakes                   |
| Variant size must be unique | Per product                        |

---

## 🔐 9. Security Considerations

| Area           | Rule                                               |
| -------------- | -------------------------------------------------- |
| Admin panel    | Only authenticated admins can create/update/delete |
| API requests   | Must include `store_id` (or derived from JWT)      |
| Access control | Only return products tied to that store            |

---

## ✅ 10. Success Checklist

| ✅                                               | Task |
| ------------------------------------------------ | ---- |
| 🔄 Product CRUD works via API                    |      |
| 🔍 Label-based search returns correct product    |      |
| 🎯 Variants load with correct price and stock    |      |
| 🛒 Cart uses selected variant price              |      |
| 🧮 Stock is updated on bill submit or exchange   |      |
| ⚙️ Admin can manage all products for their store |      |

---

## 🔜 Future Enhancements

| Feature                   | Why                          |
| ------------------------- | ---------------------------- |
| Barcode auto-scan         | Speed up search              |
| Image per product         | Visual confirmation          |
| Bulk product import (CSV) | Faster setup                 |
| Soft delete               | Prevent accidental loss      |
| Low stock alerts          | Notifications in admin panel |

---

## 📦 Implementation Plan

### Backend

- [x] `Product` model with variant schema
- [x] Controllers for CRUD
- [x] Store ID scoping
- [ ] Stock update inside billing controller

### Frontend

- [x] Product form (admin)
- [x] Product list (admin)
- [x] Label-based search (billing)
- [x] Variant selection
- [ ] Stock handling on billing

---

# ✅ Summary

This module gives your system a full **inventory engine**:

- Easy admin product management
- Fast cashier lookup via label ID
- Dynamic variants (size, stock, price)
- Auto-updating stock system
- Store-scoped data, fully secured

Once it's implemented, your app is ready to **generate live bills** using this inventory.

---
