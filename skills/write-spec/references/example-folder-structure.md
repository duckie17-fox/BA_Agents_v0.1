# Example Folder Structure — Display Rules Feature

> Real-world example showing how specs are organized with versioning & CRs.
> 
> **Note:** This example uses `artifacts/` as the base folder. Your project may use `artifacts/`, `docs/`, `specs/`, or another folder. The folder structure is the same — only the base path changes.

---

## Scenario

Feature: **VPR-001 — Display Rules**
- v1.0: Initial release (2026-05-01)
- v1.1: Add planogram support (CR-VPR-001-001, approved 2026-06-15)
- v1.2: Fix store scope validation (CR-VPR-001-002, approved 2026-07-20)

---

## Folder Structure

```
artifacts/
└── VPR-001--display-rules/
    ├── _versions.md               ← Version history
    ├── _meta.json                 ← Current pointers
    ├── v1.0/                      ← Initial release
    │   ├── 01-intake--qualified.md
    │   ├── 02-discovery--problem-statement.md
    │   ├── 03-planning--prd.md
    │   ├── 04-spec--display-rules.md
    │   ├── 05-design--instruction.md
    │   ├── diagrams/
    │   │   ├── workflow-create-display-rule.html
    │   │   └── workflow-apply-rule.html
    │   └── figma-code/
    │       ├── display-rule-screen-v1.json
    │       └── components.json
    │
    ├── v1.1/                      ← Patch CR (planogram mode)
    │   ├── 02-discovery--cr-problem-statement.md
    │   ├── 03-planning--cr-prd.md
    │   ├── 04-spec--display-rules.md  (UPDATED: added planogram mode)
    │   ├── 05-design--instruction.md  (UPDATED if design changed)
    │   ├── _cr-info.json
    │   ├── diagrams/
    │   │   ├── workflow-create-display-rule.html  (updated)
    │   │   └── workflow-apply-rule.html
    │   └── figma-code/
    │       ├── display-rule-screen-v1.1.json
    │       └── components.json
    │
    └── v1.2/                      ← Patch CR (store scope fix)
        ├── 02-discovery--cr-problem-statement.md
        ├── 03-planning--cr-prd.md
        ├── 04-spec--display-rules.md  (UPDATED: fixed store scope)
        ├── _cr-info.json
        ├── diagrams/
        │   ├── workflow-create-display-rule.html  (unchanged)
        │   └── workflow-apply-rule.html
        └── figma-code/
            └── display-rule-screen-v1.2.json
```

---

## File Contents — Key Files

### Root: _versions.md

```markdown
# VPR-001--Display-Rules — Version History

| Version | Type | Release Date | CR ID | Title | Status | Links |
|---------|------|--------------|-------|-------|--------|-------|
| v1.0 | Initial Release | 2026-05-01 | — | Display Rules Management | Approved | [04-spec](v1.0/04-spec--display-rules.md) |
| v1.1 | Patch CR | 2026-06-15 | CR-VPR-001-001 | Add planogram mode support | Approved | [04-spec](v1.1/04-spec--display-rules.md) \| [CR Info](v1.1/_cr-info.json) |
| v1.2 | Patch CR | 2026-07-20 | CR-VPR-001-002 | Fix store scope validation bug | Approved | [04-spec](v1.2/04-spec--display-rules.md) \| [CR Info](v1.2/_cr-info.json) |
```

### Root: _meta.json

```json
{
  "feature-id": "VPR-001",
  "feature-slug": "display-rules",
  "feature-name": "Display Rules",
  "latest-version": "v1.2",
  "current-version-links": {
    "spec": "v1.2/04-spec--display-rules.md",
    "prd": "v1.2/03-planning--prd.md",
    "design": "v1.2/05-design--instruction.md",
    "diagrams": "v1.2/diagrams/",
    "figma-code": "v1.2/figma-code/"
  },
  "all-versions": ["v1.0", "v1.1", "v1.2"],
  "status": "active",
  "last-updated": "2026-07-20"
}
```

### v1.1: _cr-info.json

```json
{
  "cr-id": "CR-VPR-001-001",
  "version": "v1.1",
  "base-version": "v1.0",
  "type": "patch",
  "status": "Approved",
  "created-date": "2026-06-01",
  "approved-date": "2026-06-15",
  "deployed-date": "2026-06-20",
  
  "title": "Add planogram mode support to Display Rules",
  
  "problem-statement": "Current Display Rules only support shelf-level configuration. Brands need to define rules based on planogram (photo-based layout templates) to improve display accuracy in photo-based evaluation workflows.",
  
  "affected-modules": [
    "Display Rule (main)",
    "AI Evaluation Plan (uses Display Rule levels)",
    "Post-Audit Plan (references Display Rule)"
  ],
  
  "breaking-changes": false,
  
  "changes-summary": [
    "Added new config mode: Planogram (alongside existing Shelf-Level)",
    "Extended Display Rule form with planogram image upload field",
    "Added planogram reference layer in Display Rule editor",
    "Updated validation rules to handle planogram constraints (e.g., product positions vs. shelf levels)"
  ],
  
  "technical-details": {
    "new-fields": [
      {
        "name": "config_mode",
        "type": "enum",
        "values": ["SHELF_LEVEL", "PLANOGRAM"],
        "default": "SHELF_LEVEL"
      },
      {
        "name": "planogram_image_url",
        "type": "string",
        "required-if": "config_mode = PLANOGRAM"
      }
    ],
    "schema-changes": [
      "Added display_rule.config_mode column",
      "Added display_rule_planogram table"
    ]
  },
  
  "related-crs": [],
  
  "approved-by": "Product Manager: Nguyen Van A",
  "implemented-by": "Engineering: Feature Team 2",
  
  "notes": "Deployed with feature flag DISPLAY_RULE_PLANOGRAM_MODE initially disabled. Rolled out to 10% of users on 2026-06-20, 100% by 2026-06-22."
}
```

