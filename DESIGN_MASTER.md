# OpsAgent — Master Design Document
## UI/UX & Interaction Guidelines

> **Target Audience:** Non-technical small business owners (Shopkeepers)
> **Primary Device Context:** Low-to-mid range laptops & desktops, potentially poor quality monitors (TN panels, low brightness).
> **Design Vibe:** "Premium Utility" — Clean, trustworthy, high-contrast, zero clutter.

---

## 1. Core Design Philosophy

**"Clarity Over Style"**
The interface must be instantly readable. Information density should be managed carefully—show what matters *now*, hide the rest.

**"One Tap, One Action"**
Avoid buried menus. Primary actions (Confirm Order, Record Payment) must be surface-level and obvious.

**"System Feedback"**
Every action (save, delete, update) must have a clear toast notification or UI state change. The user should never guess if something worked.

---

## 2. Visual Identity (Light Mode Optimized)

We strictly use **Light Mode** as the default and primary mode, as it offers the best readability on lower-quality displays in bright shop environments.

### 2.1 Color Palette (Tailwind CSS based)

| Role | Color | Hex | Usage |
|------|-------|-----|-------|
| **Background** | `slate-50` | `#F8FAFC` | Main app background (soft, not harsh white) |
| **Surface** | `white` | `#FFFFFF` | Cards, Sidebar, Modals |
| **Primary** | `indigo-600` | `#4F46E5` | Main buttons, active states, key links |
| **Primary Hover** | `indigo-700` | `#4338CA` | Hover states |
| **Text Primary** | `slate-900` | `#0F172A` | Headings, main data (High Contrast) |
| **Text Secondary** | `slate-500` | `#64748B` | Labels, timestamps, secondary info |
| **Border** | `slate-200` | `#E2E8F0` | Dividers, card outlines |
| **Success** | `emerald-600` | `#059669` | "Money In", "Order Completed", "Stock Good" |
| **Warning** | `amber-500` | `#F59E0B` | "Low Stock", "Payment Due Soon" |
| **Danger** | `rose-600` | `#E11D48` | "Stockout", "Overdue", "Delete" |

**Why Indigo?** It feels tech-forward but trustworthy (like Stripe/Linear), differentiating from generic "Bootstrap Blue".

### 2.2 Typography

**Font Family:** `Inter` (Google Fonts) or system sans-serif (`ui-sans-serif`).
**Weights:**
- **Regular (400):** Body text.
- **Medium (500):** Button text, table headers, important labels.
- **Semibold (600):** Section headings, metrics, active states.

**Sizing (Desktop):**
- **Headings:** `text-2xl` (24px) or `text-3xl` (30px)
- **Subheadings:** `text-lg` (18px)
- **Body:** `text-sm` (14px) — **Avoid `text-xs` for critical data** as it's hard to read on bad screens.
- **Inputs:** `text-base` (16px) — prevent zoom on mobile, clear readability.

---

## 3. Layout & Structure

### 3.1 The Shell (Desktop)
- **Sidebar (Fixed Left):**
    - Width: `w-64`
    - Color: `bg-white` with right border `border-slate-200`.
    - **Logo:** Top left, clean sans-serif text + simple icon.
    - **Nav Items:** Vertical list.
        - *Active:* `bg-indigo-50 text-indigo-600 border-r-2 border-indigo-600`
        - *Inactive:* `text-slate-600 hover:bg-slate-50`
    - **User Profile:** Bottom fixed. Minimal (Avatar + Name).

