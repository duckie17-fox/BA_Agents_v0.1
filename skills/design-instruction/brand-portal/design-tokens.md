# Brand Portal — Design Tokens

> Design reference for `design-instruction` skill.
> Values only — no framework code, no class names.
> Source of truth: `tokens.css` + `colors_and_type.css` in Brand Portal Design System skill.

---

## Colors

### Primary

| Name | Hex | Usage |
|---|---|---|
| Primary | #071640 | Buttons, CTAs, active nav, links, sidebar |
| Primary Hover | #15254d | Button hover, interactive hover states |
| Primary Soft | #EEF0F6 | Tinted surface, selected row background |
| Primary Tint | #D8DCE6 | Subtle accent background |

### Semantic

| Name | Hex | Soft | Usage |
|---|---|---|---|
| Success | #059669 | #ECFDF5 | Positive states, completed, approved |
| Warning | #D97706 | #FFFBEB | Caution, pending states |
| Danger | #DC2626 | #FEF2F2 | Destructive actions, errors, delete |
| Info | #2563EB | #EFF6FF | Informational, syncing states |

### Neutral (slate)

| Hex | Usage |
|---|---|
| #F8FAFC | Alternate row background, input background |
| #F1F5F9 | App background |
| #E2E8F0 | Border default, card edge |
| #CBD5E1 | Border strong, input border |
| #94A3B8 | Muted text, placeholder |
| #64748B | Secondary text (medium) |
| #475569 | Secondary text |
| #334155 | — |
| #1E293B | Accent, dark text |
| #0F172A | Deepest foreground |

### Surfaces

| Name | Hex | Usage |
|---|---|---|
| App background | #F1F5F9 | Page background |
| Card | #FFFFFF | Card, panel, dialog surfaces |
| Alt surface | #F8FAFC | Alternate rows, input fill |
| Sunken | #EEF2F6 | Recessed areas, disabled input bg |
| Sidebar | #162040 | Sidebar shell (dark navy — đậm hơn primary) |

### Borders

| Name | Hex | Usage |
|---|---|---|
| Border | #E2E8F0 | Default divider, card edge, table border |
| Border Strong | #CBD5E1 | Input border, stronger separator |
| Divider | #EEF2F6 | Subtle section divider |

### Category / Accent Colors

Used for tags, pills, data series, alternate themes. One accent per surface.

| Name | Hex |
|---|---|
| Teal | #0D9488 |
| Rose | #E11D48 |
| Purple | #9333EA |
| Amber | #F59E0B |
| Indigo | #4F46E5 |

---

## Typography

### Font Family

| Token | Value |
|---|---|
| Font Family/Headings | Inter |
| Font Family/Body | Inter |
| Font Family/Mono | IBM Plex Mono (số, ID, code) |

### Font Weight

| Token | Value |
|---|---|
| Regular | 400 |
| Medium | 500 |
| Semi Bold | 600 |
| Bold | 700 |

### Headings

| Style | Size | Line Height | Weight |
|---|---|---|---|
| Headings/H1 | 60px | 72px | Semi Bold (600) |
| Headings/H2 | 48px | 56px | Semi Bold (600) |
| Headings/H3 | 40px | 48px | Semi Bold (600) |
| Headings/H4 | 32px | 40px | Semi Bold (600) |
| Headings/H5 | 24px | 28px | Semi Bold (600) |
| Headings/H6 | 20px | 24px | Semi Bold (600) |

> Heading scale dành cho marketing / landing pages. Trong admin UI, dùng Body/lg trở xuống. H1 admin = Body/lg-semibold (20px), H2 admin = Body/md-semibold (16px).

### Body

| Style | Size | Line Height | Weight | Decoration |
|---|---|---|---|---|
| Body/lg | 20px | 24px | Regular (400) | — |
| Body/lg-semibold | 20px | 24px | Semi Bold (600) | — |
| Body/lg-link | 20px | 24px | Regular (400) | Underline |
| Body/md | 16px | 20px | Regular (400) | — |
| Body/md-semibold | 16px | 20px | Semi Bold (600) | — |
| Body/md-link | 16px | 20px | Regular (400) | Underline |
| Body/sm | 14px | 16px | Regular (400) | — |
| Body/sm-semibold | 14px | 16px | Semi Bold (600) | — |
| Body/sm-link | 14px | 16px | Regular (400) | Underline |
| Body/xsm | 12px | 16px | Regular (400) | — |
| Body/xsm-semibold | 12px | 16px | Semi Bold (600) | — |
| Body/xsm-link | 12px | 16px | Regular (400) | Underline |

