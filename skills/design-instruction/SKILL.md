---
name: design-instruction
description: >
  Converts an approved functional spec (write-spec output) into screens drawn directly
  in Figma via figma-mcp-go, using Brand Portal design system tokens and visual patterns.
  Use when ready to hand off to design after spec is HITL-approved, or when someone says
  "tạo design instruction", "vẽ màn hình lên Figma", "design instruction",
  "convert spec to Figma", or "chuẩn bị design cho Figma MCP".
  Do NOT use before write-spec is approved — design instruction requires confirmed spec.
---

# Design Instruction Skill

## Handoff Input

**Receives from:** `write-spec` (HITL approved) + `write-prd` (khi feature có nhiều màn)
**Expected artifact:** Spec (`artifact: Spec`, `status: Approved`)
**Required fields:** Mermaid diagram (có happy path + ít nhất 1 error path), Bảng mô tả trường (đủ 4 cột), Data Model, Edge Cases
**Hard gate:** Nếu `status` ≠ Approved → từ chối, yêu cầu BA hoàn thành HITL trước. Nếu thiếu Mermaid diagram hoặc field table → dừng, yêu cầu bổ sung.
**Navigation gate:** Nếu feature có từ 2 màn trở lên → bắt buộc đọc PRD Section 4 (Screen Map & Navigation) trước khi vẽ. Nếu PRD chưa có Section 4 → dừng, yêu cầu BA bổ sung trước.

## Output

Screens drawn live trong Figma, grouped by user story (screenshot-ready). Sau khi hoàn thành:
1. Spec là `04-spec--[feature].md` → save log tại `artifacts/[folder]/05-design--[feature].md`
2. Log file ghi: Figma page link + story groups + screens list + trạng thái từng screen
3. Notify: `✓ [N] screens drawn in Figma · [M] story groups · Log: artifacts/[folder]/05-design--[feature].md`

## Position in pipeline

```
write-spec (approved)
        │
        ▼
design-instruction
  ├── Parse screens from Mermaid diagram
  ├── Map fields → Brand Portal components
  ├── Derive all states per component
  ├── Load design-tokens + visual reference
  └── Execute figma-mcp-go → screens live in Figma
```

## Input to collect

Extract from spec. Ask only for what's missing.

1. **Spec document** — write-spec output (required): Mermaid diagram, field table, data model, edge cases
2. **PRD** — write-prd output (**bắt buộc khi feature có nhiều màn**): đọc Section 4 (Screen Map & Navigation) để biết navigation pattern và shared shell elements trước khi vẽ màn đầu tiên
3. **User Persona** — extract from spec/PRD nếu có; nếu không có thì derive từ "Người dùng bị ảnh hưởng" trong qualified ticket:
   - **Role**: job title / role trong hệ thống (e.g., Brand Admin, Salesman, Nhà phân phối)
   - **Goal**: họ đang cố làm gì khi dùng tính năng này?
   - **Context**: khi nào / ở đâu họ dùng? (web desktop, mobile, back-office...)
   - **Pain point**: vấn đề gì dẫn đến tính năng này?
   - Nếu nhiều persona → liệt kê hết, đánh dấu **primary**
4. **Screen dimensions** — desktop/tablet/mobile? Default: desktop 1440px
5. **Figma file link** — nếu thêm vào file có sẵn (optional)

## Core extraction logic

| Spec element | Design element |
|---|---|
| Mermaid diagram | Screen list + user flow |
| Field table — Textbox | Input field + states |
| Field table — Dropdown | Select component + options |
| Field table — Switch | Toggle + active/inactive states |
| Field table — Date picker | Date picker + calendar states |
| Field table — Button | Button + show/hide/disable conditions |
| Field table — Text (display) | Text label, read-only style |
| Validation constraints | Error states per field |
| Default values | Default state per component |
| Enable/disable conditions | Disabled state per component |
| Edge cases | Empty / loading / timeout states |
| Data model | Data fields needed per component |

---

## Workflow

### Step 1 — Parse screens from Mermaid diagram
Read the Mermaid diagram. Each major node = one screen/frame.
List all screens in order. Include popup/modal as separate frames.

### Step 1.5 — Pre-flight: Confirm navigation shell

Nếu feature có nhiều màn (PRD Section 4 tồn tại), xác nhận navigation shell trước:

```
Navigation pattern: [Tabs / Sidebar sub-nav / Drill-through]
Shared shell elements:
  - [Tab bar: Tab A │ Tab B │ Tab C]
  - [Filter bar persist: TM Program, Date Range, ...]

Mỗi spec sẽ vẽ 1 tab/view. Shell (tab bar + filter bar) xuất hiện ở tất cả frames.
Confirm pattern này trước khi vẽ frame đầu tiên?
```

**Chờ BA confirm.** Nếu navigation pattern sai → cập nhật PRD Section 4 rồi mới vẽ.

---

### Step 1.6 — Confirm screen list + story grouping

Trước khi vẽ bất kỳ frame nào, trình bày cho BA xác nhận:

```
Tìm thấy [N] screens từ spec. Đề xuất tổ chức như sau:

📋 Story: [Tên user story 1]
   → [Screen A — type: list]
   → [Screen B — type: form]

📋 Story: [Tên user story 2]
   → [Screen C — type: dialog]
   → [Screen D — type: empty state]

Mỗi story group sẽ là 1 section trong Figma
→ Screenshot 1 section = 1 ảnh attach vào Jira story

Tiếp tục vẽ không?
```

