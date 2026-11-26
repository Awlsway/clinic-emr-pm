# Milestone 3: RBAC - Staff Roles (Revised)

**Status:** 🟡 In Progress  
**Branch:** main  
**Start Date:** 2026-11-25  
**Target Date:** 2027-01-13 (7 weeks)  
**Completion Date:** TBD  
**Owner:** ANSVK (PM), Claude Code (Developer)

---

## Objective

Implement Role-Based Access Control (RBAC) system with 7 staff roles to secure EMR access based on job functions, ensuring HIPAA compliance and following HL7 FHIR standards. Build complete authorization layer on top of Milestone 2 authentication foundation.

---

## Success Criteria

- [ ] All 7 roles implemented (Admin, Doctor, Nurse, Pharmacist, Receptionist, Peer Counselor, Manager)
- [ ] CRUDE operations enforced (Create, Read, Update, Delete, Execute)
- [ ] Permission checks active on all patient operations
- [ ] Admin can assign users to roles via UI
- [ ] Audit logging captures all access control events
- [ ] RBAC policy document complete (`docs/security/rbac-policy.md`)
- [ ] All HIPAA access control requirements met
- [ ] Role-based permissions tested (95%+ pass rate)
- [ ] Documentation complete (setup guide, troubleshooting, compliance checklist)
- [ ] Works on Windows and Ubuntu
- [ ] Zero blocking security vulnerabilities

---

## Scope

**In Scope:**

**Phase 1: Foundation & Infrastructure**
- Database schema: roles table, user_roles junction, permission audit log
- RBAC middleware for route protection
- Permission checking service layer
- Admin role with full permissions
- RBAC policy document creation
- Audit logging for access control events

**Phase 2: Clinical & Support Roles**
- 4 specialized roles (Doctor, Nurse, Pharmacist, Receptionist)
- Permission matrix for each role (CRUDE × resources)
- Role-specific permission tests
- Integration testing

**Phase 3: Management Roles & UI**
- 2 specialized roles (Manager, Peer Counselor)
- Role assignment UI for administrators
- Integration with user management

**Phase 4: Integration & Delivery**
- Comprehensive integration testing (all 7 roles together)
- Security audit and performance testing
- Setup guides and troubleshooting docs
- HIPAA compliance checklist
- Milestone retrospective

**Standards & Compliance:**
- HL7 FHIR RBAC specifications (CRUDE operations model)
- HIPAA Security Rule (45 CFR § 164.308, § 164.312)
- Minimum necessary standard (least privilege)
- Typical healthcare role permissions (industry best practices)

**Out of Scope:**
- Multi-factor authentication (MFA) - Future
- Break-glass emergency access - Milestone 4
- Fine-grained resource-level permissions (e.g., per-patient access) - Future
- API token-based authorization - Future
- Role hierarchy (role inheritance) - Future
- Dynamic permission assignment - Future
- Audit log UI/reporting - Milestone 4
- Just-in-time privilege elevation - Future

---

## Issues (15 Total)

### **Phase 1: Foundation & Infrastructure (Issues #017-#020) - Weeks 1-2**

| # | Issue | Duration | Status | Owner |
|---|-------|----------|--------|-------|
| #017 | Permission Matrix & RBAC Policy | 2 days | 🔴 To Do | Claude Code |
| #018 | Technical Design - Infrastructure | 2 days | 🔴 To Do | Claude Code |
| #019 | Implement RBAC Infrastructure | 3 days | 🔴 To Do | Claude Code |
| #020 | Admin Role Implementation & Testing | 3 days | 🔴 To Do | Claude Code |

**Deliverables:**
- Complete permission matrix for all 7 roles
- RBAC policy document (`docs/security/rbac-policy.md`)
- Database schema (roles, user_roles, audit_log tables)
- RBAC middleware implemented
- Permission checking service functional
- Admin role working and tested

**Checkpoint A:** Infrastructure validated, Admin role functional

---

### **Phase 2: Clinical & Support Roles (Issues #021-#025) - Weeks 3-4**

