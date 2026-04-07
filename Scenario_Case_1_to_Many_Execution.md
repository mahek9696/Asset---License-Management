# Scenario-Case Mapping (1-to-Many)
## Sprint 1 - API-Aware Execution Fields

This file follows a strict 1-to-many model:
- One Scenario (SC-*) maps to many Test Cases (TC-*).
- Every test case includes: API Calls, Methods, Header, Pre-Script, Post-Script, Input, Expected Result.

## User Story 1 - Create Category

### SC-1.1: Valid Category Creation
| Test Case | API Calls | Methods | Header | Pre-Script | Post-Script | Input | Expected Result |
|---|---|---|---|---|---|---|---|
| TC-1.1 Create Category with Asset Type | /api/admin/createcategory | POST | Authorization: Bearer <token>; Content-Type: application/json | Login as IT Admin | Verify category listed | categoryName=Laptop, typeId=1, statusId=1 | 201 created, success message, visible in list |
| TC-1.2 Create Category with License Type | /api/admin/createcategory | POST | Authorization: Bearer <token>; Content-Type: application/json | Login as IT Admin | Verify category listed | categoryName=MS365 License, typeId=2, statusId=1 | 201 created, success message, visible in list |

### SC-1.2: Duplicate Category Validation
| Test Case | API Calls | Methods | Header | Pre-Script | Post-Script | Input | Expected Result |
|---|---|---|---|---|---|---|---|
| TC-1.3 Duplicate Category Same Case | /api/admin/createcategory | POST | Authorization: Bearer <token>; Content-Type: application/json | Ensure Laptop exists | Verify count unchanged | categoryName=Laptop, typeId=1, statusId=1 | 400 bad request, duplicate message |
| TC-1.4 Duplicate Category Different Case | /api/admin/createcategory | POST | Authorization: Bearer <token>; Content-Type: application/json | Ensure Laptop exists | Log behavior for AC clarification | categoryName=laptop, typeId=1, statusId=1 | Duplicate handling as per configured rule |

### SC-1.3: Category Name Boundary Validation
| Test Case | API Calls | Methods | Header | Pre-Script | Post-Script | Input | Expected Result |
|---|---|---|---|---|---|---|---|
| TC-1.5 Name Minimum Boundary | /api/admin/createcategory | POST | Authorization: Bearer <token>; Content-Type: application/json | Login as IT Admin | Verify record created | categoryName=PC, typeId=1, statusId=1 | Accepted with min valid length |
| TC-1.6 Name Below Minimum | /api/admin/createcategory | POST | Authorization: Bearer <token>; Content-Type: application/json | Login as IT Admin | Verify no create | categoryName=P, typeId=1, statusId=1 | 400 validation error |
| TC-1.7 Name Maximum Boundary | /api/admin/createcategory | POST | Authorization: Bearer <token>; Content-Type: application/json | Login as IT Admin | Verify record created | categoryName=<50 chars>, typeId=1, statusId=1 | Accepted with max valid length |
| TC-1.8 Name Above Maximum | /api/admin/createcategory | POST | Authorization: Bearer <token>; Content-Type: application/json | Login as IT Admin | Verify no create | categoryName=<51 chars>, typeId=1, statusId=1 | 400 validation error |

### SC-1.4: Mandatory Field Validation
| Test Case | API Calls | Methods | Header | Pre-Script | Post-Script | Input | Expected Result |
|---|---|---|---|---|---|---|---|
| TC-1.9 Type Required | /api/admin/createcategory | POST | Authorization: Bearer <token>; Content-Type: application/json | Login as IT Admin | Verify no create | categoryName=Server, typeId=null, statusId=1 | 400 required field error |
| TC-1.10 Name Required | /api/admin/createcategory | POST | Authorization: Bearer <token>; Content-Type: application/json | Login as IT Admin | Verify no create | categoryName=null, typeId=1, statusId=1 | 400 required field error |

