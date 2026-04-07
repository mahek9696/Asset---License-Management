# Test Case Examples - Asset & License Management System
## Sprint 1 - Detailed Test Scenarios

---

## 📝 USER STORY 1: CREATE CATEGORY - TEST CASES

### Standard Execution Fields
Every test case in this document should be written with the following execution fields in order:
- API Calls
- Methods
- Header
- Pre-Script
- Post-Script
- Input
- Expected Result

Swagger mapping rule:
- Use the project Swagger contract to fill `API Calls`, `Methods`, and `Header` only when the case is API-driven.
- Use `N/A` for fields that do not apply to a UI-only case.
- `Pre-Script` should contain setup needed before execution.
- `Post-Script` should contain cleanup or verification after execution.
- `Input` should contain request payload or UI input values.
- `Expected Result` should contain the expected response or UI outcome.

### SC-1.1: Valid Category Creation
Applies to:
- TC-1.1
- TC-1.2

### TC-1.1: Create Category with Asset Type
```
Test ID: TC-1.1
Title: Create category using valid required fields with type Asset
Precondition: User is IT Admin, logged in, on Create Category screen
API Calls: POST /api/admin/createcategory
Methods: POST
Header: Authorization: Bearer <token>, Content-Type: application/json
Pre-Script: Create or confirm IT Admin session and category master data is available.
Post-Script: Verify new category appears in Category List and clean up test data if needed.
Input: categoryName="Laptop", typeId=1, statusId=1
Steps:
1. Enter Category Name: "Laptop"
2. Select Type: "Asset"
3. Click Save
Expected Result:
- Success message: "Category has been added successfully"
- User is redirected to Category List
- New category "Laptop" is visible with Type="Asset"
Data: Category Name="Laptop", Type="Asset"
Pass/Fail: [ ]
```

### TC-1.2: Create Category with License Type
```
Test ID: TC-1.2
Title: Create category using valid required fields with type License
Precondition: User is IT Admin, logged in, on Create Category screen
API Calls: POST /api/admin/createcategory
Methods: POST
Header: Authorization: Bearer <token>, Content-Type: application/json
Pre-Script: Create or confirm IT Admin session and category master data is available.
Post-Script: Verify new category appears in Category List and clean up test data if needed.
Input: categoryName="MS365 License", typeId=2, statusId=1
Steps:
1. Enter Category Name: "MS365 License"
2. Select Type: "License"
3. Click Save
Expected Result:
- Success message: "Category has been added successfully"
- User is redirected to Category List
- New category "MS365 License" is visible with Type="License"
Data: Category Name="MS365 License", Type="License"
Pass/Fail: [ ]
```

### SC-1.2: Duplicate Category Validation.
Applies to:
- TC-1.3
- TC-1.4

### TC-1.3: Duplicate Category Same Case
```
Test ID: TC-1.3
Title: Prevent duplicate category creation with same casing
Precondition: Category "Laptop" already exists
Steps:
1. Enter Category Name: "Laptop"
2. Select Type: "Asset"
3. Click Save
Expected Result:
- Error message: "Category Name already exists"
- Save is blocked
- Category count remains unchanged
Data: Existing="Laptop", New="Laptop"
Pass/Fail: [ ]
```

### TC-1.4: Duplicate Category Different Case
```
Test ID: TC-1.4
Title: Validate duplicate handling for case variation
Precondition: Category "Laptop" already exists
Steps:
1. Enter Category Name: "laptop"
2. Select Type: "Asset"
3. Click Save
Expected Result:
- Duplicate behavior follows approved business rule
- If case-insensitive, save is blocked with duplicate message
- If case-sensitive, ambiguity is logged for product clarification
Data: Existing="Laptop", New="laptop"
Pass/Fail: [ ]
```

### SC-1.3: Category Name Boundary Validation.
Applies to:
- TC-1.5
- TC-1.6
- TC-1.7
- TC-1.8

### TC-1.5: Category Name Minimum Boundary
```
Test ID: TC-1.5
Title: Accept category name with exactly minimum length
Precondition: User is on Create Category form
Steps:
1. Enter Category Name: "PC"
2. Select Type: "Asset"
3. Click Save
Expected Result:
- Category is created successfully
- No minimum length validation error appears
Data: Category Name length=2
Pass/Fail: [ ]
```

