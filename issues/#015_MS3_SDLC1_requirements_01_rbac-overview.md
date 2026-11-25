# Issue #015: RBAC Overview Requirements

**Milestone:** Milestone 3 - RBAC Staff Roles  
**Phase:** SDLC Phase 1 - Requirements  
**Status:** ✅ Complete  
**Owner:** ANSVK  
**Created:** 2026-11-25  
**Started:** 2026-11-25  
**Completed:** 2026-11-25

---

## Objective

Document high-level requirements for the Role-Based Access Control (RBAC) system, including business objectives, regulatory compliance needs, and system-level requirements.

---

## Scope

**In Scope:**
- Business objectives for RBAC implementation
- Regulatory requirements (HIPAA, healthcare standards)
- High-level functional requirements
- Non-functional requirements (security, performance, auditability)
- Assumptions and constraints
- Success criteria for RBAC system

**Out of Scope:**
- Detailed permission matrix (covered in #017)
- Specific role definitions (covered in #016)
- Technical design details (Phase 3)
- Implementation specifics (Phase 4)

---

## Requirements

### Business Requirements

**BR-001: Secure Patient Data Access**
- **Description:** System must restrict access to patient data based on staff job functions
- **Rationale:** Protect patient privacy and comply with HIPAA minimum necessary standard
- **Priority:** Critical

**BR-002: Multiple Staff Roles**
- **Description:** System must support 7 distinct staff roles with different permission levels
- **Roles:** Admin, Doctor, Nurse, Pharmacist, Receptionist, Peer Counselor, Manager
- **Rationale:** Reflect real-world clinic organizational structure
- **Priority:** Critical

**BR-003: Role Assignment Management**
- **Description:** Administrators must be able to assign users to appropriate roles
- **Rationale:** Enable flexible staff management as personnel changes
- **Priority:** High

**BR-004: Audit Trail**
- **Description:** All access control decisions must be logged for compliance
- **Rationale:** HIPAA requires audit controls for PHI access
- **Priority:** Critical

---

### Regulatory Requirements

**REG-001: HIPAA Security Rule Compliance**
- **Standard:** 45 CFR § 164.308(a)(3) - Workforce Security
- **Requirement:** Implement procedures to ensure workforce members have appropriate access to PHI
- **Status:** Mandatory

**REG-002: HIPAA Access Control**
- **Standard:** 45 CFR § 164.312(a)(1) - Access Control
- **Requirement:** Implement technical policies that allow only authorized persons to access PHI
- **Status:** Mandatory

**REG-003: HIPAA Audit Controls**
- **Standard:** 45 CFR § 164.312(b) - Audit Controls
- **Requirement:** Implement hardware, software, and procedural mechanisms that record and examine activity
- **Status:** Mandatory

**REG-004: Minimum Necessary Standard**
- **Standard:** 45 CFR § 164.502(b) - Minimum Necessary
- **Requirement:** Limit PHI use/disclosure to minimum necessary to accomplish purpose
- **Status:** Mandatory

**REG-005: HL7 FHIR RBAC Standards**
- **Standard:** HL7 FHIR v5.0.0 Security Specification
- **Requirement:** Follow CRUDE operations model (Create, Read, Update, Delete, Execute)
- **Status:** Recommended (industry best practice)

**IMPORTANT NOTE ON HIPAA COMPLIANCE:**
This system is designed to be **HIPAA-aligned** by implementing the technical requirements of the HIPAA Security Rule. Full HIPAA **compliance certification** requires:
- External security audit by qualified assessor
- Organizational policies and procedures (beyond software)
- Business Associate Agreements (BAAs)
- Risk assessments and security management processes

This milestone delivers HIPAA-aligned software; organizational compliance is the clinic's responsibility.

---

### Functional Requirements

**FR-001: Permission Enforcement**
- **Description:** System must check permissions before allowing any operation on patient data
- **Requirement:** Every CRUDE operation must be permission-checked
- **Priority:** Critical

**FR-002: Role-Based Permissions**
- **Description:** Permissions must be assigned to roles, not individual users
- **Requirement:** User permissions determined by assigned role(s)
- **Priority:** Critical

**FR-003: Graceful Permission Denial**
- **Description:** When permission denied, system must provide clear error message without exposing system details
- **Requirement:** User-friendly error messages, security-conscious
- **Priority:** High

**FR-004: Admin Override**
- **Description:** Admin role must have full access to all system functions
- **Requirement:** Admin can perform all CRUDE operations on all resources
- **Priority:** Critical

**FR-005: Role Assignment UI**
- **Description:** Admin must have interface to assign/change user roles
- **Requirement:** Web-based UI for role management
- **Priority:** High

**FR-006: Documentation Requirements**
- **Description:** System must have complete documentation for deployment, operation, and compliance
- **Requirements:**
  - RBAC setup guide (how to configure roles and permissions)
  - Troubleshooting guide for common permission issues
  - HIPAA compliance checklist documenting how each requirement is met
  - API documentation for RBAC endpoints (if any)
- **Priority:** High

**FR-007: Existing User Migration**
- **Description:** System must handle existing users from Milestone 2 authentication
- **Requirements:**
  - Existing users assigned to default role during migration (Admin role initially)
  - Migration must not break existing authentication/sessions
  - Migration must be reversible/rollback-able
  - Clear migration procedure documented
- **Priority:** Critical

---

### Non-Functional Requirements

**NFR-001: Performance**
- **Description:** Permission checks must not significantly degrade system performance
- **Requirement:** <50ms overhead per operation for permission checking
- **Priority:** High

**NFR-002: Security**
- **Description:** RBAC system must be secure against common attacks
- **Requirements:**
  - No privilege escalation vulnerabilities
  - Session-based authorization (leverage Milestone 2 auth)
  - Fail-secure (deny by default)
- **Priority:** Critical

**NFR-003: Auditability**
- **Description:** All permission checks and denials must be auditable
- **Requirement:** Log all access control decisions with timestamp, user, action, outcome
- **Priority:** Critical

**NFR-004: Maintainability**
- **Description:** Permission matrix must be easy to update as requirements change
- **Requirement:** Configuration-based permissions stored in database/config files (not hardcoded in Go source code)
- **Note:** Permissions are configured at deployment/setup time, not runtime-editable via UI (initially)
- **Priority:** Medium

**NFR-005: Scalability**
- **Description:** RBAC system must support future expansion without major refactoring
- **Requirements:**
  - Architecture supports up to 20 roles
  - Design allows adding new resources (e.g., Prescription, Appointment, Visit)
  - Design allows adding new operations beyond CRUDE
  - Permission matrix extensible without code changes
- **Priority:** Medium

**NFR-006: Testability**
- **Description:** RBAC system must be thoroughly testable
- **Requirements:**
  - Unit tests for permission checking logic (80%+ code coverage)
  - Integration tests for each of the 7 roles
  - Automated permission matrix validation tests
  - 95%+ test pass rate required for milestone completion
- **Priority:** High

**NFR-007: Cross-Platform Compatibility**
- **Description:** RBAC system must work identically on all supported platforms
- **Requirements:**
  - Identical behavior on Windows and Ubuntu
  - No platform-specific permission logic
  - Database migrations work on both platforms
- **Priority:** High

---

## Assumptions

1. **Milestone 2 Complete:** Authentication system (users, sessions) is working and production-ready
2. **Single Role per User:** Each user assigned to exactly one role (no multi-role initially; future enhancement possible)
3. **Static Permission Configuration:** Permissions are stored in database/config (not hardcoded), but are configured at deployment time, not editable via UI (initially)
4. **Resource-Type Permissions:** Permissions apply to resource types (all patients), not individual resources (specific patient #123)
5. **Fictional Data:** System tested with fictional patient data (not real PHI during development)
6. **PostgreSQL:** Database platform remains PostgreSQL (no other DB support needed)
7. **Go Standard Library:** Leverage existing Go patterns from Milestone 1-2

---

## Constraints

1. **Timeline:** Part A (infrastructure + Admin) must complete in 3 weeks
2. **Resources:** Solo developer (ANSVK)
3. **Technology Stack:** Go, PostgreSQL, HTML/template (existing stack)
4. **No External Services:** No third-party RBAC services; implement in-house
5. **Backward Compatibility:** Must not break existing patient module functionality

---

## Success Criteria

- [x] All business requirements documented (BR-001 to BR-004)
- [x] All regulatory requirements identified with standard references (REG-001 to REG-005 + HIPAA caveat)
- [x] All functional requirements clear and testable (FR-001 to FR-007)
- [x] All non-functional requirements measurable (NFR-001 to NFR-007)
- [x] Assumptions validated and documented (7 assumptions)
- [x] Constraints identified and accepted (5 constraints)
- [x] Dependencies to next issues documented (#016-#022)
- [x] No contradictions in requirements (NFR-004/Assumption #3 clarified)
- [x] Requirements reviewed for completeness (validation performed 2026-11-25)
- [x] Requirements approved by project owner (ANSVK) ✅ **APPROVED 2026-11-25**

---

## Dependencies

**Completed:**
- ✅ Milestone 2: Authentication system (users, sessions, login)
- ✅ Milestone 3 kickoff: Research on RBAC standards complete

**Pending (Requirements Phase):**
- 🔴 Issue #016: Role definitions (7 roles with detailed responsibilities)
- 🔴 Issue #017: Permission matrix (7 roles × CRUDE operations × resources)
- 🔴 Issue #018: RBAC policy document (HIPAA/HL7 compliance documentation)

**Next Phase (Analysis):**
- 🔴 Issue #019: Current state analysis (review Milestone 2 authentication)
- 🔴 Issue #020: RBAC approaches (evaluate 3-4 implementation approaches)
- 🔴 Issue #021: Impact assessment (database, code, testing, documentation impacts)
- 🔴 Issue #022: Migration strategy (how to migrate existing users to RBAC)

**Note:** Requirements documented in #015-#018 will be validated and refined during analysis phase #019-#022.

---

## Deliverables

- [x] This requirements document
- [ ] Requirements review meeting notes (if needed)
- [ ] Updated milestone plan if requirements change scope

---

## Notes

**2026-11-25: Issue Created**
- Created as first requirements issue for Milestone 3
- Based on comprehensive RBAC research (HL7, HIPAA, industry practices)
- Research documented in meeting notes: `meetings/2026-11-25-milestone-3-kickoff.md`

**2026-11-25: Validation & Fixes Applied**
- Validated against Milestone 3 plan (100% alignment achieved)
- Fixed NFR-004/Assumption #3 contradiction
- Added FR-006: Documentation requirements
- Added FR-007: Existing user migration requirements
- Added NFR-006: Testability requirements (95%+ pass rate)
- Added NFR-007: Cross-platform compatibility requirements
- Clarified NFR-005: Specific scalability targets
- Added HIPAA compliance caveat (aligned vs certified)
- Updated dependencies to reference Analysis phase (#019-#022)

**Validation Results:**
- Success Criteria Coverage: 11/11 (100%)
- Part A Scope Alignment: 6/6 (100%)
- Standards Compliance: 4/4 (100%)
- Out-of-Scope Respect: 8/8 (100%)
- Decision Alignment: 5/5 (100%)
- Overall: PASS ✅

**2026-11-25: Approved by ANSVK**
- Requirements complete and validated
- Ready to proceed to Issue #016 (Role definitions)

---

## Status History

- 2026-11-25 09:00: Created (🔴 To Do)
- 2026-11-25 09:15: Started (🟡 In Progress)
- 2026-11-25 10:30: Validation performed, fixes applied
- 2026-11-25 10:45: Validated against milestone plan (PASS)
- 2026-11-25 11:00: Approved by ANSVK (✅ Complete)