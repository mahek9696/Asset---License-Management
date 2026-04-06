---
name: sprint1-qa-test-template
description: "Use when creating or reviewing Sprint 1 test scenarios and test cases for the Asset & License Management System using the project scope document as the source of truth. Keywords: test scenario, test case template, sprint 1 QA, role-based access, IT Admin, Employee, scope coverage, traceability matrix."
---

# Sprint 1 QA Test Scenario and Test Case Template

## Purpose
Use this skill to produce consistent, review-ready Sprint 1 QA artifacts for the Asset & License Management System.

This skill is mandatory when preparing:
- Test scenarios
- Detailed test cases
- Requirement coverage matrix
- Scope-based validation
- API-aligned checks only where relevant

## Sprint 1 Scope Sources
Always use these workspace files as the source of truth, in this order of priority:
- Project scope document / user story scope content
- `QA_Review_Sprint1.md`
- `QA_Checklist_QuickReference.md`
- `Test_Cases_Detailed.md`
- `AssetManagementSwaggerV1.yaml`
- Scope docs and user story docs available in workspace (PDF/DOCX)

If scope docs conflict with QA markdown or Swagger, record the conflict under `Open Questions / Ambiguities` and continue with the safest scope-based assumption.

## Scope Definition Rule
Define scope from the project scope document, not from the API contract.

Use the project scope areas as the top-level organizing structure when writing scenarios and test cases:
- User & Role Management
- Asset Management
- License Management
- Request Management
- Inventory Tracking
- Notifications & Alerts
- Reporting & Export
- Audit & Activity Logs
- Dashboard

## Role Scope Rule
This sprint is role-based for:
- IT Admin: the primary operational user of the system. This role should be described as the person who logs in, creates and updates assets and licenses, assigns and returns assets, approves or rejects requests, changes statuses, reviews inventory, generates reports, exports data, and monitors audit or dashboard information. Use this role for scenarios that require full control, administrative permissions, and system-wide visibility.
- Employee: the restricted self-service user of the system. This role should be described as the person who logs in to view only permitted information, raise requests, check assigned assets or licenses, receive notifications, and perform only actions explicitly allowed by policy. Use this role for scenarios that focus on limited access, request submission, visibility of assigned items, and permission-based behavior.

Exclude Auditor from the test scope unless the user explicitly requests a separate auditor-focused set.

Swagger is only used to validate request/response behavior for scenarios that already belong to scope.

## User Stories In Scope
Create tests for the project scope areas above only.

Do not add out-of-scope items such as:
- Mobile Application
- Barcode / QR Code Integration
- Procurement & Vendor Management
- Purchase of assets
- Vendor onboarding or management
- Purchase order (PO) management
- Auditor-specific flows, unless explicitly requested

## Mandatory Rules
1. Every scope area under test must include Positive, Negative, Boundary, and Error Handling scenarios where applicable.
2. Every API-dependent scenario must map to relevant Swagger endpoint(s) and expected status code(s).
3. Every test case must include unique Test ID, preconditions, test data, steps, and expected results.
4. Include explicit validation for known Sprint 1 gaps (critical/high issues from QA review/checklist).
5. Flag ambiguity instead of inventing business rules.
6. Use deterministic language such as "must", "should not", and "expected".
7. Keep one assertion focus per test case where possible.
8. Add traceability: each test case must map to Story ID / AC ID or the closest scope requirement label.
9. Final deliverable must include execution columns for Actual Response, Status, Date, and Comments.

## Contract-Validation Rules (Swagger)
For relevant tests only, align with `AssetManagementSwaggerV1.yaml` after the scope has been defined from the project document.

Use Swagger only for features already in scope, such as:
- Auth: `POST /auth/login`
- Asset create: `POST /api/admin/createasset` (201, 400)
- Asset details: `GET /api/admin/getasset/{id}` (200, 404)
- Asset update: `PUT /api/admin/updateasset/{id}` (200, 400, 404)
- Assign asset: `POST /assets/{id}/assign` (200)
- Return asset: `POST /api/admin/returnasset/{id}` (200, 400, 404)
- Report/list: `GET /api/admin/reportlist` (200)

Always validate:
- Required fields and data types
- Invalid payload behavior (400)
- Missing resource behavior (404)
- Auth guard behavior (401/403 if implemented)
- Response envelope shape: `data`, `error`, `statusCode`

Do not use Swagger paths to decide what belongs in scope. Use Swagger only to check how a scoped feature is exercised through the API.