### v1.1: 02-discovery--cr-problem-statement.md (excerpt)

```markdown
# CR-VPR-001-001 Discovery — Problem Statement

## Problem

Brands increasingly use planogram (photo-based layout templates) to ensure product displays match exact photo references. Current Display Rules only support shelf-level configuration (which tiers, how many facings, which products). This misses the detailed compliance validation that planogram-based evaluation provides.

## Impact

- High-compliance brands (e.g., premium beverages) can't enforce exact display positions
- AI evaluation can't verify compliance against planogram references
- Manual evaluators spending time checking against external planogram images

## Proposed Solution

Extend Display Rules with a new "Planogram" config mode that allows:
1. Upload reference planogram image
2. Define product positions on planogram (instead of/alongside shelf levels)
3. AI evaluation matches against planogram reference

## Scope

- Add planogram config mode to Display Rule
- Extend AI Evaluation Plan to use planogram-based rules
- Keep shelf-level mode as default for backward compatibility
```

### v1.2: _cr-info.json (excerpt)

```json
{
  "cr-id": "CR-VPR-001-002",
  "version": "v1.2",
  "base-version": "v1.1",
  "type": "patch",
  "status": "Approved",
  
  "title": "Fix store scope validation in Display Rules",
  
  "problem-statement": "Bug: When applying Display Rule to store group scope, validation incorrectly fails if any store in the group has conflicting rules. Should only fail if applying to same store, not group.",
  
  "breaking-changes": false,
  
  "changes-summary": [
    "Fixed store scope validation logic: check scope level, not individual stores",
    "Updated validation error messages for clarity",
    "Added test cases for store group scope validation"
  ],
  
  "approved-by": "QA Lead: Tran Thi B"
}
```

---

## How Files Flow

### Creating v1.0 (Initial Release)

1. **write-spec skill** generates:
   ```
   v1.0/04-spec--display-rules.md
   v1.0/diagrams/workflow-*.html
   v1.0/figma-code/display-rule-screen-v1.json
   ```

2. **design-instruction skill** (if called) generates:
   ```
   v1.0/05-design--instruction.md
   ```

3. **Root files created manually:**
   ```
   _versions.md (with v1.0 row)
   _meta.json (latest-version: v1.0)
   ```

### Creating v1.1 (First CR)

1. **CR comes in:** CR-VPR-001-001
2. **Copy v1.0 → v1.1:**
   ```
   cp -r v1.0/* v1.1/
   ```

3. **write-spec updates v1.1:**
   ```
   v1.1/02-discovery--cr-problem-statement.md (NEW)
   v1.1/03-planning--cr-prd.md (NEW)
   v1.1/04-spec--display-rules.md (UPDATED with planogram mode)
   v1.1/_cr-info.json (NEW)
   ```

4. **Root files updated:**
   ```
   _versions.md (add v1.1 row)
   _meta.json (update latest-version to v1.1)
   ```

### Creating v1.2 (Second CR)

1. **CR comes in:** CR-VPR-001-002
2. **Copy v1.1 → v1.2** (not v1.0, to preserve v1.1 changes):
   ```
   cp -r v1.1/* v1.2/
   ```

3. **write-spec updates v1.2:**
   ```
   v1.2/02-discovery--cr-problem-statement.md (NEW — different from v1.1)
   v1.2/03-planning--cr-prd.md (NEW — different from v1.1)
   v1.2/04-spec--display-rules.md (UPDATED with store scope fix)
   v1.2/_cr-info.json (NEW — different from v1.1)
   ```

4. **Root files updated:**
   ```
   _versions.md (add v1.2 row)
   _meta.json (update latest-version to v1.2)
   ```

---

## Key Observations

1. **v1.0 never changes** — it's the baseline
2. **Each CR gets its own folder** — v1.1, v1.2, etc.
3. **Root metadata points to latest** — `_meta.json` always shows current version
4. **CR files are prefixed** — `02-discovery--cr-*`, `03-planning--cr-*`
5. **Spec is always complete** — `04-spec` in v1.2 includes all changes from v1.0 + v1.1 + v1.2
6. **Diagrams may be shared or versioned** — depends on if they changed in the CR

---

## Multiple Features in One PRD

If Display Rules had two sub-features (e.g., "Display Rule Config" + "Display Rule Application"):

```
VPR-001--display-rules/
└── v1.0/
    ├── 01-intake--qualified.md
    ├── 02-discovery--problem-statement.md
    ├── 03-planning--prd.md
    ├── 04-spec--display-rules-config.md      ← Feature 1
    ├── 04-spec--display-rules-application.md ← Feature 2
    ├── 05-design--display-rules-config.md
    ├── 05-design--display-rules-application.md
    └── diagrams/
```

Each 04-spec and 05-design file is independent, written by separate `write-spec` and `design-instruction` calls.

