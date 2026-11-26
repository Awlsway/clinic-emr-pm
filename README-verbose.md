# Clinic EMR - Project Management Methodology

**Version:** 1.1  
**Last Updated:** 2026-11-25  
**For:** AI Agents, Developers, and Project Collaborators

---

## 📋 For AI Agents: Read This First

**Before starting any work on the Clinic EMR project, read this document completely.**

This document explains:
- My development methodology (SDLC-based)
- Repository structure (two separate repos)
- Documentation standards (where files go)
- Workflow process (step-by-step)
- Quality expectations (what I require)

**Following this methodology is mandatory for all work.**

---

## 🎯 Purpose of This Repository

**Repository Name:** \`clinic-emr-pm\`  
**Purpose:** Project Management workspace for Clinic EMR development

**What's tracked here:**
- Milestones (planning and reviews)
- Issues (detailed task tracking)
- Decisions (architectural and strategic choices)
- Meetings (notes and action items)
- Project board (overall status dashboard)

**What's NOT here:**
- Source code (in \`clinic-emr\` repo)
- Technical documentation (in \`clinic-emr\` repo)
- Test results (in \`clinic-emr\` repo)

---

## 🏗️ My Development Philosophy

### Core Principles

**1. Process Over Speed**
- Follow SDLC systematically
- Never skip phases to save time
- Document before implementing
- Quality over rushing

**2. Documentation is Mandatory**
- Every feature has requirements
- Every decision is recorded
- Every milestone has review
- Future me needs to understand why

**3. Separation of Concerns**
- PM tracking separate from code
- Process documentation separate from technical docs
- Planning separate from execution

**4. Discipline Equals Freedom**
- Structure enables scaling
- Templates ensure consistency
- Standards prevent chaos
- Methodology is reusable

---

## 📂 Two-Repository Structure

### Repository 1: `clinic-emr` (Technical/Product)

**URL:** `https://github.com/Awlsway/clinic-emr`  
**Purpose:** The actual EMR system

**Contains:**
- Source code (Go application)
- Technical documentation (`docs/`)
  - `requirements/` - Feature requirements
  - `analysis/` - Impact analysis and options
  - `design/` - Architecture and design specs
  - `testing/` - Test plans and results
  - `deployment/` - Setup guides and procedures
  - `project-management/` - Milestone reviews
- Tests
- Build scripts
- README (product overview)

**Audience:** Developers, users, clinics, technical contributors

---

### Repository 2: `clinic-emr-pm` (Project Management)

