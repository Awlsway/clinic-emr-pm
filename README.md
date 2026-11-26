# Clinic EMR - Project Management Methodology

**Version:** 1.1 | **Updated:** 2026-11-25 | **For:** AI Agents, Developers, Collaborators

---

## 📋 AI Agents: Read First

**Read this document completely before starting any work.**

**Contents:** Development methodology (SDLC), repository structure, documentation standards, workflow process, quality expectations.

**Following this methodology is mandatory.**

---

## 🎯 Repository Purpose

**Name:** `clinic-emr-pm`  
**Purpose:** PM workspace for Clinic EMR development

**Tracked:** Milestones, issues, decisions, meetings, project board  
**Not tracked:** Source code, technical docs, test results (in `clinic-emr` repo)

---

## 🏗️ Development Philosophy

**Core Principles:**
1. **Process Over Speed** - Follow SDLC systematically, never skip phases, document before implementing
2. **Documentation Mandatory** - Every feature has requirements, every decision recorded, every milestone reviewed
3. **Separation of Concerns** - PM tracking ≠ code, process docs ≠ technical docs, planning ≠ execution
4. **Discipline = Freedom** - Structure enables scaling, templates ensure consistency, standards prevent chaos

---

## 📂 Two-Repository Structure

### `clinic-emr` (Technical/Product) - Private
**URL:** https://github.com/Awlsway/clinic-emr  
**Contains:** Source code, technical docs (`requirements/`, `analysis/`, `design/`, `testing/`, `deployment/`), tests, build scripts  
**Audience:** Developers, users, clinics

### `clinic-emr-pm` (Project Management) - Public
**URL:** https://github.com/Awlsway/clinic-emr-pm  
**Local:** `D:\Native\.pm\`  
**Contains:** `overview/`, `milestones/`, `issues/`, `decisions/`, `meetings/`, templates  
**Audience:** PMs, stakeholders, process learners, AI agents

**Why Two?** Clean product repo, reusable PM structure, different audiences, independent evolution, portfolio showcase

---

## 🔄 SDLC Phases (Never Skip)

| Phase | Deliverables (PM Repo) | Deliverables (Tech Repo) | Key Questions |
|-------|------------------------|--------------------------|---------------|
| **1. Requirements** | `issues/#NNN_MS*_SDLC1_*` | `docs/requirements/*.md` | What problem? Success criteria? Scope? Constraints? |
| **2. Analysis** ⚠️ | `issues/#NNN_MS*_SDLC2_*`<br>`decisions/#NNN_MS*_*.md` | `docs/analysis/*-impact.md`<br>`docs/analysis/*-options.md` | Current state? 3-4 approaches? Pros/cons? Risks? Effort? |
| **3. Design** | `issues/#NNN_MS*_SDLC3_*` | `docs/design/*-architecture.md`<br>`docs/design/*-schema.md`<br>`docs/design/*-api.md` | How it works? Architecture? DB/API changes? Integration? |
| **4. Implementation** | `issues/#NNN_MS*_SDLC4_*` | Code + `docs/deployment/*-setup.md` | Code follows design? Tests written? Docs complete? |
| **5. Testing** | `issues/#NNN_MS*_SDLC5_*` | `docs/testing/*-test-plan.md`<br>`docs/testing/uat-*.md`<br>`docs/testing/*-results.md` | Test plan exists? All cases documented? Results recorded? |
| **6. Deployment** | `issues/#NNN_MS*_SDLC6_*` | `docs/deployment/*-setup.md`<br>`docs/deployment/*-troubleshooting.md` | Setup guide? Troubleshooting? Migration? |
| **7. Review** | `issues/#NNN_MS*_SDLC7_*`<br>`milestones/milestone-N-review.md` | - | What worked? What improve? Tech debt? Lessons? |

**⚠️ Analysis Phase:** Mandatory - prevents wrong approach, missing risks, underestimating effort.

---

## 📝 File Naming Conventions (Milestone 3+)

**PM Repo Issues:**
```
#[num]_MS[milestone]_SDLC[phase]_[phase-name]_[seq]_[description].md
```
- `#015` = Issue number
- `MS3` = Milestone 3
- `SDLC1` = Phase (1-7)
- `requirements` = Phase name (lowercase)
- `01` = Sequence within phase
- `rbac-overview` = Description (kebab-case)

**SDLC Phase Names:**
`requirements`, `analysis`, `design`, `implementation`, `testing`, `deployment`, `review`

**Examples:**
```
#015_MS3_SDLC1_requirements_01_rbac-overview.md
#018_MS3_SDLC2_analysis_01_current-state.md
#021_MS3_SDLC3_design_01_database-schema.md
#024_MS3_SDLC4_implementation_01_migrations.md
#027_MS3_SDLC5_testing_01_unit-tests.md
#030_MS3_SDLC6_deployment_01_setup-guide.md
#033_MS3_SDLC7_review_01_retrospective.md
```

**PM Repo Decisions:**
```
#[num]_MS[milestone]_[decision-topic].md
Example: #003_MS3_rbac-permission-model.md
```

**PM Repo Milestones:**
```
milestone-[N].md              (Planning)
milestone-[N]-review.md       (Retrospective)
```