| # | Issue | Duration | Status | Owner |
|---|-------|----------|--------|-------|
| #021 | Doctor Role | 1.5 days | 🔴 To Do | Claude Code |
| #022 | Nurse Role | 1.5 days | 🔴 To Do | Claude Code |
| #023 | Pharmacist Role | 1.5 days | 🔴 To Do | Claude Code |
| #024 | Receptionist Role | 1.5 days | 🔴 To Do | Claude Code |
| #025 | 4-Role Integration Testing | 1 day | 🔴 To Do | Claude Code |

**Deliverables:**
- Doctor role: Full clinical CRUDE access
- Nurse role: Limited clinical CRUDE access
- Pharmacist role: Prescription access
- Receptionist role: Demographics + limited clinical
- Integration tests for all 5 roles (Admin + 4 specialized)

**Checkpoint B:** 5 roles working (Admin + 4 specialized)

---

### **Phase 3: Management Roles & UI (Issues #026-#028) - Week 5**

| # | Issue | Duration | Status | Owner |
|---|-------|----------|--------|-------|
| #026 | Manager Role | 1.5 days | 🔴 To Do | Claude Code |
| #027 | Peer Counselor Role | 1.5 days | 🔴 To Do | Claude Code |
| #028 | Role Assignment UI | 2 days | 🔴 To Do | Claude Code |

**Deliverables:**
- Manager role: Read-all data, user management
- Peer Counselor role: Assigned patients only
- Role assignment UI for administrators
- Workflow tested (create user → assign role → verify permissions)

**Checkpoint C:** All 7 roles implemented + assignment UI functional

---

### **Phase 4: Integration & Delivery (Issues #029-#031) - Weeks 6-7**

| # | Issue | Duration | Status | Owner |
|---|-------|----------|--------|-------|
| #029 | Comprehensive Integration Testing | 2 days | 🔴 To Do | Claude Code |
| #030 | Deployment Documentation | 2 days | 🔴 To Do | Claude Code |
| #031 | Milestone Retrospective | 0.5 days | 🔴 To Do | ANSVK + Claude Code |

**Deliverables:**
- Integration tests (all 7 roles together)
- Security audit report
- Performance baseline metrics
- Setup guide (`docs/deployment/rbac-setup-guide.md`)
- Troubleshooting guide
- HIPAA compliance checklist
- Milestone retrospective document

**🎯 Milestone 3 Complete**

---

## Timeline

**Total Duration:** 7 weeks (2026-11-25 to 2027-01-13)  
**Working Days:** ~35 days  
**Issues:** 15  
**Average:** ~2.3 days per issue

**Weekly Breakdown:**
- **Weeks 1-2:** Foundation & Infrastructure (4 issues)
- **Weeks 3-4:** Clinical & Support Roles (5 issues)
- **Week 5:** Management Roles & UI (3 issues)
- **Weeks 6-7:** Integration & Delivery (3 issues)

---

## Deliverables

**Documentation:**
- [ ] RBAC policy document (`docs/security/rbac-policy.md`)
- [ ] Permission matrix spreadsheet (7 roles × CRUDE operations)
- [ ] Setup guide (`docs/deployment/rbac-setup-guide.md`)
- [ ] Troubleshooting guide
- [ ] HIPAA compliance checklist
- [ ] API documentation for RBAC endpoints

**Code:**
- [ ] Database migrations (roles, user_roles, audit_log tables)
- [ ] RBAC middleware package
- [ ] Permission checking service
- [ ] Role assignment UI
- [ ] 7 role configurations with permission matrices

**Testing:**
- [ ] Unit tests for RBAC middleware (80%+ coverage)
- [ ] Permission tests for all 7 roles
- [ ] Integration tests (roles working together)
- [ ] Audit logging tests
- [ ] UAT scripts for each role

---

## Risks

