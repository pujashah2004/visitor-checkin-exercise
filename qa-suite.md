# QA Suite

**Project:** Visitor Check-in Application  
**Assessment:** QA Assessment – Chulo Solutions / Namespace Inc.  
**Tester:** Puja Shah  
**Feature:** Visitor Check-in

---

## Detailed Test Cases

The complete test cases and execution results are maintained in the QA test sheet.

**QA Test Sheet:** [Open QA Test Sheet](https://docs.google.com/spreadsheets/d/1vYoH05QQ9K8mP3gz_kLAUYwn1GRnKdRB/edit?usp=sharing)

---

## Test Case Execution Legend

- `[ ]` Not executed
- `[pass]` Passed
- `[fail]` Failed

---

# 1. Registration

## REG-001 — Verify registration form loads successfully

**Status:** [pass]

**Preconditions:**
- Application is running.
- Receptionist is on the registration page.

**Steps:**
1. Confirm the registration page is displayed.
2. Confirm Full Name, Company, Host, and Purpose fields are displayed.
3. Confirm the Submit button is available.

**Expected Result:**
The registration form should load successfully with all required fields and the Submit button.

---

## REG-002 — Verify visitor registration with valid data

**Status:** [pass]

**Preconditions:**
- Registration form is open.
- At least one active Host is available.

**Steps:**
1. Enter a valid Full Name.
2. Enter a valid Company.
3. Select an active Host.
4. Enter a valid Purpose.
5. Click Submit.
6. Confirm the visitor appears in the Active Visitors list.

**Expected Result:**
The visitor should be registered successfully and appear in the Active Visitors list with a check-in time.

---

## REG-003 — Verify registration with missing Full Name

**Status:** [fail]

**Preconditions:**
- Registration form is open.
- A valid Host is available.

**Steps:**
1. Leave Full Name empty.
2. Enter valid values in the other fields.
3. Select a Host.
4. Click Submit.

**Expected Result:**
The visitor should not be registered and a validation message should be displayed for Full Name.

**Actual Result:**
The API accepted the request and created a visitor without a Full Name.

**Related Defect:** DEF-003

---

## REG-004 — Verify registration with whitespace-only Full Name

**Status:** [fail]

**Preconditions:**
- Registration form is open.

**Steps:**
1. Enter only spaces in Full Name.
2. Select a Host.
3. Click Submit.

**Expected Result:**
The system should reject whitespace-only Full Name input.

**Actual Result:**
The visitor was created with a whitespace-only Full Name.

**Related Defect:** DEF-003

---

## REG-005 — Verify registration with missing Host

**Status:** [fail]

**Preconditions:**
- Registration form is open.

**Steps:**
1. Enter a valid Full Name.
2. Leave Host unselected.
3. Enter valid values in the other fields.
4. Click Submit.

**Expected Result:**
The visitor should not be registered and a validation message should be displayed for Host.

**Actual Result:**
The API accepted the request without a Host.

---

## REG-006 — Verify registration with empty Company

**Status:** [pass]

**Preconditions:**
- Registration form is open.

**Steps:**
1. Enter a valid Full Name.
2. Select a valid Host.
3. Leave Company empty.
4. Enter a valid Purpose.
5. Click Submit.

**Expected Result:**
The visitor should be registered successfully when Company is left empty.

---

## REG-007 — Verify registration with empty Purpose

**Status:** [pass]

**Preconditions:**
- Registration form is open.

**Steps:**
1. Enter a valid Full Name.
2. Select a valid Host.
3. Enter a valid Company.
4. Leave Purpose empty.
5. Click Submit.

**Expected Result:**
The visitor should be registered successfully when Purpose is left empty.

---

## REG-008 — Verify registration with very long Full Name

**Status:** [fail]

**Preconditions:**
- Registration form is open.

**Steps:**
1. Enter approximately 500 characters in Full Name.
2. Select a valid Host.
3. Click Submit.

**Expected Result:**
The application should reject the input with a clear validation message or handle it safely without crashing.

**Actual Result:**
The application crashed after submitting the long value.

**Related Defect:** DEF-006

---

## REG-009 — Verify rapid double submission

**Status:** [fail]

**Preconditions:**
- Registration form is open.
- Valid visitor information is entered.

**Steps:**
1. Enter valid registration information.
2. Click Submit twice quickly.
3. Confirm the Active Visitors list.

**Expected Result:**
Only one visitor record should be created for one registration attempt.

**Actual Result:**
Two visitor records were created.

**Related Defect:** DEF-011

---

# 2. Active Visitor List

## VIS-001 — Verify active visitors are displayed

**Status:** [pass]

**Preconditions:**
- At least one active visitor exists.

**Steps:**
1. Open the Active Visitors list.
2. Confirm the registered visitor is displayed.

**Expected Result:**
Active visitors should be displayed with their visitor information and check-in time.

---

## VIS-002 — Verify active visitor list displays maximum 20 records per page

**Status:** [pass]

**Preconditions:**
- More than 20 active visitors exist.

**Steps:**
1. Open the Active Visitors list.
2. Confirm the number of visitors displayed on Page 1.
3. Navigate to Page 2.
4. Confirm the number of visitors displayed.

**Expected Result:**
A maximum of 20 visitors should be displayed per page.

---

## VIS-003 — Verify user cannot navigate beyond the last populated page

**Status:** [fail]

**Preconditions:**
- Exactly 60 active visitors exist.

**Steps:**
1. Open the Active Visitors list.
2. Navigate to Page 2.
3. Navigate to Page 3.
4. Click Next after Page 3.
5. Confirm the page displayed.

**Expected Result:**
Page 3 should be the final page and Next should be disabled.

**Actual Result:**
Page 4 can be opened even though it contains no visitor records.

**Related Defect:** DEF-002

---

## VIS-004 — Verify checked-in time uses receptionist local timezone

**Status:** [fail]

**Preconditions:**
- Receptionist timezone is Asia/Kathmandu.

**Steps:**
1. Confirm the current local time in Kathmandu.
2. Register a new visitor.
3. Confirm the Checked In value.

**Expected Result:**
The Checked In time should be displayed in the receptionist's local timezone.

**Actual Result:**
The displayed time was in UTC instead of Asia/Kathmandu.

**Related Defect:** DEF-004

---

## VIS-005 — Verify checked-in time is clearly formatted

**Status:** [fail]

**Preconditions:**
- At least one active visitor exists.

**Steps:**
1. Open the Active Visitors list.
2. Confirm the Checked In value.

**Expected Result:**
The time should be displayed in a clear and unambiguous format.

**Actual Result:**
The application displays a bare HH:MM value without AM/PM or timezone information.

**Related Defect:** DEF-005

---

# 3. Checkout

## CHK-001 — Verify active visitor can be checked out

**Status:** [pass]

**Preconditions:**
- An active visitor exists.

**Steps:**
1. Locate an active visitor.
2. Confirm the visitor is active.
3. Check out the visitor.
4. Refresh the Active Visitors list.
5. Confirm the visitor is no longer shown as active.

**Expected Result:**
The visitor should be checked out successfully and removed from the Active Visitors list.

---

## CHK-002 — Verify an already checked-out visitor cannot be checked out again

**Status:** [fail]

**Preconditions:**
- A visitor has already been checked out.

**Steps:**
1. Check out a visitor.
2. Attempt to check out the same visitor again.
3. Confirm the response.

**Expected Result:**
The system should prevent a second checkout or return an appropriate validation response.

**Actual Result:**
The API returned a successful response for the repeated checkout.

**Note:** Confirm the expected behavior with the Product Owner because the requirement does not explicitly define repeat checkout behavior.

---

# 4. Deactivation

## DEACT-001 — Verify admin can deactivate a visitor record

**Status:** [pass]

**Preconditions:**
- Admin access is available.
- A visitor record exists.

**Steps:**
1. Open the visitor record.
2. Deactivate the visitor.
3. Confirm the visitor status.

**Expected Result:**
The visitor record should be marked as inactive.

---

## DEACT-002 — Verify deactivated visitors do not appear in the active list

**Status:** [ ]

**Preconditions:**
- A visitor record has been deactivated.

**Steps:**
1. Deactivate a visitor.
2. Refresh the Active Visitors list.
3. Confirm the deactivated visitor is not displayed.

**Expected Result:**
The deactivated visitor should not appear in the Active Visitors list.

---

## DEACT-003 — Verify deactivated visitors cannot be selected

**Status:** [ ]

**Preconditions:**
- A visitor record has been deactivated.

**Steps:**
1. Open the registration flow.
2. Open the applicable visitor/host selection.
3. Confirm the deactivated record is not available for selection.

**Expected Result:**
Deactivated records should not be selectable.

---

# 5. Negative and Boundary Testing

## NEG-001 — Verify whitespace-only Full Name is rejected

**Status:** [fail]

**Preconditions:**
- Registration form is open.

**Steps:**
1. Enter only spaces in Full Name.
2. Select a valid Host.
3. Click Submit.

**Expected Result:**
The system should reject the input as invalid.

**Actual Result:**
The visitor was created with a whitespace-only Full Name.

**Related Defect:** DEF-003

---

## NEG-002 — Verify very long Full Name is handled safely

**Status:** [fail]

**Preconditions:**
- Registration form is open.

**Steps:**
1. Enter approximately 500 characters in Full Name.
2. Select a valid Host.
3. Click Submit.

**Expected Result:**
The application should safely reject or handle the value.

**Actual Result:**
The application crashed.

**Related Defect:** DEF-006

---

## NEG-003 — Verify rapid repeated submission does not create duplicate visitors

**Status:** [fail]

**Preconditions:**
- Registration form is open.

**Steps:**
1. Enter valid visitor information.
2. Click Submit twice rapidly.
3. Confirm the Active Visitors list.

**Expected Result:**
Only one visitor record should be created.

**Actual Result:**
Two visitor records were created.

**Related Defect:** DEF-011

---

# 6. API Test Coverage

The API was also tested independently to verify backend behavior without relying only on the UI.

The detailed API test cases and execution results are available in the QA test sheet.

### API Coverage

- `GET /api/visitors`
- `GET /api/hosts`
- `POST /api/visitors`
- Visitor checkout
- Visitor deactivation
- Visitor search
- Required field validation
- Negative input
- Boundary input
- Invalid IDs
- Duplicate submission
- Error handling

**QA Test Sheet:** [Open QA Test Sheet](https://docs.google.com/spreadsheets/d/1vYoH05QQ9K8mP3gz_kLAUYwn1GRnKdRB/edit?usp=sharing)

---

# 7. Regression Subset

The following tests should be included in regression testing after a minor registration-form update.

| Test Case | Include | Reason |
|---|---|---|
| REG-001 | Yes | Confirms the registration form still loads correctly. |
| REG-002 | Yes | Core visitor registration flow must continue working. |
| REG-003 | Yes | Required-field validation may be affected by form changes. |
| REG-004 | Yes | Confirms whitespace validation for Full Name. |
| REG-005 | Yes | Host selection and validation are part of registration. |
| REG-006 | Yes | Confirms optional Company behavior remains unchanged. |
| REG-007 | Yes | Confirms optional Purpose behavior remains unchanged. |
| REG-009 | Yes | Prevents duplicate registration after submission changes. |
| VIS-001 | Yes | Registration changes can affect the Active Visitors list. |
| VIS-004 | Yes | Registration creates the check-in timestamp. |
| CHK-001 | Yes | Confirms the complete visitor lifecycle still works. |
| DEACT-002 | Yes | Confirms inactive visitors remain excluded from the active list. |
| VIS-003 | No | Pagination is not directly affected by a minor registration-form change. |
| API Search | No | Search behavior is outside the registration-form change. |
| Performance/N+1 | No | Requires separate performance-focused testing. |

---

# 8. Open Questions

1. Should Full Name allow numbers and special characters?
2. Are Company and Purpose intentionally unrestricted free-text fields?
3. What should happen when an already checked-out visitor is checked out again?
4. Should a deactivated visitor be available for a future visit, or should a new visitor record always be created?

---

# 9. QA Test Sheet

The detailed test execution, API results, actual results, and evidence are available here:

**[Open QA Test Sheet](https://docs.google.com/spreadsheets/d/1vYoH05QQ9K8mP3gz_kLAUYwn1GRnKdRB/edit?usp=sharing)**