**URL:** `https://github.com/Awlsway/clinic-emr-pm`  
**Location:** `D:\Native\.pm\` (local)  
**Purpose:** How we manage building the EMR

**Contains:**
- `overview/` - Project board and status dashboard
- `milestones/` - Milestone planning and retrospectives
- `issues/` - Detailed issue tracking (one file per issue)
- `decisions/` - Decision records (ADRs - Architecture Decision Records)
- `meetings/` - Meeting notes and action items
- Templates for all document types

**Audience:** Project managers, stakeholders, process learners, AI agents

---

### Why Two Repositories?

**Separation Benefits:**
1. ✅ Clean product repo (no PM "meta" files)
2. ✅ Reusable PM structure (template for future projects)
3. ✅ Different audiences (developers vs managers)
4. ✅ Independent evolution (PM improves without cluttering product)
5. ✅ Portfolio showcase (demonstrates PM skills separately)

**Cross-Linking:**
- PM issues reference technical repo commits/docs
- Technical docs reference PM issues/milestones
- Both READMEs link to each other

---

## 🔄 SDLC Phases (The Seven Phases)

**NEVER skip phases. Always follow this order:**

### Phase 1: Requirements
**What:** Define what needs to be built  
**Deliverables:**
- PM repo: `issues/NNN-requirements-feature.md`
- Technical repo: `docs/requirements/feature.md`

**Questions answered:**
- What problem are we solving?
- What does success look like?
- What's in scope / out of scope?
- What are the constraints?

---

### Phase 2: Analysis ⚠️ NEVER SKIP THIS
**What:** Analyze current state, evaluate options, assess impact  
**Deliverables:**
- PM repo: `issues/NNN-analyze-feature-impact.md`
- PM repo: `decisions/NNN-chosen-approach.md`
- Technical repo: `docs/analysis/feature-impact.md`
- Technical repo: `docs/analysis/feature-options.md`

**Questions answered:**
- What's the current state?
- What are 3-4 possible approaches?
- What are pros/cons of each?
- What's the recommended approach and why?
- What are the risks?
- How much effort required?

**Why mandatory:** Prevents choosing wrong approach, missing risks, underestimating effort

---

### Phase 3: Design
**What:** Design the solution in detail  
**Deliverables:**
- PM repo: `issues/NNN-design-feature.md`
- Technical repo: `docs/design/feature-architecture.md`
- Technical repo: `docs/design/feature-schema.md` (if database)
- Technical repo: `docs/design/feature-api.md` (if HTTP endpoints)

**Questions answered:**
- How will it work?
- What's the architecture?
- What changes to database/API?
- How does it integrate with existing system?

---

### Phase 4: Implementation
**What:** Build the solution  
**Deliverables:**
- PM repo: `issues/NNN-implement-feature.md`
- Technical repo: Code changes
- Technical repo: `docs/deployment/feature-setup-guide.md`

**Requirements:**
- Code follows design
- Tests written alongside code
- Documentation written before closing issue

---

### Phase 5: Testing
**What:** Verify the solution works  
**Deliverables:**
- PM repo: `issues/NNN-test-feature.md`
- Technical repo: `docs/testing/feature-test-plan.md`
- Technical repo: `docs/testing/uat-feature.md` (UAT scripts)
- Technical repo: `docs/testing/feature-test-results.md`

**Requirements:**
- Test plan before execution
- All test cases documented
- Results recorded (pass/fail with evidence)
- Bugs filed as separate issues

---

### Phase 6: Deployment
**What:** Ship the solution and document usage  
**Deliverables:**
- PM repo: `issues/NNN-document-feature.md`
- Technical repo: `docs/deployment/feature-setup-guide.md`
- Technical repo: `docs/deployment/feature-troubleshooting.md`

**Requirements:**
- Setup guide for users
- Troubleshooting common issues
- Migration guide (if breaking changes)

---

### Phase 7: Review
**What:** Retrospective and lessons learned  
**Deliverables:**
- PM repo: `issues/NNN-milestone-review.md`
- PM repo: `milestones/milestone-N-review.md`

**Questions answered:**
- What went well?
- What could improve?
- Any technical debt created?
- Lessons learned for next milestone?

---

## 📝 Documentation Standards

### PM Repository File Structure

```
clinic-emr-pm/
├── README.md                    (this file)
│
├── overview/
│   ├── project-board.md              (current status dashboard)
│   └── _PROJECT-BOARD-TEMPLATE.md    (template)
│
├── milestones/
│   ├── milestone-1.md                (M1 plan)
│   ├── milestone-1-review.md         (M1 retrospective)
│   ├── milestone-2.md                (M2 plan)
│   └── _TEMPLATE.md                  (template)
│
├── issues/
│   ├── 001-create-uat-scripts.md    (completed)
│   ├── 002-execute-uat.md           (completed)
│   ├── 010-requirements-feature.md  (example)
│   ├── 011-analyze-feature.md       (example)
│   └── _TEMPLATE.md                 (template)
│
├── decisions/
│   ├── 001-decision-topic.md        (example)
│   └── _TEMPLATE.md                 (template)
│
└── meetings/
    ├── 2027-01-15-planning.md       (example)
    └── _TEMPLATE.md                 (template)
```

---

### Technical Repository File Structure

```
clinic-emr/
└── docs/
    ├── requirements/
    │   ├── patient-module.md
    │   └── feature-name.md
    │
    ├── analysis/              ⚠️ Don't skip this folder
    │   ├── current-state.md
    │   ├── feature-impact.md
    │   └── feature-options.md
    │
    ├── design/
    │   ├── feature-architecture.md
    │   ├── feature-schema.md
    │   └── feature-api.md
    │
    ├── testing/
    │   ├── feature-test-plan.md
    │   ├── uat-feature.md
    │   └── feature-test-results.md
    │
    └── deployment/
        ├── feature-setup-guide.md
        └── feature-troubleshooting.md