### TC-1.6: Category Name Below Minimum
```
Test ID: TC-1.6
Title: Reject category name below minimum length
Precondition: User is on Create Category form
Steps:
1. Enter Category Name: "P"
2. Select Type: "Asset"
3. Click Save
Expected Result:
- Validation error for minimum length is displayed
- Category is not created
Data: Category Name length=1
Pass/Fail: [ ]
```

### TC-1.7: Category Name Maximum Boundary
```
Test ID: TC-1.7
Title: Accept category name with exactly maximum length
Precondition: User is on Create Category form
Steps:
1. Enter Category Name of 50 characters
2. Select Type: "Asset"
3. Click Save
Expected Result:
- Category is created successfully
- No maximum length validation error appears
Data: Category Name="AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA"
Pass/Fail: [ ]
```

### TC-1.8: Category Name Above Maximum
```
Test ID: TC-1.8
Title: Reject category name above maximum length
Precondition: User is on Create Category form
Steps:
1. Enter Category Name of 51 characters
2. Select Type: "Asset"
3. Click Save
Expected Result:
- Validation error for maximum length is displayed
- Category is not created or input is restricted to 50 characters
Data: Category Name="AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA"
Pass/Fail: [ ]
```

### SC-1.4: Mandatory Field Validation.
Applies to:
- TC-1.9
- TC-1.10

### TC-1.9: Type Required Validation
```
Test ID: TC-1.9
Title: Prevent save when category type is not selected
Precondition: User is on Create Category form
Steps:
1. Enter Category Name: "Server"
2. Leave Type as default (not selected)
3. Click Save
Expected Result:
- Required validation error appears for Type
- Category is not created
Data: Category Name="Server", Type=blank
Pass/Fail: [ ]
```

### TC-1.10: Category Name Required Validation
```
Test ID: TC-1.10
Title: Prevent save when category name is empty
Precondition: User is on Create Category form
Steps:
1. Leave Category Name empty
2. Select Type: "Asset"
3. Click Save
Expected Result:
- Required validation error appears for Category Name
- Category is not created
Data: Category Name=blank, Type="Asset"
Pass/Fail: [ ]
```

### SC-1.5: Whitespace and Normalization Handling.
Applies to:
- TC-1.11
- TC-1.12

### TC-1.11: Leading and Trailing Whitespace Handling
```
Test ID: TC-1.11
Title: Validate behavior for category name with outer whitespace
Precondition: User is on Create Category form
Steps:
1. Enter Category Name: "  Laptop  "
2. Select Type: "Asset"
3. Click Save
Expected Result:
- System trims and saves as "Laptop" or rejects with explicit whitespace validation
- Actual behavior is captured for requirement confirmation
Data: Category Name="  Laptop  "
Pass/Fail: [ ]
```

### TC-1.12: Whitespace-Only Name Validation
```
Test ID: TC-1.12
Title: Reject category name containing only whitespace
Precondition: User is on Create Category form
Steps:
1. Enter Category Name as spaces only
2. Select Type: "Asset"
3. Click Save
Expected Result:
- Required/invalid input validation appears
- Category is not created
Data: Category Name="   "
Pass/Fail: [ ]
```

### SC-1.6: Optional Property Configuration.
Applies to:
- TC-1.13
- TC-1.14

### TC-1.13: Save Category with Optional Property
```
Test ID: TC-1.13
Title: Create category with one valid optional property
Precondition: User is on Create Category form
Steps:
1. Enter Category Name: "Laptop"
2. Select Type: "Asset"
3. Add Property: Name="RAM", DataType="Integer", Mandatory=checked
4. Click Save
Expected Result:
- Category is created successfully
- Property metadata is saved and visible in category details
Data: Property Name="RAM", DataType="Integer", Mandatory=true
Pass/Fail: [ ]
```

### TC-1.14: Duplicate Property Name Validation
```
Test ID: TC-1.14
Title: Reject duplicate property names within same category
Precondition: User is on Create Category form
Steps:
1. Enter Category Name: "Laptop"
2. Select Type: "Asset"
3. Add Property 1: Name="RAM", DataType="Integer"
4. Add Property 2: Name="RAM", DataType="Boolean"
5. Click Save
Expected Result:
- Duplicate property validation message is shown
- Save is blocked until duplicate is resolved
Data: Property1="RAM", Property2="RAM"
Pass/Fail: [ ]
```

