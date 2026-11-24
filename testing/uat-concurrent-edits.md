# UAT Test Script: Concurrent Edits (Version Control)

**Test ID:** UAT-006  
**Feature:** Concurrent Edit Detection and Version Control  
**Version:** 1.0  
**Created:** 2026-11-16  
**Tester:** Codex (automated HTTP UAT)
**Test Date:** 2026-11-18

---

## Execution Notes

Clinic EMR returned HTTP 200 at `http://127.0.0.1:8080/patients`. The run used independent Python HTTP sessions on Windows and read-only PostgreSQL queries on port 54329. All patient writes went through the edit form. The test fixture was P00000251, UUID `126962ca-610d-4264-8118-e26cf4e61eaf`, initially active at version 7.

The five-client race used separate threads and a common release barrier. The long-edit case waited five actual minutes. Same-user tabs were simulated with two form copies in one session. Database checks compared all seven editable fields and inspected revision snapshots. The application recorded the shared actor `practice-user`; individual staff identities were outside this test.

The first worker stopped after TC6. TC7 used a fresh copy of the live v18 form held for the full interval. Cases 1–6 were taken from the first run's saved observations.

All 23 cases were attempted. TC19 has a failing acceptance status because its browser-history observation remains open. Its HTTP checks succeeded. TC21 times receipt of the response body, TC22 is theoretical as specified, and TC23 isolates one client's connection before delivery.

After the tests, an ordinary form save restored the original name, birth date, gender, phone, email, address and active status. The UUID, MRN and creation timestamp matched the baseline. PostgreSQL contained a revision for every version from 8 through 35, including restoration. The test edits remain in the practice audit trail.

## Test Environment

**Requirements:**
- Clinic EMR application running
- Access to http://127.0.0.1:8080/patients
- TWO separate browser windows or tabs (or two different browsers)
- Test patients created
- Network connection stable

**Pre-Test Setup:**
- [X] Application is running
- [X] Independent HTTP clients can open the same edit form simultaneously
- [X] Test patient P00000251 exists

---

## Test Objective

Verify that the version control system:
- Detects concurrent edits by multiple users
- Prevents data loss from overwriting newer changes
- Provides clear error messages
- Requires user to reload before retrying
- Increments version numbers correctly
- Maintains data integrity under concurrent access

---

## Critical Note

**This is the MOST IMPORTANT test for data integrity.** Version control prevents catastrophic data loss in multi-user environments. All test cases in this script should PASS for production readiness.

---

## Test Cases

### Test Case 1: Basic Concurrent Edit Conflict

**CRITICAL TEST**

**Setup:**
1. Open Browser A (e.g., Chrome)
2. Open Browser B (e.g., Firefox or another Chrome window)
3. In both browsers, navigate to the same patient detail page
4. In both browsers, click "Edit"