```

---

### File Naming Conventions (Milestone 3+)

**Effective from Milestone 3 onwards. See "Old Conventions" below for Milestones 1-2.**

---

#### PM Repository Files

**Issues Pattern:**
```
#[number]_MS[milestone]_SDLC[phase]_[phase-name]_[sequence]_[description].md
```

**Components:**
- `#[number]` - Sequential issue number (e.g., #015, #016, #017)
- `MS[milestone]` - Milestone number (e.g., MS3 for Milestone 3)
- `SDLC[phase]` - Phase number 1-7 (e.g., SDLC1, SDLC2)
- `[phase-name]` - Lowercase phase name
- `[sequence]` - Two-digit sequence within phase (01, 02, 03)
- `[description]` - Kebab-case description

**SDLC Phase Names:**
- **SDLC1** = `requirements`
- **SDLC2** = `analysis`
- **SDLC3** = `design`
- **SDLC4** = `implementation`
- **SDLC5** = `testing`
- **SDLC6** = `deployment`
- **SDLC7** = `review`

**Examples by Phase:**

**Phase 1 - Requirements:**
```
#015_MS3_SDLC1_requirements_01_rbac-overview.md
#016_MS3_SDLC1_requirements_02_role-permissions.md
#017_MS3_SDLC1_requirements_03_success-criteria.md
```

**Phase 2 - Analysis:**
```
#018_MS3_SDLC2_analysis_01_current-state.md
#019_MS3_SDLC2_analysis_02_rbac-approaches.md
#020_MS3_SDLC2_analysis_03_impact-assessment.md
```

**Phase 3 - Design:**
```
#021_MS3_SDLC3_design_01_database-schema.md
#022_MS3_SDLC3_design_02_middleware-architecture.md
#023_MS3_SDLC3_design_03_api-endpoints.md
```

**Phase 4 - Implementation:**
```
#024_MS3_SDLC4_implementation_01_database-migrations.md
#025_MS3_SDLC4_implementation_02_rbac-middleware.md
#026_MS3_SDLC4_implementation_03_role-assignment.md
```

**Phase 5 - Testing:**
```
#027_MS3_SDLC5_testing_01_unit-tests.md
#028_MS3_SDLC5_testing_02_integration-tests.md
#029_MS3_SDLC5_testing_03_uat-execution.md
```

**Phase 6 - Deployment:**
```
#030_MS3_SDLC6_deployment_01_setup-guide.md
#031_MS3_SDLC6_deployment_02_migration-procedure.md
#032_MS3_SDLC6_deployment_03_troubleshooting.md
```

**Phase 7 - Review:**
```
#033_MS3_SDLC7_review_01_milestone-retrospective.md
#034_MS3_SDLC7_review_02_lessons-learned.md
```

---

**Decision Files Pattern:**
```
#[number]_MS[milestone]_[decision-topic].md
```

**Examples:**
```
#003_MS3_rbac-permission-model.md
#004_MS3_role-hierarchy-approach.md
#005_MS3_middleware-integration-strategy.md
```

---

**Milestone Files Pattern:**
```
milestone-[number].md                    (Planning document)
milestone-[number]-review.md            (Retrospective document)
```

**Examples:**
```
milestones/milestone-3.md
milestones/milestone-3-review.md
```

---

#### Technical Repository Files

**Simple naming (NO prefix pattern):**

```
docs/requirements/[feature-name].md
docs/analysis/[feature-name]-impact.md
docs/analysis/[feature-name]-options.md
docs/design/[feature-name]-architecture.md
docs/design/[feature-name]-schema.md
docs/design/[feature-name]-api.md
docs/testing/[feature-name]-test-plan.md
docs/testing/uat-[feature-name].md
docs/testing/[feature-name]-test-results.md
docs/deployment/[feature-name]-setup-guide.md
docs/deployment/[feature-name]-troubleshooting.md
```

**Examples for RBAC:**
```
docs/requirements/rbac.md
docs/analysis/rbac-impact.md
docs/analysis/rbac-options.md
docs/design/rbac-architecture.md
docs/design/rbac-schema.md
docs/design/rbac-middleware.md
docs/testing/rbac-test-plan.md
docs/testing/uat-rbac.md
docs/testing/rbac-test-results.md
docs/deployment/rbac-setup-guide.md
docs/deployment/rbac-troubleshooting.md
```

---

#### Old File Naming Conventions (Milestones 1-2)

**For reference only. Do not use for new work.**

