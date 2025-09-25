## 🧱 Key Pages

| URL                  | Purpose                         |
| -------------------- | ------------------------------- |
| `/billing/:id/view`  | Shows invoice summary + actions |
| `/billing/:id/print` | Renders HTML invoice for print  |
| `/billing/:id/pdf`   | API that returns PDF file       |

---

## 📑 Page: `/billing/:id/view`

#### Components:

| Element               | Type    | Notes                                      |
| --------------------- | ------- | ------------------------------------------ |
| Invoice Summary       | Display | Shows total, customer, payment, etc.       |
| Button: Print Invoice | Button  | Opens `/billing/:id/print` in new tab      |
| Button: Download PDF  | Button  | Calls `GET /billing/:id/pdf` with download |

---

## 🖨️ Page: `/billing/:id/print`

This is a clean, **HTML-rendered invoice** intended for printing directly from the browser (or mobile POS devices).

### UI Features:

- **Auto Print Prompt:** When opened, it should call `window.print()` automatically after a short delay.
- **Responsive:** Works on A4 or small-width receipt printers.
- **Hide UI Elements:** No navigation bar, footer, or buttons.
- **Print Styles:** Apply `@media print` styles for clean layout.

---

## 📤 Action: Download PDF

### Triggered From:

- "📥 Download PDF" button on `/billing/:id/view`

### Call:

```http
GET /billing/:id/pdf?format=download
```

- Response: PDF binary stream or file download
- Default format: A4 Portrait
- Optional query:

  - `template=custom1` (for template switching)
  - `format=inline` (for preview)

---

## 🔐 Permissions

| Role    | Can Print/View PDF? |
| ------- | ------------------- |
| Owner   | ✅                  |
| Admin   | ✅                  |
| Cashier | ✅                  |
| Viewer  | ❌                  |

---