**Steps:**
1. **Browser A:** Change name to `User A Update`
2. **Browser B:** Change name to `User B Update`  
   *(Don't save yet in either browser)*
3. **Browser A:** Click "Save" **FIRST**
4. Wait for Browser A to complete and show success
5. **Browser B:** Click "Save" **SECOND**

**Expected Results:**
- ✅ **Browser A:** Save succeeds immediately
- ✅ **Browser A:** Redirected to patient detail page
- ✅ **Browser A:** Success message displayed
- ✅ **Browser A:** Name shows "User A Update"
- ❌ **Browser B:** Save FAILS
- ✅ **Browser B:** Error message displayed prominently
- ✅ **Browser B:** Error: "This patient changed after you opened the form. Your changes have not been saved. Reload the latest record before editing again."
- ✅ **Browser B:** Form retains "User B Update" (not lost)
- ✅ **Browser B:** No redirect (stays on edit form)
- ✅ Patient name in database is "User A Update" (first save wins)

**Actual Results:**

Both clients opened v7. A received 303, followed by the patient detail with the saved notice and User A Update at v8. B received 409 with the reload message, no redirect, and User B Update in its form. PostgreSQL retained A.

**Status:** [X] Pass / [ ] Fail
**Critical:** Must PASS

---

### Test Case 2: Reload After Conflict

**Continuing from Test Case 1:**

**Steps:**
1. In Browser B (which got conflict error)
2. Navigate back to patient list or detail page
3. Click on the patient again
4. Observe current name
5. Click "Edit" again
6. Change name to `User B Second Attempt`
7. Click "Save"

**Expected Results:**
- ✅ Patient detail shows "User A Update" (Browser A's change)
- ✅ Edit form shows "User A Update" as current name
- ✅ User B can now successfully save their changes
- ✅ Name changes to "User B Second Attempt"
- ✅ Save succeeds (no conflict this time)

**Actual Results:**

B saw A's name on detail and edit, reloaded v8, then saved User B Second Attempt (303, database v9).

**Status:** [X] Pass / [ ] Fail

---

### Test Case 3: Three-Way Concurrent Edit

**Setup:**
1. Open THREE browser windows (A, B, C)
2. All three navigate to same patient
3. All three click "Edit"

**Steps:**
1. Note patient version: 9
2. **Browser A:** Change phone to `555-0001`, Save
3. **Browser B:** Change phone to `555-0002`, Save (should fail)
4. **Browser C:** Change phone to `555-0003`, Save (should fail)

**Expected Results:**
- ✅ Browser A: Success (version N → N+1)
- ❌ Browser B: Conflict error
- ❌ Browser C: Conflict error
- ✅ Patient phone is `555-0001`

**Actual Results:**

All three opened v9. A returned 303, B and C each returned 409. PostgreSQL retained 555-0001 at v10.

**Status:** [X] Pass / [ ] Fail

---

### Test Case 4: Version Number Verification

**Steps:**
1. View patient, note version: V1 = 10
2. Browser A: Edit and save
3. Note new version: V2 = 11
4. Browser B: Open edit form (with old version)
5. Browser A: Edit and save again
6. Note new version: V3 = 12
7. Browser B: Try to save (has V1)

**Expected Results:**
- ✅ V2 = V1 + 1
- ✅ V3 = V2 + 1
- ❌ Browser B save fails (stale version V1, current is V3)

**Actual Results:**

Database versions were 10, 11, 12. B's original v10 received 409 after both commits.

**Status:** [X] Pass / [ ] Fail

---

### Test Case 5: Different Field Edits (Still Conflicts)

**Setup:**
1. Browser A and B both edit same patient

**Steps:**
1. **Browser A:** Change name only, Save
2. **Browser B:** Change phone only, Save (different field!)

**Expected Results:**
- ✅ Browser A: Success
- ❌ Browser B: Conflict error
- ✅ Version control checks entire record, not just changed fields
- ✅ Prevents any stale edits

**Actual Results:**

A's name-only save returned 303. B's phone-only save returned 409. At v13 the name changed and the phone remained 555-0001.

**Status:** [X] Pass / [ ] Fail

---

### Test Case 6: Rapid Sequential Edits (No Conflict)

**Scenario:** Single user editing rapidly

**Steps:**
1. Edit patient, change name to `Test 1`, Save
2. Immediately edit again, change to `Test 2`, Save
3. Immediately edit again, change to `Test 3`, Save
4. Do this 5 times rapidly

**Expected Results:**
- ✅ All saves succeed (no conflicts)
- ✅ Version increments: 1→2→3→4→5
- ✅ Final name is `Test 5`
- ✅ Single user doesn't conflict with themselves

**Actual Results:**

Five consecutive fresh-form saves returned 303 in 0.34 s. Database versions [14, 15, 16, 17, 18]; final name Test 5.

**Status:** [X] Pass / [ ] Fail

---

### Test Case 7: Long Edit Time

**Steps:**
1. **Browser A:** Open patient for editing
2. Wait 5 minutes (simulate user thinking/typing slowly)
3. **Browser B:** Edit same patient, Save immediately
4. **Browser A:** After 5 minutes, Save

**Expected Results:**
- ✅ Browser B: Success
- ❌ Browser A: Conflict error (version is stale after 5 min)
- ✅ Time elapsed doesn't bypass version check

**Actual Results:**

A held v18 for 318.53 s. B saved (303), then A received 409. Only B persisted at v19.

**Status:** [X] Pass / [ ] Fail

---

### Test Case 8: Version in Form Hidden Field

**Steps:**
1. Open patient for editing
2. Right-click → View Page Source
3. Search for `name="version"`

**Expected Results:**
- ✅ Hidden input field exists: `<input type="hidden" name="version" value="X">`
- ✅ Value matches current patient version
- ✅ This is how version is submitted

**Actual Results:**

Edit HTML contained a hidden input named version with value 19, matching patients.version.

**Status:** [X] Pass / [ ] Fail

---

### Test Case 9: Manual Version Manipulation (Security Test)

**Steps:**
1. Open patient (version 5) for editing
2. Open browser Developer Tools (F12)
3. Find hidden version input
4. Change value from 5 to 4
5. Save

**Expected Results:**
- ❌ Save fails (version 4 is old)
- ✅ Server validates version server-side
- ✅ Cannot bypass version check via client manipulation

**Actual Results:**

Submitted version 18 from a current v19 form. The server returned 409, retained the submitted draft, and left the entire patient row unchanged.

**Status:** [X] Pass / [ ] Fail

---

### Test Case 10: Conflict on Deactivation

**Steps:**
1. **Browser A:** Edit patient, change name
2. **Browser B:** Edit same patient, deactivate (uncheck Active)
3. **Browser B:** Save first
4. **Browser A:** Try to save

**Expected Results:**
- ✅ Browser B: Success (patient deactivated)
- ❌ Browser A: Conflict error
- ✅ Must reload to see deactivation before saving name change

**Actual Results:**

B cleared Active and saved (303, v20). A's stale name edit returned 409. Reload showed an unchecked Active box; the database retained the prior name and inactive status.

**Status:** [X] Pass / [ ] Fail

---

### Test Case 11: Error Message Clarity

**Steps:**
1. Trigger a concurrent edit conflict
2. Read the error message carefully

**Expected Results:**
- ✅ Error message is clear and specific
- ✅ Explains WHAT happened (patient was changed)
- ✅ Explains WHEN (after you opened the form)
- ✅ Explains WHAT TO DO (reload latest record)
- ✅ Error is prominently displayed (not easy to miss)
- ✅ Form data is retained (user's changes not lost)

**Actual Results:**

The 409 page placed an error alert before the form with a Reload latest record link. It said “This patient changed after you opened the form. Your changes have not been saved. Reload the latest record before editing again.” The draft remained in the fields. Retrying that form without reloading returned 409 again.

**Status:** [X] Pass / [ ] Fail

---

### Test Case 12: Form Data Retention on Conflict

**Steps:**
1. **Browser A:** Edit patient, make extensive changes:
   - Name, birth date, phone, email, address
2. **Browser B:** Edit same patient, save quick change
3. **Browser A:** Try to save (will conflict)
4. Observe Browser A's form

**Expected Results:**
- ✅ All of Browser A's changes still in form fields
- ✅ User can copy their changes if needed
- ✅ User doesn't lose work
- ✅ Can reload and re-enter

**Actual Results:**

The 409 form retained all seven submitted demographics, including the partial birth date, two-line address and checked Active box. Only B's phone was stored at v22. A reloaded, reapplied the draft and saved at v23.

**Status:** [X] Pass / [ ] Fail

---

### Test Case 13: Conflict After Browser Refresh

**Steps:**
1. **Browser A:** Edit patient (version 3)
2. **Browser B:** Edit and save (version→4)
3. **Browser A:** Refresh page (F5) WITHOUT saving
4. **Browser A:** Edit form reloads
5. Observe version number in hidden field

**Expected Results:**
- ✅ After refresh, version is 4 (latest)
- ✅ Form shows latest data
- ✅ Save will now succeed (no conflict)

**Actual Results:**

A held v23. A fresh GET of the edit URL returned B's phone and v24. A's next POST returned 303 and committed v25.

**Status:** [X] Pass / [ ] Fail

---

### Test Case 14: Database Transaction Atomicity

**Note:** Requires database access or monitoring

**Steps:**
1. During concurrent edit conflict
2. Check database `patients` table

**Expected Results:**
- ✅ Only the successful save (Browser A) is in database
- ✅ Failed save (Browser B) NOT in database
- ✅ No partial updates
- ✅ Data integrity maintained

**Actual Results:**

After 303/409, the v26 database row contained A's name and address. All seven fields matched A's submission, permanent identifiers were preserved, and the latest audit snapshot matched the entire row.

**Status:** [X] Pass / [ ] Fail

---

### Test Case 15: Audit Trail with Conflicts

**Note:** Requires database access

**Steps:**
1. Trigger concurrent edit (Browser A succeeds, B fails)
2. Check `patient_revisions` table

**Expected Results:**
- ✅ Revision created for Browser A's save
- ✅ NO revision created for Browser B's failed save
- ✅ Audit trail only records successful updates

**Actual Results:**

Revisions increased from 26 to 27. The only new entry was v27, actor practice-user, with A's full committed snapshot. B's rejected submission added zero entries.

**Status:** [X] Pass / [ ] Fail

---

### Test Case 16: Same User, Two Tabs

**Scenario:** One user with two browser tabs

**Steps:**
1. Open patient in Tab 1, click Edit
2. Open same patient in Tab 2, click Edit
3. Tab 1: Make changes, Save
4. Tab 2: Make different changes, Save

**Expected Results:**
- ✅ Tab 1: Success
- ❌ Tab 2: Conflict error
- ✅ Same protection even for same user

**Actual Results:**

Two open forms in one HTTP session shared v27. Tab 1 saved 555-1601 (303); tab 2 received 409 and retained 555-1602 in the form. The database remained at v28.

**Status:** [X] Pass / [ ] Fail

---

### Test Case 17: Conflict Error Status Code

**Steps:**
1. Trigger concurrent edit conflict
2. Check HTTP response (browser dev tools, Network tab)

**Expected Results:**
- ✅ HTTP Status: 409 Conflict
- ✅ Proper status code for conflict scenario

**Actual Results:**

The rejected POST returned HTTP 409 Conflict without redirection. The accepted edit remained at v29.

**Status:** [X] Pass / [ ] Fail

---

### Test Case 18: Validation Error vs. Version Conflict

**Steps:**
1. **Browser A:** Edit patient
2. **Browser B:** Edit same patient, Save successfully
3. **Browser A:** Clear name field (validation error) AND has stale version
4. **Browser A:** Save

**Expected Results:**
- ✅ Version conflict error OR validation error (either is acceptable)
- ✅ Prefer version conflict error (more critical)
- ✅ Both issues prevented

**Actual Results:**

The stale form with an empty name returned HTTP 422. B's valid name and full database row remained at v30. The name validation message took precedence, as allowed by this case.

**Status:** [X] Pass / [ ] Fail

---

### Test Case 19: Back Button After Conflict

**Steps:**
1. Trigger conflict in Browser B
2. Click browser Back button
3. Click Forward button

**Expected Results:**
- ✅ Error message still visible on forward
- ✅ Form data retained
- ✅ Browser history handles conflict state correctly

**Actual Results:**

The original 409 carried Cache-Control: no-store. Returning to the edit URL and replaying the original POST returned the 409 alert and draft again. The row stayed at v31. The HTTP clients could not observe a browser's Back/Forward display or a resubmission prompt.

**Status:** [ ] Pass / [X] Fail (browser acceptance evidence incomplete)

---

### Test Case 20: Stress Test - 5 Concurrent Users

**Setup:**
1. Open 5 browser windows (or use 5 actual devices/users if available)
2. All editing same patient

**Steps:**
1. All 5 browsers open edit form simultaneously
2. All 5 make different changes
3. All 5 click Save at nearly the same time

**Expected Results:**
- ✅ ONE save succeeds (first one to reach server)
- ❌ FOUR saves fail with conflict errors
- ✅ Data integrity maintained
- ✅ Final patient data reflects one complete save (not partial from multiple)

**Actual Results:**

Five independent clients fetched v31 and released their POSTs from a thread barrier within 1.02 ms (client-side start spread). Statuses in client order: [409, 409, 303, 409, 409]. Client 3's entire form matched the v32 row and audit snapshot. Each loser retained its own draft.

**Status:** [X] Pass / [ ] Fail

---

## Performance Tests

### Test Case 21: Conflict Detection Speed

**Steps:**
1. Trigger concurrent edit
2. Measure time for conflict error to appear

**Expected Results:**
- ✅ Error appears within 1-2 seconds
- ✅ Fast feedback

**Actual Results:**

0.0013 seconds from stale POST to receipt of the complete 409 HTML on loopback. This measures HTTP response time, excluding browser paint.

**Status:** [X] Pass / [ ] Fail

---

## Edge Cases

### Test Case 22: Version Number Overflow

**Note:** Theoretical test

**Question:** What happens at very high version numbers (e.g., version 999999)?

**Expected:**
- ✅ Version continues to increment
- ✅ No overflow (bigint type should handle large numbers)

**Actual Results:**

Read-only PostgreSQL arithmetic on the deployed patients.version type returned bigint, 999999 and 1000000. This verifies the script's theoretical boundary; a live patient was not advanced to that number.

**Status:** [X] Pass / [ ] Fail / [ ] Not Tested

---

### Test Case 23: Network Interruption During Save

**Steps:**
1. Start editing patient
2. Disconnect network (simulate)
3. Try to save
4. Reconnect network

**Expected Results:**
- ✅ Save fails with network error (not version conflict)
- ✅ Clear error message about connection
- ✅ Can retry when network restored

**Actual Results:**

The isolated client received a connection error (ProxyError, connection refused) before any HTTP response. PostgreSQL stayed at v33. Restoring connectivity and resubmitting the same draft returned 303 and committed v34. The error was observed at the HTTP client; browser-specific wording was outside this simulation.

**Status:** [X] Pass / [ ] Fail

---

## Test Results Summary

| Measure | Result |
|---|---:|
| Total cases attempted | 23 |
| Passed | 22 |
| Failed acceptance | 1 |
| Cases wholly untested | 0 |
| Pass rate (22 of 23) | 95.7% |

TC19 accounts for the one case without a pass. Its HTTP portion ran, and its browser portion remains unverified. Confirmed application failures in this suite: 0. All five designated critical cases (1, 3, 5, 9 and 20) passed.

## Critical Checks

| Check | Result | Evidence |
|---|---|---|
| Version control and conflict detection | Yes | Valid stale submissions returned HTTP 409, including an edit to a different field and an edit across deactivation. |
| Lost updates prevented | Yes | The five clients produced one accepted save and four conflicts. The winner's complete row and audit snapshot matched. |
| Unsaved form data preserved | Yes | The 409 form retained all seven editable fields, including the checkbox and multiline address. |
| Clear error and recovery | Yes | The response contained an alert and a reload link. Retrying the stale form failed again. Reloading and reapplying the draft succeeded. |
| Consecutive versions and data integrity | Yes | PostgreSQL showed consecutive revisions through restoration. Rejected submissions left the winning row intact. |

## Issues Found

| Issue | Case | Severity | Observation |
|---|---|---|---|
| Browser history acceptance remains open | 19 | Medium — verification gap | HTTP replay retained the alert and draft. Actual Back/Forward rendering, including any browser resubmission prompt, was unavailable in this run. No data loss was observed. |

## Production Readiness Assessment

**Concurrent edit protection passed the critical checks. Overall production sign-off: No.** Browser history recovery remains open. This run used a local practice database and a shared audit actor. It establishes how the application handles competing saves, while staff authentication, authorization and individual audit attribution need separate production evidence.

## Recommendations

Repeat TC19 in a supported staff browser. After the losing save, press Back, then Forward, and record whether a prompt appears, whether the conflict notice returns, and whether the exact draft survives.

The runnable suite is [run.py](uat-006-scripts/run.py), with cases 8–23 in [remaining.py](uat-006-scripts/remaining.py). Run it from the repository root with Python 3 and the requests package. It updates the designated practice fixture and restores its original demographics on normal completion.

## Sign-Off

**Tester:** Codex (automated HTTP UAT)
**Date completed:** 2026-11-18
**Disposition:** Critical concurrency checks passed. Production sign-off withheld pending the open evidence above.
