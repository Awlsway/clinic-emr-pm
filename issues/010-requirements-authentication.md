# Issue #10: Requirements - Authentication

*Status:** ✅ Complete  
**Priority:** 🔴 Critical  
**Branch:** main  
**Milestone:** Milestone 2 - Authentication System  
**Issue Type:** Requirements (Phase 1)  
**Owner:** ANSVK  
**Created:** 2026-11-22  
**Updated:** 2026-11-22  
**Started:** 2026-11-22  
*Completed:** 2026-11-22

---

## Objective

Gather and document complete requirements for the authentication system including functional requirements, non-functional requirements, security requirements, and acceptance criteria.

---

## Description

This is Phase 1 (Requirements) of the SDLC for Milestone 2. The goal is to understand and document what the authentication system must do before moving to analysis and design phases.

**Scope:**
- Document functional requirements (what the system must do)
- Document non-functional requirements (performance, security, usability)
- Define security requirements specific to authentication
- Establish acceptance criteria for each requirement
- Identify constraints and assumptions

---

## Tasks

- [x] Review existing system architecture (Patient module, database)
- [x] Document functional requirements (login, logout, session management)
- [x] Document non-functional requirements (performance, security, compatibility)
- [x] Define password policy requirements
- [x] Define session management requirements
- [x] Document security requirements (bcrypt, secure cookies, HTTPS planning)
- [x] Establish acceptance criteria for each requirement
- [x] Review requirements with stakeholder (user)
- [x] Create requirements document in technical repo
- [x] Update this issue with findings
- [x] Mark as complete and move to Issue #11 (Analysis)

---

## Acceptance Criteria

- [ ] Requirements document created at `docs/requirements/authentication.md`
- [ ] All functional requirements documented with clear acceptance criteria
- [ ] Security requirements explicitly stated
- [ ] Password policy defined (length, complexity, hashing)
- [ ] Session management approach outlined
- [ ] Performance requirements specified (login time, logout time)
- [ ] Cross-platform compatibility addressed (Windows + Ubuntu)
- [ ] Requirements reviewed and approved by user
- [ ] No ambiguous or unclear requirements remain

---

## Deliverables

**PM Repository:**
- This issue document (complete)

**Technical Repository:**
- `docs/requirements/authentication.md` - Complete requirements specification

---

## Dependencies

- Milestone 1 complete ✅
- Patient module architecture understood ✅
- Database schema v2 in place ✅

---

## Blockers

None currently.

---

## Notes

**2026-11-22:** Issue created. Starting requirements gathering. This phase is critical - thorough requirements prevent rework in later phases. Security is paramount since this protects patient data access.

**Key Focus Areas:**
1. **Security first** - Authentication protects all patient data
2. **Clear password policy** - Balance security with usability
3. **Session management** - Secure, performant, reliable
4. **Cross-platform** - Must work identically on Windows and Ubuntu
5. **Future-proof** - Will support RBAC (Milestone 3) and production HTTPS (Milestone 4)

---

## Status History

- 2026-11-22: Created (🟡 In Progress)
- (To be updated as work progresses)

---

## Related Issues

- Issue #11: Analysis - Authentication Impact (next phase)
- Issue #12: Design - Authentication (follows analysis)
- Milestone 2: Authentication System

---

## Metrics

**Time Tracking:**
- Estimated: 1 day
- Actual: (to be recorded)

**Quality:**
- Requirements clarity: (to be assessed)
- Requirements completeness: (to be assessed)
- Security coverage: (to be assessed)
**2026-11-22 (Update):** Requirements document completed and approved as version 1.4 (976 lines). Went through 4 major revisions based on detailed user feedback:
- v1.0: Initial draft with basic requirements
- v1.1: Added account administration, CSRF protection, login throttling, timing attack protections
- v1.2: Added pre-login CSRF sessions, explicit timeout checks, password-change revocation
- v1.3: Fixed wording inconsistencies (throttling timing, queue size, bcrypt cost, timeout operators)
- v1.4 (Final): Explicit login processing sequence, sessions table with csrf_token field and nullable user_id, overall concurrency test

**Key Requirements Finalized:**
- 22 UAT test scenarios (15 security-critical requiring 100% pass, 7 functional requiring 95%+)
- Explicit 9-step login processing sequence (throttle → CSRF → bcrypt → status check)
- Bcrypt cost 12 (cost 11 exception with performance justification and approval)
- Sessions table supports both pre-login (user_id NULL) and authenticated sessions
- Pre-login sessions provide CSRF tokens before authentication
- Overall concurrency limit: 10 authentication requests, per-username limit: 3 total (1+2)
- Timeout boundaries: idle >= 30 minutes OR absolute >= 8 hours

**Security Highlights:**
- Username enumeration timing protection (dummy hash for unknown users)
- Account status checked after bcrypt (timing attack prevention)
- CSRF tokens stored server-side in sessions table
- Session revocation on account disable and password change
- Login throttling with resource exhaustion bounds

Issue complete. Ready to proceed to Issue #11 (Analysis phase).
