## ✳️ `/signup` Page

- **Layout**: Centered card, brand logo top left or center
- **Validation**:

  - Name: Required
  - Email: Must be unique and valid
  - Password: Min 8 chars

- **States**:

  - Disabled button when invalid
  - Inline error messages
  - Success redirect to `/login`

---

## 🔐 `/login` Page

- **Form layout**: Minimal + clean
- **Validation**:

  - Required fields
  - Show “invalid credentials” on backend error

- **States**:

  - Button loading state after submit
  - Error inline: “Incorrect email or password”

---

## ✉️ `/users/invite` Page

- **Role selection**: Dropdown with role descriptions as tooltips
- **States**:

  - Loading spinner on invite button
  - Inline success: “Invitation sent to [email]”
  - Handle duplicate invite gracefully

---

## 👤 `/users` Management

- **Table interaction**:

  - Pagination if user count > 10
  - Role change triggers a PUT request
  - Delete user asks for confirmation modal

- **Status Badges**:

  - `active` → green
  - `invited` → yellow
  - `suspended` → red (future support)

---

## 🔒 Middleware UX

- If user lacks permission:

  - Show `403 Forbidden` page
  - Message: “You do not have access to this page”
  - Link back to dashboard

---
