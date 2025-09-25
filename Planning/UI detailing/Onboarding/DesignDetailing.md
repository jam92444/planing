## Figma / Wireframe Guidance

Below is a **screen-by-screen spec** you can hand off to your UI/UX designer. Use this text + the JSON schema to build Figma layouts.

---

### 📄 Screen 1: **Signup Page** (`/signup`)

**Layout suggestions:**

- Centered card or modal panel (responsive for mobile + desktop)
- Branding or logo on top
- Title: “Create Your Account”
- Subtitle / caption: “Enter your details to get started”

**Form fields (vertically stacked):**

1. Full Name — input (placeholder “John Doe”)
2. Email — input (placeholder “[you@example.com](mailto:you@example.com)”)
3. Password — password input + toggle “Show / Hide” + optional strength meter
4. Mobile (optional) — input with placeholder “+91 9876543210”
5. Terms & Privacy — checkbox: “I agree to the Terms & Privacy Policy” (link those)
6. Primary CTA button (disabled until validation passes): “Create Account”
7. Secondary link under form: “Already have an account? Log in”
8. Optionally social login buttons: “Continue with Google / Apple” (if supported)

**States / Variants:**

- Input error states (red underline + error message)
- Disabled button — when required input missing or invalid
- Hover / active states for inputs and buttons
- Loading / spinner state on button after submit

---

### 🏢 Screen 2: **Business Type Selection** (`/onboarding/business-type`)

**Layout suggestions:**

- Title at top: “What type of business are you setting up?”

- Subtitle / guidance text below — e.g. “Help us tailor your settings”

- Center area: grid or row of **business type cards**. Each card includes:

  - Icon / illustration
  - Label (Retail, Services, Restaurant, Manufacturing, Others)
  - Optional short description or tooltip (small text under label or hover)

- Cards should be selectable. On selection, the card is visually “active” (e.g. border accent, shadow, checkmark).

- Below cards: “Next” button (disabled until one is selected).

- Optionally: “Back” link if user can go back to signup.

**States / Variants:**

- Unselected card vs selected card
- Hover / focus states
- Disabled Next vs enabled
- Mobile view — cards stack vertically

---

### 🧾 Screen 3: **Business Setup & Preferences** (`/onboarding/setup`)

This is the most complex form, so consider grouping into **sections or accordion / tabs**:

#### Suggested layout:

- Title: “Company Details & Preferences”

- Subtitle / instruction text: “Let’s get your business settings ready”

- Use a multi-section form (either a single scroll or split into collapsible segments):

**Section A: Business Information**

- Business Name — text input
- GST / VAT ID — text input with hint (e.g. “Optional”)
- Country / Region — dropdown
- Currency — dropdown
- Time Zone — dropdown
- Logo Upload — “Upload logo” component (drag-drop / browse) with preview

**Section B: Preferences / Tax & Invoice**

- Tax Enabled — toggle switch (Yes / No)
- When tax is enabled, show:

  - Tax Type — radio buttons (“Inclusive” / “Exclusive”)
  - Default Tax Rate (%) — numeric input

- Invoice Prefix — text input (e.g. “ABC”)
- Stock Tracking — toggle (Yes / No)

**Section C: Initialization (optional / advanced / toggles)**

- Create Default Categories — toggle (Yes / No)
- Initialize Default Tax Rates — toggle (Yes / No)

  - (Alternatively, auto apply defaults silently)

**Bottom Buttons:**

- Back (go to business type screen)
- Complete Setup / Continue (submit)

**Validation & UX:**

- Inline validation for required fields
- If tax enabled and tax_type or rate missing, show error
- Show hints / tooltips for fields (e.g. “What’s invoice prefix?”)
- Upload logo preview, allow remove or change
- Disable “Complete Setup” until valid

---

### ✅ Screen 4: **Success Page** (`/onboarding/success`)

**Layout suggestions:**

- Centered card or panel
- Large success icon / illustration (checkmark, confetti)
- Heading: “Congratulations! Your business is ready.”
- Subtext: “Your account is now set up. You can now go to Dashboard or start adding items.”
- Primary button: “Go to Dashboard”
- Secondary link or button: “Set up first item” / “Invite team members”
- Optionally show “What’s next” list or short checklist

**States / Variants:**

- This is mostly static. You might have hover states for buttons, or optional options to skip further steps.

---

### 📐 Component / UI System Notes for Designer

- Use consistent spacing, fonts, and UI components (input, toggle, button) across screens
- Define color tokens for primary / secondary / disabled / error
- Use icon set or illustration set for business types
- Responsiveness: mobile-first — make sure forms stack, buttons are full width on mobile
- Animation / micro-interactions: card selection, button press, logo upload feedback, success confetti