**Tech Repo (Simple Names - No Prefix):**
```
docs/requirements/rbac.md
docs/analysis/rbac-impact.md, rbac-options.md
docs/design/rbac-architecture.md, rbac-schema.md, rbac-middleware.md
docs/testing/rbac-test-plan.md, uat-rbac.md, rbac-test-results.md
docs/deployment/rbac-setup-guide.md, rbac-troubleshooting.md
```

**Old Convention (MS1-2 Reference Only):**
```
NNN-phase-feature-name.md
Example: 001-requirements-authentication.md
```

---

## 🔄 Commit & Push Strategy (Hybrid Approach)

**Commit:** After each SDLC phase completes (local checkpoint)  
**Push:** At 3 breakpoints (backup to GitHub)

**Breakpoints:**
1. **After Planning** (Phases 1-3) - Requirements, Analysis, Design complete
2. **After Execution** (Phases 4-5) - Implementation, Testing complete
3. **After Milestone** (Phases 6-7) - Deployment, Review complete

**Commit Format:**
```
docs(MSX): complete SDLC Phase N - Phase Name

- Deliverable 1
- Deliverable 2
```

**Tech Repo Commit Format:**
```
feat(feature): implement feature name

- Change 1
- Change 2

Co-Authored-By: Claude Sonnet 5.5 <noreply@anthropic.com>
```

**Workflow:**
```
Phase 1 → commit → Phase 2 → commit → Phase 3 → commit → PUSH
Phase 4 → commit → Phase 5 → commit → PUSH
Phase 6 → commit → Phase 7 → commit → PUSH
```

---

## 📂 Repository File Structure

**PM Repo:**
```
clinic-emr-pm/
├── README.md
├── overview/
│   ├── project-board.md
│   └── _PROJECT-BOARD-TEMPLATE.md
├── milestones/
│   ├── milestone-N.md
│   ├── milestone-N-review.md
│   └── _TEMPLATE.md
├── issues/
│   ├── #NNN_MSX_SDLCX_phasename_NN_description.md
│   └── _TEMPLATE.md
├── decisions/
│   ├── #NNN_MSX_decision-topic.md
│   └── _TEMPLATE.md
└── meetings/
    ├── YYYY-MM-DD-topic.md
    └── _TEMPLATE.md
```

**Tech Repo:**
```
clinic-emr/
└── docs/
    ├── requirements/feature.md
    ├── analysis/feature-impact.md, feature-options.md
    ├── design/feature-architecture.md, feature-schema.md, feature-api.md
    ├── testing/feature-test-plan.md, uat-feature.md, feature-results.md
    └── deployment/feature-setup.md, feature-troubleshooting.md
```

---

## ✅ Quality Checklist

**Before Closing Any Issue:**
- [ ] All tasks checked off
- [ ] All deliverables created
- [ ] Documentation written
- [ ] Committed and pushed to correct repo
- [ ] Cross-links added
- [ ] File naming convention followed
- [ ] Issue status = Complete

**Before Completing Milestone:**
- [ ] All issues closed
- [ ] All 7 SDLC phases complete
- [ ] Milestone review written
- [ ] Project board updated
- [ ] Technical docs complete
- [ ] Code tagged/versioned

**Before Any Commit:**
- [ ] Clear commit message
- [ ] Co-Authored-By attribution (if AI-assisted)
- [ ] Correct repository
- [ ] Related issue referenced

---

## ❌ Never Do

1. **Never skip Analysis** - Prevents wrong approach, missing risks, underestimating effort
2. **Never mix PM and technical docs** - PM → `clinic-emr-pm`, Code/tech → `clinic-emr`
3. **Never implement without design** - Requirements → Analysis → Design → Implementation
4. **Never close issue without docs** - Code done + docs written + tests pass = close
5. **Never push to wrong repo** - Check which repo before committing
6. **Never guess format** - Copy template, follow naming conventions

---

## 🤖 AI Agent Quick Reference

**When user says "Build Feature X":**
1. Read README (this doc)
2. Confirm: "Following SDLC: Requirements → Analysis → Design → Implementation → Testing → Deployment → Review"
3. Ask clarifying questions
4. Create Phase 1 issue: `#NNN_MSX_SDLC1_requirements_01_feature-x.md`
5. Write `docs/requirements/feature-x.md` (tech repo)
6. Close Phase 1 issue
7. Create Phase 2 issue: `#NNN_MSX_SDLC2_analysis_01_current-state.md`
8. Continue through all 7 phases

**When user says "Skip Analysis":**
"Analysis phase is mandatory per methodology. It prevents wrong approach and missing risks. Proceed with analysis or update methodology?"

**File Destination:**
- Tracking/process → PM repo
- Technical/code → Tech repo
- Decision record → PM repo
- Milestone/issue → PM repo

---

## 📚 Templates

**Copy from:**
- `milestones/_TEMPLATE.md` → new milestone
- `issues/_TEMPLATE.md` → new issue
- `decisions/_TEMPLATE.md` → new decision
- `meetings/_TEMPLATE.md` → new meeting

**Never edit templates directly.**

---

## 🔄 Version History

**v1.1 (2026-11-25):**
- Added file naming convention (MS3+ with SDLC prefix)
- Added commit & push strategy (hybrid approach)
- Updated for Milestone 3 workflow

**v1.0 (2026-11-21):**
- Initial version
- Established SDLC methodology
- Defined two-repository structure

---

**End of Methodology Documentation**

For detailed examples and full templates, see individual `_TEMPLATE.md` files in each folder.