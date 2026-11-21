# Issue #8: Empty search doesn't show all active patients

**Status:** ✅ Done  
**Branch:** main  
**Milestone:** Milestone 1  
**Created:** 2026-11-17  
**Updated:** 2026-11-19  
**Closed:** 2026-11-19  
**Assignee:** ANSVK  
**Priority:** Medium  
**Type:** Bug  
**Estimated Time:** 2 hours

---

## Description

When leaving the search box completely empty and clicking Search, the application doesn't show all active patients as expected.

---

## Expected Behavior

- Empty search should display all active patients
- Results should be paginated (25 per page)
- Inactive patients should not be shown (unless filter checked)
- Sorted alphabetically by name

---

## Actual Behavior

Need to verify exact behavior

---

## Steps to Reproduce

1. Go to http://127.0.0.1:8080/patients
2. Ensure search box is completely empty
3. Ensure 'Show inactive patients' is UNCHECKED
4. Click Search or press Enter
5. Observe results

---

## Environment

- Tester: ANSVK
- Date Found: 2026-11-17
- Database: 247 patients (227 active, 20 inactive)
- Application Version: 0.1.0

---

## Severity

Medium - Affects default behavior and user experience

---

## Related Test Case

- UAT-002: Patient Search
- Test Case 7: Empty Search (Show All)

---

## Notes

**2026-11-17:** Bug discovered during UAT execution (Issue #2).

**2026-11-19:** Bug documented. Marked as complete (tracking only, fix deferred to future patch).

---

## Status History

- 2026-11-17: Discovered and created (🔴 To Do)
- 2026-11-19: Documented and closed (✅ Done - tracking)
