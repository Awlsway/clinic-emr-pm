# Milestone 3: RBAC - Staff Roles

**Status:** 🔴 Not Started  
**Branch:** main  
**Start Date:** 2026-11-25  
**Target Date:** 2027-02-03  
**Completion Date:** TBD  
**Owner:** ANSVK

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

**Part A: Infrastructure + Admin Role (Weeks 1-3)**
- Database schema: roles table, user_roles junction, permission audit log
- RBAC middleware for route protection
- Permission checking service layer
- Admin role with full permissions
- RBAC policy document creation
- Audit logging for access control events

**Part B: Specialized Roles (Weeks 4-6)**
- 6 specialized roles (Doctor, Nurse, Pharmacist, Receptionist, Peer Counselor, Manager)
- Permission matrix for each role (CRUDE × resources)
- Role assignment UI for administrators
- Role-specific permission tests

**Part C: Integration & Documentation (Weeks 7-8)**
- Integration testing (all 7 roles together)
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

## Issues

**Part A: Infrastructure + Admin Role (19 issues)**

**Week 1: Requirements & Analysis**

| # | Issue | Status | Progress | Owner |
|---|-------|--------|----------|-------|
| #015 | SDLC1_requirements_01_rbac-overview | 🔴 To Do | 0% | ANSVK |
| #016 | SDLC1_requirements_02_role-definitions | 🔴 To Do | 0% | ANSVK |
| #017 | SDLC1_requirements_03_permission-matrix | 🔴 To Do | 0% | ANSVK |
| #018 | SDLC1_requirements_04_rbac-policy-doc | 🔴 To Do | 0% | ANSVK |
| #019 | SDLC2_analysis_01_current-state | 🔴 To Do | 0% | ANSVK |
| #020 | SDLC2_analysis_02_rbac-approaches | 🔴 To Do | 0% | ANSVK |
| #021 | SDLC2_analysis_03_impact-assessment | 🔴 To Do | 0% | ANSVK |
| #022 | SDLC2_analysis_04_migration-strategy | 🔴 To Do | 0% | ANSVK |

**Week 2: Design**

| # | Issue | Status | Progress | Owner |
|---|-------|--------|----------|-------|
| #023 | SDLC3_design_01_database-schema | 🔴 To Do | 0% | ANSVK |
| #024 | SDLC3_design_02_middleware-architecture | 🔴 To Do | 0% | ANSVK |
| #025 | SDLC3_design_03_permission-checking-service | 🔴 To Do | 0% | ANSVK |
| #026 | SDLC3_design_04_admin-permissions-matrix | 🔴 To Do | 0% | ANSVK |
| #027 | SDLC3_design_05_audit-logging-spec | 🔴 To Do | 0% | ANSVK |
| #028 | SDLC3_design_06_api-endpoints | 🔴 To Do | 0% | ANSVK |

**Week 3: Implementation + Testing Admin**

| # | Issue | Status | Progress | Owner |
|---|-------|--------|----------|-------|
| #029 | SDLC4_implementation_01_database-migrations | 🔴 To Do | 0% | ANSVK |
| #030 | SDLC4_implementation_02_rbac-middleware | 🔴 To Do | 0% | ANSVK |
| #031 | SDLC4_implementation_03_admin-role-config | 🔴 To Do | 0% | ANSVK |
| #032 | SDLC5_testing_01_admin-permissions | 🔴 To Do | 0% | ANSVK |
| #033 | SDLC5_testing_02_audit-logging | 🔴 To Do | 0% | ANSVK |

**Checkpoint A:** Admin role working, infrastructure validated, commit + push

---

**Part B: 6 Specialized Roles (24 issues)**

**Week 4: Doctor + Nurse**

| # | Issue | Status | Progress | Owner |
|---|-------|--------|----------|-------|
| #034 | SDLC3_design_07_doctor-permissions-matrix | 🔴 To Do | 0% | ANSVK |
| #035 | SDLC4_implementation_04_doctor-role-config | 🔴 To Do | 0% | ANSVK |
| #036 | SDLC5_testing_03_doctor-permissions | 🔴 To Do | 0% | ANSVK |
| #037 | SDLC3_design_08_nurse-permissions-matrix | 🔴 To Do | 0% | ANSVK |
| #038 | SDLC4_implementation_05_nurse-role-config | 🔴 To Do | 0% | ANSVK |
| #039 | SDLC5_testing_04_nurse-permissions | 🔴 To Do | 0% | ANSVK |
| #040 | SDLC5_testing_05_doctor-nurse-integration | 🔴 To Do | 0% | ANSVK |