**Chờ BA confirm** trước khi chuyển sang Step 2.
Nếu BA muốn điều chỉnh grouping → update rồi mới vẽ.

**Cách map screen → story:**
- Screens cùng 1 flow (vd: list → create → success) = 1 story
- Dialog xuất phát từ list = thuộc story của list
- State khác nhau của cùng 1 screen (default, error, empty) = cùng story

### Step 2 — Token declaration (hợp đồng thiết kế)

**Trước khi wireframe hay Figma** — khai báo đầy đủ spec cho **từng component** sẽ xuất hiện trong screen. Bảng này là "hợp đồng" mà Step 7 sẽ verify lại.

| Component | Variant / State | Fill | Border | Text | h / w | Padding | Radius |
|---|---|---|---|---|---|---|---|
| Button (Primary) | Default | `#071640` | `#071640` | `#ffffff` | 44px | 16px 16px | 8px |
| Button (Primary) | Disabled | `#e2e8f0` | `#e2e8f0` | `#94a3b8` | 44px | 16px 16px | 8px |
| Field / Input | Default | `#ffffff` | `#cbd5e1` | placeholder `#94a3b8` | 44px | 12px 16px | 8px |
| _(thêm row cho mỗi component)_ | | | | | | | |

**STOP condition:** Nếu bất kỳ ô nào chưa rõ → tra cứu trong Component quick-ref (Section 6.6). Nếu vẫn không rõ và file `brand-portal/design-tokens.md` tồn tại → đọc file đó. Không được điền giá trị tự đoán.

---

### Step 2.5 — ASCII wireframe + component map

**Trước khi gọi bất kỳ figma-mcp-go tool nào**, output wireframe đầy đủ của từng màn hình và declare component cho mọi element.

```
┌──────────────────────────────────────────────────────────────┐
│  SECTION NAME · h:Xpx · bg:#XXXXXX · padding:Xpx             │
│                                                               │
│  [Element label]  ← Component: [tên] · [variant]             │
│                     Token: [hex / size / spacing / radius]    │
└──────────────────────────────────────────────────────────────┘
```

Rules:
- Mọi interactive element → tên component + variant (vd: `Button primary · fill #071640 · h 44px · radius 8px`)
- Mọi text node → typography token (vd: `Body/sm 14px Regular #0F172A`)
- Mọi màu → exact hex từ design-tokens.md, không tự đặt màu
- Mọi spacing & padding → bội số 4px (4, 8, 12, 16, 20, 24, 32...)
- Mọi radius → từ scale: 0 / 2 / 4 / 8 / 12 / 16 / 24 / 100px

**Chờ BA confirm wireframe trước khi chạy Figma.** Sửa ở đây rẻ hơn nhiều so với sửa trong Figma.

**Pipeline mode (Override HITL):** Không chờ confirm từ BA. Thay vào đó: lưu wireframe ASCII tất cả screens vào `05-design-state.md` (section per-screen `### ASCII Wireframe`) TRƯỚC khi gọi bất kỳ figma-mcp-go tool nào. Wireframe phải có trong state file — không được bỏ qua.

---

### Icon System — áp dụng toàn bộ file, không ngoại lệ

**Chỉ dùng 1 trong 2 cách sau — không bao giờ dùng emoji, Unicode ký tự, hay text thường:**

| Trường hợp | Cách dùng |
|---|---|
| Font "Material Icons" có trong file | `create_text` font="Material Icons", size=20, fill theo context |
| Font "Material Icons" KHÔNG có | `create_rectangle` 20×20, fill theo context, opacity phù hợp |

**Tên icon Material Icons thường dùng:**
- Navigation: `arrow_back`, `chevron_right`, `expand_more`, `menu`
- Action: `add`, `edit`, `delete`, `save`, `close`, `search`, `filter_list`, `download`, `upload`
- Status: `check_circle`, `error`, `warning`, `info`
- Data: `bar_chart`, `table_chart`, `trending_up`
- UI: `more_vert`, `refresh`, `settings`, `visibility`, `visibility_off`

**Kiểm tra font 1 lần duy nhất** (Step 6.4 đã có) → ghi nhớ kết quả → áp dụng cho TẤT CẢ icons trong file, kể cả ngoài sidebar.

---

### Step 3 — Map fields to components
For each screen, map each field to a Brand Portal component type:
- Textbox → Input field (radius 8px, 44px height)
- Dropdown → Select field
- Button primary → raised navy button
- Table → data table with pagination
- Toggle → pill-shaped toggle (radius 100px)
- Date picker → date input with calendar
- Numeric display → IBM Plex Mono, right-aligned

### Step 4 — Identify states (phạm vi giới hạn)

Chỉ vẽ các states sau — không thêm:

| Loại màn hình | States cần vẽ |
|---|---|
| List / Danh sách | Default (có data) + Empty state |
| Form Create / Edit | Default (form trống, button Lưu active) |
| Detail / View | Default |
| Dialog / Modal | Default popup only |

Skip hoàn toàn: error/validation state, loading state, disabled state, edge case screens.

### Step 5 — Load Brand Portal design reference (optional — đọc nếu tồn tại)