### SC-1.5: Whitespace and Normalization
| Test Case | API Calls | Methods | Header | Pre-Script | Post-Script | Input | Expected Result |
|---|---|---|---|---|---|---|---|
| TC-1.11 Leading/Trailing Spaces | /api/admin/createcategory | POST | Authorization: Bearer <token>; Content-Type: application/json | Login as IT Admin | Capture final stored value | categoryName="  Laptop  ", typeId=1, statusId=1 | Trimmed save or explicit reject |
| TC-1.12 Whitespace-Only Name | /api/admin/createcategory | POST | Authorization: Bearer <token>; Content-Type: application/json | Login as IT Admin | Verify no create | categoryName="   ", typeId=1, statusId=1 | 400 invalid/required error |

### SC-1.6: Optional Property Configuration
| Test Case | API Calls | Methods | Header | Pre-Script | Post-Script | Input | Expected Result |
|---|---|---|---|---|---|---|---|
| TC-1.13 Save with Optional Property | /api/admin/createcategory | POST | Authorization: Bearer <token>; Content-Type: application/json | Login as IT Admin | Verify property saved | categoryName=Laptop, typeId=1, statusId=1, properties=[RAM] | 201 created with properties |
| TC-1.14 Duplicate Property Name | /api/admin/createcategory | POST | Authorization: Bearer <token>; Content-Type: application/json | Login as IT Admin | Verify no create | properties=[RAM,RAM] | 400 duplicate property validation |

### SC-1.7: Cancel and Save Failure Recovery
| Test Case | API Calls | Methods | Header | Pre-Script | Post-Script | Input | Expected Result |
|---|---|---|---|---|---|---|---|
| TC-1.15 Cancel Form Data Discard | N/A | N/A | N/A | Open create form with draft data | Verify no record created | UI only cancel action | Data discarded and return to list |
| TC-1.16 Save Failure Recovery | /api/admin/createcategory | POST | Authorization: Bearer <token>; Content-Type: application/json | Simulate 500/timeout | Retry after failure | valid payload under failure condition | Error shown, no duplicate create, retry works |

## User Story 2 - Asset List

### SC-2.1: Asset List Display and Search
| Test Case | API Calls | Methods | Header | Pre-Script | Post-Script | Input | Expected Result |
|---|---|---|---|---|---|---|---|
| TC-2.1 Display All Assets | /api/admin/getasset/dashboard | GET | Authorization: Bearer <token> | Seed 30 assets | Verify table and pagination | pageNo=1,pageSize=15,search="" | Asset list renders with actions |
| TC-2.2 Search by Asset Name | /api/admin/getasset/dashboard | GET | Authorization: Bearer <token> | Seed searchable assets | Verify filtered rows | search=Laptop | Matching rows only |
| TC-2.3 Search Special Characters | /api/admin/getasset/dashboard | GET | Authorization: Bearer <token> | Seed serial with special chars | Verify safe search | search=ABC-123/XYZ | Match returned, no injection side effect |

### SC-2.2: Asset List Filtering
| Test Case | API Calls | Methods | Header | Pre-Script | Post-Script | Input | Expected Result |
|---|---|---|---|---|---|---|---|
| TC-2.4 Filter by Category | /api/admin/getasset/dashboard | GET | Authorization: Bearer <token> | Seed mixed categories | Verify category subset | category=Laptop | Only Laptop assets shown |
| TC-2.5 Category and Status Filter | /api/admin/getasset/dashboard | GET | Authorization: Bearer <token> | Seed mixed status/category | Verify AND logic output | category=Laptop,status=Assigned | Only matching intersection |
| TC-2.6 Clear Filters | /api/admin/getasset/dashboard | GET | Authorization: Bearer <token> | Apply filters first | Verify full list reset | category=All,status=All,search="" | Full list restored |

### SC-2.3: Pagination and Deletion
| Test Case | API Calls | Methods | Header | Pre-Script | Post-Script | Input | Expected Result |
|---|---|---|---|---|---|---|---|
| TC-2.7 Pagination Navigation | /api/admin/getasset/dashboard | GET | Authorization: Bearer <token> | Seed >1 page | Verify page movement | pageNo=1->2 | Correct records per page |
| TC-2.8 Delete Asset Pagination Impact | N/A (delete API not defined in current Swagger) | N/A | N/A | Seed 90 assets and navigate to end | Verify page fallback logic | delete last item on last page | Redirect to valid page and item removed |

