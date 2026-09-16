# Defect Report

**Project:** Visitor Check-in Application  
**Assessment:** QA Assessment – Chulo Solutions / Namespace Inc.  
**Feature:** Visitor Check-in  
**Tester:** Puja Shah  

## Test Environment

- OS: Windows 10 (Build 19045)
- Browser: Google Chrome
- Ruby: 3.3.12
- Rails: 7.2.3.2
- Puma: 8.0.2
- Database: SQLite3
- Timezone: Asia/Kathmandu (UTC+5:45)

---

## DEF-001 — N+1 Queries on Active Visitor List

**Type:** Performance  
**Priority:** Medium  
**Severity:** Medium  

### Description
The active visitor list makes one query to load visitors and additional queries to load each visitor's host. This can increase database load as the number of visitors grows.

### Steps to Reproduce
1. Seed at least 20 active visitors with hosts.
2. Start the Rails server.
3. Request `GET /api/visitors?page=1`.
4. Check the Rails server logs.

### Expected Result
Visitors and their hosts should be loaded efficiently without making one additional query for every visitor.

### Actual Result
The server makes one visitor query followed by individual `Host Load` queries for the visitors on the page.

---

## DEF-002 — User Can Navigate to Empty Page After Last Page

**Type:** Functional / Data  
**Priority:** Low  
**Severity:** Low–Medium  

### Description
When there are exactly 60 active visitors, the application allows navigation to Page 4 even though 60 records should occupy exactly 3 pages when using 20 records per page.

### Steps to Reproduce
1. Create exactly 60 active visitors.
2. Open the Active Visitors list.
3. Navigate through the pages.
4. Click **Next** after Page 3.

### Expected Result
Page 3 should be the last page and **Next** should be disabled.

### Actual Result
Page 4 can be opened and displays an empty table.

---

## DEF-003 — Full Name Accepts Whitespace-Only Input

**Type:** Functional / Validation  
**Priority:** Medium  
**Severity:** Medium  

### Description
The required Full Name field accepts input containing only spaces.

### Steps to Reproduce
1. Open the registration form.
2. Enter only spaces in Full Name.
3. Select a Host.
4. Submit the form.

### Expected Result
The form should reject whitespace-only input and display a validation message.

### Actual Result
The visitor is successfully created with a whitespace-only Full Name.