> Token quick-ref đã nhúng trong Section 6.6 — đủ để vẽ mà không cần file ngoài.
> Đọc các file dưới đây **nếu có** để tăng accuracy. Không có → tiếp tục bình thường.

**Đọc nếu tồn tại (bổ sung token detail):**

| File | Dùng để |
|---|---|
| `.claude/skills/design-instruction/brand-portal/design-tokens.md` | Tokens đầy đủ, component specs chi tiết hơn Section 6.6 |
| `.claude/skills/design-instruction/brand-portal/app-layout.html` | App shell structure: sidebar accordion, topbar |
| `.claude/skills/design-instruction/brand-portal/design-reference.html` | Visual demos: colors, type scale, components |

**Đọc nếu tồn tại (visual reference — chỉ khi vẽ loại màn hình tương ứng):**

| File | Dùng khi |
|---|---|
| `.claude/skills/design-instruction/brand-portal/screenshots/form-create.png` | Vẽ màn hình create/edit form |
| `.claude/skills/design-instruction/brand-portal/screenshots/form2.png` | Vẽ form với validation errors |
| `.claude/skills/design-instruction/brand-portal/screenshots/paginator2.png` | Reference pagination |
| `.claude/skills/design-instruction/brand-portal/screenshots/Quản lý đơn hàng.png` | Reference table page với tabs |
| `.claude/skills/design-instruction/brand-portal/screenshots/Quản lý sản phẩm.png` | Reference filter grid |
| `.claude/skills/design-instruction/brand-portal/screenshots/Signin.png` | Màn hình đăng nhập |
| `.claude/skills/design-instruction/brand-portal/assets/logo.png` | Logo BonBon Shop |

---

### Step 6 — Vẽ màn hình lên Figma (figma-mcp-go)

#### 6.1 — Chuẩn bị Figma file

```
1. get_document          → hiểu state file hiện tại
2. get_pages             → kiểm tra page đã có chưa
3. navigate_to_page      → đi đến page đang làm việc
   HOẶC add_page         → tạo page mới nếu chưa có
                            (đặt tên theo feature: vd "Quản lý đơn hàng")
```

#### 6.1b — Tổ chức Figma theo user story (screenshot-ready)

Mỗi story group = 1 **Section frame** lớn chứa tất cả screens liên quan.
Screenshot 1 section = 1 ảnh → attach thẳng vào Jira ticket.

```
Page: "Quản lý đơn hàng"
│
├── [Section] "Story: Xem danh sách đơn hàng"     ← screenshot toàn section
│   ├── Frame "Danh sách — Default"
│   └── Frame "Danh sách — Empty"
│
├── [Section] "Story: Tạo đơn hàng mới"
│   ├── Frame "Thêm mới — Form"
│
└── [Section] "Story: Xóa đơn hàng"
    └── Frame "Dialog — Xác nhận xóa"
```

**Tạo section frame:**
```
create_frame  name="Story: [Tên]"  fills=transparent  stroke=none
→ width = (số screens × 1440) + (gap × (n-1))  height = auto
→ Frames xếp ngang, gap 64px giữa các screen
```

**Đặt tên frame chuẩn:** `[Tên màn hình] — [State]`
- Default state: `Danh sách đơn hàng — Default`
- Empty: `Danh sách đơn hàng — Empty`
- Dialog: `Dialog — Xác nhận xóa`

#### 6.2 — Naming conventions

| Element | Convention | Ví dụ |
|---|---|---|
| Page | Feature name (Vietnamese) | `Quản lý đơn hàng` |
| Frame (screen) | Screen name from Mermaid | `Danh sách đơn hàng`, `Thêm đơn hàng` |
| Frame (dialog) | `Dialog / [Name]` | `Dialog / Xác nhận xóa` |
| Layer group | Component role | `Topbar`, `Sidebar`, `Page Header`, `Filter Panel`, `Table`, `Pagination` |
| Component | Semantic name | `Button / Primary`, `Input / Default`, `Status / Success` |

#### 6.3 — App shell vs Content-only

**RULE: Mọi màn hình chính (list, form, detail, dashboard) ĐỀU PHẢI có sidebar + topbar.**
Chỉ dialog/modal mới được vẽ không có sidebar.

**Full app shell** (BẮTBUỘC cho tất cả màn hình không phải dialog):
```
Frame 1440×900 "Screen Name"
├── Sidebar [280×900, fill #162040]           ← LUÔN VẼ
│   ├── Logo [text bonbon shop]
│   └── Nav [accordion groups + sub-items]
└── Content [1160×900, fill #F1F5F9]
    ├── Topbar [1160×64, fill #FFFFFF, shadow-sm]
    ├── Tab Bar [1160×48, fill #FFFFFF, border-bottom #E2E8F0]  ← VẼ NẾU PRD define tabs
    │   ├── Tab Active [padding 12 16, border-bottom 2px #071640, text #071640 600]
    │   └── Tab Inactive [padding 12 16, text #64748B 400]
    │   (Active tab = tab đúng với màn đang vẽ trong spec này)
    └── Content Body [VERTICAL layout, NO padding — padding handled per zone]
        ├── Page Header [fill #FFFFFF, full-width]     ← KHÔNG margin
        ├── Filter Bar  [fill #FFFFFF, full-width]     ← KHÔNG margin (nếu chưa ở Topbar)
        └── Cards Container [fill #F1F5F9,             ← WRAPPER bắt buộc
             VERTICAL layout, paddingL:24, paddingR:24,
             paddingT:16, paddingB:24, gap:16]
            ├── Card / Chart / Table
            └── ...thêm cards
```

