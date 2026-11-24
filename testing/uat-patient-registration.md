# UAT Test Script: Patient Registration

**Test ID:** UAT-001  
**Feature:** Patient Registration  
**Version:** 1.0  
**Created:** 2026-11-16  
**Tester:** _______________  
**Test Date:** _______________

---

## Test Environment

**Requirements:**
- Clinic EMR application running
- Access to http://127.0.0.1:8080/patients
- Fresh database or ability to verify new patient creation
- Web browser (Chrome, Firefox, or Edge)

**Pre-Test Setup:**
- [ ] Application is running
- [ ] Can access patient list page
- [ ] Note the current highest MRN number: _______________

---

## Test Objective

Verify that the patient registration feature correctly:
- Creates new patient records with all field types
- Auto-assigns sequential MRN numbers
- Validates all input fields appropriately
- Handles Unicode names and special characters
- Supports flexible birth date formats
- Provides clear error messages for invalid input

---

## Test Cases

### Test Case 1: Basic Patient Registration

**Scenario:** Register a patient with all fields filled

**Steps:**
1. Navigate to http://127.0.0.1:8080/patients
2. Click "Register new patient" button
3. Fill in the form:
   - Name: `John Smith`
   - Birth Date: `1985-06-15`
   - Gender: Select `male`
   - Phone: `+1-555-0100`
   - Email: `john.smith@example.com`
   - Address: `123 Main Street\nApt 4B\nSpringfield, IL 62701`
   - Active: Leave checked (default)
4. Click "Register patient" button

**Expected Results:**
- ✅ Patient is created successfully
- ✅ Redirected to patient detail page
- ✅ Success message displayed: "Patient record saved"
- ✅ Patient details match entered data exactly
- ✅ MRN is auto-assigned (format: P00000XXX)
- ✅ MRN is sequential (one more than previous highest)
- ✅ Address displays with line breaks preserved
- ✅ Active status shows as "Active"
- ✅ Created timestamp is present
- ✅ UUID is displayed (36-character format with dashes)

**Actual Results:**

**Status:** [ ] Pass / [ ] Fail  
**Notes:** _______________________________________________

---

### Test Case 2: Minimum Required Fields

**Scenario:** Register patient with only required field (name)

**Steps:**
1. Navigate to http://127.0.0.1:8080/patients/new
2. Fill in only:
   - Name: `Jane Doe`
3. Leave all other fields empty
4. Click "Register patient"

**Expected Results:**
- ✅ Patient is created successfully
- ✅ Name: `Jane Doe`
- ✅ Birth Date: Empty (blank)
- ✅ Gender: `unknown` (default)
- ✅ Phone: Empty
- ✅ Email: Empty
- ✅ Address: Empty
- ✅ Active: `true` (default)
- ✅ MRN auto-assigned correctly

**Actual Results:**

**Status:** [ ] Pass / [ ] Fail  
**Notes:** _______________________________________________

---

### Test Case 3: Unicode Name (Non-Latin Characters)

**Scenario:** Register patient with Unicode name in various scripts

**Test 3A: Japanese**

**Steps:**
1. Navigate to registration form
2. Name: `山田太郎` (Yamada Taro in Japanese)
3. Birth Date: `1990`
4. Click "Register patient"

**Expected Results:**
- ✅ Patient created successfully
- ✅ Name displays correctly: `山田太郎`
- ✅ No character corruption or replacement
- ✅ Can search and find patient by Japanese name

**Actual Results:**

**Status:** [ ] Pass / [ ] Fail

---

**Test 3B: Arabic**

**Steps:**
1. Navigate to registration form
2. Name: `محمد أحمد` (Muhammad Ahmad in Arabic)
3. Click "Register patient"

**Expected Results:**
- ✅ Patient created successfully
- ✅ Name displays correctly with right-to-left text
- ✅ Can search and find patient

**Actual Results:**

**Status:** [ ] Pass / [ ] Fail