**Issues:** `NNN-phase-feature-name.md`
- `001-requirements-authentication.md`
- `002-analyze-authentication-impact.md`

**Decisions:** `NNN-decision-topic.md`
- `001-white-label-config-approach.md`

**Note:** Milestone 3+ uses the new naming convention with milestone and SDLC phase prefixes

---

## 🔄 Commit & Push Strategy

### Hybrid Approach (Recommended)

**Work locally, commit per phase, push at breakpoints**

This strategy balances regular backups with clean git history.

---

### Commit Frequency

**Commit after each SDLC phase completes:**

```bash
# After Phase 1 (Requirements) completes
git add .pm/issues/#015_MS3_SDLC1_* .pm/milestones/milestone-3.md
git commit -m "docs(MS3): complete SDLC Phase 1 - Requirements

- Add RBAC requirements overview
- Document role permissions matrix
- Define success criteria"

# After Phase 2 (Analysis) completes
git add .pm/issues/#018_MS3_SDLC2_* .pm/decisions/#003_MS3_*
git commit -m "docs(MS3): complete SDLC Phase 2 - Analysis

- Analyze current authentication state
- Evaluate 3 RBAC approaches
- Document decision: chosen approach"

# Continue for each phase...
```

**Benefits:**
- ✅ Local commits create checkpoints
- ✅ Easy to rollback if needed
- ✅ Clear phase boundaries in git history
- ✅ No work lost if something fails

---

### Push Frequency

**Push to GitHub at logical breakpoints:**

**Breakpoint 1: After Planning Complete (Phases 1-3)**
```bash
# After Requirements, Analysis, and Design all complete
git push origin main
```
*What's ready:* Requirements documented, approaches analyzed, decisions made, design complete

**Breakpoint 2: After Execution Complete (Phases 4-5)**
```bash
# After Implementation and Testing complete
git push origin main
```
*What's ready:* Code implemented, tests written and passing, feature functional

**Breakpoint 3: After Milestone Complete (Phases 6-7)**
```bash
# After Deployment docs and Review complete
git push origin main
```
*What's ready:* Full milestone done, retrospective written, ready to close

---

### Commit Message Format

**Pattern:**
```
docs(MSX): complete SDLC Phase N - Phase Name

- Bullet point describing deliverable 1
- Bullet point describing deliverable 2
- Bullet point describing deliverable 3
```

**Examples:**
```bash
docs(MS3): complete SDLC Phase 1 - Requirements
docs(MS3): complete SDLC Phase 2 - Analysis
docs(MS3): complete SDLC Phase 3 - Design
docs(MS3): complete SDLC Phase 4 - Implementation
docs(MS3): complete SDLC Phase 5 - Testing
docs(MS3): complete SDLC Phase 6 - Deployment
docs(MS3): complete SDLC Phase 7 - Review
```

**For technical repo (code commits):**
```bash
feat(rbac): implement role-based access control

- Add roles table and migrations
- Implement middleware for permission checks
- Add role assignment UI

Co-Authored-By: Claude Sonnet 5.5 <noreply@anthropic.com>
```

---

### Workflow Summary

```
Local Work:
Phase 1 → Commit → Phase 2 → Commit → Phase 3 → Commit → PUSH (Breakpoint 1)
Phase 4 → Commit → Phase 5 → Commit → PUSH (Breakpoint 2)
Phase 6 → Commit → Phase 7 → Commit → PUSH (Breakpoint 3)
```

**Key Principles:**
1. **Commit often** - After each phase
2. **Push strategically** - At logical breakpoints
3. **Never lose work** - Commits protect progress
4. **Clean history** - Pushes create clear milestones

---

## 📋 Template Formats (Examples)

### Milestone Format