### SC-2.4: Asset List Export
| Test Case | API Calls | Methods | Header | Pre-Script | Post-Script | Input | Expected Result |
|---|---|---|---|---|---|---|---|
| TC-2.9 Export Filtered Data | /api/admin/reportlist | GET | Authorization: Bearer <token> | Apply filters in list | Validate exported file rows | category=Laptop,status=Available | Export contains filtered records |
| TC-2.10 Export Large Dataset | /api/admin/reportlist | GET | Authorization: Bearer <token> | Seed large dataset | Verify size/limit behavior | no filters | Export success or limit message |

## User Story 3 - Asset Details

### SC-3.1: Asset Details Overview
| Test Case | API Calls | Methods | Header | Pre-Script | Post-Script | Input | Expected Result |
|---|---|---|---|---|---|---|---|
| TC-3.1 Navigate to Details | /api/admin/getasset/{id} | GET | Authorization: Bearer <token> | Seed asset id | Verify detail page rendered | id=123 | Correct detail loaded |
| TC-3.2 View Read-Only Info | /api/admin/getasset/{id} | GET | Authorization: Bearer <token> | Seed full asset data | Verify fields are read-only | id=123 | All fields shown with format rules |

### SC-3.2: Assignment Visibility
| Test Case | API Calls | Methods | Header | Pre-Script | Post-Script | Input | Expected Result |
|---|---|---|---|---|---|---|---|
| TC-3.3 Assigned Asset View | /api/admin/getasset/{id} | GET | Authorization: Bearer <token> | Seed assigned asset | Verify assignment section | id=assigned asset | Assigned details visible |
| TC-3.4 Unassigned Asset View | /api/admin/getasset/{id} | GET | Authorization: Bearer <token> | Seed unassigned asset | Verify unassigned state | id=available asset | Unassigned message and assign action |

### SC-3.3: History and Edit Handling
| Test Case | API Calls | Methods | Header | Pre-Script | Post-Script | Input | Expected Result |
|---|---|---|---|---|---|---|---|
| TC-3.5 View Asset History | /api/admin/getasset/{id} | GET | Authorization: Bearer <token> | Seed history records | Verify ordering and visibility | id=asset with history | History visible and sorted |
| TC-3.6 Edit Asset Inline | /api/admin/update/{id} | PUT | Authorization: Bearer <token>; Content-Type: application/json | Open editable asset | Verify save and updated values | id=123, payload updated fields | 200 updated and visible in details |
| TC-3.7 Concurrent Edit Conflict | /api/admin/update/{id} | PUT | Authorization: Bearer <token>; Content-Type: application/json | Two sessions on same asset | Verify conflict handling behavior | overlapping updates | Conflict message or controlled overwrite behavior |

### SC-3.4: Status and Asset Transfer
| Test Case | API Calls | Methods | Header | Pre-Script | Post-Script | Input | Expected Result |
|---|---|---|---|---|---|---|---|
| TC-3.8 Change Asset Status | /api/admin/update/{id} | PUT | Authorization: Bearer <token>; Content-Type: application/json | Seed Available asset | Verify status and history update | id=123, assetStatus=Damaged | 200 status changed |
| TC-3.9 Invalid Status Transition | /api/admin/update/{id} | PUT | Authorization: Bearer <token>; Content-Type: application/json | Seed Lost asset | Verify block for invalid transition | id=123, invalid status target | Validation or business rule block |
| TC-3.10 Assign Asset to Employee | /assets/{id}/assign | POST | Authorization: Bearer <token>; Content-Type: application/json | Seed unassigned asset and employee | Verify assignment persisted | id=123, employeeId=45, expectedReturnDate | 200 assigned and status updated |
| TC-3.11 Return Assigned Asset | /api/admin/returnasset/{id} | POST | Authorization: Bearer <token>; Content-Type: application/json | Seed assigned asset | Verify unassignment and status | id=123, returnDate, conditionOnReturn | 200 returned and available |