### SC-1.7: Cancel and Save Failure Recovery.
Applies to:
- TC-1.15
- TC-1.16

### TC-1.15: Cancel Form Data Discard
```
Test ID: TC-1.15
Title: Discard entered data when cancel is clicked
Precondition: User is on Create Category form with entered values
Steps:
1. Enter valid Category Name and Type
2. Add at least one property
3. Click Cancel
Expected Result:
- User returns to Category List
- No new category is created
- Draft data is not persisted
Data: Category Name="Monitor", Type="Asset", Property="Size"
Pass/Fail: [ ]
```

### TC-1.16: Save Failure Recovery
```
Test ID: TC-1.16
Title: Show safe error handling on backend/network save failure
Precondition: User has valid form data and backend/network failure can be simulated
Steps:
1. Enter valid Category Name and Type
2. Click Save while simulating server/network failure
3. Retry after failure notification
Expected Result:
- Error notification is displayed
- No duplicate category is created
- User can retry save safely once service is restored
Data: Category Name="Tablet", Type="Asset", Failure=HTTP 500/timeout
Pass/Fail: [ ]
```

---

## 📝 USER STORY 2: ASSET LIST - TEST CASES

### SC-2.1: Asset List Display and Search.
Applies to:
- TC-2.1
- TC-2.2
- TC-2.3

### TC-2.1: Display All Assets in Table
```
Test ID: TC-2.1
Title: Asset List displays all assets in tabular format
Precondition: System has 30 assets in database, user is IT Admin
Steps:
1. Navigate to Asset List page
2. Observe table display
Expected Result:
- Table shows columns: Asset Name, Category, Asset ID, Serial No, Status
- First 15 records displayed (per acceptance criteria)
- Pagination shows: "Page 1 of 2" (30 total ÷ 15 per page)
- Each row has Edit, Assign, Delete action buttons
Data: 30 test assets pre-populated
Pass/Fail: [ ]
```

### TC-2.2: Search by Asset Name
```
Test ID: TC-2.2
Title: Search assets by name (triggers on typing)
Precondition: Asset List loaded with 30 assets including "Laptop-001", "Laptop-002", "Desktop-001"
Steps:
1. Enter search term: "Laptop" in Search field
2. Wait for auto-search trigger (as you type)
Expected Result:
- Table filters to show only assets with "Laptop" in name
- Shows 2 records: "Laptop-001", "Laptop-002"
- Pagination updates: "Page 1 of 1"
- Search field retains value
Data: Search: "Laptop"
Pass/Fail: [ ]
```

### TC-2.3: Search with Special Characters
```
Test ID: TC-2.3
Title: Search handles special characters in serial numbers
Precondition: Assets exist with serial: "ABC-123/XYZ", "DEF_456-789"
Steps:
1. Search for: "ABC-123/XYZ"
2. System searches and displays result
Expected Result:
- Asset with matching serial displays
- Special chars (-,/,_) handled correctly
- No SQL injection attempts processed
Data: Serial: "ABC-123/XYZ"
Pass/Fail: [ ]
```

### SC-2.2: Asset List Filtering.
Applies to:
- TC-2.4
- TC-2.5
- TC-2.6

### TC-2.4: Filter by Category
```
Test ID: TC-2.4
Title: Filter assets by category
Precondition: Asset List has assets in multiple categories
Steps:
1. Click Category Filter dropdown
2. Select: "Laptop"
3. Table updates immediately
Expected Result:
- Only assets with Category="Laptop" displayed
- Count shows filtered result count
- Other categories hidden
- Can combine with Status filter (see TC-2.5)
Data: Category: "Laptop"
Pass/Fail: [ ]
```

### TC-2.5: Combine Category AND Status Filters
```
Test ID: TC-2.5
Title: Apply multiple filters with AND logic
Precondition: Assets with varied categories and statuses
Steps:
1. Select Category Filter: "Laptop"
2. Select Status Filter: "Assigned"
3. Observe table update
Expected Result:
- Only assets matching BOTH conditions shown
- Example: Only "Laptop" category AND "Assigned" status
- If no matches: "No records found" message
- Pagination updates accordingly
Data: Category: "Laptop" AND Status: "Assigned"
Pass/Fail: [ ]
```