**Default body text:** Body/sm (14px) cho nội dung trang. Body/xsm (12px) cho meta, label, helper text.

### Usage mapping trong admin UI

| Context | Style |
|---|---|
| Page title | Body/lg-semibold (20px 600) |
| Section header | Body/md-semibold (16px 600) |
| Card title | Body/md-semibold (16px 600) |
| Body text, table cell | Body/sm (14px 400) |
| Form label (uppercase) | Body/xsm-semibold (12px 600) |
| Caption, meta, timestamp | Body/xsm (12px 400) |
| KPI / stat number | Body/lg-semibold + IBM Plex Mono (20px+) |
| ID / code | IBM Plex Mono 12px |

---

## Spacing

4px base grid. **Tất cả padding, gap, margin phải là bội số của 4px.** Không dùng giá trị lẻ như 5px, 6px, 9px, 10px, 11px, 14px, 18px.

| Token | Value | Common use |
|---|---|---|
| s-1 | 4px | Gap nhỏ nhất, margin-bottom label |
| s-2 | 8px | Gap icon+text, padding nav item vertical |
| s-3 | 12px | Padding input horizontal, gap form fields, padding filter th |
| s-4 | 16px | Padding section horizontal, padding nav item horizontal, gap medium |
| s-5 | 20px | Card padding comfortable, page section gap |
| s-6 | 24px | Page horizontal padding, large section gap |
| s-8 | 32px | Large layout gap |
| s-10 | 40px | Page section padding vertical |
| s-12 | 48px | Large page padding |
| s-16 | 64px | Topbar height |
| s-20 | 80px | Extra large |

**Snap guide — values bị cấm và thay thế:**

| Sai | Đúng |
|---|---|
| 5px | 4px hoặc 8px |
| 6px | 4px hoặc 8px |
| 9px | 8px |
| 10px | 8px hoặc 12px |
| 11px | 12px |
| 14px | 12px hoặc 16px |
| 18px | 16px hoặc 20px |

---

## Border Radius

Scale: **0, 1, 2, 4** — sau 4 trở đi là bội số của 4 (**8, 12, 16, 24**). Pill = 100px. Không dùng giá trị lẻ như 3px, 5px, 6px, 10px.

| Token | Value | Usage |
|---|---|---|
| none | 0px | Sharp corners — table cells, dividers |
| xs | 2px | Micro elements (accent bar, checkbox) |
| sm | 4px | Tags, small chips, badges, ID badge |
| md | 8px | Buttons (default), inputs, nav items, alerts, search |
| lg | 12px | **Cards** — default radius cho tất cả card surfaces |
| xl | 16px | Dialogs, modals, large containers |
| 2xl | 24px | Large modals, bottom sheets |
| pill | 100px | Pills, toggles, badges, count chips |

---

## Shadows / Elevation

Cool-tinted (slate base), no warm shadows.

| Level | Value | Usage |
|---|---|---|
| xs | `0 1px 2px rgba(15,23,42,.05)` | Subtle row hover, minimal lift |
| sm | `0 1px 3px rgba(15,23,42,.06), 0 1px 2px rgba(15,23,42,.04)` | **Card default (resting)** |
| md | `0 4px 12px rgba(15,23,42,.08)` | Card hover state |
| lg | `0 10px 28px rgba(15,23,42,.10)` | Dropdown menus, popovers |
| xl | `0 20px 50px rgba(15,23,42,.14)` | Modals, dialogs, sticky overlays |
| brand | `0 6px 18px rgba(7,22,64,.28)` | Primary CTA button |

---

## Layout

| Element | Value |
|---|---|
| Sidebar width | 280px |
| Sidebar (compact) | 112px |
| Sidebar (thin) | 80px |
| Topbar height | 64px |
| Content max-width | 1440px |

---

## Icons

- Style: Material Outlined, 24×24, stroke-weight 2
- Color: inherits from surrounding text (currentColor)
- Never use emoji as icons

---

## Component Visual Specs