⚠️ **FULL-WIDTH STRETCH RULE — áp dụng cho MỌI child của Content Body và Cards Container:**
Sau khi `reparent_nodes` BẤT KỲ frame nào vào VERTICAL auto-layout container, phải gọi ngay:
```
set_constraints(nodeId=<child_id>, horizontal="STRETCH", vertical="TOP")
```
Áp dụng cho: **Page Header, Filter Bar, Sort Control, Cards Container, mọi Card/Panel/Table.**
KHÔNG được để width cố định (vd: `width=416`, `width=261`) trên bất kỳ full-width element nào.
Nếu không set STRETCH → Figma giữ nguyên width lúc tạo → element bị cụt so với container (lỗi phổ biến nhất).

**Tab Bar rules:**
- Chỉ vẽ khi PRD Section 4 define navigation pattern = Tabs
- Tất cả frames trong cùng feature đều có tab bar — chỉ khác tab nào đang active
- Tab labels lấy từ PRD Screen Map, giữ nguyên thứ tự
- Không tự sáng tạo tab bar khi PRD không define
- **Platform rule:** Tab Bar dùng cho **SFA mobile app**. **Portal web admin KHÔNG dùng Tab Bar** — dùng Sidebar sub-nav thay thế. Nếu feature là Portal screen mà PRD define Tabs → flag lại cho BA kiểm tra pattern trước khi vẽ.

> ⚠️ **Cards Container là bắt buộc.** Figma auto-layout đặt children tại x=0 mặc định — nếu đặt card trực tiếp vào Content Body không có wrapper, card sẽ flush-left (sát mép trái) dù width nhỏ hơn 1160px. Chỉ Page Header, Filter Bar, Topbar là full-width (không margin). Tất cả cards/charts/tables phải nằm trong `Cards Container` với paddingLeft:24, paddingRight:24.

**Sidebar drawing spec (chi tiết — lý do nav hay bị xấu):**

```
Sidebar: width=280, height=900, fill=#162040
  auto-layout: VERTICAL, padding=0, gap=0, clip-content=true

Logo zone:
  Frame: width=280, height=auto, padding=20 16 16 16
  Row 1 — Logo text:
    create_text "bonbon shop"
    font=Inter, size=24, weight=900, fill=#FFFFFF
  Row 2 — Tagline:
    create_text "giá ngon, bán gọn"
    font=Inter, size=9, fill=#FFFFFF, opacity=40
    margin-top=4

Nav parent item (COLLAPSED — chevron right):
  Frame: width=280, height=48
  auto-layout: HORIZONTAL, padding=12 16, gap=8
  fill=transparent (no fill — để lộ sidebar bg)
  → Icon: create_text, font="Material Icons", size=20, fill=#FFFFFF, opacity=50
    text = tên icon (ví dụ: "bar_chart", "store", "settings", "event_note", "people")
    ← font Material Icons render ligature name thành icon vector, KHÔNG phải chữ thường
    KHÔNG dùng emoji (📊🎛️...) và KHÔNG dùng regular text làm icon
    Fallback nếu font "Material Icons" không có trong file: Rectangle 20×20, fill=#FFFFFF, opacity=50
  → Label: Inter 14/600, fill=#FFFFFF, opacity=85
    gap=8 từ icon   ← khoảng cách icon → title
  → Spacer: width=auto (fill remaining space)
  → Chevron: create_text, font="Material Icons", size=16, fill=#FFFFFF, opacity=55
    text="chevron_right"   ← KHÔNG dùng "›" hay ">" text thường

Nav parent item (EXPANDED — chevron down):
  Tương tự collapsed, đổi chevron text="keyboard_arrow_down"   ← KHÔNG dùng "expand_more" (renders như chữ v mỏng), KHÔNG dùng "∨" hay "v" text thường

Sub-item (INACTIVE):
  Frame: width=280, height=40
  auto-layout: HORIZONTAL, paddingLeft=48, paddingRight=16,
               paddingTop=8, paddingBottom=8
  fill=transparent
  → Text: Inter 14/600, fill=#FFFFFF, opacity=65
    ← weight=600 (cùng font đậm như active), chỉ khác opacity

Sub-item (ACTIVE — quan trọng nhất):
  Outer Frame: width=280, height=44, padding=2 8 (top/bottom 2, left/right 8)
  fill=transparent
  Inner Frame: width=264, height=40, border-radius=4
    fill=#FFFFFF, opacity=12
  → Text: Inter 14/700, fill=#FFFFFF, opacity=100
    ← weight=700 + opacity=100 để nổi bật rõ hơn nền mờ
```

**⚠️ Lỗi phổ biến với màu rgba trong Figma API:**
```
SAI:  set_fills(color="rgba(255,255,255,0.1)")   ← API không nhận string rgba
ĐÚNG: set_fills(color="#FFFFFF") + set_opacity(10)   ← fill hex + opacity %
```

**Content-only** (CHỈ dùng cho dialogs, modals, overlays):
```
Frame [width: 576px confirm / 70vw form, centered]
├── Header [title, border-bottom 1px #EEF2F6]
├── Body [form fields / message]
└── Footer [Hủy ghost + Lưu/Xác nhận navy, right-aligned]
```

