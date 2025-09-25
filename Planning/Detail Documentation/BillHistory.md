## 📘 Module Documentation: **Billing History**

---

## 🧩 1. Purpose

The **Billing History module** enables users to:

- Browse, search, and filter all past bills
- View full details of any bill (sale or exchange)
- Reprint or export any past bill
- Generate sales reports in CSV format
- Get data insights like daily/monthly totals

---

## 🎯 2. Features Overview

| Feature               | Description                          |
| --------------------- | ------------------------------------ |
| ✅ View all bills     | Paginated list, sortable by date     |
| ✅ Filter bills       | By date range, total amount, or type |
| ✅ View bill details  | All line items, discounts, totals    |
| ✅ Reprint bill       | Print or download PDF of old bill    |
| ✅ Export reports     | Daily/monthly reports in CSV         |
| ✅ Support bill types | Sale, Exchange, Refund (future)      |

---

## 🗃️ 3. Data Model (Recap)

Bills are already stored in MongoDB with this schema (see previous modules):

```js
{
  _id: "billId123",
  store_id: "store123",
  type: "sale" | "exchange",
  items: [...],            // or new_items + returned_items for exchange
  subtotal: 1000,
  tax: 50,
  total: 1050,
  created_at: "2025-09-21T08:00:00Z",
  receipt_size: "A5",
  ...
}
```

---

## 📡 4. Backend API Endpoints

| Method | Endpoint                          | Description                       |
| ------ | --------------------------------- | --------------------------------- |
| `GET`  | `/api/bills?store_id=xxx`         | Get paginated bills               |
| `GET`  | `/api/bills/:id`                  | Get single bill (details + items) |
| `GET`  | `/api/bills/export?range=monthly` | Export CSV                        |

---

### 🔍 `GET /api/bills?store_id=xxx&limit=10&page=1&from=2025-09-01&to=2025-09-21&type=sale`

**Query Params:**

| Param                     | Description           |
| ------------------------- | --------------------- |
| `page` / `limit`          | Pagination            |
| `from` / `to`             | Date range            |
| `type`                    | `sale` or `exchange`  |
| `min_total` / `max_total` | Filter by bill amount |

**Response:**

```json
{
  "data": [ ... ],
  "total": 135,
  "page": 1,
  "limit": 10
}
```

---

### 📦 `GET /api/bills/export?range=daily`

Returns downloadable **CSV file** with:

| Date       | Type     | Subtotal | Tax | Discount | Total |
| ---------- | -------- | -------- | --- | -------- | ----- |
| 2025-09-21 | Sale     | 1000     | 50  | 0        | 1050  |
| 2025-09-21 | Exchange | 400      | 20  | 0        | 420   |

> You can build this using Node + `json2csv` or `xlsx` npm package.

---

## 💻 5. Frontend Components

| Component             | Description                        |
| --------------------- | ---------------------------------- |
| `<BillHistoryPage />` | Page to list all bills             |
| `<BillFilter />`      | Filter by date/type/total          |
| `<BillCard />`        | Individual bill preview            |
| `<BillDetailModal />` | Full bill details (items, pricing) |
| `<ExportButton />`    | Export daily/monthly CSV           |
| `<ReprintButton />`   | Open PDF / trigger print           |

---

### 🎯 Bill List UI

```plaintext
---------------------------------------------------------
| #BILL123 | Sale | ₹1050 | 2025-09-21 | [View] [Print] |
---------------------------------------------------------
| #BILL124 | Exchange | ₹760 | 2025-09-20 | [View] [Print] |
```

Click "View" → open modal or navigate to detail view
Click "Print" → re-render receipt layout for print

---

## 📊 6. Filters & Sorting

| Filter       | Control Type                     |
| ------------ | -------------------------------- |
| Date range   | Date picker                      |
| Bill type    | Dropdown (All / Sale / Exchange) |
| Amount       | Range slider or input            |
| Search by ID | Text input                       |

---

## 🖨️ 7. Reprint Logic

### 🧾 Flow

- Click "Print" or "Export PDF"
- Fetch full bill by ID from `/api/bills/:id`
- Render receipt layout (same as during billing)
- Call `window.print()` or use `react-to-print`
- PDF via `jspdf` for export

### Receipt will auto-load:

- Logo, store info
- Item list
- Discounts, tax, total
- Type = `sale` or `exchange`

---

## 📂 8. CSV Export (Reports)

**Daily Report:**

| Field       | Type            |
| ----------- | --------------- |
| Bill Number | Text            |
| Date        | ISO date        |
| Type        | sale / exchange |
| Subtotal    | Number          |
| Tax         | Number          |
| Total       | Number          |

**Monthly Report Totals:**

| Day   | Sale Total | Exchange Total | Total   |
| ----- | ---------- | -------------- | ------- |
| Sep 1 | ₹12,500    | ₹900           | ₹13,400 |
| Sep 2 | ₹11,300    | ₹0             | ₹11,300 |
| ...   | ...        | ...            | ...     |

---

## 🔒 9. Security & Access

| Rule               | Enforcement                            |
| ------------------ | -------------------------------------- |
| Auth required      | Only logged-in users                   |
| Scoped to store    | Users can only see their store’s bills |
| CSV access         | Only admins/export role users          |
| Print sanitization | Prevent script injection in bills      |

---

## ✅ 10. Success Criteria

| ✅                                     | Requirement |
| -------------------------------------- | ----------- |
| Paginated list of bills                |             |
| Filter by date, amount, type           |             |
| View bill details (modal or full page) |             |
| Reprint as PDF or print directly       |             |
| Export CSV for daily/monthly           |             |
| Handles both sale & exchange bills     |             |
| Performance optimized for 1000+ bills  |             |

---

## 🔮 Future Enhancements

| Feature                      | Benefit                  |
| ---------------------------- | ------------------------ |
| Search by product (label_id) | "Who bought AC70002?"    |
| Customer field on bill       | Lookup bills by customer |
| Return requests from history | Direct return/exchange   |
| Mobile-friendly history view | For store managers       |
| Graph analytics              | Visual sales tracking    |

---

# ✅ Summary

The **Billing History** module adds powerful features for:

- Admins → auditing and reports
- Cashiers → reprinting lost receipts
- Accountants → downloading monthly sales
- Store owners → analyzing performance

It ties directly into the billing and exchange modules — reusing receipts and stored bill data.

---