### TC-2.6: Clear Filters and Search
```
Test ID: TC-2.6
Title: Reset all filters to default view
Precondition: Filters and search applied (Category="Laptop", Search="XYZ")
Steps:
1. Click "Clear Filters" button (if exists) OR
2. Delete search text and click Refresh
3. Set Category to "All"
4. Set Status to "All"
Expected Result:
- All 30 assets display again
- Pagination shows "Page 1 of 2"
- Search field empty
- Filters show defaults
Data: N/A
Pass/Fail: [ ]
```

### SC-2.3: Asset List Pagination and Deletion.
Applies to:
- TC-2.7
- TC-2.8

### TC-2.7: Pagination Navigation
```
Test ID: TC-2.7
Title: Navigate between pages in asset list
Precondition: Asset List showing 30+ assets (multiple pages)
Steps:
1. View Page 1 (shows records 1-15)
2. Click "Next" or Page 2 button
3. Current page shows 16-30
4. Click "Previous" or Page 1 button
Expected Result:
- Page 2 shows assets 16-30
- Page indicator shows "Page 2 of 2"
- Previous button enabled when on page 1
- Navigation buttons work correctly
Data: 30 total assets
Pass/Fail: [ ]
```

### TC-2.8: Delete Asset from List - Pagination Impact
```
Test ID: TC-2.8
Title: Asset removed from list on deletion
Precondition: Asset List on Page 3 of 5 (90 total assets)
Steps:
1. Delete last asset on Page 5 (leaving 89 assets)
2. Observe pagination
Expected Result:
- Asset removed from list
- Page count decreases to "Page 1 of 6" (if deletion causes page change)
- If on invalid page, redirect to last valid page
- Confirmation message shown before delete
Data: 90 assets → delete 1 → 89 remaining
Pass/Fail: [ ]
```

### SC-2.4: Asset List Export.
Applies to:
- TC-2.9
- TC-2.10

### TC-2.9: Export to Excel - Filtered Data
```
Test ID: TC-2.9
Title: Export currently filtered list to Excel
Precondition: Asset List filtered to show 5 assets
Steps:
1. Apply filter: Category="Laptop", Status="Available"
2. Click "Export to Excel" button
3. File download triggered
4. Open downloaded file
Expected Result:
- File downloads as: "assets_[date]_[timestamp].xlsx"
- Excel contains only 5 filtered assets
- Columns match table: Name, Category, ID, Serial No, Status
- File is valid Excel format (.xlsx)
Data: 5 filtered assets
Pass/Fail: [ ]
```

### TC-2.10: Export Large Dataset
```
Test ID: TC-2.10
Title: Handle export of 50,000+ assets
Precondition: Asset List with 50,000+ records
Steps:
1. Click "Export to Excel" without filters
2. System processes export request
Expected Result OPTION A:
- Export succeeds for all 50,000 records
- File size reasonable (< 50MB)

OR Expected Result OPTION B:
- Error message: "Export limited to 50K records. Please apply filters"
- User guided to export by category/date range

Current Spec: NEEDS CLARIFICATION
Data: 50,000 assets
Pass/Fail: [ ]
```

---

## 📝 USER STORY 3: ASSET DETAILS - TEST CASES

### SC-3.1: Asset Details Overview.
Applies to:
- TC-3.1
- TC-3.2

### TC-3.1: Navigate to Asset Details
```
Test ID: TC-3.1
Title: Open asset details by clicking row
Precondition: Asset List displayed with assets
Steps:
1. Click on "Laptop-001" row
2. Asset Details page loads
Expected Result:
- Asset Details page displays for Laptop-001
- URL changes to: /assets/123 (or asset ID)
- Breadcrumb shows: "Asset List > Details > [Asset Name]"
- All asset information displayed as read-only
Data: Asset ID: 123, Name: "Laptop-001"
Pass/Fail: [ ]
```

### TC-3.2: View Asset Information Section (Read-Only)
```
Test ID: TC-3.2
Title: Display asset information in read-only format
Precondition: Asset Details page opened for Laptop-001
Steps:
1. Observe Asset Information section
2. Verify all fields displayed
Expected Result:
- Fields shown as read-only (gray background or locked icon)
- Displays: Asset Name, Category, ID, Serial No, Purchase Date, Cost, 
  Vendor, Status, Description, Assigned Employee
- Date fields in MM-DD-YYYY format (e.g., "04-03-2026")
- Empty optional fields show: "Not Available"
Data: Asset: Laptop-001
Pass/Fail: [ ]
```