```markdown
# Milestone X: [Milestone Name]

**Status:** 🔴 Not Started / 🟡 In Progress / ✅ Complete  
**Branch:** [e.g., main, develop, milestone/2-visit-module]  
**Start Date:** YYYY-MM-DD  
**Target Date:** YYYY-MM-DD  
**Completion Date:** YYYY-MM-DD (when done)  
**Owner:** [Your Name]

## Objective
[Clear statement of what this milestone aims to achieve]

## Success Criteria
- [ ] [Measurable outcome 1]
- [ ] [Measurable outcome 2]
- [ ] [Measurable outcome 3]
- [ ] Documentation complete
- [ ] Production ready

## Scope

**In Scope:**
- [Feature/deliverable 1]
- [Feature/deliverable 2]

**Out of Scope:**
- [What's explicitly NOT included]
- [Future work for next milestone]

## Issues

| # | Issue | Status | Progress | Owner |
|---|-------|--------|----------|-------|
| #X | [Issue title] | 🔴 To Do | 0% | [Name] |
| #Y | [Issue title] | 🟡 In Progress | 50% | [Name] |
| #Z | [Issue title] | ✅ Done | 100% | [Name] |

## Timeline
- **Start:** YYYY-MM-DD
- **Milestones:** [Key dates]
- **Target Completion:** YYYY-MM-DD

## Deliverables
- [Deliverable 1]
- [Deliverable 2]

## Risks

| Risk | Severity | Probability | Mitigation |
|------|----------|-------------|------------|
| [Risk description] | High/Medium/Low | High/Medium/Low | [How to mitigate] |

## Dependencies
**External Dependencies:**
- [e.g., Third-party API, hardware, etc.]

**Milestone Dependencies:**
- Milestone X must complete before this starts

## Metrics
**Key Performance Indicators:**
- [Metric 1: e.g., Test pass rate target]
- [Metric 2: e.g., Code coverage target]

## Notes
**YYYY-MM-DD:** [Milestone kickoff notes, decisions, changes]

## Retrospective (Complete after milestone done)
**What Went Well:**
- [Success 1]

**What Could Be Improved:**
- [Challenge 1]

**Lessons Learned:**
- [Lesson 1]

**Action Items for Next Milestone:**
- [Action 1]

## Status History
- YYYY-MM-DD: Created (🔴 Not Started)
- YYYY-MM-DD: Started (🟡 In Progress)
- YYYY-MM-DD: Completed (✅ Complete)
```

---

### Issue Format

```markdown
# Issue #XXX: [Issue Title]

**Status:** 🔴 To Do / 🟡 In Progress / ✅ Done / ⏸️ Blocked  
**Branch:** [e.g., feature/visit-crud, bugfix/search-spaces, main]  
**Milestone:** Milestone X  
**Created:** YYYY-MM-DD  
**Updated:** YYYY-MM-DD  
**Assignee:** [Your Name]  
**Priority:** High / Medium / Low  
**Estimated Time:** [e.g., 2 weeks, 3 days, 4 hours]

## Description
[Brief description of what needs to be done and why]

## Success Criteria
- [ ] [Specific measurable outcome 1]
- [ ] [Specific measurable outcome 2]
- [ ] [Specific measurable outcome 3]
- [ ] Documentation complete
- [ ] Tests passing

## Tasks
- [ ] [Specific task 1]
- [ ] [Specific task 2]
- [ ] [Specific task 3]

## Dependencies
**Requires:**
- Issue #XX: [Title] - [Status]

**Blocks:**
- Issue #YY: [Title]

## Notes
**YYYY-MM-DD:** [Note about progress, decisions, or blockers]

## Decision Log
**YYYY-MM-DD:** [Key decision made related to this issue]

## Related Documents
- [Link to related test scripts, design docs, etc.]

## Status History
- YYYY-MM-DD: Created (🔴 To Do)
- YYYY-MM-DD: Started work (🟡 In Progress)
- YYYY-MM-DD: Completed (✅ Done)
```

---

### Decision Record Format

```markdown
# Decision: [Decision Title]

**Date:** YYYY-MM-DD  
**Branch:** [branch-name]  
**Decision Maker:** [Your Name]  
**Status:** 🟡 Proposed / ✅ Approved / 🚀 Implemented  
**Related Issue:** #XXX (if applicable)  
**Related Milestone:** Milestone X (if applicable)

## Context
[Describe the situation that requires a decision. What problem are we solving?]

## Options Considered

### Option A: [Option Name]
**Description:** [What this option involves]

**Pros:**
- [Advantage 1]
- [Advantage 2]

**Cons:**
- [Disadvantage 1]
- [Disadvantage 2]

**Cost/Effort:** [Estimate]

### Option B: [Option Name]
**Description:** [What this option involves]

**Pros:**
- [Advantage 1]

**Cons:**
- [Disadvantage 1]

**Cost/Effort:** [Estimate]

### Option C: [Option Name]
**Description:** [What this option involves]

**Pros:**
- [Advantage 1]

**Cons:**
- [Disadvantage 1]

**Cost/Effort:** [Estimate]

## Decision
**Selected Option:** [Option X]

**Rationale:** [Why this option was chosen over the others]

## Impact
**Affected Areas:**
- [Area 1: e.g., Architecture]
- [Area 2: e.g., Timeline]

**Changes Required:**
- [Change 1]
- [Change 2]

**Risks:**
- [Risk 1]

## Implementation Plan
- [ ] [Step 1]
- [ ] [Step 2]

**Target Implementation Date:** YYYY-MM-DD

## Follow-Up
**Review Date:** YYYY-MM-DD  
**Success Criteria:** [How we'll know this decision was good]

## Notes
[Additional context, discussions, or related information]

## Status History
- YYYY-MM-DD: Proposed
- YYYY-MM-DD: Approved
- YYYY-MM-DD: Implemented
```

