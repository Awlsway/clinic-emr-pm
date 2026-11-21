# Issue #7: Search fails with multiple consecutive spaces

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

When searching for a patient name with multiple consecutive spaces between words (e.g., 'John    Smith' with 4 spaces), the search returns no results.

---

## Expected Behavior

The search should handle multiple spaces gracefully:
- Trim leading/trailing spaces
- Treat multiple spaces as single space
- Return matching patients

---

## Actual Behavior

Returns no results / empty search results

---

## Steps to Reproduce

1. Go to http://127.0.0.1:8080/patients
2. In search box, type: 'John    Smith' (with 4 spaces between)
3. Click Search
4. Observe: No results returned

---

## Environment

- Tester: ANSVK
- Date Found: 2026-11-17
- Database: 247 patients
- Application Version: 0.1.0

---

## Severity

Medium - Affects user experience if users accidentally type extra spaces

---

## Workaround

Use single spaces between words

---

## Related Test Case

- UAT-002: Patient Search
- Test Case 13: Multiple Spaces Between Words

---

## Notes

**2026-11-17:** Bug discovered during UAT execution (Issue #2).

**2026-11-19:** Bug documented. Marked as complete (tracking only, fix deferred to future patch).

---

## Status History

- 2026-11-17: Discovered and created (🔴 To Do)
- 2026-11-19: Documented and closed (✅ Done - tracking)