## Coverage Expectations
For each in-scope area, include at minimum:
- 3 Positive scenarios
- 4 Negative scenarios
- 3 Boundary scenarios
- 2 Error/Recovery scenarios

If project constraints prevent this, explicitly state what is missing and why.

## Prompt Response Format

When responding, follow this order:

### 1) Scope Coverage Summary
Begin with the project scope document areas, not the API contract:
- User & Role Management: authentication, login, and role-based access control for IT Admin and Employee.
- Asset Management: asset creation, update, categorization, assignment, return, lifecycle history, and status tracking.
- License Management: license creation, assignment, renewal, expiry tracking, lifecycle history, and reminder handling.
- Request Management: employee request submission, IT Admin review, approval, rejection, and workflow transitions.
- Inventory Tracking: real-time available vs assigned tracking, category-wise visibility, and unused asset identification.
- Notifications & Alerts: expiry reminders, pending return notifications, reminder settings, and email notifications.
- Reporting & Export: asset, license, inventory, and audit reports plus Excel export validation.
- Audit & Activity Logs: action tracking, read-only audit access where applicable, and monthly summaries.
- Dashboard: role-based summary views for IT Admin and Employee.

For each area, state what is in scope and note any open risks, gaps, or ambiguities.

### 2) Test Scenario and Case Table
Use one consolidated table with these exact columns and sequence:

| MODULE | SCENARIO NO. | SCENARIO TITLE | TEST CASE NO. | TEST CASE TITLE | API CALLS | METHODS | VALUES FOR URL/BODY | INPUTS | EXPECTED RESPONSE | ACTUAL RESPONSE | STATUS | DATE | COMMENTS |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|

Column usage rules:
- MODULE: Story module name from scope.
- SCENARIO NO.: Use the naming pattern `SC-1.1`, `SC-2.1`, `SC-3.1`, `SC-4.1`.
- SCENARIO TITLE: Short scenario-level title.
- TEST CASE NO.: Use the naming pattern `TC-1.1`, `TC-1.2`, etc.
- TEST CASE TITLE: Action-oriented test title.
- API CALLS: Relevant API path only when the scenario uses an API; otherwise `N/A`.
- METHODS: HTTP method (`GET`, `POST`, `PUT`) or `N/A`.
- VALUES FOR URL/BODY: Path, query, or body values used in the test.
- INPUTS: UI input values, payload, preconditions, and test data.
- EXPECTED RESPONSE: Expected UI/API behavior, including status code when applicable.
- ACTUAL RESPONSE: Execution-time observed response.
- STATUS: `Pass`, `Fail`, `Blocked`, or `Not Executed`.
- DATE: Execution date in `YYYY-MM-DD`.
- COMMENTS: Defects, blockers, assumptions, or clarifications.

Relationship rule:
- One Scenario can have many Test Cases.
- Repeat the same SCENARIO NO. and SCENARIO TITLE across all rows that belong to that scenario.
- Each TEST CASE NO. must be unique within the scenario and should carry one clear validation point.
- Use the scenario row group to represent the test design, and the test case rows to represent the executable checks.

### 3) Open Questions / Ambiguities
List unresolved points, especially known Sprint 1 critical gaps:
- Asset ID generation format undefined
- Asset List pagination inconsistency (15 vs 20)
- Filter logic (AND/OR) ambiguity
- Asset Details edit mode ambiguity
- Status transition rule gaps
- Serial number uniqueness rule ambiguity
- Category duplicate case-sensitivity ambiguity
- Any Auditor-related requirement if it appears in source docs but is excluded from this sprint

## Quality Gates (Before Final Output)
Do not finalize until all checks pass:
- All in-scope areas present
- Scenario types balanced (positive/negative/boundary/error)
- Swagger mapping included where relevant
- Critical Sprint 1 gaps represented in tests
- IDs are unique and sequential
- No placeholder text left
- All required template columns are present exactly once
- Auditor flows are excluded unless explicitly requested

## Naming Convention
- Scenario IDs: `SC-1.1`, `SC-1.2`, ..., `SC-n.x`
- Test IDs: `TC-1.1`, `TC-1.2`, ..., `TC-n.x`

## Review Mode
When asked to review existing test cases, return:
1. Missing coverage
2. Duplicate/redundant tests
3. AC mismatches
4. Swagger mismatch risks
5. Prioritized fixes (Critical, High, Medium, Low)