---

**Test 3C: Thai**

**Steps:**
1. Navigate to registration form
2. Name: `สมชาย ใจดี` (Somchai Jaidee in Thai)
3. Click "Register patient"

**Expected Results:**
- ✅ Patient created successfully
- ✅ Name displays correctly with tone marks
- ✅ Can search and find patient

**Actual Results:**

**Status:** [ ] Pass / [ ] Fail

---

**Test 3D: Mixed Scripts**

**Steps:**
1. Navigate to registration form
2. Name: `Jean-François O'Brien 김철수`
3. Click "Register patient"

**Expected Results:**
- ✅ Patient created successfully
- ✅ All characters display correctly
- ✅ Accents (ç), apostrophes, and Korean characters preserved

**Actual Results:**

**Status:** [ ] Pass / [ ] Fail

---

### Test Case 4: Single-Word Name (Mononym)

**Scenario:** Register patient with only one name (no family name)

**Steps:**
1. Navigate to registration form
2. Name: `Madonna`
3. Birth Date: `1958-08-16`
4. Gender: `female`
5. Click "Register patient"

**Expected Results:**
- ✅ Patient created successfully
- ✅ Single-word name accepted without error
- ✅ Name displays as: `Madonna`
- ✅ FHIR export handles single name correctly

**Actual Results:**

**Status:** [ ] Pass / [ ] Fail  
**Notes:** _______________________________________________

---

### Test Case 5: Long Name (Boundary Test)

**Scenario:** Test maximum name length (200 characters)

**Steps:**
1. Navigate to registration form
2. Name: Use exactly 200 characters:
   ```
   Hubert Blaine Wolfeschlegelsteinhausenbergerdorff Sr. von Wolfeschlegelsteinhausenbergerdorff Jr. III Esq. MD PhD DDS MBA MSc BSc Prof. Dr. Honoris Causa Extra Long Name Test Maximum Length Two Hun
   ```
   (Count: 200 chars exactly)
3. Click "Register patient"

**Expected Results:**
- ✅ Patient created successfully
- ✅ Full 200-character name saved
- ✅ Name displays completely without truncation
- ✅ Name searchable

**Actual Results:**

**Status:** [ ] Pass / [ ] Fail

---

**Exceed Limit Test:**

**Steps:**
1. Try to enter 201 characters (add one more letter)
2. Click "Register patient"

**Expected Results:**
- ✅ Validation error displayed
- ✅ Error message: "This value is too long."
- ✅ Patient NOT created
- ✅ Form retains entered data

**Actual Results:**

**Status:** [ ] Pass / [ ] Fail

---

### Test Case 6: Birth Date Formats

**Test 6A: Full Date (YYYY-MM-DD)**

**Steps:**
1. Navigate to registration form
2. Name: `Test Full Date`
3. Birth Date: `1985-06-15`
4. Click "Register patient"

**Expected Results:**
- ✅ Patient created successfully
- ✅ Birth date displayed as: `1985-06-15`

**Actual Results:**

**Status:** [ ] Pass / [ ] Fail

---

**Test 6B: Year and Month (YYYY-MM)**

**Steps:**
1. Navigate to registration form
2. Name: `Test Year Month`
3. Birth Date: `1990-03`
4. Click "Register patient"

**Expected Results:**
- ✅ Patient created successfully
- ✅ Birth date displayed as: `1990-03`
- ✅ No automatic completion to full date

**Actual Results:**

**Status:** [ ] Pass / [ ] Fail

---

**Test 6C: Year Only (YYYY)**

**Steps:**
1. Navigate to registration form
2. Name: `Test Year Only`
3. Birth Date: `1975`
4. Click "Register patient"

**Expected Results:**
- ✅ Patient created successfully
- ✅ Birth date displayed as: `1975`

**Actual Results:**

**Status:** [ ] Pass / [ ] Fail

---