#### 6.4 — Execution sequence mỗi screen

**SEQUENTIAL RULE — depth-first, không batch:** Hoàn thành TOÀN BỘ 1 screen (sidebar + topbar + content + tất cả components) trước khi tạo bất kỳ frame nào cho screen tiếp theo. **Không tạo placeholder frames rỗng.** Nếu context window sắp đầy → dừng lại, ghi log screens đã xong và screens chưa vẽ, raise lại để tiếp tục sau.

> **Clone flow exception:** Steps 1–7 bên dưới áp dụng cho draw-from-scratch (MASTER screen). Với screens clone từ MASTER → bỏ qua steps 1–7, thực hiện theo **Clone strategy** (cuối section 6.4).

Lý do: breadth-first (tạo tất cả frames trước, fill sau) dẫn đến tất cả screens đều 70% hoàn chỉnh khi context cạn. Depth-first đảm bảo mỗi screen hoặc là 100% hoặc là chưa tồn tại — không có trạng thái "nửa vời".

```
Với MỖI screen (xử lý tuần tự, xong screen này mới sang screen khác):

1. create_frame        → tạo frame với đúng width/height
2. set_fills           → bg color (#F1F5F9 cho app shell, #FFFFFF cho dialog)
3. set_auto_layout     → direction VERTICAL, padding 0, gap 0
4. Vẽ SIDEBAR đầy đủ trước (bắt buộc với mọi non-dialog screen):
   PRE-CHECK font (1 lần duy nhất, trước khi vẽ sidebar đầu tiên trong file):
     Gọi get_fonts → kiểm tra "Material Icons" có trong danh sách không.
     Nếu CÓ → dùng create_text font="Material Icons" cho icon.
     Nếu KHÔNG → dùng create_rectangle 20×20 fill=#FFFFFF opacity=50 cho icon.
     Ghi nhớ kết quả này, áp dụng cho TẤT CẢ screens trong lần chạy — không check lại mỗi screen.
   a. create_frame sidebar (width=280, height=900, fill=#162040) → lưu sidebar_id
      → Sau khi reparent sidebar vào main frame: set_constraints(sidebar_id, horizontal="LEFT", vertical="STRETCH")
      → Main frame phải dùng auto-layout HORIZONTAL với counterAxisAlignItems="STRETCH" để sidebar fill full chiều dọc
   b. Logo zone: create frame → reparent_nodes vào sidebar_id ngay lập tức → verify bằng get_node(sidebar_id)
   c. Mỗi nav parent item (tạo và reparent từng cái, không batch):
      - create_frame (280×48) → reparent_nodes vào sidebar_id ngay → sau đó mới tạo icon + label bên trong
      - KHÔNG tạo tất cả items rồi mới reparent một lần — lý do: reparent batch dễ silent fail
   d. Mỗi nav sub-item INACTIVE: create frame (280×40, paddingLeft=48) → reparent_nodes vào sidebar_id ngay
   e. Sub-item ACTIVE: tạo outer frame (280×44) → reparent vào sidebar_id → tạo inner frame (264×40) → reparent vào outer frame
   f. Verify sau khi vẽ xong toàn bộ sidebar:
      get_node(sidebar_id) → đếm số children.
      Số children phải = 1 (logo zone) + số nav items + sub-items.
      Nếu số lượng sai → search_nodes trên page để tìm node "loose" (nằm ngoài sidebar) → reparent_nodes lại.
      Node loose = node xuất hiện trên canvas ngoài sidebar frame = BUG, phải fix trước khi vẽ topbar.
5. Vẽ TOPBAR (height=64, fill=#FFFFFF, stroke bottom #E2E8F0)
   → Sau khi reparent topbar vào Content frame: set_constraints(topbar_id, horizontal="STRETCH", vertical="TOP")
   → Content frame dùng auto-layout VERTICAL với counterAxisAlignItems="STRETCH" → topbar tự fill full chiều ngang
   → KHÔNG hard-code width=1160 cho topbar — để Figma tự stretch theo Content width
6. Vẽ CONTENT (page header → filter bar → cards container → table/form/cards)
   - Với mỗi zone: create_frame → set_fills → set_auto_layout → add components → reparent
7. reparent_nodes: sidebar + topbar + content vào main frame

Screen Done Gate — kiểm tra trước khi sang screen tiếp:
□ Sidebar: logo zone + đúng số nav items + active sub-item highlighted
□ Sidebar stretch: sidebar fill full chiều dọc của frame (không bị cắt ngắn giữa chừng)
□ Topbar stretch: topbar fill full chiều ngang của Content frame (không có khoảng trắng bên phải)
□ Topbar: đúng height, bottom border #E2E8F0
□ Chevron nav: verify fontFamily="Material Icons" bằng get_node — KHÔNG được là text Unicode ("›", "∨", "v", ">")
   Cách verify: get_node(chevron_node_id) → xem style.fontFamily = "Material Icons"
   Nếu sai → xóa node → tạo lại với create_text, font="Material Icons", text="chevron_right" (hoặc "keyboard_arrow_down")
□ Content: tất cả components theo spec đã được vẽ
□ Borders trên cards, inputs, filter chips, table đã có (set_strokes)
□ KHÔNG có annotation text node trong frame (xem quy tắc bên dưới)
→ Nếu bất kỳ ô nào chưa ✓ → sửa ngay, không sang screen tiếp
```