---

### Meeting Notes Format

```markdown
# Meeting: [Meeting Title]

**Date:** YYYY-MM-DD  
**Time:** HH:MM - HH:MM  
**Branch:** [current-branch]  
**Type:** Planning / Review / Standup / Retrospective / Design / Decision  
**Facilitator:** [Your Name]

## Attendees

**Present:**
- [Name 1] - [Role]
- [Name 2] - [Role]

**Absent:**
- [Name 3] - [Role]

## Agenda
1. [Topic 1]
2. [Topic 2]
3. [Topic 3]

## Discussion

### Topic 1: [Topic Title]
**Summary:**
[What was discussed]

**Key Points:**
- [Point 1]
- [Point 2]

**Questions Raised:**
- [Question 1]

### Topic 2: [Topic Title]
**Summary:**
[What was discussed]

**Key Points:**
- [Point 1]

## Decisions Made

| Decision | Owner | Due Date | Status |
|----------|-------|----------|--------|
| [Decision 1] | [Name] | YYYY-MM-DD | ✅ / 🟡 / 🔴 |
| [Decision 2] | [Name] | YYYY-MM-DD | ✅ / 🟡 / 🔴 |

## Action Items
- [ ] [Action 1] - **Owner:** [Name] - **Due:** YYYY-MM-DD
- [ ] [Action 2] - **Owner:** [Name] - **Due:** YYYY-MM-DD

## Blockers / Issues
- [Blocker 1] - **Impact:** [Description]
- [Issue 1] - **Action:** [What needs to happen]

## Next Steps
1. [Next step 1]
2. [Next step 2]

**Next Meeting:** YYYY-MM-DD

## Notes
[Additional notes, context, or follow-up items]

## Related Documents
- Issue #XXX: [Link or reference]
- Decision: [Link to decision document]
- Milestone: [Link to milestone]
```
## 📄 Template Files Reference

### Available Templates

Use these template files when creating new documents:

**Location:** Each folder contains \`_TEMPLATE.md\`

- \`milestones/_TEMPLATE.md\` - Copy for new milestone plans
- \`issues/_TEMPLATE.md\` - Copy for new issues
- \`decisions/_TEMPLATE.md\` - Copy for new decision records
- \`meetings/_TEMPLATE.md\` - Copy for new meeting notes
- \`overview/_PROJECT-BOARD-TEMPLATE.md\` - Copy for project board

### Usage

```bash
# Create new milestone
cp milestones/_TEMPLATE.md milestones/milestone-5.md
# Edit milestone-5.md with your content

# Create new issue
cp issues/_TEMPLATE.md issues/025-requirements-billing.md
# Edit with your content

# Create new decision
cp decisions/_TEMPLATE.md decisions/005-payment-gateway-choice.md
# Edit with your content
```

**Rule:** Always copy template, never edit template directly.

---

## 🔨 Workflow Example: Building a New Feature

### Example: Add "Visit/Encounter Module"

**Timeline:** 4-6 weeks (Milestone 5)

---

### Week 1: Requirements + Analysis

**Day 1-2: Requirements**

```bash
# In PM repo
cd D:/Native/.pm
cp issues/_TEMPLATE.md issues/020-requirements-visit-module.md
# Edit issue #20
git add issues/020-requirements-visit-module.md
git commit -m "Start Issue #20: Visit module requirements"
git push