### SC-3.2: Assignment Visibility.
Applies to:
- TC-3.3
- TC-3.4

### TC-3.3: View Current Assignment Details
```
Test ID: TC-3.3
Title: Display current assignment information
Precondition: Asset assigned to employee "John Doe"
Steps:
1. View Current Assignment Details section
Expected Result:
- Shows: "Assigned To: John Doe"
- Shows: "Assignment Date: 12-15-2025"
- Shows: "Expected Return Date: 06-15-2026"
- Status shows: "Assigned"
Data: Assigned To: John Doe (Employee ID: 45)
Pass/Fail: [ ]
```

### TC-3.4: View Current Assignment - Unassigned Asset
```
Test ID: TC-3.4
Title: Display unassigned asset indicator
Precondition: Asset is available and not assigned
Steps:
1. View Current Assignment Details section
Expected Result:
- Shows: "This asset is currently unassigned"
- "Assign Asset" button displayed (instead of "Return Asset")
- No assignment dates shown
- Status shows: "Available"
Data: Asset Status: "Available"
Pass/Fail: [ ]
```

### SC-3.3: History and Edit Handling
Applies to:
- TC-3.5
- TC-3.6
- TC-3.7

### TC-3.5: View Asset History Table
```
Test ID: TC-3.5
Title: Display asset lifecycle history
Precondition: Asset has multiple history records
Steps:
1. Scroll to Asset History section
2. Observe history table
Expected Result:
- Columns shown: Date & Time, Action, Details, Performed By
- History sorted by Date & Time (descending - newest first)
- Sample rows visible:
  * "04-01-2026 10:30 | Assigned | Assigned to John Doe | Admin User"
  * "03-15-2026 14:20 | Status Changed | Available → Assigned | Tech Joe"
- Shows last 10 records (pagination or scroll)
Data: Asset with 15+ history records
Pass/Fail: [ ]
```

### TC-3.6: Edit Asset - Inline Mode
```
Test ID: TC-3.6
Title: Switch to edit mode for asset modification
Precondition: Asset Details page opened, user is IT Admin
Steps:
1. Click "Edit Asset" button
2. Page switches to edit mode
3. Fields become editable (white background)
4. Modify: Asset Name from "Laptop-001" to "Laptop-001-Updated"
5. Click "Save" button
Expected Result:
- Fields become white/editable
- "Save" and "Cancel" buttons appear
- After Save: Success message "Asset updated successfully"
- Page refreshes showing updated name
- History records new edit action
Data: Asset Name: "Laptop-001"
Pass/Fail: [ ]
```

### TC-3.7: Concurrent Edit - Version Conflict
```
Test ID: TC-3.7
Title: Handle concurrent edits with optimistic locking
Precondition: User A & B both open same asset
Steps:
1. User A: Opens asset, clicks Edit, modifies Name, Saves
2. User B: Has same asset open, modifies Description, clicks Save
Expected Result:
- User A save succeeds
- User B sees error: "Asset was modified by Admin at 10:30. Refresh to see latest"
- User B data retained in form
- User B clicks Refresh, sees User A's changes, can re-apply their edit
Data: Asset ID: 123, Version: 3 (User A) → 4, Version conflict for User B (3)
Pass/Fail: [ ]
```

### SC-3.4: Status and Asset Transfer
Applies to:
- TC-3.8
- TC-3.9
- TC-3.10
- TC-3.11

### TC-3.8: Change Asset Status
```
Test ID: TC-3.8
Title: Change asset status via status button
Precondition: Asset Status is "Available"
Steps:
1. Click "Change Status" button
2. Modal appears with status options
3. Select "Damaged"
4. Click Confirm
Expected Result:
- Status changes from "Available" to "Damaged"
- Success message: "Asset status updated successfully"
- Page refreshes with new status
- History records: "04-03-2026 | Status Changed | Available → Damaged | Current User"
Data: Current Status: "Available", New Status: "Damaged"
Pass/Fail: [ ]
```

### TC-3.9: Invalid Status Transition Blocked
```
Test ID: TC-3.9
Title: Prevent invalid status transitions
Precondition: Asset Status is "Lost"
Steps:
1. Click "Change Status" button
2. Observe available status options
Expected Result:
- Available options: Only "Available" (marked as "Found")
- Status "Assigned", "Damaged" NOT available
- Tooltip explains: "Lost items can only be marked as Found"
Data: Current Status: "Lost"
Pass/Fail: [ ]
```