- **Main Content Area:**
    - Background: `bg-slate-50`
    - Padding: `p-6` or `p-8`
    - Max Width: Centered container `max-w-7xl` for ultra-wide screens (don't stretch tables to infinity).

### 3.2 Mobile Responsiveness (Critical)
- **Sidebar:** Becomes a bottom tab bar or a hamburger drawer.
- **Tables:** Must scroll horizontally or transform into card lists on mobile.

---

## 4. Component Library Guidance (Shadcn/UI + Tailwind)

### 4.1 Buttons
- **Primary:** `bg-indigo-600 text-white hover:bg-indigo-700 shadow-sm rounded-md px-4 py-2`
- **Secondary:** `bg-white text-slate-700 border border-slate-300 hover:bg-slate-50 shadow-sm rounded-md`
- **Ghost:** `text-slate-600 hover:bg-slate-100 hover:text-slate-900`
- **Destructive:** `bg-rose-600 text-white hover:bg-rose-700`

### 4.2 Cards (The Core Container)
- **Style:** `bg-white rounded-xl border border-slate-200 shadow-sm`
- **Padding:** `p-6` standard.
- **Header:** Title (`text-lg font-semibold text-slate-900`) + Optional Action (top right).

### 4.3 Data Tables
- **Header Row:** `bg-slate-50 border-b border-slate-200`. Text `text-xs font-medium text-slate-500 uppercase tracking-wider`.
- **Rows:** `bg-white border-b border-slate-100 hover:bg-slate-50 transition-colors`.
- **Cells:** `py-4 px-6 text-sm text-slate-700`.
- **Status Badges:** `inline-flex items-center px-2.5 py-0.5 rounded-full text-xs font-medium`
    - *Success:* `bg-emerald-100 text-emerald-800`
    - *Warning:* `bg-amber-100 text-amber-800`
    - *Error:* `bg-rose-100 text-rose-800`

### 4.4 Form Inputs
- **Style:** `block w-full rounded-md border-slate-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm`
- **Labels:** `block text-sm font-medium text-slate-700 mb-1`
- **Validation:** Red text below input, red border.

### 4.5 Alerts / Toasts
- **Toast:** Bottom-right popup. White background, heavy shadow, colored accent border (Green/Red).
- **In-Page Alert:** `rounded-md bg-blue-50 p-4` with `text-blue-700`.

---

## 5. Page-Specific Design Patterns

### 5.1 Dashboard (The "Command Center")
- **Greeting:** "Good morning, [Name]" (Personal touch).
- **KPI Row:** 3-4 Cards.
    - *Icon:* Rounded square background (`bg-indigo-100 text-indigo-600`).
    - *Value:* Large text (`text-3xl font-bold text-slate-900`).
    - *Label:* Small muted text (`text-sm text-slate-500`).
- **Two-Column Layout:**
    - *Left (2/3):* Recent Activity Feed / Orders.
    - *Right (1/3):* "Action Items" (Alerts, Reminders).

### 5.2 Conversations (Chat Interface)
- **Layout:** Three panes (List | Chat | Details) or Two panes (List | Chat).
- **Message Bubbles:**
    - *Customer:* `bg-white border border-slate-200 text-slate-800 rounded-tl-none` (Left).
    - *Bot/Agent:* `bg-indigo-600 text-white rounded-tr-none` (Right).
    - *System Note:* Centered, small, gray pill `bg-slate-100 text-slate-500`.
- **Intent Tags:** Small visible badges on conversations in the list (e.g., "🔴 Complaint", "🟢 Order").

### 5.3 Orders (Kanban or List)
- **Kanban Stages:** Pending -> Confirmed -> Shipped -> Delivered.
- **Card Content:** Order ID, Customer Name, Total Amount, Time Elapsed.
- **Quick Actions:** "Approve" (Tick icon), "Reject" (X icon) directly on the card.

---

## 6. Auth Screens (Firebase)
- **Layout:** Centered card on a clean background.
- **Social Auth:** "Continue with Google" button should be prominent (white with border, Google logo).
- **Email/Pass:** Secondary option below.

---

## 7. Accessibility Checklist
- [ ] **Contrast:** Ensure light gray text is accessible against white.
- [ ] **Focus States:** Visible outline on focused inputs/buttons for keyboard nav.
- [ ] **Touch Targets:** Buttons must be at least 44x44px clickable area.
- [ ] **Labels:** Do not rely on placeholders alone; concise labels are required.

## 8. Tone of Voice (Copywriting)
- **Friendly but Professional.**
- Avoid jargon ("Heuristic Engine" -> "Smart Rules").
- **Proactive:** "We found a new order" vs "New order received".
- **Encouraging:** "All clear! No pending tasks."

---

*Use this document as the source of truth for all frontend styling decisions.*