# In technical repo
cd D:/Native/clinic-emr
# Create docs/requirements/visit-module.md
git add docs/requirements/visit-module.md
git commit -m "docs: add visit module requirements"
git push

# Back to PM repo
# Update issue #20 status to "Complete"
git commit -am "Close Issue #20: Requirements documented"
git push
```

**Day 3-5: Analysis**

```bash
# In PM repo
cp issues/_TEMPLATE.md issues/021-analyze-visit-module-impact.md
# Edit issue #21
git add issues/021-analyze-visit-module-impact.md
git commit -m "Start Issue #21: Visit module impact analysis"
git push

# In technical repo
# Create docs/analysis/visit-module-impact.md
# Create docs/analysis/visit-module-options.md
# Evaluate 3-4 approaches
git add docs/analysis/
git commit -m "docs: analyze visit module options"
git push

# In PM repo (after choosing approach)
cp decisions/_TEMPLATE.md decisions/002-visit-module-approach.md
# Document chosen approach
git add decisions/002-visit-module-approach.md
git commit -m "Decision #2: Visit module approach"
git push

# Close issue #21
git commit -am "Close Issue #21: Analysis complete"
git push
```

---

### Week 2: Design

```bash
# In PM repo
cp issues/_TEMPLATE.md issues/022-design-visit-module.md
git add issues/022-design-visit-module.md
git commit -m "Start Issue #22: Visit module design"
git push

# In technical repo
# Create docs/design/visit-module-architecture.md
# Create docs/design/visit-module-schema.md
# Create docs/design/visit-module-api.md
git add docs/design/
git commit -m "docs: design visit module architecture"
git push

# In PM repo
git commit -am "Close Issue #22: Design complete"
git push
```

---

### Week 3-4: Implementation

```bash
# In PM repo
cp issues/_TEMPLATE.md issues/023-implement-visit-module.md
git commit -m "Start Issue #23: Visit module implementation"
git push

# In technical repo
# Code changes: internal/visit/*, cmd/emr/*, internal/web/*
# Write docs/deployment/visit-module-setup-guide.md
git add .
git commit -m "feat: implement visit module

- Add visit domain model
- Add database schema and migrations
- Add HTTP handlers and templates
- Add setup documentation

Co-Authored-By: Claude Opus 5.5 <noreply@anthropic.com>"
git push

# In PM repo
git commit -am "Close Issue #23: Implementation complete"
git push
```

---

### Week 5: Testing

```bash
# In PM repo
cp issues/_TEMPLATE.md issues/024-test-visit-module.md
git commit -m "Start Issue #24: Visit module testing"
git push

# In technical repo
# Create docs/testing/visit-module-test-plan.md
# Create docs/testing/uat-visit-module.md
# Execute tests, create docs/testing/visit-module-test-results.md
git add docs/testing/
git commit -m "docs: add visit module test plan and results"
git push

# In PM repo
git commit -am "Close Issue #24: Testing complete"
git push
```

---

### Week 6: Documentation + Review

```bash
# In PM repo
cp issues/_TEMPLATE.md issues/025-document-visit-module.md
git commit -m "Start Issue #25: Finalize visit module docs"
git push

# In technical repo
# Finalize all documentation
git commit -m "docs: finalize visit module documentation"
git push

# In PM repo
git commit -am "Close Issue #25: Documentation complete"

# Create milestone review
cp issues/_TEMPLATE.md issues/026-milestone-5-review.md
git commit -m "Start Issue #26: Milestone 5 review"

# Write milestones/milestone-5-review.md
git add milestones/milestone-5-review.md
git commit -m "Complete Milestone 5 review"
git push