**Test 6D: Empty Birth Date**

**Steps:**
1. Navigate to registration form
2. Name: `Test No Birth Date`
3. Birth Date: Leave empty
4. Click "Register patient"

**Expected Results:**
- ✅ Patient created successfully
- ✅ Birth date field is blank/empty in patient detail

**Actual Results:**

**Status:** [ ] Pass / [ ] Fail

---

### Test Case 7: Birth Date Validation

**Test 7A: Future Date (Invalid)**

**Steps:**
1. Navigate to registration form
2. Name: `Test Future Date`
3. Birth Date: `2030-01-01` (future year)
4. Click "Register patient"

**Expected Results:**
- ❌ Patient NOT created
- ✅ Validation error displayed
- ✅ Error message: "Use YYYY, YYYY-MM, or YYYY-MM-DD, with a real date that is not in the future."
- ✅ Form retains entered name
- ✅ Birth date field highlighted in red

**Actual Results:**

**Status:** [ ] Pass / [ ] Fail

---

**Test 7B: Invalid Date (February 30)**

**Steps:**
1. Navigate to registration form
2. Name: `Test Invalid Date`
3. Birth Date: `2000-02-30`
4. Click "Register patient"

**Expected Results:**
- ❌ Patient NOT created
- ✅ Validation error displayed
- ✅ Error message about invalid date

**Actual Results:**

**Status:** [ ] Pass / [ ] Fail

---

**Test 7C: Invalid Format**

**Steps:**
1. Navigate to registration form
2. Name: `Test Bad Format`
3. Birth Date: `15/06/1985` (wrong format)
4. Click "Register patient"

**Expected Results:**
- ❌ Patient NOT created
- ✅ Validation error displayed
- ✅ Error message mentions correct formats

**Actual Results:**

**Status:** [ ] Pass / [ ] Fail

---

### Test Case 8: Gender Options

**Scenario:** Verify all gender options work

**Test 8A: Male**
- Name: `Test Male`, Gender: `male`
- Expected: ✅ Created, displays "Male"

**Test 8B: Female**
- Name: `Test Female`, Gender: `female`
- Expected: ✅ Created, displays "Female"

**Test 8C: Other**
- Name: `Test Other`, Gender: `other`
- Expected: ✅ Created, displays "Other"

**Test 8D: Unknown (Default)**
- Name: `Test Unknown`, Gender: Leave default
- Expected: ✅ Created, displays "Unknown"

**Actual Results:**

**Status:** [ ] Pass / [ ] Fail  
**Notes:** _______________________________________________

---

### Test Case 9: Phone Number Formats

**Scenario:** Test various phone number formats

**Test 9A: US Format**
- Phone: `+1-555-0199`
- Expected: ✅ Accepted

**Test 9B: International Format**
- Phone: `+44 20 7946 0958`
- Expected: ✅ Accepted

**Test 9C: Extension**
- Phone: `555-0100 ext. 123`
- Expected: ✅ Accepted

**Test 9D: Very Long (80 chars - limit)**
- Phone: `+1-555-0100 extension 12345 mobile alternate contact during business hours only`
- Expected: ✅ Accepted if ≤80 chars

**Test 9E: Exceeds Limit (81+ chars)**
- Expected: ❌ Validation error

**Actual Results:**

**Status:** [ ] Pass / [ ] Fail

---

### Test Case 10: Email Validation

**Test 10A: Valid Email**

**Steps:**
1. Name: `Test Email Valid`
2. Email: `valid.email+tag@example.com`
3. Click "Register patient"

**Expected Results:**
- ✅ Patient created successfully

**Actual Results:**

**Status:** [ ] Pass / [ ] Fail

---

**Test 10B: Invalid Email (Missing @)**

**Steps:**
1. Name: `Test Email Invalid`
2. Email: `notanemail.com`
3. Click "Register patient"

**Expected Results:**
- ❌ Patient NOT created
- ✅ Error message: "Enter a valid email address."