**Evidence:** [Screenshot](https://prnt.sc/WgxruZlwCRzJ)

---

## DEF-004 — Checked-In Time Is Displayed in UTC

**Type:** Functional  
**Priority:** Critical  
**Severity:** High  

### Description
The specification requires times to be displayed in the receptionist's local timezone. During testing in Asia/Kathmandu, the Checked In time was displayed in UTC instead of local time.

### Steps to Reproduce
1. Confirm the current time in Asia/Kathmandu.
2. Register a new visitor.
3. Check the Checked In value in the Active Visitors list.

### Expected Result
The Checked In time should be displayed in the receptionist's local timezone.

### Actual Result
The application displayed the UTC time instead of the Kathmandu local time.

**Evidence:** [Screenshot](https://prnt.sc/Phq_ETuLBUQO)

---

## DEF-005 — Checked-In Time Has No Timezone Indicator

**Type:** Usability  
**Priority:** Low  
**Severity:** Low  

### Description
The Checked In value is displayed as a bare time such as `07:34`, without AM/PM or timezone information.

### Steps to Reproduce
1. Register a visitor.
2. Check the Checked In column.

### Expected Result
The displayed time should use an unambiguous format and clearly communicate the applicable timezone.

### Actual Result
Only the time is displayed, without AM/PM or timezone information.

**Evidence:** [Screenshot](https://prnt.sc/iiLF2qjLBxq2)

---

## DEF-006 — Application Crashes for Very Long Full Name

**Type:** Functional / Crash  
**Priority:** High  
**Severity:** High  

### Description
Submitting an unusually long Full Name causes the application to crash instead of handling the input gracefully.

### Steps to Reproduce
1. Open the registration form.
2. Enter approximately 500 characters in Full Name.
3. Submit the form.

### Expected Result
The application should either reject the input with a clear validation message or handle it safely.

### Actual Result
The application crashes after submitting the long value.

**Evidence:** [Screenshot](https://prnt.sc/X9nmGt7ZbsKQ)

---

## DEF-007 — Full Name Accepts Numeric Characters

**Type:** Data / Validation  
**Priority:** Low  
**Severity:** Low–Medium  

### Description
The Full Name field accepts numeric characters such as `Rabi12345`.

### Steps to Reproduce
1. Open the registration form.
2. Enter `Rabi12345` in Full Name.
3. Select a Host.
4. Submit the form.

### Expected Result
The system should follow the defined validation rules for names.

### Actual Result
The visitor was successfully created with numeric characters in Full Name.

**Evidence:** [Screenshot](https://prnt.sc/bxRN9zpn2edN)

**Note:** Confirm with the Product Owner whether numeric characters are allowed before treating this as a confirmed defect.

---

## DEF-008 — Full Name Accepts Special Characters

**Type:** Data / Validation  
**Priority:** Low  
**Severity:** Low–Medium  

### Description
The Full Name field accepts values containing only special characters.

### Steps to Reproduce
1. Open the registration form.
2. Enter special characters such as `*&&^%$#$%^&**&^%`.
3. Select a Host.
4. Submit the form.

### Expected Result
The system should follow the defined validation rules for names.

### Actual Result
The visitor was successfully created with the special-character value.

**Evidence:** [Screenshot](https://prnt.sc/7YHz3B3luvng)

**Note:** Confirm the intended character rules with the Product Owner.

---

## DEF-009 — Company Field Accepts Special Characters and Numbers

**Type:** Data / Validation  
**Priority:** Low  
**Severity:** Low  

### Description
The Company field accepts values containing numbers and special characters.

### Steps to Reproduce
1. Enter a valid Full Name.
2. Enter `ABC-123 & Co.` in Company.
3. Select a Host.
4. Submit the form.

### Expected Result
The value should be handled according to the defined Company field validation rules.

### Actual Result
The visitor was successfully created with the entered value.

**Evidence:** [Screenshot](https://prnt.sc/8xwYsbhmeu3s)

**Note:** Confirm whether Company is intended to be unrestricted free text.

---

## DEF-010 — Purpose Field Accepts Unrestricted Special Characters

**Type:** Data / Validation  
**Priority:** Low  
**Severity:** Low  

### Description
The Purpose field accepts a value containing only special characters.

### Steps to Reproduce
1. Enter a valid Full Name.
2. Enter special characters in Purpose.
3. Leave Company empty.
4. Select a Host.
5. Submit the form.

### Expected Result
The value should be handled according to the defined Purpose field validation rules.

### Actual Result
The visitor was successfully created with the special-character value.

**Evidence:** [Screenshot](https://prnt.sc/rFQRxuPO9Xpc)

**Note:** Confirm whether Purpose is intended to be unrestricted free text.

---

## DEF-011 — Rapid Double Submit Creates Duplicate Visitors

**Type:** Functional  
**Priority:** High  
**Severity:** Medium–High  

### Description
Submitting the registration form twice in rapid succession creates two visitor records for the same registration attempt.

### Steps to Reproduce
1. Fill in all required registration fields with valid data.
2. Click Submit twice quickly.
3. Check the Active Visitors list.

### Expected Result
Only one visitor record should be created for a single registration attempt.

### Actual Result
Two separate visitor records are created.

**Evidence:** [Screenshot](https://prnt.sc/WEdpV5oOy9gw)

---

# Open Questions / Notes

### 1. Full Name Validation
The requirements do not clearly define whether numbers and special characters are allowed in Full Name.

**Question:** Should Full Name allow only letters and spaces, or should it support a wider range of characters?

### 2. Company and Purpose Validation
The requirements do not clearly define character restrictions for Company and Purpose.

**Question:** Are these fields intended to support unrestricted free-text input?

### 3. Empty Company and Purpose
The source test sheet marked the empty Company/Purpose case as failed, although the observed behavior matched the expected behavior.

This should be reviewed and corrected in the test execution record if the fields are optional.

### 4. API-Level Confirmation
The issues involving whitespace-only names, very long names, and rapid duplicate submission were also checked at the API level to determine whether the behavior originates from the backend.

---

# Highest-Priority Confirmed Defect

**DEF-004 — Checked-In Time Is Displayed in UTC**

This directly conflicts with the requirement that times must be displayed in the receptionist's local timezone. The issue affects normal visitor records rather than only an unusual edge case.