**Week 5: Pharmacist + Receptionist**

| # | Issue | Status | Progress | Owner |
|---|-------|--------|----------|-------|
| #041 | SDLC3_design_09_pharmacist-permissions-matrix | 🔴 To Do | 0% | ANSVK |
| #042 | SDLC4_implementation_06_pharmacist-role-config | 🔴 To Do | 0% | ANSVK |
| #043 | SDLC5_testing_06_pharmacist-permissions | 🔴 To Do | 0% | ANSVK |
| #044 | SDLC3_design_10_receptionist-permissions-matrix | 🔴 To Do | 0% | ANSVK |
| #045 | SDLC4_implementation_07_receptionist-role-config | 🔴 To Do | 0% | ANSVK |
| #046 | SDLC5_testing_07_receptionist-permissions | 🔴 To Do | 0% | ANSVK |
| #047 | SDLC5_testing_08_all-4-roles-integration | 🔴 To Do | 0% | ANSVK |

**Week 6: Peer Counselor + Manager + UI**

| # | Issue | Status | Progress | Owner |
|---|-------|--------|----------|-------|
| #048 | SDLC3_design_11_peer-counselor-permissions-matrix | 🔴 To Do | 0% | ANSVK |
| #049 | SDLC4_implementation_08_peer-counselor-role-config | 🔴 To Do | 0% | ANSVK |
| #050 | SDLC5_testing_09_peer-counselor-permissions | 🔴 To Do | 0% | ANSVK |
| #051 | SDLC3_design_12_manager-permissions-matrix | 🔴 To Do | 0% | ANSVK |
| #052 | SDLC4_implementation_09_manager-role-config | 🔴 To Do | 0% | ANSVK |
| #053 | SDLC5_testing_10_manager-permissions | 🔴 To Do | 0% | ANSVK |
| #054 | SDLC3_design_13_role-assignment-ui-spec | 🔴 To Do | 0% | ANSVK |
| #055 | SDLC4_implementation_10_role-assignment-ui | 🔴 To Do | 0% | ANSVK |
| #056 | SDLC5_testing_11_role-assignment-ui | 🔴 To Do | 0% | ANSVK |
| #057 | SDLC5_testing_12_all-7-roles-matrix-verify | 🔴 To Do | 0% | ANSVK |

**Checkpoint B:** All 7 roles working, assignment UI functional, commit + push

---

**Part C: Integration + Documentation (8 issues)**

**Week 7: Integration Testing**

| # | Issue | Status | Progress | Owner |
|---|-------|--------|----------|-------|
| #058 | SDLC5_testing_13_integration-all-roles | 🔴 To Do | 0% | ANSVK |
| #059 | SDLC5_testing_14_performance-baseline | 🔴 To Do | 0% | ANSVK |
| #060 | SDLC5_testing_15_security-permissions-audit | 🔴 To Do | 0% | ANSVK |

**Week 8: Deployment Documentation**

| # | Issue | Status | Progress | Owner |
|---|-------|--------|----------|-------|
| #061 | SDLC6_deployment_01_setup-guide | 🔴 To Do | 0% | ANSVK |
| #062 | SDLC6_deployment_02_troubleshooting | 🔴 To Do | 0% | ANSVK |
| #063 | SDLC6_deployment_03_compliance-checklist | 🔴 To Do | 0% | ANSVK |

**Weeks 9-10: Review + Buffer**

| # | Issue | Status | Progress | Owner |
|---|-------|--------|----------|-------|
| #064 | SDLC7_review_01_milestone-retrospective | 🔴 To Do | 0% | ANSVK |
| #065 | SDLC7_review_02_lessons-learned | 🔴 To Do | 0% | ANSVK |

**Week 10:** Buffer/contingency time for overruns or unexpected issues

**Checkpoint C:** Milestone 3 complete, final commit + push

---

**Total Issues:** 51 (+23 from original plan)  
**Completed:** 0 (0%)  
**In Progress:** 0  
**To Do:** 51