**Actual Results:**

**Status:** [ ] Pass / [ ] Fail

---

**Test 10C: Invalid Email (Multiple @)**

**Steps:**
1. Email: `user@@example.com`
2. Click "Register patient"

**Expected Results:**
- ❌ Validation error

**Actual Results:**

**Status:** [ ] Pass / [ ] Fail

---

**Test 10D: Empty Email (Valid)**

**Steps:**
1. Email: Leave empty
2. Click "Register patient"

**Expected Results:**
- ✅ Patient created (email is optional)

**Actual Results:**

**Status:** [ ] Pass / [ ] Fail

---

### Test Case 11: Multi-Line Address

**Scenario:** Test address with line breaks

**Steps:**
1. Navigate to registration form
2. Name: `Test Address`
3. Address: 
   ```
   Building A, Floor 3
   123 Main Street
   Apt 4B
   Springfield, IL 62701
   United States
   ```
4. Click "Register patient"

**Expected Results:**
- ✅ Patient created successfully
- ✅ Address displays with all line breaks preserved
- ✅ All lines visible in patient detail view

**Actual Results:**

**Status:** [ ] Pass / [ ] Fail

---

### Test Case 12: Active Status Default

**Scenario:** Verify Active checkbox is checked by default

**Steps:**
1. Navigate to http://127.0.0.1:8080/patients/new
2. Observe the "Active" checkbox state
3. WITHOUT checking/unchecking, register a patient

**Expected Results:**
- ✅ Active checkbox is CHECKED by default
- ✅ Patient created with Active = true
- ✅ Patient appears in default search (active patients only)

**Actual Results:**

**Status:** [ ] Pass / [ ] Fail

---

### Test Case 13: Inactive Patient at Registration

**Scenario:** Register an inactive patient (unchecked Active)

**Steps:**
1. Navigate to registration form
2. Name: `Test Inactive Patient`
3. UNCHECK the "Active" checkbox
4. Click "Register patient"

**Expected Results:**
- ✅ Patient created successfully
- ✅ Patient detail shows Active = false / "Inactive"
- ✅ Patient does NOT appear in default patient list
- ✅ Patient DOES appear when "Show inactive patients" is checked

**Actual Results:**

**Status:** [ ] Pass / [ ] Fail

---

### Test Case 14: Special Characters in Name

**Scenario:** Test names with hyphens, apostrophes, accents

**Test 14A: Hyphenated Name**
- Name: `Mary-Jane O'Connor-Smith`
- Expected: ✅ Created successfully

**Test 14B: Accented Characters**
- Name: `José María Azúcar`
- Expected: ✅ Created successfully

**Test 14C: Apostrophe**
- Name: `O'Brien D'Angelo`
- Expected: ✅ Created successfully

**Test 14D: Period in Name**
- Name: `Dr. Martin Luther King Jr.`
- Expected: ✅ Created successfully

**Actual Results:**

**Status:** [ ] Pass / [ ] Fail

---

### Test Case 15: Control Characters (Invalid)

**Scenario:** Verify control characters are rejected

**Steps:**
1. Navigate to registration form
2. Name: Try to enter `John` + TAB character + `Smith` (if possible)
3. Or copy/paste text with embedded null bytes or control chars
4. Click "Register patient"

**Expected Results:**
- ❌ Patient NOT created
- ✅ Error message: "Control characters are not allowed."

**Actual Results:**

**Status:** [ ] Pass / [ ] Fail  
**Notes:** May be difficult to test depending on browser behavior

---

### Test Case 16: MRN Sequential Assignment

**Scenario:** Verify MRN numbers increment sequentially

**Steps:**
1. Note current highest MRN: ________________
2. Register Patient A: Name `MRN Test A`
3. Note Patient A's MRN: ________________
4. Register Patient B: Name `MRN Test B`
5. Note Patient B's MRN: ________________
6. Register Patient C: Name `MRN Test C`
7. Note Patient C's MRN: ________________

