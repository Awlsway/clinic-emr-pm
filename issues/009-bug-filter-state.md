# Issue #9: Filter state not preserved in navigation link

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

When viewing a patient detail page after filtering for inactive patients, clicking the '← All patients' link resets the view to show only active patients instead of preserving the filter state.

---

## Expected Behavior

The 'All patients' link should preserve the filter state and return to /patients?inactive=1 when that was the previous list view.

---

## Actual Behavior

The link always goes to /patients (without query parameter), resetting to the default active-only view.

---

## Steps to Reproduce

1. Navigate to /patients?inactive=1 (show inactive patients)
2. Click on any patient to view their detail page
3. Click the '← All patients' link in the navigation
4. Observe: Filter is reset, only active patients are shown

---

## Workaround

Use the browser's Back button to return to the filtered list with state preserved.

---

## Environment

- Tester: ANSVK
- Date Found: 2026-11-17
- Application Version: 0.1.0

---

## Severity

Medium - Annoying but not blocking functionality

---

## Impact

- User Experience: Annoying but not blocking functionality
- Frequency: Occurs every time a user navigates from a filtered view
- Workaround Available: Yes (browser Back button works correctly)

---

## Technical Notes

- The native browser Back button preserves filter state correctly
- This issue affects only the in-app navigation link
- The URL parameter handling works correctly; the issue is with the link target

---

## Related Test Case

- UAT-004: Patient Deactivate
- Test Case 12: Filter State Persistence

---

## Notes

**2026-11-17:** Bug discovered during UAT execution (Issue #2).

**2026-11-19:** Bug documented. Marked as complete (tracking only, fix deferred to future patch).

---

## Status History

- 2026-11-17: Discovered and created (🔴 To Do)
- 2026-11-19: Documented and closed (✅ Done - tracking)