### TC-3.10: Assign Asset to Employee
```
Test ID: TC-3.10
Title: Assign unassigned asset to employee
Precondition: Asset Status is "Available", unassigned
Steps:
1. Click "Assign Asset" button
2. Modal appears with employee search
3. Search for and select "Jane Smith"
4. Enter "Expected Return Date": "06-30-2026"
5. Click "Assign"
Expected Result:
- Asset assigned to Jane Smith
- Status changes to "Assigned"
- Current Assignment Details updated
- Success: "Asset assigned successfully"
- History records: "04-03-2026 | Assigned | Assigned to Jane Smith | Current User"
Data: Employee: Jane Smith, Return Date: 06-30-2026
Pass/Fail: [ ]
```

### TC-3.11: Return Assigned Asset
```
Test ID: TC-3.11
Title: Return assigned asset back to available
Precondition: Asset assigned to "John Doe"
Steps:
1. Click "Return Asset" button
2. Confirm return dialog
3. Optional: Enter return condition (Good/Damaged/For Repair)
4. Click "Confirm Return"
Expected Result:
- Asset status changes to "Available"
- Current Assignment Details shows "Unassigned"
- Success: "Asset returned successfully"
- History records: "04-03-2026 | Returned | Returned from John Doe | Current User"
Data: Current Assignee: John Doe
Pass/Fail: [ ]
```

### SC-3.5: Export Asset Details.
Applies to:
- TC-3.12

### TC-3.12: Export Asset Details to PDF
```
Test ID: TC-3.12
Title: Export complete asset information to PDF
Precondition: Asset Details page open for Laptop-001
Steps:
1. Click "Export Asset Details" button
2. Select "PDF" format
3. File download triggered
Expected Result:
- File downloads: "asset_LAPTOP-001_04-03-2026.pdf"
- PDF contains:
  * Asset Information section
  * Current Assignment Details
  * Asset History table (all records or summary)
- Generated timestamp: "Exported on 04-03-2026 10:45 by Admin"
- PDF is readable and properly formatted
Data: Asset: Laptop-001
Pass/Fail: [ ]
```

---

## 📝 USER STORY 4: ASSET CREATION - TEST CASES

### SC-4.1: Basic Asset Creation.
Applies to:
- TC-4.1
- TC-4.2

### TC-4.1: Create Asset with Required Fields Only
```
Test ID: TC-4.1
Title: Successful asset creation with mandatory fields
Precondition: User is IT Admin, on Add Asset form
Steps:
1. Asset Name: "Laptop-New-001"
2. Asset Category: "Laptop"
3. Purchase Date: "03-15-2026"
4. Click Save
Expected Result:
- Asset ID auto-generated (e.g., "ASSET-000451")
- Success: "Asset created successfully"
- Redirected to Asset List
- New asset visible in list with Status="Available"
- All fields populated correctly
Data: Name: "Laptop-New-001", Category: "Laptop", Date: "03-15-2026"
Pass/Fail: [ ]
```

### TC-4.2: Auto-Generated Asset ID
```
Test ID: TC-4.2
Title: Verify automatic Asset ID generation
Precondition: Form loaded for new asset
Steps:
1. Observe Asset ID field (read-only)
2. Field shows pre-generated ID: "ASSET-000451"
3. Note the ID
4. Fill other fields and Save
Expected Result:
- Asset ID field is read-only (cannot edit)
- Generated ID is unique
- Format is consistent: "ASSET-" + sequential number
- ID assigned to created asset in database
Data: Generated ID: "ASSET-000451"
Pass/Fail: [ ]
```

### SC-4.2: Asset Name and Category Validation.
Applies to:
- TC-4.3
- TC-4.4
- TC-4.5

### TC-4.3: Asset Name - Minimum Length
```
Test ID: TC-4.3
Title: Reject asset name with less than 2 characters
Precondition: Form loaded
Steps:
1. Asset Name: "L"
2. Select Category: "Laptop"
3. Select Purchase Date: "03-15-2026"
4. Click Save
Expected Result:
- Error below Asset Name: "Minimum 2 characters are required"
- Form not submitted
- User can correct and resubmit
Data: Input: "L"
Pass/Fail: [ ]
```

