# UAT Test Script: Patient Search

**Test ID:** UAT-002  
**Feature:** Patient Search  
**Version:** 1.0  
**Created:** 2026-11-16  
**Tester:** ________ANSVK_______  
**Test Date:** _______2026-11-17________

---

## Test Environment

**Requirements:**
- Clinic EMR application running
- Access to http://127.0.0.1:8080/patients
- Database with at least 30+ test patients (for pagination testing)
- Mix of active and inactive patients
- Patients with diverse names (Unicode, single-word, compound, etc.)

**Pre-Test Setup:**
- [ x] Application is running
- [x ] Test patients created (see Test Data section below)
- [ x] Can access patient list page

---

## Test Objective

Verify that the patient search feature correctly:
- Searches by patient name (case-insensitive)
- Searches by MRN
- Handles partial name searches
- Filters active/inactive patients
- Paginates results properly (25 per page)
- Handles Unicode search terms
- Provides clear feedback for no results
- Validates search term length

---

## Test Data Required

Create these test patients before starting (or use existing patients):

**Active Patients:**
1. John Smith (MRN: P00000101)
2. Jane Smith (MRN: P00000102)
3. Mary Johnson (MRN: P00000103)
4. Robert Williams (MRN: P00000104)
5. 山田太郎 (Yamada Taro - Japanese, MRN: P00000105)
6. محمد أحمد (Muhammad Ahmad - Arabic, MRN: P00000106)
7. José García (Spanish, MRN: P00000107)
8. O'Brien (single name, MRN: P00000108)
9. Jean-François (hyphenated, MRN: P00000109)
10. Smith-Jones (hyphenated surname, MRN: P00000110)

**Inactive Patients:**
11. Inactive Test Patient (MRN: P00000111) - Set as inactive
12. Another Inactive Smith (MRN: P00000112) - Set as inactive

---

## Test Cases

### Test Case 1: Search by Full Name (Exact Match)

**Steps:**
1. Navigate to http://127.0.0.1:8080/patients
2. In the search box, enter: `John Smith`
3. Click Search (or press Enter)

**Expected Results:**
- ✅ Results display patients matching "John Smith"
- ✅ John Smith (P00000101) appears in results
- ✅ Other patients with "Smith" may also appear
- ✅ Search is case-insensitive
- ✅ Results show: Name, MRN, Active status
- ✅ Page title shows "Patients"

**Actual Results:**

**Status:** [ ] Pass / [X] Fail  
**Notes:** _I found that result showing Name, Record Number, Birth Date, Phone, Status______________________________________________

---

### Test Case 2: Search by Partial Name

**Test 2A: Partial First Name**

**Steps:**
1. Clear search box
2. Enter: `Joh`
3. Click Search

**Expected Results:**
- ✅ Returns all patients with "Joh" in their name
- ✅ Should include: John Smith, Mary Johnson
- ✅ Partial match works

**Actual Results:**

**Status:** [x ] Pass / [] Fail

---

**Test 2B: Partial Last Name**

**Steps:**
1. Enter: `Smi`
2. Click Search

**Expected Results:**
- ✅ Returns: John Smith, Jane Smith, Smith-Jones
- ✅ Partial surname matching works

**Actual Results:**

**Status:** [x ] Pass / [ ] Fail

---

**Test 2C: Middle of Name**

**Steps:**
1. Enter: `mit`
2. Click Search

**Expected Results:**
- ✅ Returns patients with "mit" anywhere in name
- ✅ Should include Smith patients

**Actual Results:**

**Status:** [ x] Pass / [ ] Fail

---

### Test Case 3: Case-Insensitive Search

**Test 3A: All Lowercase**

**Steps:**
1. Enter: `john smith` (all lowercase)
2. Click Search

**Expected Results:**
- ✅ Returns John Smith
- ✅ Case does not matter

**Actual Results:**

**Status:** [ x] Pass / [ ] Fail

---

**Test 3B: All Uppercase**

**Steps:**
1. Enter: `MARY JOHNSON` (all uppercase)
2. Click Search

