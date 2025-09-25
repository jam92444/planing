
## We’ll cover:

1. ✅ Purpose
2. ✅ Use Case & User Flow
3. ✅ Exchange Logic
4. ✅ Data Model Changes
5. ✅ Backend API
6. ✅ Frontend Behavior
7. ✅ Validations & Constraints
8. ✅ Examples (UI + Receipt)
9. ✅ Future Enhancements
10. ✅ Success Criteria

---

# 📘 Module Documentation: **Exchange Flow**

---

## 🧩 1. Purpose

The **Exchange Flow** allows a store to handle **returns with new purchases**, adjusting stock and billing accordingly.

A typical flow:

* Customer brings back old items (from a previous bill)
* They select new items in exchange
* System calculates if a refund is due or extra amount needs to be paid
* Saves as an `exchange` bill in DB
* Tracks both return and new items with updated stock

---

## 🎯 2. Use Cases

| Scenario                          | Action               |
| --------------------------------- | -------------------- |
| Product return without purchase   | Show refund amount   |
| Exchange with higher price item   | Show amount to pay   |
| Exchange with lower value item    | Show refund due      |
| Partial return, partial new items | Calculate difference |

---

## 🛠️ 3. Key Features

| Feature                 | Description                                 |
| ----------------------- | ------------------------------------------- |
| ✅ Select old bill       | Lookup by bill number or date               |
| ✅ Choose returned items | User selects what is returned               |
| ✅ Return stock update   | Add returned items back to inventory        |
| ✅ Add new items         | Same flow as sale billing                   |
| ✅ Adjust difference     | Auto-calculate difference (refund/pay)      |
| ✅ Save exchange bill    | Store in DB with `type = "exchange"`        |
| ✅ Reference old bill    | Stored for traceability                     |
| ✅ Receipt generation    | Shows both return and new purchase sections |

---

## 🧮 4. Exchange Calculation Logic

```txt
Returned Amount = Sum of returned items (after discount)
New Purchase Amount = Sum of new items (after discount + tax)

Final Total = New Purchase - Return Amount

If positive → Customer pays
If negative → Customer is refunded
```

---

## 📦 5. Data Model (MongoDB)

### 👕 `Bill` Schema (with Exchange Support)

```js
const BillSchema = new mongoose.Schema({
  store_id: { type: mongoose.Schema.Types.ObjectId, ref: 'Store' },
  type: { type: String, enum: ['sale', 'exchange'], default: 'sale' },

  original_bill_id: { type: mongoose.Schema.Types.ObjectId, ref: 'Bill', default: null },

  returned_items: [ // ← only for exchange
    {
      label_id: String,
      name: String,
      size: String,
      quantity: Number,
      price: Number,
      discount: Number,
      discount_type: String
    }
  ],

  new_items: [ // ← standard billing items
    {
      label_id: String,
      name: String,
      size: String,
      quantity: Number,
      price: Number,
      discount: Number,
      discount_type: String
    }
  ],

  subtotal: Number,
  tax: Number,
  discount_total: Number,
  total: Number, // New Purchase Total
  return_total: Number,
  final_total: Number, // total - return_total

  created_at: { type: Date, default: Date.now }
});
```

---

## 📡 6. Backend API Endpoints

### 🔍 `GET /api/bills/:billId`

* Fetch original bill for exchange
* Used to show old items

### 🔄 `POST /api/exchange`

* Create exchange bill
* Update stock accordingly
* Type is set to `"exchange"`

**Request Body:**

```json
{
  "store_id": "store123",
  "original_bill_id": "64ef...",
  "returned_items": [
    {
      "label_id": "AC70001",
      "size": "M",
      "quantity": 1,
      "price": 400,
      "discount": 0,
      "discount_type": "fixed"
    }
  ],
  "new_items": [
    {
      "label_id": "AC80001",
      "size": "L",
      "quantity": 1,
      "price": 500,
      "discount": 0,
      "discount_type": "percent"
    }
  ],
  "subtotal": 500,
  "tax": 25,
  "return_total": 400,
  "final_total": 125
}
```

---

### ✅ Server-Side Logic

#### Step 1: Validate original bill

* Must belong to same store
* Must exist
* Cannot exchange more than sold

#### Step 2: Update stock

* Returned items → `+stock`
* New items → `-stock`

#### Step 3: Save as new bill

* `type: "exchange"`
* Include link to `original_bill_id`

---

## 💻 7. Frontend UI Flow

### Step-by-step Exchange:

1. 🔍 Search old bill by:

   * Bill number / date / label ID

2. ✅ Show old bill details:

   * Item list, quantity, sizes

3. 🧾 Select items to return:

   * Choose quantity for each

4. ➕ Add new items (like sale):

   * Label ID → Size → Quantity → Discount

5. 🧮 Final calculation:

   * Show "Return Total", "New Total", "Pay/Refund Amount"

6. 🎯 Select receipt size + preview

7. 💾 Submit exchange

---

## 🧑‍💻 Components Required

| Component                    | Purpose                                  |
| ---------------------------- | ---------------------------------------- |
| `<ExchangeStart />`          | Search and load old bill                 |
| `<ReturnItemSelector />`     | Choose which items to return             |
| `<NewItemCart />`            | Same as sale flow                        |
| `<ExchangeSummary />`        | Calculates return, new total, difference |
| `<ExchangeReceiptPreview />` | Two sections: return + purchase          |
| `<PrintExchange />`          | Export/print exchange receipt            |

---

## 🧪 8. Validations

| Rule                          | Description                   |
| ----------------------------- | ----------------------------- |
| Cannot return more than sold  | Based on original bill        |
| Stock available for new items | Validate before submit        |
| Store match                   | Prevent cross-store returns   |
| Return required for exchange  | Must select at least one item |
| Type = `exchange`             | For identification in DB      |

---

## 🖼️ 9. Receipt Format (Exchange)

```
--- STORE NAME & LOGO ---
Exchange Bill (Ref: #BILL123)

Returned Items:
- T-Shirt M x1 — ₹400.00

New Items:
- Hoodie L x1 — ₹500.00

Subtotal: ₹500.00
Tax (5%): ₹25.00
Total: ₹525.00
Return Amount: ₹400.00

Amount to Pay: ₹125.00

--- Thank You ---
```

---

## ✅ 10. Success Criteria

| ✅                                          | Feature |
| ------------------------------------------ | ------- |
| Can load and search old bill               |         |
| Can select return items from original      |         |
| Can add new items as replacement           |         |
| Return increases stock                     |         |
| New items decrease stock                   |         |
| Total adjusted correctly                   |         |
| Bill saved as `type: exchange`             |         |
| Receipt shows both return and new sections |         |
| Print/export works with selected size      |         |

---

## 🔮 Future Enhancements

| Feature                              | Benefit              |
| ------------------------------------ | -------------------- |
| Barcode scan to load old bill        | Faster flow          |
| Return reason per item               | For quality tracking |
| Partial refunds to wallet            | For credit customers |
| Exchange limit window (e.g., 7 days) | Policy enforcement   |
| Return approval system               | Multi-user stores    |

---

# ✅ Summary

The Exchange Flow adds **full lifecycle support** for real-world retail billing:

* Items can be returned and exchanged in one operation
* Stock is accurately adjusted
* Receipts clearly show return + purchase
* Cashier gets clear feedback: refund or charge extra
* Database tracks both original and exchanged bill