### SC-3.5: Export Asset Details
| Test Case | API Calls | Methods | Header | Pre-Script | Post-Script | Input | Expected Result |
|---|---|---|---|---|---|---|---|
| TC-3.12 Export Asset Details PDF | /api/admin/getasset/{id} | GET | Authorization: Bearer <token> | Open details page | Validate downloaded document content | id=123, format=PDF (UI export) | Exported file includes details/history |

## User Story 4 - Asset Creation

### SC-4.1: Basic Asset Creation
| Test Case | API Calls | Methods | Header | Pre-Script | Post-Script | Input | Expected Result |
|---|---|---|---|---|---|---|---|
| TC-4.1 Create with Required Fields | /api/admin/createasset | POST | Authorization: Bearer <token>; Content-Type: application/json | Login as IT Admin | Verify asset appears in list | assetName, assetCategory, purchaseDate | 201 created with generated assetId |
| TC-4.2 Auto-Generated Asset ID | /api/admin/createasset | POST | Authorization: Bearer <token>; Content-Type: application/json | Open create form | Verify generated id format and uniqueness | valid payload | assetId generated and persisted |

### SC-4.2: Asset Name and Category Validation
| Test Case | API Calls | Methods | Header | Pre-Script | Post-Script | Input | Expected Result |
|---|---|---|---|---|---|---|---|
| TC-4.3 Name Minimum Length | /api/admin/createasset | POST | Authorization: Bearer <token>; Content-Type: application/json | Open create form | Verify no create on invalid | assetName=L | 400 validation error |
| TC-4.4 Name Maximum Length | /api/admin/createasset | POST | Authorization: Bearer <token>; Content-Type: application/json | Open create form | Verify no create on invalid | assetName=<51 chars> | 400 validation error |
| TC-4.5 Category Mandatory | /api/admin/createasset | POST | Authorization: Bearer <token>; Content-Type: application/json | Open create form | Verify no create on missing category | category missing | 400 required field error |

### SC-4.3: Purchase Date and Cost Validation
| Test Case | API Calls | Methods | Header | Pre-Script | Post-Script | Input | Expected Result |
|---|---|---|---|---|---|---|---|
| TC-4.6 Purchase Date Future Rejection | /api/admin/createasset | POST | Authorization: Bearer <token>; Content-Type: application/json | Open create form | Verify no create on future date | purchaseDate=future date | 400 validation error |
| TC-4.7 Negative Cost Rejection | /api/admin/createasset | POST | Authorization: Bearer <token>; Content-Type: application/json | Open create form | Verify no create on negative cost | assetCost=-5000 | 400 validation error |
| TC-4.8 Decimal Precision | /api/admin/createasset | POST | Authorization: Bearer <token>; Content-Type: application/json | Open create form | Verify persisted precision behavior | assetCost=1500.99 or 1500.999 | Accepted precision or rejected extra decimals |
| TC-4.9 Vendor Optional Validation | /api/admin/createasset | POST | Authorization: Bearer <token>; Content-Type: application/json | Open create form | Verify optional field handling | vendorName empty/short/long | Optional accepted; invalid lengths rejected |

### SC-4.4: Property Handling on Category Selection
| Test Case | API Calls | Methods | Header | Pre-Script | Post-Script | Input | Expected Result |
|---|---|---|---|---|---|---|---|
| TC-4.10 Dynamic Property Loading | /api/getcategory | GET | Authorization: Bearer <token> | Load category metadata | Verify property fields render | category=Laptop | Required/optional properties loaded |
| TC-4.11 Property Reset on Category Change | /api/getcategory | GET | Authorization: Bearer <token> | Fill initial category properties | Verify previous values cleared | category change Laptop->Desktop | New properties shown, old values cleared |

### SC-4.5: Optional Assignment and Cancel Handling
| Test Case | API Calls | Methods | Header | Pre-Script | Post-Script | Input | Expected Result |
|---|---|---|---|---|---|---|---|
| TC-4.12 Assign Employee During Creation | /api/admin/createasset | POST | Authorization: Bearer <token>; Content-Type: application/json | Prepare employee list | Verify assignment behavior after create | assignEmployee=John Doe | Created with assigned/available flow as configured |
| TC-4.13 Cancel Discard Data | N/A | N/A | N/A | Fill create form | Verify no record created | UI cancel action | Form data discarded and return to list |