**Expected Results:**
- ✅ Returns Mary Johnson
- ✅ Case-insensitive search confirmed

**Actual Results:**

**Status:** [ x] Pass / [ ] Fail

---

**Test 3C: Mixed Case**

**Steps:**
1. Enter: `JoHn SmItH` (mixed case)
2. Click Search

**Expected Results:**
- ✅ Returns John Smith
- ✅ Search ignores case variations

**Actual Results:**

**Status:** [x ] Pass / [ ] Fail

---

### Test Case 4: Search by MRN

**Test 4A: Complete MRN**

**Steps:**
1. Enter: `P00000101`
2. Click Search

**Expected Results:**
- ✅ Returns exactly one patient: John Smith
- ✅ MRN search works

**Actual Results:**

**Status:** [ x] Pass / [ ] Fail

---

**Test 4B: Partial MRN**

**Steps:**
1. Enter: `P0000010`
2. Click Search

**Expected Results:**
- ✅ Returns patients with MRNs starting with P0000010
- ✅ Should include: P00000101, P00000102, ... P00000109
- ✅ Partial MRN search works

**Actual Results:**

**Status:** [ x] Pass / [ ] Fail

---

**Test 4C: MRN Without Prefix**

**Steps:**
1. Enter: `00000101` (without 'P')
2. Click Search

**Expected Results:**
- ✅ Returns patient with MRN P00000101
- ✅ Prefix is optional in search

**Actual Results:**

**Status:** [ x] Pass / [ ] Fail

---

### Test Case 5: Unicode Name Search

**Test 5A: Japanese Characters**

**Steps:**
1. Enter: `山田` (partial Japanese name)
2. Click Search

**Expected Results:**
- ✅ Returns: 山田太郎 (Yamada Taro)
- ✅ Unicode search works correctly

**Actual Results:**

**Status:** [x ] Pass / [ ] Fail

---

**Test 5B: Arabic Characters**

**Steps:**
1. Enter: `محمد` (Muhammad in Arabic)
2. Click Search

**Expected Results:**
- ✅ Returns: محمد أحمد
- ✅ Arabic characters searchable

**Actual Results:**

**Status:** [x ] Pass / [ ] Fail

---

**Test 5C: Accented Characters**

**Steps:**
1. Enter: `José`
2. Click Search

**Expected Results:**
- ✅ Returns: José García
- ✅ Accented characters work in search

**Actual Results:**

**Status:** [x ] Pass / [ ] Fail

---

### Test Case 6: Special Characters in Search

**Test 6A: Hyphenated Name**

**Steps:**
1. Enter: `Jean-François`
2. Click Search

**Expected Results:**
- ✅ Returns: Jean-François
- ✅ Hyphen included in search

**Actual Results:**

**Status:** [x ] Pass / [ ] Fail

---

**Test 6B: Apostrophe**

**Steps:**
1. Enter: `O'Brien`
2. Click Search

**Expected Results:**
- ✅ Returns: O'Brien
- ✅ Apostrophe handled correctly

**Actual Results:**

**Status:** [ x] Pass / [ ] Fail

---

### Test Case 7: Empty Search (Show All)

**Steps:**
1. Leave search box completely empty
2. Ensure "Show inactive patients" is UNCHECKED
3. Click Search or press Enter

**Expected Results:**
- ✅ Displays all active patients
- ✅ Paginated (25 per page)
- ✅ Inactive patients NOT shown
- ✅ Sorted by name (alphabetically)

**Actual Results:**

**Status:** [ ] Pass / [x ] Fail
**Comment:** Not sorted by name
---

### Test Case 8: No Results Found

