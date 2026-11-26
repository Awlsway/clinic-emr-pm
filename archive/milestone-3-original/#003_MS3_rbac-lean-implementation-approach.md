# Decision: RBAC Lean Implementation Approach with HL7/HIPAA Standards

**Date:** 2026-11-25  
**Branch:** main  
**Decision Maker:** ANSVK (PM/Developer)  
**Status:** ✅ Approved  
**Related Issue:** #015 (to be created - Requirements)  
**Related Milestone:** Milestone 3 - RBAC Staff Roles

---

## Context

Milestone 3 aims to implement Role-Based Access Control (RBAC) for the Clinic EMR system to secure access before production deployment with real patient data. The system currently has authentication (Milestone 2: user accounts, login, sessions) but no authorization (who can do what).

**Problem to Solve:**
- Need to restrict system access based on staff job functions
- Must comply with HIPAA Security Rule requirements
- Must follow healthcare industry standards (HL7, FHIR)
- Need balance between completeness and manageable implementation scope

**Key Questions:**
1. Should we implement all 7 roles at once or phased across milestones?
2. Which standards should guide our RBAC design?
3. How do we ensure HIPAA compliance?
4. What permission model should we use?

**Research Conducted:**
Comprehensive research on healthcare RBAC standards (documented in meeting 2026-11-25-milestone-3-kickoff.md):
- HIPAA Security Rule requirements
- HL7 FHIR RBAC specifications
- Industry best practices from healthcare IT vendors
- Typical healthcare role permission matrices

---

## Options Considered

### Option A: Split Across Milestones
**Description:** Implement RBAC infrastructure and Admin role in Milestone 3, defer 6 specialized roles to Milestone 4.

**Pros:**
- Smaller, more focused Milestone 3 (3-4 weeks)
- Can test infrastructure separately before adding complexity
- Allows production deployment with Admin-only access
- Easier to manage and track progress

**Cons:**
- Incomplete RBAC system in Milestone 3
- Cannot assign staff to appropriate roles until Milestone 4
- Two milestones needed for complete feature
- Potential rework when adding specialized roles

**Effort:** Milestone 3: 3-4 weeks, Milestone 4: 3-4 weeks (6-8 weeks total)

---

### Option B: All Roles At Once (Big Bang)
**Description:** Implement complete RBAC system with all 7 roles (Admin + 6 specialized) simultaneously from start.

**Pros:**
- Complete RBAC system delivered in one milestone
- All roles available immediately
- No follow-up milestone needed
- Comprehensive testing of all role interactions

**Cons:**
- Large, complex milestone (difficult to track)
- High cognitive load designing all 7 permission matrices simultaneously
- Longer time before any RBAC functionality delivered
- Higher risk of scope creep or delays

**Effort:** 8-10 weeks (single milestone but very large)

---

### Option C: Lean Approach Within Single Milestone (CHOSEN)
**Description:** Start lean (Admin + infrastructure first), validate it works, then expand to 6 specialized roles - all within Milestone 3.

**Implementation Phases:**
- **Part A (Weeks 1-3):** RBAC infrastructure + Admin role + testing
- **Part B (Weeks 4-6):** 6 specialized roles + permission matrix + testing  
- **Part C (Weeks 7-8):** Integration testing + documentation + RBAC policy

**Pros:**
- Complete RBAC system in one milestone (like Option B)
- Manageable phases with early validation (like Option A)
- Can test infrastructure before adding complexity
- Reduces risk through incremental building
- Follows industry best practice: "start simple, expand carefully"
- Clear internal checkpoints for progress tracking

**Cons:**
- Longer single milestone (6-8 weeks)
- Requires discipline to not skip validation between parts
- More complex milestone planning

**Effort:** 6-8 weeks (single milestone with internal phases)

---

## Decision

**Selected Option:** Option C - Lean Approach Within Single Milestone

**Rationale:**

1. **Completeness:** Delivers full RBAC system (all 7 roles) in Milestone 3
2. **Risk Management:** Validates infrastructure works before adding complexity
3. **Industry Alignment:** Follows best practice of "start simple, expand carefully"
4. **Testing Quality:** Can test Admin role thoroughly before adding 6 specialized roles
5. **Timeline Acceptable:** 6-8 weeks is acceptable (not in a rush)
6. **Clear Structure:** Part A/B/C provides clear internal milestones

**Standards Adopted:**