### TC-4.4: Asset Name - Maximum Length
```
Test ID: TC-4.4
Title: Reject asset name exceeding 50 characters
Precondition: Form loaded
Steps:
1. Asset Name: "A" x 51
2. System prevents further input
Expected Result:
- Field accepts max 50 characters
- Display error (if user tries to paste longer):
  "Maximum 50 characters are required"
- Cannot submit with > 50 chars
Data: Input: 51 character string
Pass/Fail: [ ]
```

### TC-4.5: Category Selection - Mandatory
```
Test ID: TC-4.5
Title: Prevent submission without category selection
Precondition: Form loaded
Steps:
1. Asset Name: "Laptop-001"
2. Category: [Leave as "Select Category" - no selection]
3. Click Save
Expected Result:
- Error message: "Please select asset category"
- Form not submitted
- Other fields retained
Data: Category: Not selected
Pass/Fail: [ ]
```

### SC-4.3: Purchase Date and Cost Validation.
Applies to:
- TC-4.6
- TC-4.7
- TC-4.8
- TC-4.9

### TC-4.6: Purchase Date - Past Date Validation
```
Test ID: TC-4.6
Title: Accept past purchase dates, reject future dates
Precondition: Form loaded
Steps:
1. Asset Name: "Laptop-001"
2. Category: "Laptop"
3. Purchase Date: Select today + 1 day (future date)
4. Click Save
Expected Result:
- Error: "Purchase date cannot be a future date"
- Form not submitted
- User can select past/current date
Data: Selected Date: 04-04-2026 (future relative to 04-03-2026)
Pass/Fail: [ ]
```

### TC-4.7: Asset Cost - Negative Value Rejection
```
Test ID: TC-4.7
Title: Reject negative asset cost values
Precondition: Form loaded, Category selected
Steps:
1. Asset Cost: "-5000"
2. Click Save
Expected Result:
- Error: "Cost must be 0 or greater"
- Negative value rejected
- Field cleared or set to 0
Data: Input: "-5000"
Pass/Fail: [ ]
```

### TC-4.8: Asset Cost - Decimal Precision
```
Test ID: TC-4.8
Title: Handle asset cost with up to 2 decimal places
Precondition: Form loaded
Steps:
1. Asset Cost: "1500.99"
2. Fill other required fields
3. Click Save
Expected Result:
- Value accepted: "1500.99"
- Database stores with 2 decimal places
- Display shows: "₹ 1,500.99"

Variant: Cost = "1500.999" (3 decimals)
Expected: Rejected or rounded to "1500.99"
Data: Input: "1500.99"
Pass/Fail: [ ]
```

### TC-4.9: Vendor Name - Optional Field with Validation
```
Test ID: TC-4.9
Title: Validate vendor name length when provided
Precondition: Form loaded
Steps:
1. Leave Vendor Name empty (optional)
2. Create asset - should succeed

Variant A: Vendor = "A" (1 character)
Expected: Error - "Minimum 2 characters required" (if entered)

Variant B: Vendor = "Dell Technologies" (valid)
Expected: Accepted

Variant C: Vendor = "A" x 51 characters
Expected: Error - "Maximum 50 characters are required"
Data: Optional field
Pass/Fail: [ ]
```

### SC-4.4: Property Handling on Category Selection
Applies to:
- TC-4.10
- TC-4.11

### TC-4.10: Dynamic Category Properties Loading
```
Test ID: TC-4.10
Title: Load category properties dynamically on selection
Precondition: Form loaded, "Laptop" category has properties: [RAM, CPU, Storage]
Steps:
1. Select Category: "Laptop"
2. Properties section appears with fields:
   - RAM (Integer, Mandatory)
   - CPU (String, Optional)
   - Storage (Integer, Mandatory)
3. Enter values: RAM=16, CPU="Intel i7", Storage=512
4. Click Save
Expected Result:
- Properties load immediately after category selection
- Mandatory properties marked with *
- Property values accepted and saved
- Asset created with property data
Data: Category: "Laptop", Properties: {RAM: 16, CPU: "Intel i7", Storage: 512}
Pass/Fail: [ ]
```

