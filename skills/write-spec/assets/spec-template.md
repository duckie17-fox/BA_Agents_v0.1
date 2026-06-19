---
artifact: Spec
ticket-id: [Jira ID hoặc temp ID]
source-skill: write-spec
prepared-by: [Tên BA]
date: [YYYY-MM-DD]
status: Draft
handoff-to: design-instruction
version: v1.0
cr-id: null
---

# Spec [N]. [Feature Name]

## Lịch sử thay đổi

| Version | Create by | Content before adjustment | Content after adjustment |
| --- | --- | --- | --- |
| V1.0 | [BA Name] | Tạo mới tài liệu | |

## Mô tả tài liệu

**A. Link Jira:** TBD

**B. User Story:** Là một [role], tôi muốn [action] để [goal]

**C. Workflow:**

```mermaid
flowchart TD
    Start([Bắt đầu]) --> Step1[Bước 1 — User action]
    Step1 --> Step2[Bước 2 — System response]
    Step2 --> Valid{Điều kiện?}
    Valid -->|Có| Happy[Happy path action]
    Happy --> End([End])
    Valid -->|Không| Error[Error handling]
    Error --> Fix[User sửa lại]
    Fix --> Step2
```

**D. Design:** TBD — link to Figma / design-instruction file

**E. Business Rules:**

| # | ID | Description |
|---|----------|-----------------|
| 1 | BR-01 | [Description] |


**F. Functional requirement:**
| # | ID | Description |
|---|----------|-----------------|
| 1 | FR-01 | [Description] |
| 2 | FR-02 | [Description] |

**G. Non-Functional requirement:**
| # | Scenario | Input/Điều kiện |
|---|----------|-----------------|
| 1 | NFR-01 | [Description] |
| 2 | NFR-02 | [Description] |

**H. Edge Cases:**

| # | Scenario | Input/Điều kiện | Expected behavior |
|---|----------|-----------------|-------------------|
| 1 | EC-01 | [Description] | [Description] |
| 2 | EC-02 | [Description] | [Description] |

**I. Bảng mô tả trường:**

***[Section name]***

|STT| Trường thông tin | Loại control | Mô tả ý nghĩa |Required|Max length| Ràng buộc |Thông báo lỗi| Ví dụ |Design instruction|Nguồn dữ liệu|
| --- | --- | --- | --- | --- |---|---|---|---|---|---|
|1| [Field] | [Loại] | [Description] | Yes/No| [Số] | [Constraints] | [Error message] |[Ví dụ]|[Mô tả thông tin về UI/Behavior/Design ]|[Nguồn dữ liệu]|

---

**J. Use cases / User stories liên quan**
- Xóa mục này nếu không có

| Ticket | Tên tính năng / User Story | Quan hệ | Ghi chú |
|--------|---------------------------|---------|---------|
| [ID] | [Tên] | Ảnh hưởng đến / Bị ảnh hưởng bởi / Liên kết | [Mô tả ngắn về điểm ảnh hưởng] |

---

**K. Open Questions**
- Xóa mục này nếu không có

| # | Câu hỏi | Owner | Blocking? | Deadline |
|---|---------|-------|-----------|---------|
| 1 | | | | |

---

## HITL Checklist

- [ ] User Story đúng intent không?
- [ ] Workflow (C) đúng flow không? Có error path covering không?
- [ ] Business Rules (E) đủ không?
- [ ] Functional Requirements (F) đã cover đủ capability?
- [ ] Non-Functional Requirements (G) có con số phù hợp không?
- [ ] Edge Cases (H) đã cover đủ?
- [ ] Field table (I) đủ tất cả trường + validation rules?
- [ ] Use cases liên quan (J) có đúng + đủ không?

---

## L. Figma Design Code

```json
{
  "id": "[Node ID]",
  "name": "[Component Name]",
  "type": "[FRAME/COMPONENT]",
  "styles": {
    "fills": ["[colors]"],
    "fontFamily": "[font]",
    "fontSize": [size]
  },
  "children": [
    {
      "id": "[Child Node ID]",
      "name": "[Child Name]",
      "type": "[TEXT/FRAME]",
      "characters": "[content]"
    }
  ]
}
```

*Ghi chú: Section này chứa JSON code từ Figma MCP, dùng cho reference hoặc handoff sang design/engineering. Xóa nếu không cần.*