1. **HL7 FHIR RBAC Standards:**
   - CRUDE operations model (Create, Read, Update, Delete, Execute)
   - Resource-based permissions (operations on objects)
   - Reference: [FHIR v5.0.0 Security Specification](http://hl7.org/fhir/security.html)
   - Reference: [HL7 Healthcare Access Control Catalog](https://www.hl7.org/implement/standards/product_brief.cfm?product_id=72)

2. **HIPAA Security Rule Requirements:**
   - Unique user identification (already implemented in MS2)
   - Role-based access control
   - Minimum necessary standard (least privilege)
   - Audit controls (access logging)
   - Emergency access procedure (break-glass - future)
   - References: 45 CFR § 164.308(a)(3), § 164.308(a)(4), § 164.312(a)(1)

3. **Typical Healthcare Role Permissions:**
   - 7 roles: Admin, Doctor, Nurse, Pharmacist, Receptionist, Peer Counselor, Manager
   - Permission matrix based on industry research
   - "Need to know" principle for clinical data access

**Additional Decision:**
Create formal RBAC policy document (`docs/security/rbac-policy.md`) as compliance asset documenting HIPAA and HL7 requirements. This will be created during SDLC Phase 1 (Requirements).

---

## Impact

**Affected Areas:**
- **Architecture:** New middleware layer for permission checking, roles table, permissions system
- **Database:** Schema changes (roles, role assignments, permission audit log)
- **Timeline:** Milestone 3 extends to 6-8 weeks
- **Testing:** Need comprehensive permission testing for all 7 roles
- **Documentation:** RBAC policy document, role setup guides, compliance documentation
- **Compliance:** System becomes HIPAA-compliant for access control

**Changes Required:**
- Database migrations: roles table, user_roles junction, permission audit log
- Middleware: Permission checking on all routes
- Service layer: Permission validation in business logic  
- UI: Role assignment interface for administrators
- Documentation: RBAC policy, setup guides, compliance checklist
- Testing: Permission test cases for all 7 roles × CRUDE operations

**Risks:**
- **Scope Creep:** 6-8 weeks might extend if requirements unclear
  - Mitigation: Strict adherence to SDLC phases, clear requirement definition
- **Complexity:** 7 roles × multiple resources = large permission matrix
  - Mitigation: Lean approach validates infrastructure first
- **Testing Burden:** Comprehensive permission testing required
  - Mitigation: Automated permission tests, clear test matrix
- **HIPAA Compliance:** Must ensure all requirements met
  - Mitigation: RBAC policy document, compliance checklist, audit controls

**Dependencies:**
- Milestone 2 authentication system (users, sessions) - ✅ Complete
- RBAC policy document creation (Phase 1) - 🟡 Pending
- Permission matrix definition (Phase 1) - 🟡 Pending

---

## Implementation Plan

**Part A: Infrastructure + Admin Role (Weeks 1-3)**
- [ ] SDLC Phase 1: Requirements (RBAC overview, role definitions, permission matrix)
- [ ] SDLC Phase 2: Analysis (current state, RBAC approaches, impact assessment)
- [ ] SDLC Phase 3: Design (database schema, middleware architecture, API endpoints)
- [ ] Create RBAC policy document (`docs/security/rbac-policy.md`)
- [ ] SDLC Phase 4: Implementation (roles table, middleware, Admin role only)
- [ ] SDLC Phase 5: Testing (Admin role permission tests)
- [ ] **Checkpoint:** Admin role + infrastructure validated

**Part B: Specialized Roles (Weeks 4-6)**
- [ ] Implement 6 specialized roles (Doctor, Nurse, Pharmacist, Receptionist, Peer Counselor, Manager)
- [ ] Configure permission matrix for each role
- [ ] Implement role assignment UI
- [ ] Create role-specific tests
- [ ] **Checkpoint:** All 7 roles implemented

**Part C: Integration & Documentation (Weeks 7-8)**
- [ ] Integration testing (all roles working together)
- [ ] SDLC Phase 6: Deployment docs (setup guide, troubleshooting, role assignment procedures)
- [ ] SDLC Phase 7: Review (milestone retrospective, lessons learned)
- [ ] Update RBAC policy with implementation details
- [ ] **Checkpoint:** Milestone 3 complete

**Target Completion:** 2027-01-20 (8 weeks from 2026-11-25)

---

## Follow-Up

**Review Date:** 2027-01-20 (at Milestone 3 completion)  

**Success Criteria:**
- [ ] All 7 roles implemented and testable
- [ ] Permission checks enforced on all patient operations (CRUDE)
- [ ] Admin can assign users to roles
- [ ] Audit log captures all access control events
- [ ] RBAC policy document complete and accurate
- [ ] All HIPAA access control requirements met
- [ ] Role-based permissions tested and verified
- [ ] Documentation complete (setup, troubleshooting, compliance)

**Metrics:**
- Test pass rate: 95%+ for permission tests
- Code coverage: 80%+ for RBAC middleware
- Documentation completeness: All 7 sections of RBAC policy complete

---

## Notes

**Research Sources:**
All decisions based on authoritative healthcare RBAC standards:
- HIPAA Security Rule (45 CFR Part 164)
- HL7 FHIR v5.0.0 Security Specification
- HL7 Healthcare Access Control Catalog Release 3
- Industry best practices from healthcare IT vendors

**Key Insights from Research:**
1. Industry consensus: Start with least privilege, expand carefully
2. HIPAA requires audit logging for all PHI access
3. FHIR CRUDE model maps well to our existing patient operations
4. Emergency access (break-glass) can be deferred to future milestone

**Decision Context:**
- Not in a rush: 6-8 weeks acceptable for quality implementation
- Professional approach: Base on published standards, not personal preferences
- Compliance-first: RBAC policy document ensures audit-ready system
- Learning project: Demonstrating systematic SDLC approach

**Related Meeting:**
Full discussion and research findings documented in `meetings/2026-11-25-milestone-3-kickoff.md`

---

## Status History

- 2026-11-25: Proposed (kickoff meeting)
- 2026-11-25: Approved (ANSVK decision)
- Target Implementation: 2027-01-20 (Milestone 3 completion)