**Granularity Improvements:**
- Added RBAC policy document issue (#018)
- Added migration strategy issue (#022)
- Each role now has 3 issues (design → implement → test)
- Added integration testing after each role set
- Added performance and security explicit testing
- Added lessons learned as separate issue

---

## Timeline

**Part A: Infrastructure + Admin Role (Weeks 1-3)**

**Week 1: Requirements & Analysis (8 issues)**
- Days 1-3: Requirements (#015-#018) including RBAC policy doc initiation
- Days 4-5: Analysis (#019-#022) including migration strategy
- Deliverable: Complete requirements, RBAC policy started, migration plan

**Week 2: Design (6 issues)**
- Days 1-2: Database schema + middleware (#023-#024)
- Days 3-4: Permission service + admin matrix (#025-#026)
- Day 5: Audit logging + API endpoints (#027-#028)
- Deliverable: Technical design complete, ready to implement

**Week 3: Implementation + Testing Admin (5 issues)**
- Days 1-2: Database migrations + middleware (#029-#030)
- Day 3: Admin role configuration (#031)
- Days 4-5: Test admin permissions + audit (#032-#033)
- **Checkpoint A:** Admin role validated, infrastructure working, commit + push Part A

---

**Part B: Specialized Roles (Weeks 4-6)**

**Week 4: Doctor + Nurse (7 issues)**
- Day 1: Design Doctor permissions (#034)
- Day 2: Implement + test Doctor (#035-#036)
- Day 3: Design Nurse permissions (#037)
- Day 4: Implement + test Nurse (#038-#039)
- Day 5: Integration test Doctor + Nurse (#040)
- Deliverable: 2 clinical roles working together

**Week 5: Pharmacist + Receptionist (7 issues)**
- Day 1: Design Pharmacist permissions (#041)
- Day 2: Implement + test Pharmacist (#042-#043)
- Day 3: Design Receptionist permissions (#044)
- Day 4: Implement + test Receptionist (#045-#046)
- Day 5: Integration test 4 roles (#047)
- Deliverable: 4 roles working together (Admin + Doctor + Nurse + Pharmacist + Receptionist)

**Week 6: Peer Counselor + Manager + UI (10 issues)**
- Day 1: Design Peer Counselor + Manager permissions (#048, #051)
- Day 2: Implement Peer Counselor + Manager (#049, #052)
- Day 3: Test Peer Counselor + Manager (#050, #053)
- Day 4: Design + implement role assignment UI (#054-#055)
- Day 5: Test UI + all 7 roles matrix (#056-#057)
- **Checkpoint B:** All 7 roles working, UI functional, commit + push Part B

---

**Part C: Integration + Documentation (Weeks 7-10)**

**Week 7: Integration Testing (3 issues)**
- Days 1-2: Integration testing all roles (#058)
- Day 3: Performance baseline testing (#059)
- Days 4-5: Security permissions audit (#060)
- Deliverable: Verified all roles work correctly together, performance acceptable

**Week 8: Deployment Documentation (3 issues)**
- Days 1-2: Setup guide (#061)
- Days 2-3: Troubleshooting guide (#062)
- Days 4-5: Compliance checklist (#063)
- Deliverable: Complete deployment documentation

**Week 9: Review (2 issues)**
- Days 1-3: Milestone retrospective (#064)
- Days 4-5: Document lessons learned (#065)
- Deliverable: Retrospective complete, lessons captured

**Week 10: Buffer/Contingency**
- Reserved for any overruns from Weeks 1-9
- Bug fixes discovered during testing
- Documentation polish
- Unexpected complexity

**Checkpoint C:** Milestone 3 complete, final commit + push

---

**Total Duration:** 10 weeks (2026-11-25 to 2027-02-03)  
**Working Days:** 50 days  
**Issues:** 51  
**Average:** ~1 issue per day (some days multiple small issues, some days one complex issue)

---

## Deliverables

**Documentation:**
- [ ] RBAC policy document (`docs/security/rbac-policy.md`) - 7 sections, HIPAA/HL7 requirements
- [ ] Permission matrix spreadsheet (7 roles × CRUDE operations)
- [ ] Setup guide (`docs/deployment/rbac-setup-guide.md`)
- [ ] Troubleshooting guide (`docs/deployment/rbac-troubleshooting.md`)
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
| **Scope creep** - 28 issues could expand | High | Medium | Strict SDLC adherence, clear requirements in Phase 1, no feature additions mid-milestone |
| **Permission matrix complexity** - 7 roles × multiple operations | Medium | High | Start with Admin only (Part A validation), use permission matrix spreadsheet, automated tests |
| **Testing burden** - Comprehensive testing needed | Medium | High | Automated permission tests, clear test matrix, dedicate full Week 6 to testing |
| **HIPAA compliance gaps** - Missing requirements | High | Low | RBAC policy document as checklist, compliance review in Phase 2, external audit (future) |
| **Performance impact** - Permission checks on every operation | Medium | Low | Efficient middleware design, caching (if needed), performance testing in Week 7 |
| **Timeline overrun** - 8 weeks might extend | Medium | Medium | Weekly checkpoints, early warning if falling behind, adjust scope if needed |
| **Integration issues** - Roles conflicting | Low | Medium | Integration testing in Week 7, test all role combinations |

---

## Dependencies

**Internal Dependencies:**
- **Milestone 2 (Authentication)** - ✅ Complete
  - User accounts and authentication working
  - Session management operational
  - Database schema v3 ready

**External Dependencies:**
- None (all implementation internal)

**Knowledge Dependencies:**
- HL7 FHIR RBAC specifications (researched ✅)
- HIPAA Security Rule requirements (researched ✅)
- Healthcare role permissions best practices (researched ✅)

**Milestone Dependencies:**
- Milestone 2 must be complete - ✅ Done (2026-11-25)
- No blocking dependencies for this milestone

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

**Documentation:**
- RBAC policy: 7 sections complete
- Setup guide: Covers all 7 roles
- Troubleshooting: Common issues documented

**Timeline:**
- Complete within 8 weeks (target: 2027-01-20)
- Weekly checkpoints met (3 checkpoints: Parts A, B, C)

---

## Notes

**2026-11-25: Milestone Kickoff**

**Key Decisions:**
- Lean approach within single milestone (start Admin, expand to 6 specialized roles)
- Reference HL7 FHIR CRUDE model and HIPAA requirements
- Create RBAC policy document as compliance asset
- 10-week timeline with 2-week buffer (not in a rush)

**2026-11-25: Milestone Plan Revised (Post-PM Review)**

**Changes Made:**
- Extended timeline from 8 to 10 weeks (added 2-week buffer for realism)
- Increased granularity from 28 to 51 issues (better tracking)
- Each specialized role now has 3 issues (design → implement → test)
- Added explicit issues for: RBAC policy doc, migration strategy, performance testing, security audit
- Added integration testing after each role set (early problem detection)
- Added Week 10 as explicit buffer/contingency

**PM Review Findings:**
- Original Week 1 timeline too compressed (fixed: extended to proper time)
- Part B roles too coarse-grained (fixed: 3 issues per role)
- Testing timeline unrealistic (fixed: test each role as built + integration tests)
- Missing RBAC policy tracking (fixed: issue #018)
- No contingency buffer (fixed: Week 10 buffer)

**Granularity Improvement Rationale:**
- Each issue = 1-2 days work (more predictable)
- Progress tracking clearer (51 small milestones vs 28 large)
- Early problem detection (issues overrunning visible immediately)
- Better estimates (small tasks easier to estimate)
- Testing integrated throughout (not just at end)

**Research Completed:**
- Comprehensive research on healthcare RBAC standards
- 15+ authoritative sources consulted (HIPAA, HL7, industry best practices)
- Typical healthcare role permissions matrix defined
- Full findings documented in meeting notes

**Standards Adopted:**
- HL7 FHIR v5.0.0 Security Specification
- HL7 Healthcare Access Control Catalog Release 3
- HIPAA Security Rule (45 CFR Part 164)
- Industry best practices (least privilege, audit logging)

**Related Documents:**
- Meeting: `meetings/2026-11-25-milestone-3-kickoff.md`
- Decision: `decisions/#003_MS3_rbac-lean-implementation-approach.md`
- Previous Milestone: `milestones/milestone-2.md` (Authentication - Complete)

**Implementation Approach:**
- Part A validates infrastructure before adding complexity (lean principle)
- Part B implements all 6 specialized roles systematically
- Part C ensures everything works together (integration testing)

**Permission Matrix Structure:**
7 roles × 5 CRUDE operations × N resources (Patient, User, etc.)
- Admin: Full CRUDE on all resources
- Doctor: Full CRUDE on clinical resources
- Nurse: Create/Read/Update clinical data, limited Delete
- Pharmacist: Read prescriptions, Update dispensing
- Receptionist: Create/Read/Update demographics, Read-only limited clinical
- Peer Counselor: Read assigned patients, Create/Update counseling notes
- Manager: Read all data, Manage users, Configure system

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

- 2026-11-25: Created (🔴 Not Started) - Kickoff meeting completed, planning finalized
- Target: 2027-01-20 (8 weeks) - Milestone completion