**Expected Results:**
- ✅ Patient A MRN = Previous + 1
- ✅ Patient B MRN = Patient A + 1
- ✅ Patient C MRN = Patient B + 1
- ✅ Format: P00000XXX (8 digits with leading zeros)

**Actual Results:**

**Status:** [ ] Pass / [ ] Fail

---

### Test Case 17: UUID Generation

**Scenario:** Verify each patient gets unique UUID

**Steps:**
1. Register 3 patients
2. Note their UUIDs:
   - Patient 1: ____________________________________
   - Patient 2: ____________________________________
   - Patient 3: ____________________________________

**Expected Results:**
- ✅ All UUIDs are unique (no duplicates)
- ✅ Format: 8-4-4-4-12 hexadecimal with dashes (e.g., a1b2c3d4-e5f6-7890-abcd-ef1234567890)
- ✅ UUIDs are stable (don't change on page reload)

**Actual Results:**

**Status:** [ ] Pass / [ ] Fail

---

### Test Case 18: Empty Name (Invalid)

**Scenario:** Verify name is required

**Steps:**
1. Navigate to registration form
2. Leave Name field completely empty
3. Fill other optional fields
4. Click "Register patient"

**Expected Results:**
- ❌ Patient NOT created
- ✅ Error message: "Enter the patient's name."
- ✅ Name field highlighted in red
- ✅ Other field data retained

**Actual Results:**

**Status:** [ ] Pass / [ ] Fail

---

### Test Case 19: Whitespace-Only Name (Invalid)

**Scenario:** Verify whitespace-only name is rejected

**Steps:**
1. Navigate to registration form
2. Name: Enter only spaces: `     ` (5 spaces)
3. Click "Register patient"

**Expected Results:**
- ❌ Patient NOT created
- ✅ Error message about required name

**Actual Results:**

**Status:** [ ] Pass / [ ] Fail

---

### Test Case 20: Rapid Sequential Registration

**Scenario:** Register multiple patients quickly (stress test)

**Steps:**
1. Register 5 patients in rapid succession (as fast as possible)
2. Use names: `Rapid Test 1` through `Rapid Test 5`

**Expected Results:**
- ✅ All 5 patients created successfully
- ✅ All have unique MRNs (no duplicates)
- ✅ MRNs are sequential
- ✅ All have unique UUIDs
- ✅ All appear in patient list

**Actual Results:**

**Status:** [ ] Pass / [ ] Fail

---

## Edge Cases Summary

Test the following additional edge cases if time permits:

- [ ] Very long email (254 characters - max)
- [ ] Very long phone (80 characters - max)
- [ ] Very long address (1000 characters - max)
- [ ] Copy/paste data with unusual formatting
- [ ] Browser back button after registration
- [ ] Browser refresh during form fill
- [ ] Registration with browser JavaScript disabled (should still work)

---

## Test Results Summary

**Total Test Cases:** 20+ main cases with sub-tests  
**Passed:** _______  
**Failed:** _______  
**Blocked:** _______  
**Not Tested:** _______

**Pass Rate:** _______% 

---

## Issues Found

List any bugs or issues discovered during testing:

| Issue # | Test Case | Description | Severity |
|---------|-----------|-------------|----------|
|         |           |             |          |
|         |           |             |          |
|         |           |             |          |

---

## Recommendations

Based on testing, document any recommendations for:
- Field improvements
- Validation changes
- UI/UX enhancements
- Error message clarity
- Additional features needed

**Notes:**
_______________________________________________________________
_______________________________________________________________
_______________________________________________________________

---

## Sign-Off

**Tester Name:** _______________________  
**Date Completed:** _______________________  
**Status:** [ ] Ready for Production / [ ] Needs Fixes  

---

**Document Status:** Ready for Use  
**Next Step:** Execute this test script with fictional patient data
