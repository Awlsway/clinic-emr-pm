# Issue #017: Conflict Resolution

**Date:** 2026-11-26  
**Reviewer:** ANSVK (via Fable 5.2 subagent)  
**Documents Reviewed:** rbac-permission-matrix.md, rbac-policy.md

---

## Conflicts Identified

### Conflict #1: Doctor Prescription Permissions (HIGH PRIORITY)

**Issue:** Role summaries stated "Full CRUDE on prescriptions" but detailed matrix showed only CRU (no Delete, no Execute)

**Location:**
- `rbac-policy.md` line 258
- `rbac-permission-matrix.md` line 233

**Root Cause:** Summary text did not match detailed permission matrix table

**Resolution:**
- Changed from: "Full CRUDE on clinical data, prescriptions"
- Changed to: "Full CRUDE on clinical data; CRU on prescriptions (cannot delete or execute)"

**Verification:** Both documents now consistently show Doctor has CRU on prescriptions, matching the detailed matrix showing ✓Create, ✓Read, ✓Update, ✗Delete, ✗Execute

---

### Conflict #2: Receptionist Clinical Access (MEDIUM PRIORITY)

**Issue:** Policy stated "limited clinical read access (identity verification only)" but permission matrix showed zero access to Medical History & Clinical Notes

**Location:**
- `rbac-policy.md` lines 320-322

**Root Cause:** Ambiguous language suggested some clinical access when matrix showed none

**Resolution:**
- Changed from: "limited clinical read access (identity verification only)"
- Changed to: "NO clinical data access"
- Changed from: "Cannot access detailed clinical notes"
- Changed to: "Cannot access clinical notes or medical history"

**Verification:** Policy now clearly states zero clinical access, matching permission matrix showing ✗✗✗✗✗ for Medical History & Clinical Notes

---

## Verification Results

**Post-Fix Consistency Check:**
- ✅ Doctor permissions: Both documents show "CRU on prescriptions"
- ✅ Receptionist permissions: Both documents show "NO clinical data access"
- ✅ Summary table (rbac-policy.md line 202): Shows "CRU" for Doctor prescriptions
- ✅ No remaining instances of conflicting language found

---

## Lessons Learned

**Root Cause Analysis:**
1. Summary sections were written independently of detailed matrix
2. "Full CRUDE" language was used loosely without checking against actual permission assignments
3. "Limited clinical read access" was ambiguous (demographics vs clinical notes)

**Prevention for Future Issues:**
1. Always write summaries AFTER completing detailed matrices
2. Use grep/search to verify consistency of key phrases across documents
3. Avoid ambiguous terms like "limited access" - be explicit (YES/NO, specific operations)
4. Have subagent review for conflicts before final approval

---

## Sign-Off

**Conflicts Resolved By:** Claude Code  
**Verified By:** ANSVK  
**Date:** 2026-11-26  
**Status:** ✅ Resolved - Documents now consistent