⛔ **TUYỆT ĐỐI KHÔNG tạo annotation text trong Figma frame:**
Không được tạo text node, frame, hay layer nào bên trong screen frame với mục đích giải thích state, điều kiện, hay logic kỹ thuật (ví dụ: "STATE A: Đang chạy → nút navy", "↑ xem state B", "TODO: thêm tooltip"). Những ghi chú này làm hỏng canvas và không có giá trị cho developer.
Tất cả ghi chú kỹ thuật và state description chỉ được lưu trong:
- File log `05-design--[feature].md` (section State Coverage)
- Wireframe ASCII (section Wireframes trong log)

**Icon trong sidebar:** Dùng `create_text` với font `"Material Icons"` — font này render tên icon thành vector glyph (ví dụ text `"bar_chart"` → icon biểu đồ cột). Nếu font "Material Icons" không có trong file: dùng `create_rectangle` 20×20 fill=#FFFFFF opacity=50 làm placeholder, KHÔNG dùng emoji hay Unicode ký tự.

**Clone strategy — tối ưu từ screen 2 trở đi (kể cả screens từ feature khác):**
- Screen 1 của story group đầu tiên trong queue (MASTER): vẽ đầy đủ từ đầu (sidebar + topbar + content). Sau khi hoàn thành → ghi frame ID vào `05-design-state.md` với key `MASTER_FRAME_ID: [node_id]`. Ba-lead đọc giá trị này và truyền cho mọi @design-agent tiếp theo.
- Nếu ba-lead truyền `master_frame_id` ≠ "NONE": bắt buộc dùng `clone_node(master_frame_id)` làm điểm xuất phát cho screen đầu tiên trong story group — kể cả khi story group này thuộc feature khác.
- Nếu `master_frame_id = "NONE"` (story group đầu tiên của toàn bộ CR): vẽ đầy đủ từ đầu → ghi MASTER_FRAME_ID vào 05-design-state.md.
- Từ screen 2 trở đi trong cùng story group: tiếp tục `clone_node` từ MASTER frame → rename → cập nhật 2 điểm:

  **Bước A — Tìm node sau khi clone:**
  Sau `clone_node(MASTER_FRAME_ID)` tất cả node ID đều mới. Dùng naming convention (section 6.5) để tìm:
  ```
  search_nodes(query="Nav / Sub / [tên active hiện tại] — Active", nodeId=cloned_frame_id)
  search_nodes(query="Nav / Sub / [tên active mới]", nodeId=cloned_frame_id)
  ```

  **Bước B — Đổi active nav (cấu trúc khác nhau, không chỉ đổi style):**

  Active → Inactive (xóa inner frame):
  ```
  1. search_nodes → lấy outer_frame_id của sub-item đang active (tên: "Nav / Sub / [label] — Active")
  2. get_node(outer_frame_id) → lấy inner_frame_id (child có fill=#FFFFFF opacity=12, width=264)
  3. delete_nodes(inner_frame_id)
  4. resize outer frame: height=40
  5. set_auto_layout: paddingLeft=48, paddingTop=8, paddingBottom=8, paddingRight=16
  6. rename_node: bỏ " — Active" khỏi tên
  7. get text node → style: fontWeight=600, opacity=65
  ```

  Inactive → Active (tạo inner frame):
  ```
  1. search_nodes → lấy inactive_frame_id (tên: "Nav / Sub / [label]")
  2. get_node(inactive_frame_id) → lấy text_node_id
  3. create_frame: width=264, height=40, radius=4 → lưu inner_frame_id
  4. set_fills(inner_frame_id, color="#FFFFFF") + set_opacity(inner_frame_id, 12)
  5. reparent_nodes([text_node_id], inner_frame_id)
  6. reparent_nodes([inner_frame_id], inactive_frame_id)
  7. resize inactive_frame_id: height=44
  8. set_auto_layout: paddingTop=2, paddingBottom=2, paddingLeft=8, paddingRight=8
  9. rename_node: thêm " — Active" vào tên
  10. get text node → style: fontWeight=700, opacity=100
  ```

  **Bước C — Replace content zone:**
  ```
  search_nodes(query="Content", nodeId=cloned_frame_id) → lấy content_id
  delete_nodes(content_id)
  → vẽ lại content đúng spec cho screen này (page header → filter bar → cards container)
  ```

  Không vẽ lại sidebar + topbar từ đầu — đảm bảo consistency và tiết kiệm thời gian.

#### 6.5 — Node naming convention

Đặt tên node theo pattern cố định — không dùng "Frame 1", "Rectangle 2":

| Node | Pattern | Ví dụ |
|---|---|---|
| Screen frame chính | `[Feature] / [Screen name]` | `Evaluation Plan / List`, `Display Rules / Create` |
| Sidebar | `Sidebar` | |
| Nav item active | `Nav / [label] — Active` | `Nav / Kế hoạch đánh giá — Active` |
| Nav item inactive | `Nav / [label]` | `Nav / Nguyên tắc trưng bày` |
| Nav sub-item | `Nav / Sub / [label] — Active/Inactive` | |
| Table row | `Row [N] — [data]` | `Row 3 — VPR-001`, `Row 1 — Nguyên tắc A` |
| Status badge | `badge-[status]` | `badge-Active`, `badge-Stopped` |
| Icon node | `icon/[name]` | `icon/search`, `icon/chevron-right` |
| Button | `btn-[variant]` | `btn-primary`, `btn-ghost` |