# Update project board
# Edit overview/project-board.md
git commit -am "Update project board: Milestone 5 complete"
git push
```

---

## ✅ Quality Checklist

### Before Closing ANY Issue

- [ ] All tasks in issue checked off
- [ ] All deliverables created
- [ ] Documentation written
- [ ] Committed and pushed to appropriate repo(s)
- [ ] Cross-links added (PM issue references technical docs)
- [ ] Issue status updated to "Complete"

### Before Completing ANY Milestone

- [ ] All issues closed
- [ ] All SDLC phases completed (Requirements → Analysis → Design → Implementation → Testing → Deployment → Review)
- [ ] Milestone review document written
- [ ] Project board updated
- [ ] Technical documentation complete
- [ ] Code tagged/versioned (if applicable)

### Before ANY Commit

- [ ] Commit message clear and descriptive
- [ ] Attribution added (Co-Authored-By) if AI-assisted
- [ ] Correct repository (PM vs Technical)
- [ ] Related issue number mentioned (optional but helpful)

---

## ❌ What NOT to Do

### Never Skip Analysis Phase

**Wrong:**
```
Requirements → Design → Implementation
```

**Right:**
```
Requirements → Analysis → Design → Implementation
```

**Why:** Analysis prevents choosing wrong approach, missing risks, and underestimating effort.

---

### Never Mix PM and Technical Docs

**Wrong:**
- Put issue tracking in \`clinic-emr/docs/\`
- Put technical design in \`clinic-emr-pm/issues/\`

**Right:**
- PM tracking: \`clinic-emr-pm\`
- Technical docs: \`clinic-emr/docs/\`

---

### Never Implement Without Design

**Wrong:**
- Start coding immediately after requirements

**Right:**
- Requirements → Analysis → Design → Implementation

---

### Never Close Issue Without Documentation

**Wrong:**
- Code is done → close issue

**Right:**
- Code is done + docs written + tests pass → close issue

---

### Never Push to Wrong Repository

**Wrong:**
- Commit code to PM repo
- Commit PM issues to technical repo

**Right:**
- Check which repo you're in before committing
- PM work → PM repo
- Code/technical docs → technical repo

---

### Never Guess Format

**Wrong:**
- Create milestone file with random format

**Right:**
- Copy template
- Fill in template structure
- Follow naming conventions

---

## 🤖 For AI Agents: Quick Reference

### When User Says "Build Feature X"

**Your Process:**

1. **Read this README** (if not already done)
2. **Confirm understanding:** "I'll follow SDLC: Requirements → Analysis → Design → Implementation → Testing → Deployment → Review"
3. **Ask clarifying questions** about requirements
4. **Create issue #1:** Requirements phase
5. **Write:** \`docs/requirements/feature-x.md\` in technical repo
6. **Close issue #1**
7. **Create issue #2:** Analysis phase
8. **Write:** \`docs/analysis/feature-x-impact.md\` and \`docs/analysis/feature-x-options.md\`
9. **Write:** \`decisions/NNN-feature-x-approach.md\` in PM repo
10. **Close issue #2**
11. **Continue through all 7 phases**

### When User Says "Skip Analysis"

**Your Response:**
"Analysis phase is mandatory per your methodology (README.md Section: SDLC Phases). It prevents choosing wrong approach and missing risks. Should I proceed with analysis, or would you like to update your methodology?"

### When You're Unsure Where a File Goes

**Decision Tree:**
- Is it tracking work/process? → PM repo
- Is it technical/product documentation? → Technical repo
- Is it code? → Technical repo
- Is it a decision record? → PM repo
- Is it a milestone/issue? → PM repo

---

## 📚 Related Documents

### In This Repository (PM)
- Templates in each folder (\`_TEMPLATE.md\` files)
- \`overview/project-board.md\` - Current project status
- \`milestones/milestone-N.md\` - Individual milestone plans

### In Technical Repository
- \`clinic-emr/README.md\` - Product overview
- \`clinic-emr/docs/\` - All technical documentation
- \`clinic-emr/docs/PM_WORKFLOW_TEMPLATE.md\` - Detailed PM workflow (supplementary)

### External Links
- Technical repo: https://github.com/Awlsway/clinic-emr
- PM repo: https://github.com/Awlsway/clinic-emr-pm (this repo)

---

## 📞 Questions?

If this README doesn't answer your question:

**For AI Agents:**
- Ask the user for clarification
- Reference specific section of this README
- Propose approach following methodology

**For Human Collaborators:**
- Open an issue in this repo (PM repo)
- Tag with "question" label
- Reference section of README that's unclear

---

## 🔄 Document Version History

**v1.0 (2026-11-21):**
- Initial version
- Established SDLC methodology
- Defined two-repository structure
- Created template formats
- Added workflow examples

---

**End of README**

**Remember:** This methodology is designed for consistency, quality, and scalability. Following it ensures every feature is properly analyzed, designed, tested, and documented.

**For AI Agents:** Always start by reading this document. Always follow SDLC phases. Always document your work. Always maintain quality standards.
