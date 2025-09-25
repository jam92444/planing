## 📄 PDF Layout Structure

Here’s the typical **section-by-section layout** of the invoice PDF.

---

### 1. **Header**

| Element          | Description                  |
| ---------------- | ---------------------------- |
| Logo             | Tenant logo (uploadable)     |
| Business Name    | From tenant settings         |
| Business Address | Optional fields              |
| GSTIN / PAN      | Optional (for tax reporting) |
| Invoice Number   | Shown prominently            |
| Date & Time      | Invoice timestamp            |

---

### 2. **Customer Info**

| Field           | Value Source            |
| --------------- | ----------------------- |
| Customer Name   | From invoice → customer |
| Phone/Email     | If available            |
| Billing Address | Optional (can be empty) |

---

### 3. **Item Table**

| Column     | Notes                                  |
| ---------- | -------------------------------------- |
| #          | Serial number                          |
| Item Name  | From invoice items                     |
| Quantity   | As billed                              |
| Unit Price | ₹ per unit                             |
| Tax Rate   | Per line item (if enabled)             |
| Line Total | Quantity × Price + Tax (if applicable) |

---

### 4. **Summary (Right-aligned block)**

| Label       | Notes                                  |
| ----------- | -------------------------------------- |
| Subtotal    | Before tax and discounts               |
| Tax Total   | Combined tax amount                    |
| Discount    | Flat or % shown, e.g. “10% Discount”   |
| Grand Total | Bold, emphasized                       |
| Paid By     | Payment method (Cash, UPI, Card, etc.) |

---

### 5. **Footer Section**

| Element        | Description                                  |
| -------------- | -------------------------------------------- |
| Thank-you note | Customizable from tenant settings            |
| QR Code        | Optional → could be for:                     |
|                | - Payment link                               |
|                | - Invoice view online (public link)          |
| Terms & Notes  | Optional — short disclaimer or return policy |

---

## 🧩 Template Variants (optional future support)

| Template Name | Layout Differences                           |
| ------------- | -------------------------------------------- |
| `default`     | Standard A4, brand header, tabular format    |
| `compact`     | POS-style for thermal printers (58mm / 80mm) |
| `custom1`     | Custom fonts, logo alignment, watermark      |

Each tenant could pick from available templates in `/settings`.

---

## 🔧 PDF Customization Controls

| Control         | From               | Usage                         |
| --------------- | ------------------ | ----------------------------- |
| Currency Symbol | Tenant settings    | ₹ / $ / €                     |
| Page Size       | Default A4         | or POS/thermal (width < 80mm) |
| Font            | Roboto / Open Sans | Optional styling options      |
| Margins         | Default 20px       | Can be customized in backend  |
| Orientation     | Portrait (default) | Landscape (optional)          |

---

## 🧰 Recommended Stack

| Tool        | Role                                      |
| ----------- | ----------------------------------------- |
| `puppeteer` | Server-side rendering of HTML to PDF      |
| `pdfmake`   | Quick in-browser generation               |
| `html-pdf`  | Fallback for Node.js                      |
| `jsPDF`     | Lightweight (client-side, limited layout) |

**Recommended:** Puppeteer for high fidelity A4 print exports.

## 🧠 Backend Notes (For Devs)

- Build PDF with server-rendered HTML (must match branding)
- Allow header/footer overrides per tenant
- Use tenant ID to fetch logo and settings
- Use Puppeteer to generate PDF from hidden HTML route (`/print` version)

---

## 📦 Summary

| Action       | Route                 | UX Behavior                |
| ------------ | --------------------- | -------------------------- |
| View Print   | `/billing/:id/print`  | Opens print preview page   |
| Download PDF | `/billing/:id/pdf`    | Triggers file download     |
| Templates    | Configurable (future) | Branding, layout, QR codes |