---

#### 6.6 — Component states quick-ref

Dùng bảng này khi khai báo Step 2 token contract — không tự đoán giá trị:

**Button**
```
Primary default   fill=#071640 · border=#071640 · text=#FFFFFF · h=44 · pad=10 16 · r=8
Primary disabled  fill=#E2E8F0 · border=#E2E8F0 · text=#94A3B8 · h=44 · pad=10 16 · r=8
Ghost default     fill=#FFFFFF · border=#E2E8F0 · text=#475569 · h=44 · pad=10 16 · r=8
Ghost hover       fill=#F1F5F9 · border=#E2E8F0 · text=#475569
Danger            fill=#DC2626 · border=#DC2626 · text=#FFFFFF · h=44 · pad=10 16 · r=8
```

**Field / Input**
```
Default    fill=#FFFFFF · border=#CBD5E1 · placeholder=#94A3B8 · h=44 · pad=12 16 · r=8
Focus      fill=#FFFFFF · border=#071640 · text=#0F172A         · h=44
Error      fill=#FFFFFF · border=#DC2626 · text=#0F172A         · h=44
Disabled   fill=#F1F5F9 · border=#E2E8F0 · text=#94A3B8         · h=44
```

**Table**
```
Container     bg=#FFFFFF · border=#E2E8F0 · r=12
Header row    h=48 · bg=#F8FAFC · border-bottom #E2E8F0 · text=12px/600/UPPERCASE/#64748B
Data row odd  h=64 · bg=#FFFFFF · border-bottom #E2E8F0
Data row even h=64 · bg=#F8FAFC · border-bottom #E2E8F0
Row hover     bg=#F1F5F9
Row selected  bg=#EFF6FF · border-left 3px #071640
Pagination    h=56 · bg=#FFFFFF · border-top #E2E8F0
```

**Status badge (text-only)**
```
Active/Đang chạy  text=#059669 · bg=#DCFCE7 · r=4 · pad=2 8
Stopped/Dừng      text=#DC2626 · bg=#FEE2E2 · r=4 · pad=2 8
Pending/Mới       text=#D97706 · bg=#FEF9C3 · r=4 · pad=2 8
Draft             text=#475569 · bg=#F1F5F9 · r=4 · pad=2 8
```

**Dialog / Modal**
```
Overlay     fill=#0F172A · opacity=50
Container   bg=#FFFFFF · r=16 · shadow "0 10px 28px rgba(15,23,42,.10)"
Header      h=64 · border-bottom #E2E8F0 · pad=0 24
Body        pad=24 · gap=16 · direction VERTICAL
Footer      h=64 · border-top #E2E8F0 · pad=0 24 · justify=flex-end
```

**Sort Control / Filter Chip** (dùng cho dashboard toolbar: sắp xếp, tổng count)
```
width      = fill-container (layoutGrow=1) — TUYỆT ĐỐI không hard-code px
height     = 48px
padding    = 8px 16px (không phải 24px — 24px quá chật)
gap        = 8px (giữa icon và label text)
border     = 1px #E2E8F0
radius     = 8px
bg         = #FFFFFF
layout     = HORIZONTAL · justify-content=space-between · align-items=center
Left side  = icon (Material Icons 16px #64748B) + label text (Inter 14/400 #475569)
Right side = count badge hoặc meta text (Inter 12/600 #0F172A)
```
⚠️ Lỗi phổ biến: tạo Sort Control với `width=261` (fixed) → chip không fill theo container.
Luôn dùng `layoutGrow=1` hoặc `resize_nodes` theo đúng container width sau khi reparent.

---

#### 6.7 — Design tokens áp dụng khi vẽ

**HARD RULES — border bắt buộc (set_strokes):**
Đây là lý do phổ biến nhất khiến output trông "thiếu". Không được bỏ qua:

| Element | Stroke | Weight |
|---|---|---|
| Card, panel | `#E2E8F0` | 1px |
| Input field | `#CBD5E1` | 1px |
| Filter chip / tag | `#E2E8F0` | 1px |
| Button ghost | `#E2E8F0` | 1px |
| Table container | `#E2E8F0` | 1px |
| Table header/row separator | `#F1F5F9` | 1px (bottom only) |
| Dialog | `#E2E8F0` | 1px |
| Topbar | `#E2E8F0` | 1px (bottom only) |

**Colors** (từ design-tokens.md):
- Frame bg: `#F1F5F9` (app), `#FFFFFF` (card/topbar), `#162040` (sidebar)
- Primary button: fill `#071640`, text `#FFFFFF`
- Ghost button: fill `#FFFFFF`, stroke `#E2E8F0`, text `#475569`
- Input default: fill `#FFFFFF`, stroke `#CBD5E1`
- Input focus: stroke `#071640` + inset shadow
- Input error: stroke `#DC2626`
- Status text only: green `#059669` / amber `#D97706` / red `#DC2626` / blue `#2563EB`

