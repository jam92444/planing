
# 📘 Module Documentation: **Billing System**
---

## 🧩 1. Purpose

The billing system allows a user (cashier or owner) to:

- Build a bill from scanned or typed label IDs
- Select size, quantity, discounts
- Calculate subtotal, tax, and total dynamically
- Preview and print/export receipt
- Track every bill (and later support exchange/returns)
- Print receipts based on **chosen paper size**

---

## 🎯 2. Feature Table (Updated)

| Feature                        | Description                            |
| ------------------------------ | -------------------------------------- |
| ✅ Add to cart by label ID     | Auto shows product variants            |
| ✅ Choose size & quantity      | Based on available stock               |
| ✅ Apply discount              | Per item or whole bill (fixed or %)    |
| ✅ Auto subtotal + tax + total | Live calculation                       |
| ✅ Submit bill                 | Save in DB with all details            |
| ✅ Receipt preview             | Before final submission                |
| ✅ Print/export receipt        | Supports multiple receipt sizes        |
| ✅ Receipt size selection      | User selects size: 55mm / A5 / A4 etc. |

---

## 🖼️ 3. Receipt Size Selection (New Enhancement)

### ✅ Real-World Need

Different businesses need **different receipt sizes**:

| Business Type      | Common Receipt Size  |
| ------------------ | -------------------- |
| Clothing shops     | 55mm thermal, A5     |
| Electronics stores | A4                   |
| Jewelry            | A5, custom           |
| Railways/hospitals | Full-width (A4)      |
| Café/Food stalls   | 55mm or 80mm thermal |

---

### 🧾 Supported Receipt Sizes (In App)

| Label               | Size (approx) | Use Case                            |
| ------------------- | ------------- | ----------------------------------- |
| `55mm`              | 2.1 inches    | Thermal receipt printers (most POS) |
| `A5`                | 5.8 x 8.3 in  | Half-page print                     |
| `A4`                | 8.3 x 11.7 in | Full bill, formal invoice           |
| `Custom` (optional) | Dynamic       | Use CSS scale & margins             |

---

### 📦 Config Options

**Option 1 – On Each Bill:**

- Cashier chooses desired receipt size (from dropdown)
- App renders correct layout for selected size

**Option 2 – Store Settings:**

- Default receipt size stored per store
- Can override per-bill if needed

> We recommend both. Default in settings + override on each bill.

---

### 🛠️ How to Implement (Frontend)

#### Receipt Layout

Use different CSS classes or print layouts:

```tsx
<div className={`receipt-preview ${receiptSize}`}>{/* Receipt layout */}</div>
```

**CSS:**

```css
.receipt-55mm {
  width: 55mm;
  font-size: 10px;
}

.receipt-a5 {
  width: 148mm;
  font-size: 12px;
}

.receipt-a4 {
  width: 210mm;
  font-size: 14px;
}
```

> You can dynamically load styles based on selected size.

---

### 🖨️ Print Logic

Use a library or window print with print CSS:

- `window.print()` with media query
- Or use [`react-to-print`](https://www.npmjs.com/package/react-to-print)
- Or generate PDF with [`jsPDF`](https://github.com/parallax/jsPDF) for A4/A5

---

## 🛠️ 4. Data Model: Bill Schema (MongoDB)

```js
// models/Bill.js

const BillSchema = new mongoose.Schema({
  store_id: { type: mongoose.Schema.Types.ObjectId, ref: "Store" },
  type: { type: String, enum: ["sale", "exchange"], default: "sale" },
  items: [
    {
      label_id: String,
      name: String,
      size: String,
      quantity: Number,
      price: Number,
      discount: {
        type: Number,
        default: 0,
      },
      discount_type: {
        type: String,
        enum: ["percent", "fixed"],
        default: "percent",
      },
    },
  ],
  subtotal: Number,
  tax: Number,
  total: Number,
  discount_total: Number,
  receipt_size: { type: String, default: "55mm" },
  created_at: { type: Date, default: Date.now },
});
```

---

## 📡 5. Backend API

| Method | Endpoint                  | Description              |
| ------ | ------------------------- | ------------------------ |
| `POST` | `/api/bills`              | Save new bill            |
| `GET`  | `/api/bills/:id`          | Get bill for receipt     |
| `GET`  | `/api/bills?store_id=xxx` | List all bills for store |

---

## 📦 POST /api/bills – Example

```json
{
  "store_id": "store123",
  "type": "sale",
  "items": [
    {
      "label_id": "AC70001",
      "name": "T-shirt",
      "size": "M",
      "quantity": 2,
      "price": 400,
      "discount": 10,
      "discount_type": "percent"
    }
  ],
  "subtotal": 800,
  "tax": 40,
  "discount_total": 80,
  "total": 760,
  "receipt_size": "A5"
}
```

---

## 💻 6. Frontend Flow

### Billing Page Flow:

1. User scans/enters label → Fetch product
2. Select size → Set quantity
3. Apply item/total discount
4. Show live cart → Subtotal, tax, total
5. **Select receipt size (dropdown)**:

   - 55mm
   - A5
   - A4

6. Click "Preview Receipt"

   - Show preview in correct layout

7. Click "Submit & Print"

   - Save to DB
   - Open print modal / trigger auto print

---

### 📦 UI Components:

| Component            | Role                            |
| -------------------- | ------------------------------- |
| `<BillingForm />`    | Main cart/entry logic           |
| `<Cart />`           | List of items with pricing      |
| `<ReceiptPreview />` | Preview with size selection     |
| `<PrintSettings />`  | Dropdown to select receipt size |
| `<PrintButton />`    | Print trigger (with ref)        |

---

## ✅ 7. Validations

| Check               | Error/Action                                   |
| ------------------- | ---------------------------------------------- |
| Stock available?    | Prevent over-selling                           |
| Discount valid?     | Don't allow negative prices                    |
| Receipt size valid? | Must be from allowed list (`55mm`, `A5`, `A4`) |

---

## 🔐 8. Security & Access

- Only logged-in users (cashier/admin) can create bills
- Bills are always scoped to `store_id`
- Don't allow receipt layout manipulation from frontend (validate server-side or sanitize)

---

## 📌 9. Success Checklist

| ✅                                              | Feature |
| ----------------------------------------------- | ------- |
| Cart works with product search, size & quantity |         |
| Discounts (item/total) calculate correctly      |         |
| Tax is auto-applied from store config           |         |
| Receipt size is selectable before printing      |         |
| Receipt layout adjusts to selected size         |         |
| Bill is saved to DB with all details            |         |
| Printing/exporting works across sizes           |         |

---

## 🔮 Future Enhancements

| Feature                     | Why                                |
| --------------------------- | ---------------------------------- |
| QR code on receipt          | For customer verification          |
| Email/SMS receipt           | Digital copy for customer          |
| Thermal printer integration | With Node ESC/POS or USB/WebUSB    |
| Receipt logo watermark      | For professional branding          |
| Customer info on bill       | Optional input fields: name, phone |

---

# ✅ Summary

The Billing System now includes a powerful new feature: **receipt size selection**, which makes your app **fit any business type**.

You're now offering:

- Fast & accurate billing
- Variant-based inventory billing
- Discounts, tax, and totals
- **Multi-size receipt generation**
- Export or print in chosen layout

---
