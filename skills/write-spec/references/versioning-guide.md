# Versioning & CR Workflow Guide

> Hướng dẫn quản lý spec và change request trong write-spec skill.

---

## Nguyên tắc cốt lõi

- `04-spec--[feature].md` là **single source of truth** — luôn phản ánh đúng production state
- `03-planning--prd.md` cũng là file duy nhất — update khi CR thay đổi scope epic
- **Không có version subfolder** — changelog nằm trong `04-spec`
- Push lên wiki **sau khi deployed production**, không phải sau approved

---

## Folder Structure

```
artifacts/[ID]--[slug]/
├── _versions.md                       ← index tất cả version + CR (tạo ngay khi có ticket)
├── _meta.json                         ← pointer đến spec hiện tại (update sau mỗi CR deployed)
├── 01-intake--qualified.md
├── 02-discovery--problem-statement.md
├── 03-planning--prd.md
├── 04-spec--[feature].md              ← single source of truth
└── cr/
    ├── CR-001--[slug].md              ← context doc, không đổi sau deployed
    └── CR-002--[slug].md
```

**Multiple sub-features trong 1 PRD:**
```
artifacts/VPR-050--evaluation-plan/
├── _versions.md
├── _meta.json
├── 03-planning--prd.md
├── 04-spec--ai-evaluation.md
├── 04-spec--manual-evaluation.md
└── cr/
    └── CR-001--add-cycle-filter.md
```

## _versions.md — Format

Tạo khi khởi tạo folder (sau Intake Approved). Update mỗi khi có CR mới hoặc deployed.

```markdown
# Version Index — [Feature Name]

| Version | Type | CR ID | Approved | Deployed | Author | Tóm tắt |
|---------|------|-------|----------|----------|--------|---------|
| v1.0 | New Feature | — | 2026-05-01 | 2026-05-05 | An Phạm | Khởi tạo |
| v1.1 | CR | CR-001 | 2026-06-10 | 2026-06-15 | An Phạm | Thêm planogram mode |
| v1.2 | CR | CR-002 | 2026-07-15 | — | An Phạm | Fix store scope (in progress) |
```

## _meta.json — Format

```json
{
  "feature": "display-rules",
  "current_version": "v1.2",
  "current_spec": "04-spec--display-rules.md",
  "last_deployed": "v1.1",
  "open_cr": "CR-002"
}
```

`open_cr`: CR đang in-progress (chưa deployed). `null` nếu không có CR đang mở.

---

## Workflow đầy đủ

### New Feature

```
1. Viết spec → 04-spec (Draft)
2. HITL review → 04-spec (Approved)
3. Dev build + test
4. Deployed production
   → Update changelog trong 04-spec (thêm v1.0 row với Deployed date)
   → Push 04-spec lên wiki
```

### Change Request

```
1. CR vào
   → Tạo cr/CR-001--[slug].md (status: In Progress)
   → Update 04-spec: draft các section bị ảnh hưởng
   → Update 03-planning--prd.md nếu CR thay đổi scope/epic

2. HITL review → cr/CR-001.md (status: Approved)

3. Dev build + test

4. Deployed production
   → Rewrite các section bị ảnh hưởng trong 04-spec
   → Thêm row vào changelog 04-spec (có Deployed date)
   → Update cr/CR-001.md: status → Deployed, thêm deployed date
   → Update 03-planning--prd.md nếu cần
   → Push 04-spec lên wiki
```

---

## Changelog Format (trong 04-spec)

```markdown
## Lịch sử thay đổi

| Version | Approved | Deployed | Author | CR ID | Nội dung thay đổi |
|---|---|---|---|---|---|
| v1.2 | 2026-07-15 | 2026-07-20 | An Phạm | CR-002 | Fix store scope — BR-03 rewrite, Section I trường Phạm vi áp dụng |
| v1.1 | 2026-06-10 | 2026-06-15 | An Phạm | CR-001 | Thêm planogram mode — Section C workflow, BR-01 rewrite, Section I thêm 2 trường |
| v1.0 | 2026-05-01 | 2026-05-05 | An Phạm | — | Khởi tạo tài liệu |
```

**Quy tắc viết cột Nội dung thay đổi:**
- Ghi rõ section nào bị ảnh hưởng (BR-03, Section C, Section I...)
- Đủ để BA biết cần đọc lại chỗ nào khi review
- Không cần chi tiết từng trường — đó là nội dung của cr/CR-00X.md

---

## CR Context Doc (cr/CR-00X--slug.md)

```markdown
---
cr-id: CR-001
title: Thêm chế độ Planogram vào Display Rules
status: Deployed
approved: 2026-06-10
deployed: 2026-06-15
approved-by: An Phạm
---

## Vấn đề
Brand cao cấp cần enforce trưng bày theo ảnh mẫu (planogram),
không chỉ theo số tầng/số mặt.

## Root cause
Display Rules hiện chỉ hỗ trợ shelf-level — không capture được
vị trí sản phẩm theo không gian thực tế của kệ.

## Scope thay đổi
- Section C: Thêm nhánh Planogram vào workflow
- BR-01: Rewrite để support 2 chế độ
- Section I: Thêm trường Loại cấu hình + Ảnh planogram

## Ghi chú
Backward compatible — Shelf-level vẫn là default.
```

**Quy tắc:**
- Tạo ngay khi CR vào (status: In Progress)
- Không chỉnh nội dung sau khi deployed — chỉ update status và deployed date
- Đây là lịch sử cố định, không phải spec

---

## Update rules khi CR deployed

| File | Làm gì |
|---|---|
| `04-spec--[feature].md` | Rewrite sections bị ảnh hưởng + thêm changelog row |
| `03-planning--prd.md` | Update nếu CR thay đổi scope, feature list, hoặc goal ở epic level |
| `cr/CR-00X--[slug].md` | Update status → Deployed + thêm deployed date |
| Wiki | Push `04-spec` lên sau khi đã update xong |

---

## Quick Decision Tree

```
Feature mới?
└── Tạo flat folder + 04-spec → HITL → deployed → push wiki

CR vào?
├── Tạo cr/CR-00X.md (In Progress)
├── Draft 04-spec (section bị ảnh hưởng)
├── Update PRD nếu cần
└── Deployed?
    ├── Rewrite 04-spec sections
    ├── Thêm changelog row (với Deployed date)
    ├── cr/CR-00X.md → status: Deployed
    └── Push 04-spec lên wiki
```

---

## File naming convention

| Bước pipeline | File |
|---|---|
| 01 — Intake | `01-intake--qualified.md` |
| 02 — Discovery | `02-discovery--problem-statement.md` |
| 03 — PRD | `03-planning--prd.md` |
| 04 — Spec | `04-spec--[feature].md` |
| 05 — Design | `05-design--instruction.md` |
| CR context | `cr/CR-00X--[slug].md` |

**Folder ID:**
- Có Jira ticket: `VPR-001--display-rules`
- Chưa có ticket: `DRAFT-YYYYMMDD--[slug]`