**Typography** (Body/sm = default):
- Default body text: Inter 14px / 400 / lh 1.6
- Page title: Inter 20px / 600
- Section header: Inter 16px / 600
- Table header: Inter 12px / 600 / UPPERCASE
- Caption/meta: Inter 12px / 400
- Numbers/IDs: IBM Plex Mono 12–14px

**Spacing** (4px grid):
- Card padding: 24px (comfortable) / 16px (compact)
- Page horizontal padding: 24px
- Field gap: 16px
- Filter field gap: 16px
- Topbar height: 64px
- Sidebar width: 280px

**Border Radius**:
- Tags, badges: 4px
- Buttons, inputs: 8px
- Cards: 12px
- Dialogs: 16px
- Pills, toggles: 100px

**Shadows**:
- Card resting: `0 1px 3px rgba(15,23,42,.06), 0 1px 2px rgba(15,23,42,.04)`
- Card hover: `0 4px 12px rgba(15,23,42,.08)`
- Dialog: `0 10px 28px rgba(15,23,42,.10)`

#### 6.8 — Vietnamese content rules
- Currency: `482,500,000 ₫` (comma thousands, space trước ₫)
- Date: `DD/MM/YYYY` (e.g. `12/04/2026`)
- Labels: UPPERCASE (e.g. `TÊN CỬA HÀNG`, `TRẠNG THÁI`)
- Button text: Sentence case (e.g. `Thêm mới`, `Xuất dữ liệu`)
- Tab text: plain (e.g. `Tất cả 128`, `Chờ duyệt`)

### Step 7 — Screenshot + structured verify

Sau khi vẽ xong tất cả story groups:

**1. Save screenshot gốc — 1 ảnh / 1 story group:**
```
save_screenshots
  nodeId:     [Section frame ID của story group]
  outputPath: artifacts/[feature-folder]/screenshots/[story-slug].png
  scale:      1
```

**2. Đọc ảnh → verify từng row trong bảng Step 2** — so sánh Expected vs Actual:

| Component | Property | Expected (Step 2) | Actual | Pass / Fail |
|---|---|---|---|---|
| Button (Primary) | height | 44px | ? | ✓ / ✗ |
| Button (Primary) | radius | 8px | ? | ✓ / ✗ |
| Button (Primary) | fill | `#071640` | ? | ✓ / ✗ |
| Field | height | 44px | ? | ✓ / ✗ |
| Field | border | `#cbd5e1` | ? | ✓ / ✗ |
| _(thêm row cho mỗi property trong Step 2)_ | | | | |

- Mọi ✗ → kèm fix instruction cụ thể (tool call nào, set value nào cần chạy để sửa)
- Những gì cần sửa thủ công trong Figma (icon, shadow, clip content) → note riêng

**3. Fix tất cả ✗.**

**4. Sau khi fix xong — CHỈ KHI tất cả rows = ✓ — save verify screenshot:**
```
save_screenshots
  nodeId:     [Section frame ID của story group]
  outputPath: artifacts/[feature-folder]/screenshots/verify-[story-slug].png
  scale:      1
```

**5. Xóa ảnh gốc** `[story-slug].png` (có lỗi). Output cuối chỉ giữ `verify-[story-slug].png` — prefix `verify-` = đã qua QC.

> ⚠️ Nếu còn ✗ chưa fix → KHÔNG xóa ảnh gốc → fix tiếp → lặp lại từ bước 3.

**6. Chạy QC checklist** sau khi đã xem verify screenshot.

---

## Quality Control

| Check | Criteria |
|---|---|
| **Sidebar present** | Mọi màn hình chính ĐỀU có sidebar 280px navy — không có sidebar = fail |
| **Tab bar consistent** | Nếu PRD define tabs: tất cả frames ĐỀU có tab bar, đúng labels, đúng active tab — thiếu hoặc sai tab = fail |
| **Borders present** | Cards, inputs, filter chips, tables, dialogs ĐỀU có stroke 1px — không có border = fail |
| **Cards Container** | Cards/charts/tables KHÔNG được đặt trực tiếp vào Content Body — phải có `Cards Container` wrapper với paddingL:24, paddingR:24 |
| Screen coverage | Every screen in Mermaid diagram có frame trong Figma |
| Story grouping | Screens grouped theo user story, mỗi group là 1 Section frame |
| Component mapping | Every field in spec maps to a Brand Portal component |
| States complete | List: Default + Empty. Form/Detail: Default only. Dialog: popup default only. KHÔNG cần error/loading/disabled state. |
| Brand tokens | Primary #071640, sidebar #162040, Body/sm 14px Inter, IBM Plex Mono cho số |
| VN formatting | Số tiền `482,500,000 ₫`, ngày `DD/MM/YYYY` trong tất cả mock data |
| Layout | Mỗi frame đúng dimensions, auto layout, spacing theo 4px grid |

## HITL Checklist

Sau khi vẽ xong trên Figma, BA validate:
- [ ] Tất cả màn hình trong spec flow đều có frame trong Figma?
- [ ] Tab bar (nếu có) nhất quán qua tất cả frames? Active tab đúng với từng màn?
- [ ] Component names đúng với Brand Portal design system?
- [ ] List screens có Default + Empty state? Form/Detail có Default? Dialog có popup?
- [ ] Brand tokens (màu, font, spacing) áp dụng đúng?
- [ ] VN formatting (số tiền, ngày tháng) đúng?
- [ ] Component nào cần build từ đầu đã được flagged rõ?