| Risk | Severity | Probability | Mitigation |
|------|----------|-------------|------------|
| **Timeline slippage** - 7 weeks might extend with single developer | Medium | Medium | Early warning if falling behind, weekly checkpoint reviews |
| **Permission matrix complexity** - 7 roles × multiple operations | Medium | High | Use spreadsheet for clarity, automated tests catch errors |
| **HIPAA compliance gaps** - Missing requirements | High | Low | RBAC policy document as checklist, compliance review in Phase 1 |
| **Performance impact** - Permission checks on every operation | Medium | Low | Efficient middleware design, performance testing in Phase 4 |
| **Integration issues** - Roles conflicting | Low | Medium | Integration testing after each phase |

---

## Dependencies

**Internal Dependencies:**
- **Milestone 2 (Authentication)** - ✅ Complete
  - User accounts and authentication working
  - Session management operational
  - Database schema ready

**External Dependencies:**
- None (all implementation internal)

**Knowledge Dependencies:**
- HL7 FHIR RBAC specifications (researched ✅)
- HIPAA Security Rule requirements (researched ✅)
- Healthcare role permissions best practices (researched ✅)

---

## Metrics

**Code Quality:**
- Test pass rate: **95%+** for permission tests
- Code coverage: **80%+** for RBAC middleware and permission checking
- Zero critical security vulnerabilities

**Functionality:**
- All 7 roles operational
- All CRUDE operations permission-checked
- Audit log capturing 100% of access events

**Compliance:**
- All HIPAA access control requirements met (checklist 100%)
- HL7 FHIR RBAC standards followed (CRUDE model implemented)

**Timeline:**
- Complete within 7 weeks (target: 2027-01-13)
- Weekly checkpoints met (3 checkpoints: Phases 1, 2, 3)

---

## Notes

### **2026-11-25: Milestone Kickoff**

**Key Decisions:**
- Lean approach within single milestone (start Admin, expand to 6 specialized roles)
- Reference HL7 FHIR CRUDE model and HIPAA requirements
- Create RBAC policy document as compliance asset
- Original plan: 10 weeks with 51 issues

**Completed Work:**
- ✅ Issue #015: RBAC overview requirements (23 requirements documented)
- ✅ Issue #016: Role definitions (7 roles fully defined)

### **2026-11-26: Milestone Plan Revised**

**Changes Made:**
- **Revised from 51 issues to 15 issues** (70% reduction)
- **Reduced timeline from 10 weeks to 7 weeks** (30% reduction)
- **Combined SDLC phases:**
  - Requirements + Analysis → Planning phase
  - Design + Implementation + Testing → Feature delivery
- **Each role = 1 issue** (design + implement + test combined)
- **Solo developer context:** PM (ANSVK) + Developer (Claude Code)

**Rationale:**
- Original 51-issue plan designed for team-based waterfall SDLC
- Solo AI developer context requires streamlined approach
- Reduced PM overhead: ~1-2 hours per issue review vs creating 51 detailed specs
- Feature-complete iterations: each issue delivers working functionality
- Maintained quality: testing, HIPAA compliance, documentation preserved

**PM Methodology Learning:**
- Original approach: "Process Over Speed" → excessive ceremony
- Revised approach: "Process Enables Autonomy" → right-sized for context
- Key lesson: Tailor methodology to team size and composition

**Related Documents:**
- Original plan archived: `archive/milestone-3-original/milestone-3.md`
- Original decision archived: `archive/milestone-3-original/#003_MS3_rbac-lean-implementation-approach.md`
- Revised decision: `decisions/#004_MS3_revised-streamlined-approach.md`
- Research: `meetings/2026-11-25-milestone-3-kickoff.md`

---

## Retrospective (Complete after milestone done)

**What Went Well:**
- TBD

**What Could Be Improved:**
- TBD

**Lessons Learned:**
- TBD

**Action Items for Next Milestone:**
- TBD

---

## Status History

- 2026-11-25: Created original plan (51 issues, 10 weeks)
- 2026-11-25: Issues #015-#016 completed
- 2026-11-26: Revised to streamlined plan (15 issues, 7 weeks) - 🟡 In Progress
- Target: 2027-01-13 (7 weeks) - Milestone completion