### TC-4.11: Change Category - Properties Reset
```
Test ID: TC-4.11
Title: Handle property data when category changes
Precondition: User selected "Laptop" and filled properties
Steps:
1. Selected Category: "Laptop" with properties filled
2. Change Category to "Desktop"
3. Confirmation dialog shown: "Changing category will clear property values. Confirm?"
4. Click "Yes, proceed"
Expected Result:
- Desktop properties now show (different set)
- Previous Laptop properties cleared
- User can fill Desktop properties
Data: Category change: Laptop → Desktop
Pass/Fail: [ ]
```

### SC-4.5: Optional Assignment and Cancel Handling
Applies to:
- TC-4.12
- TC-4.13

### TC-4.12: Assign Employee During Creation
```
Test ID: TC-4.12
Title: Optionally assign employee while creating asset
Precondition: Form loaded, Asset details filled
Steps:
1. Fill Asset Name, Category, Purchase Date
2. Optional: Select "Assign Employee" from dropdown
3. Search for and select "John Doe"
4. Click Save
Expected Result:
- Asset created with Status="Available" or "Assigned" (depends on flow)
- If assigned: Current Assignment Details shows "Assigned to John Doe"
- If not assigned during creation: Can assign later from Asset Details
Data: Employee: John Doe (ID: 45)
Pass/Fail: [ ]
```

### TC-4.13: Cancel - Discard All Data
```
Test ID: TC-4.13
Title: Cancel button discards all form data
Precondition: Form filled with asset details
Steps:
1. Asset Name: "Laptop-New"
2. Category: "Laptop"
3. Add Properties with values
4. Click Cancel
Expected Result:
- User redirected to Asset List
- No asset created
- No data saved
- Form cleared completely
Data: Filled data: Name, Category, Properties
Pass/Fail: [ ]
```

---

## 🧪 EDGE CASE TEST MATRIX

| Edge Case | Story | TC ID | Priority |
|-----------|-------|-------|----------|
| Unicode characters in names | All | TC-X.1 | HIGH |
| Transaction rollback on error | Create Category, Create Asset | TC-1.x, TC-4.x | HIGH |
| Concurrent operations same record | Asset Details | TC-3.7 | HIGH |
| Very large file export (50K+ rows) | Asset List | TC-2.10 | MEDIUM |
| SQL injection in search | Asset List | TC-2.3 | HIGH |
| XSS in text fields | All | TC-X.2 | HIGH |
| Timezone handling for dates | Asset Creation, Details | TC-4.x, TC-3.x | MEDIUM |
| Empty result set after filter | Asset List | TC-2.5 | LOW |
| Browser back button unsaved data | All forms | TC-X.3 | MEDIUM |
| Network timeout during save | All | TC-X.4 | MEDIUM |

---

## 📊 TEST EXECUTION TIPS

1. **Test Data Preparation**
   - Create fixtures: 5 categories, 100 assets with various statuses
   - Pre-assign some assets to employees
   - Create history records for 5+ actions per asset

2. **Browser Testing**
   - Chrome (latest), Firefox (latest), Safari, Edge
   - Clear cache between test runs
   - Use incognito/private mode to avoid caching

3. **Performance Testing**
   - Measure page load time: < 3 seconds
   - Search response: < 2 seconds for 10K records
   - Export initiation: < 5 seconds for 50K records

4. **Security Testing**
   - OWASP Top 10 validation
   - SQL injection attempts in all text fields
   - XSS payloads: `<script>alert('xss')</script>`
   - CSRF token validation on post requests

5. **Accessibility Testing**
   - Screen reader: NVDA, JAWS
   - Keyboard navigation: Tab, Shift+Tab, Enter
   - Color contrast: WCAG AAA standard

---

**Test Suite Complete**: 50+ test cases covering critical paths, validations, edge cases, and error scenarios



Validate Authentication
Validate Authentication Error Handling
Validate Category Creation
Validate Duplicate Category Handling
Validate Category Name Boundary Validation
Validate Mandatory Field Validation
Validate Whitespace and Normalization Handling
Validate Optional Property Configuration
Validate Cancel and Save Failure Recovery
Validate Asset Creation
Validate Asset Name and Category Validation
Validate Purchase Date and Cost Validation
Validate Property Handling on Category Selection
Validate Optional Assignment and Cancel Handling
Validate Asset List Display and Search
Validate Asset List Filtering
Validate Asset List Pagination and Deletion
Validate Asset List Export
Validate Asset Details Overview
Validate Assignment Visibility
Validate History and Edit Handling
Validate Asset Status Change
Validate Asset Transfer
Validate Export Asset Details