### Card
- Background: #FFFFFF, border: 1px #E2E8F0, radius: 12px, padding: 20px
- Optional 3px top-border accent for category color (red / amber / teal / purple / indigo)
- Dark variant: navy background (#071640) for hero cards
- Hover: shadow sm → md

### Button
- Default radius: 8px — square variant: 6px — pill variant: 100px
- Text: sentence case Vietnamese
- Hover: darken color only — never scale, lift, or animate position
- Primary CTA uses brand shadow: `0 6px 18px rgba(7,22,64,.28)`

### Pill vs Tag
- **Pill** (radius 100px): status indicators, active filters, category labels
- **Tag** (radius 4px): smaller, denser — metadata, secondary labels
- Colors follow semantic palette — one color per surface, never decorative

### App Shell (layout)
- **Sidebar**: full height (100vh), cột trái — chứa logo + nav. KHÔNG bị topbar che.
- **Content**: cột phải — chứa topbar (trên) + body (scroll). Topbar chỉ phủ content, không span qua sidebar.

### Sidebar Navigation
- Background: dark navy #162040 (sidebar shell — đậm hơn brand primary #071640)
- Width: 280px, full height
- **Logo**: bonbon shop wordmark ở trên cùng (trong sidebar), không phải trong topbar
- **Accordion nav**: parent group (icon + label + chevron) → sub-items thụt lề (padding-left 48px)
  - Chevron collapsed: trỏ phải `›` — expanded: trỏ xuống `⌄`
  - Sub-item active: nền `rgba(255,255,255,.1)`, text trắng, semi-bold
  - Hover (parent/sub): `rgba(255,255,255,.05–.06)` overlay
- Single nav item (không có sub): vẫn hiển thị icon + label

### Topbar
- Height: 64px, white, shadow-sm
- Phủ content area (KHÔNG span sidebar)
- LEFT: hamburger toggle
- RIGHT: flag (VI) · fullscreen · divider · user (icon + email)
- KHÔNG có logo (logo ở sidebar), breadcrumb nằm ở page header

### Tabs
- Active: 2px bottom border (navy) + navy text
- No fill — underline only
- Inactive: gray text, no border

### Form Fields / Inputs
- Default: 1px #E2E8F0 border, white background, radius 8px
- Padding horizontal: 12px
- Focus: navy border + 1px inset navy shadow
- Error: #DC2626 border + red validation message below
- Disabled: reduced opacity, no interaction
- Label floats: 120ms transition
- Dense filter fields: 36px height (32px in compact mode)
- Standard fields: 44px height
- Select: `appearance:none`, custom SVG chevron at `right:12px center`

### Fieldset / Form Section
- 1px #CBD5E1 border, radius 12px, padding 16px
- Legend text: sentence case, 12–13px, color #475569
- Internal layout: 2-column grid, gap 12px
- Nested fieldsets supported

---

## Interaction States

### Hover
| Element | Hover treatment |
|---|---|
| Card | shadow-sm → shadow-md |
| Table row | bg #F8FAFC (slate-50) |
| Sidebar nav item | rgba(255,255,255,.06) overlay |
| Button | Darken to primary-hover (#15254d) — no scale/lift |
| Link / text action | Underline appears |

### Active / Selected
| Element | Active treatment |
|---|---|
| Sidebar sub-item | bg rgba(255,255,255,.1) + white text, semi-bold |
| Tab | 2px navy bottom border + navy text |
| Checkbox / Toggle | Navy fill, 200ms animation |
| Input | Navy border + inset shadow |

### Disabled
- Opacity reduced (no specific value — visually dimmed)
- No pointer interaction
- Background: sunken (#EEF2F6) or grayed fill

### Error
- Border: #DC2626
- Message text below field: #DC2626, 12px
- Never red background on the field itself — border + text only

### Loading
- Table body: opacity 0.2, no interaction
- Section: opacity 0.2, no interaction
- Button: replace label with 24px spinner, button disabled

### Empty State
- Center-aligned glyph (64px square, radius 12px, bg #F1F5F9, icon #94A3B8)
- Short informative message below — operational tone, not marketing
- Optional CTA button if user can take action

---

## Screen Layout Patterns

### App Shell
Sidebar full-height bên trái (chứa logo + nav), topbar chỉ phủ content column bên phải.
```
┌──────────────┬──────────────────────────────────────────┐
│ bonbon shop  │ ☰              🇻🇳 ⛶  ◉ user@email   ← TOPBAR │
│ ──────────── ├──────────────────────────────────────────┤
│ ▣ Sản phẩm ⌄ │ Page Title                  [Action btns] │
│   • Sub-item │ ──────────────────────────────────────────│
│   • Sub-item │ [Tabs: Tất cả · Chờ duyệt · ...]          │
│ ▣ Cửa hàng › │ [Sub-action buttons]                      │
│ ▣ Đơn hàng › │ ┌────────────────────────────────────────┐│
│ ▣ Báo cáo  › │ │  Filter / Table / Content              ││
│  ↑ SIDEBAR   │ │                                        ││
│  full height │ └────────────────────────────────────────┘│
│  navy #162040│                          ↑ CONTENT (scroll)│
└──────────────┴──────────────────────────────────────────┘
```
- Sidebar: 280px, full height, navy #162040, logo trên cùng
- Topbar: 64px, chỉ phủ content (không span sidebar) — hamburger trái, flag/fullscreen/user phải
- Page header: title + action buttons (breadcrumb tùy view)

### Table Page
- Status tabs (optional) — above filter panel
- Collapsible filter panel: 5–6 fields in a row, gap 12px
- Action button row below filter: Xuất dữ liệu / Xem lịch sử / Cập nhật ▾
- Table with pagination below
- Column visibility toggle (top-right of table)

### Form Page (full-page)
- Save button: top-right in page header actions — NOT in footer
- Content: scrollable, overflow-y auto
- Fieldsets with legend, 2-column grid inside each
- Fields: dense 36px height, gap 12px

### Detail Page
- Info summary bar: 4-column grid, label (gray 12px) + value (bold 14px) pairs
- Summary uses #F8FAFC background, uppercase labels
- Tab group below summary for content sections
- Optional table at bottom

### Dialog — Confirm
- Width: 40vw, centered
- Body: short message (1–2 lines)
- Footer: [Hủy — stroked] [Xác nhận — raised primary/danger] right-aligned

### Dialog — Form
- Width: 70vw, centered
- Body: form fields inside (same rules as Form Page)
- Footer: [Hủy — stroked] [Lưu — raised primary] right-aligned
- Backdrop: dark overlay, shadow-xl

---

## Data Display Rules

### Numbers
- All numeric values: IBM Plex Mono, tabular figures
- Currency: `482,500,000 ₫` — comma thousands separator, space before ₫
- Large numbers: `2.48 tỷ ₫`, `150 triệu ₫`
- Numeric table columns: right-aligned

### Dates
- Format: `DD/MM/YYYY` (e.g. `23/04/2026`)
- Date + time: `DD/MM/YYYY HH:mm`

### Status Badges / Chips
- Short label, sentence case
- Color maps to semantic palette:
  - Active / Completed / Approved → Success green (#059669)
  - Pending / Draft → Warning amber (#D97706)
  - Error / Rejected / Deleted → Danger red (#DC2626)
  - Syncing / Info → Info blue (#2563EB)
  - Inactive / Archived → Muted gray (#94A3B8)
- Pill shape (radius 100px), soft background variant

### IDs / Codes
- IBM Plex Mono, font-feature `zero` (slashed zero)
- Never truncate ID values

---

## Density Modes

### Comfortable (default)
- Card padding: 20px
- Form field height: 44px
- Standard spacing throughout

### Compact
- Card padding: 14px
- Form field height: 32px (dense filter fields)
- Tighter row heights in tables
- Apply via `data-density="compact"` on the page body

---

## Animation & Motion

- **Allowed:** fade, border-color transition, shadow transition, opacity
- **Never:** bounce, elastic, slide-in, scale on hover, lift/float
- **Duration matrix:**
  - 120ms — label float in inputs
  - 200ms — toggles, checkboxes, standard transitions
  - 320ms — complex panel transitions
- **Easing:** standard Material curve `cubic-bezier(.4,0,.2,1)`

---

## Design Principles

**Language**
- Vietnamese is the primary UI language
- English only for acronyms (GT, MT, NPP, KPI, AOV, SLA) and brand / campaign names

**Color**
- Navy `#071640` is the single primary color
- Red `#DC2626` is for destructive / warn only — never decorative
- One accent color per surface — no color mixing

**Surfaces**
- Flat surfaces with cool shadows — no gradients in core product
- App background #F1F5F9, cards #FFFFFF
- No photography, no illustration in core product UI

**Number & Date Formatting**
- Currency: `482,500,000 ₫` (comma thousands, space before ₫)
- Large numbers: `2.48 tỷ ₫`
- Dates: `DD/MM/YYYY`
- All numeric data: IBM Plex Mono, tabular figures

**Density**
- Default body 13px — dense, data-first
- Minimum touch target: 32px height for interactive elements
- 4px spacing grid throughout