**Steps:**
1. Enter: `ZZZZZZZZNONEXISTENT` (name that doesn't exist)
2. Click Search

**Expected Results:**
- ✅ Message displayed: "No patients found" (or similar)
- ✅ Empty results list
- ✅ Search box retains entered text
- ✅ No error/crash

**Actual Results:**

**Status:** [ x] Pass / [ ] Fail

---

### Test Case 9: Active/Inactive Filter

**Test 9A: Active Only (Default)**

**Steps:**
1. Clear search box (or search for "Smith")
2. Ensure "Show inactive patients" checkbox is UNCHECKED
3. Click Search

**Expected Results:**
- ✅ Shows: John Smith, Jane Smith, Smith-Jones
- ✅ Does NOT show: "Another Inactive Smith"
- ✅ Only active patients visible

**Actual Results:**

**Status:** [x ] Pass / [ ] Fail

---

**Test 9B: Include Inactive**

**Steps:**
1. Search for: `Smith`
2. CHECK the "Show inactive patients" checkbox
3. Click Search or observe automatic update

**Expected Results:**
- ✅ Shows: John Smith, Jane Smith, Smith-Jones
- ✅ ALSO shows: "Another Inactive Smith"
- ✅ Inactive patients marked/indicated as inactive
- ✅ All patients with "Smith" are visible

**Actual Results:**

**Status:** [ ] Pass / [ ] Fail

---

**Test 9C: Search Inactive by Name**

**Steps:**
1. CHECK "Show inactive patients"
2. Search for: `Inactive Test Patient`
3. Click Search

**Expected Results:**
- ✅ Returns: Inactive Test Patient
- ✅ Clearly marked as Inactive

**Actual Results:**

**Status:** [ x] Pass / [ ] Fail

---

### Test Case 10: Pagination

**Pre-requisite:** Need 30+ active patients in database

**Test 10A: First Page**

**Steps:**
1. Clear search, uncheck inactive filter
2. View first page of results

**Expected Results:**
- ✅ Shows exactly 25 patients (or fewer if < 25 total)
- ✅ "Next" button/link visible if more than 25 patients exist
- ✅ "Previous" button/link NOT visible (on first page)
- ✅ Page indicator shows: Page 1

**Actual Results:**

**Status:** [x ] Pass / [ ] Fail

---

**Test 10B: Navigate to Next Page**

**Steps:**
1. Click "Next" button/link
2. Observe page 2

**Expected Results:**
- ✅ Shows next 25 patients (patients 26-50)
- ✅ "Previous" button/link now visible
- ✅ "Next" button visible if more pages exist
- ✅ Page indicator shows: Page 2
- ✅ URL contains: `?page=2`

**Actual Results:**

**Status:** [ x] Pass / [ ] Fail

---

**Test 10C: Navigate to Previous Page**

**Steps:**
1. From page 2, click "Previous" button/link

**Expected Results:**
- ✅ Returns to page 1
- ✅ Shows first 25 patients again
- ✅ URL contains: `?page=1` or no page parameter

**Actual Results:**

**Status:** [ x] Pass / [ ] Fail

---

**Test 10D: Search with Pagination**

**Steps:**
1. Search for common name (e.g., "Smith")
2. If results > 25, check pagination

**Expected Results:**
- ✅ Search results paginated correctly
- ✅ "Next" button works with search query
- ✅ URL contains both search and page: `?q=Smith&page=2`
- ✅ Search term retained across pages

**Actual Results:**

**Status:** [x ] Pass / [ ] Fail

---

### Test Case 11: Search Term Length Validation

**Test 11A: Maximum Length (100 characters)**

**Steps:**
1. Enter exactly 100 characters in search box:
   ```
   This is a very long search term with exactly one hundred characters to test the maximum length limit ok
   ```
2. Click Search

**Expected Results:**
- ✅ Search executes successfully
- ✅ No validation error (100 chars is allowed)

**Actual Results:** No warning message but I can't type in more than 100 characters

**Status:** [ x] Pass / [ ] Fail

---

**Test 11B: Exceed Maximum (101+ characters)**

**Steps:**
1. Enter 101+ characters in search box
2. Click Search

**Expected Results:**
- ✅ Validation error displayed
- ✅ Error message: "Search must be 100 characters or fewer."
- ✅ Search NOT executed
- ✅ Search term retained in box

**Actual Results:** Nothing show. there is no warning message also

**Status:** [ x] Pass / [ ] Fail

---

### Test Case 12: Leading/Trailing Whitespace

**Steps:**
1. Enter: `   John Smith   ` (spaces before and after)
2. Click Search

**Expected Results:**
- ✅ Search works correctly
- ✅ Whitespace trimmed automatically
- ✅ Returns John Smith
- ✅ Same results as without extra spaces

**Actual Results:**

**Status:** [x ] Pass / [ ] Fail

---

### Test Case 13: Multiple Spaces Between Words

**Steps:**
1. Enter: `John    Smith` (multiple spaces between words)
2. Click Search

**Expected Results:**
- ✅ Search works correctly
- ✅ Returns John Smith
- ✅ Multiple spaces handled gracefully

**Actual Results:** Nothing found

**Status:** [ ] Pass / [x ] Fail

---

### Test Case 14: Search Result Details

**Steps:**
1. Search for: `John Smith`
2. Observe the search results display

**Expected Results:**
Each result shows:
- ✅ Patient name (clickable link)
- ✅ MRN
- ✅ Active/Inactive status
- ✅ Gender (optional, may be displayed)
- ✅ Proper formatting and alignment
- ✅ Click on name navigates to patient detail page

**Actual Results:**

**Status:** [x ] Pass / [ ] Fail

---

### Test Case 15: Sort Order

**Steps:**
1. Clear search (show all active patients)
2. Observe the order of results

**Expected Results:**
- ✅ Patients sorted alphabetically by name (A-Z)
- ✅ Case-insensitive sort
- ✅ Consistent sort order across pages

**Actual Results:** Not sorted

**Status:** [ ] Pass / [ x] Fail

---

### Test Case 16: Search Persistence

**Test 16A: Search Term Retained**

**Steps:**
1. Search for: `Smith`
2. Click on a patient to view details
3. Click "Back" or navigate back to patient list

**Expected Results:**
- ✅ Search term "Smith" still in search box
- ✅ Search results still showing Smith patients
- ✅ Filter state (active/inactive) retained

**Actual Results:**

**Status:** [x ] Pass / [ ] Fail

---

**Test 16B: Pagination with Search**

**Steps:**
1. Search for common name with multiple pages
2. Go to page 2
3. Refresh browser (F5)

**Expected Results:**
- ✅ Still on page 2
- ✅ Search term retained
- ✅ Results consistent

**Actual Results:**

**Status:** [ x] Pass / [ ] Fail

---

### Test Case 17: Direct URL Access

**Test 17A: Search via URL Parameter**

**Steps:**
1. Navigate directly to: `http://127.0.0.1:8080/patients?q=John`

**Expected Results:**
- ✅ Page loads with search results for "John"
- ✅ Search box contains "John"
- ✅ Results display correctly

**Actual Results:**

**Status:** [ x] Pass / [ ] Fail

---

**Test 17B: Pagination via URL**

**Steps:**
1. Navigate directly to: `http://127.0.0.1:8080/patients?page=2`

**Expected Results:**
- ✅ Shows page 2 of patients
- ✅ Pagination works via URL

**Actual Results:**

**Status:** [ x] Pass / [ ] Fail

---

**Test 17C: Combined Parameters**

**Steps:**
1. Navigate to: `http://127.0.0.1:8080/patients?q=Smith&inactive=1&page=1`

**Expected Results:**
- ✅ Searches for "Smith"
- ✅ Includes inactive patients
- ✅ Shows page 1
- ✅ All parameters work together

**Actual Results:**

**Status:** [ x] Pass / [ ] Fail

---

### Test Case 18: Clear Search

**Steps:**
1. Perform a search: `Smith`
2. Clear the search box (delete all text)
3. Click Search or press Enter

**Expected Results:**
- ✅ Shows all active patients (default view)
- ✅ Filter resets to active-only if that was changed
- ✅ Pagination resets to page 1

**Actual Results:**

**Status:** [ x] Pass / [ ] Fail

---

### Test Case 19: Special Search Scenarios

**Test 19A: Search for MRN Prefix Only**

**Steps:**
1. Enter: `P`
2. Click Search

**Expected Results:**
- ✅ Returns all patients (all MRNs start with P)
- ✅ Or returns patients with "P" in their name

**Actual Results:**all patients

**Status:** [ x] Pass / [ ] Fail

---

**Test 19B: Search with Number**

**Steps:**
1. Enter: `123`
2. Click Search

**Expected Results:**
- ✅ Returns patients with "123" in name or MRN
- ✅ No error

**Actual Results:**patient with P00000023,P00000022,P00000024

**Status:** [ ] Pass / [x ] Fail

---

**Test 19C: Search Single Character**

**Steps:**
1. Enter: `J`
2. Click Search

**Expected Results:**
- ✅ Returns all patients with "J" in name
- ✅ Single character search works

**Actual Results:**

**Status:** [x ] Pass / [ ] Fail

---

### Test Case 20: Browser Back/Forward

**Steps:**
1. Perform search: `John`
2. Click on a patient
3. Click browser Back button
4. Click browser Forward button
5. Click browser Back button again

**Expected Results:**
- ✅ Back from patient detail returns to search results
- ✅ Forward returns to patient detail
- ✅ Back again returns to search results
- ✅ Search term and results preserved throughout
- ✅ Browser history works correctly

**Actual Results:**

**Status:** [ x] Pass / [ ] Fail

---

### Test Case 21: Page Refresh

**Steps:**
1. Search for: `Smith`
2. Press F5 (browser refresh)

**Expected Results:**
- ✅ Page reloads
- ✅ Search term "Smith" still present
- ✅ Results still showing Smith patients
- ✅ No loss of state

**Actual Results:**

**Status:** [ x] Pass / [ ] Fail

---

### Test Case 22: Empty Database

**Pre-requisite:** Test with empty database or all inactive patients

**Steps:**
1. With no active patients, load patient list

**Expected Results:**
- ✅ Shows appropriate message: "No patients found" or "No active patients"
- ✅ Suggestion to register first patient or check inactive filter
- ✅ "Register new patient" button visible
- ✅ No error/crash

**Actual Results:** I don't know how to test it

**Status:** [ ] Pass / [ ] Fail

---

## Performance Tests

### Test Case 23: Large Result Set

**Pre-requisite:** Database with 100+ patients

**Steps:**
1. Perform search that returns 50+ results
2. Navigate through multiple pages
3. Observe loading time

**Expected Results:**
- ✅ Each page loads in < 1 second
- ✅ Pagination is responsive
- ✅ No lag or freezing

**Actual Results:**

**Status:** [ x] Pass / [ ] Fail  
**Load Time:** _____less than 1__ seconds

---

### Test Case 24: Rapid Searches

**Steps:**
1. Type rapidly in search box, changing search term quickly
2. Or perform multiple searches in quick succession

**Expected Results:**
- ✅ Application handles rapid searches gracefully
- ✅ No duplicate results
- ✅ No errors or crashes
- ✅ Latest search term takes precedence

**Actual Results:**

**Status:** [ x] Pass / [ ] Fail

---

## Edge Cases

**Additional scenarios to test if time permits:**

- [ ] Search with only spaces (should be treated as empty)
- [ ] Search with SQL injection attempt (e.g., `' OR '1'='1`)
- [ ] Search with HTML tags (e.g., `<script>alert('test')</script>`)
- [ ] Very long patient names in results (display correctly?)
- [ ] Page number beyond available pages (e.g., page=999999)
- [ ] Negative page number (e.g., page=-1)
- [ ] Non-numeric page parameter (e.g., page=abc)

---

## Test Results Summary

**Total Test Cases:** 24+ main cases with sub-tests  
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

---

## Recommendations

**Search Improvements:**
_______________________________________________________________

**UI/UX Enhancements:**
_______________________________________________________________

**Performance Optimizations:**
_______________________________________________________________

---

## Sign-Off

**Tester Name:** _______________________  
**Date Completed:** _______________________  
**Status:** [ ] Ready for Production / [ ] Needs Fixes  

---

**Document Status:** Ready for Use  
**Next Step:** Execute this test script with test patient data
