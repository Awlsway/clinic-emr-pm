# Decision: Revised Streamlined Approach for Milestone 3

**Date:** 2026-11-26  
**Branch:** main  
**Decision Maker:** ANSVK (PM)  
**Status:** ✅ Approved  
**Related Milestone:** Milestone 3 - RBAC Staff Roles  
**Related Decision:** #003 (original 51-issue approach - archived)

---

## Context

On 2026-11-25, Milestone 3 was planned with 51 issues across 10 weeks following strict 7-phase SDLC methodology. After completing Issues #015-#016 (requirements work), it became clear that this approach created excessive PM overhead for a solo AI developer context.

**Original Plan:**
- 51 issues across 10 weeks
- Strict SDLC phases: Requirements → Analysis → Design → Implementation → Testing → Deployment → Review
- Each phase as separate issues
- Each role = 3 issues (design → implement → test)
- Estimated PM effort: 50+ hours creating detailed specifications

**Problem Identified:**
- Day 1 spent 6.5 hours on documentation, 0 hours on code
- Each issue required ~1-1.5 hours of specification writing
- 51 issues = 51 context switches, 51 reviews, 51 commits
- PM overhead >> value delivered
- Process designed for team handoffs, not solo developer

**Team Context Clarification:**
Initial assumption was two AI developers (Codex + Claude Code) working in parallel. Actual context is PM (ANSVK) + single developer (Claude Code), as Codex unavailable for this project.

---

## Options Considered

### Option A: Continue with 51-Issue Plan
**Description:** Proceed as planned with strict 7-phase SDLC and 51 granular issues.

**Pros:**
- Already planned and documented
- Follows "proper" SDLC methodology
- Maximum tracking granularity
- Learning formal PM processes

**Cons:**
- Excessive PM overhead (50+ hours specification writing)
- 10 weeks timeline with solo developer
- Process designed for teams, not solo developer
- Risk of burnout or abandoning methodology
- Not learning "pragmatic PM," learning "bureaucratic PM"

**Effort:** 10 weeks, 50+ hours PM overhead

---

### Option B: Modify Plan Incrementally
**Description:** Keep 51-issue structure but combine issues as we go, reduce documentation depth.

**Pros:**
- No need to restart milestone planning
- Can adjust on-the-fly as issues completed
- Preserves some of original structure

**Cons:**
- Still carrying baggage of 51-issue plan
- Unclear which issues to combine until encountered
- PM overhead still high (unclear specifications)
- Difficult to change course mid-milestone
- Timeline still ~8-9 weeks

**Effort:** 8-9 weeks, 30-40 hours PM overhead

---

### Option C: Streamlined Feature-Complete Iterations (CHOSEN)
**Description:** Restart milestone with 15 issues structured as feature deliveries, combined SDLC phases, solo developer optimized.

**Implementation:**
- 15 issues instead of 51 (70% reduction)
- 7 weeks instead of 10 (30% reduction)
- Combined phases: Requirements+Analysis, Design+Implementation for roles
- Each role = 1 issue (design + implement + test combined)
- Lightweight specifications: scope + acceptance criteria, not exhaustive requirements
- Feature-complete iterations: each issue delivers working functionality

**Pros:**
- Drastically reduced PM overhead (~20-30 hours vs 50+ hours)
- Faster delivery (7 weeks vs 10 weeks)
- Right-sized for solo AI developer context
- Learn pragmatic PM (tailor to context) vs bureaucratic PM
- Maintained quality: testing, compliance, documentation preserved
- Early value delivery: working features at checkpoints

**Cons:**
- Need to restart milestone planning (sunk cost: 6.5 hours)
- Less granular progress tracking
- Requires documenting why we changed approach

**Effort:** 7 weeks, 20-30 hours PM overhead

---

## Decision

**Selected Option:** Option C - Streamlined Feature-Complete Iterations

**Rationale:**

1. **Context-Appropriate Methodology:**
   - Team-based SDLC ≠ Solo AI developer approach
   - Original plan designed for team handoffs, not solo execution
   - Pragmatic PM = tailor process to team composition

2. **PM Overhead Reduction:**
   - 51 issues = 51 specifications, 51 reviews, 51 acceptances
   - 15 issues = 15 specifications, 15 reviews, 15 acceptances
   - PM time: 50+ hours → 20-30 hours (60% reduction)

3. **Developer Efficiency:**
   - AI developer can handle larger work packages
   - Design + implement + test as continuous flow (not artificial separation)
   - Each issue delivers working feature (not just documentation phase)

4. **Timeline Improvement:**
   - 10 weeks → 7 weeks (30% faster)
   - Same quality (testing, HIPAA compliance, documentation preserved)
   - Faster feedback loops at feature boundaries

5. **Learning Value:**
   - Original approach taught "follow rigid process"
   - Revised approach teaches "adapt methodology to context"
   - Latter is more valuable PM skill

6. **Minimal Sunk Cost:**
   - Only 2 issues completed (#015-#016)
   - 6.5 hours invested (acceptable to restart)
   - Work from #015-#016 incorporated into new Issue #017

---

## Implementation Plan

### **Revised Milestone Structure:**

**15 Issues Total:**
- Phase 1 (Foundation): 4 issues - Weeks 1-2
- Phase 2 (Clinical/Support Roles): 5 issues - Weeks 3-4
- Phase 3 (Management Roles + UI): 3 issues - Week 5
- Phase 4 (Integration + Delivery): 3 issues - Weeks 6-7

### **SDLC Phase Consolidation:**

**For Planning/Design Issues:**
- Requirements + Analysis → Combined "Planning" phase
- One document instead of separate requirements/analysis issues

**For Role Implementation:**
- Design + Implementation + Testing → Combined "Feature Delivery"
- One issue per role (not 3 separate issues)

**For Infrastructure:**
- Keep separate design and implementation (complexity warrants separation)

**Preserved Separately:**
- Integration testing (critical quality gate)
- Deployment documentation (needed for production)
- Retrospective (learning artifact)

### **Issue Instruction Format:**

Lightweight specifications:
```markdown
# Issue #NNN: [Feature Name]

**Objective:** [One sentence - what needs to work]

**Scope:**
- [Key deliverable 1]
- [Key deliverable 2]

**Acceptance Criteria:**
- [ ] [Testable criterion]
- [ ] [Testable criterion]

**Standards/Constraints:**
- [HIPAA requirements]
- [Technical constraints]

**Definition of Done:**
- Code working and tested
- Brief implementation notes
- Ready for UAT
```

**What PM provides:** Scope + acceptance criteria (1-2 hours per issue)  
**What developer provides:** Implementation approach + working code + tests

---

## Impact

**Affected Areas:**
- **Milestone 3 Timeline:** 10 weeks → 7 weeks (30% reduction)
- **Issue Count:** 51 → 15 issues (70% reduction)
- **PM Workload:** 50+ hours → 20-30 hours (60% reduction)
- **Developer Autonomy:** Increased - larger work packages, less micro-management
- **Documentation Depth:** Reduced - decision records + code comments vs exhaustive specs
- **Methodology Learning:** Shifted from "rigid SDLC" to "pragmatic PM"

**Changes Required:**
- Archive original milestone-3.md and decision #003
- Create revised milestone-3.md (15-issue structure)
- Document decision rationale (this document)
- Update project board with new issue structure
- Brief developer (Claude Code) with revised approach

**Benefits:**
- Faster delivery (7 weeks vs 10 weeks)
- Reduced PM overhead (60% less specification writing)
- Developer can work in larger, more coherent chunks
- Feature-complete iterations (testable deliverables at checkpoints)
- Learning pragmatic PM (not bureaucratic PM)

**Tradeoffs:**
- Less granular progress tracking (15 checkpoints vs 51)
- Larger issue scope (requires developer autonomy)
- Less detailed up-front planning (more discovery during implementation)

**Risks Mitigated:**
- **Process burnout:** Reduced overhead prevents abandoning methodology
- **Slow delivery:** 7 weeks vs 10 weeks maintains motivation
- **Wrong methodology learning:** Teaching pragmatic PM vs rigid process
- **Developer underutilization:** Larger work packages use AI capabilities better

---

## Follow-Up

**Review Date:** 2027-01-13 (at Milestone 3 completion)

**Success Criteria:**
- [ ] Milestone 3 completed within 7 weeks
- [ ] PM overhead stayed within 20-30 hours total
- [ ] All quality gates met (testing, HIPAA compliance, documentation)
- [ ] Developer able to execute autonomously within issue boundaries
- [ ] Learned valuable PM lesson: tailor methodology to context

**Metrics to Track:**
- Actual timeline vs 7-week estimate
- Actual PM hours vs 20-30 hour estimate
- Issue completion rate (average days per issue)
- Developer autonomy level (questions asked per issue)
- Quality metrics (test coverage, HIPAA compliance, security audit)

**Questions to Answer in Retrospective:**
- Was 15 issues the right granularity, or should it be more/fewer?
- Did combined SDLC phases work, or did we miss important separation?
- Was developer autonomy appropriate, or did PM need more oversight?
- What's the right balance for Milestone 4?

---

## Notes

**Key Insight:**
"Process Over Speed" (original README principle) is misleading. Better principle: **"Process Enables Autonomy"** - right-sized process for the team context enables faster delivery, not slower.

**PM Methodology Learning:**
- Original approach: Apply textbook SDLC regardless of context
- Revised approach: Understand team composition, tailor methodology
- Real PM skill: Knowing when to add process vs when to remove it

**Solo AI Developer vs Team:**
Team context needs:
- Detailed specs (different people implementing)
- Phase separation (handoffs between roles)
- Granular issues (coordinating multiple people)
- Extensive documentation (knowledge sharing)

Solo AI developer context needs:
- Scope + acceptance criteria (AI fills in details)
- Combined phases (no handoffs, continuous flow)
- Feature-complete issues (deliverable chunks)
- Decision records + code comments (enough for maintenance)

**Sunk Cost Fallacy Avoided:**
Could have continued with 51-issue plan ("we already planned it"), but 6.5 hours sunk cost << 30+ hours saved by switching approaches. Good PM decision: recognize mistake early, correct course.

**Related Documents:**
- Archived original plan: `archive/milestone-3-original/milestone-3.md`
- Archived original decision: `archive/milestone-3-original/#003_MS3_rbac-lean-implementation-approach.md`
- Revised milestone plan: `milestones/milestone-3.md`
- Completed work preserved: Issues #015, #016

---

## Status History

- 2026-11-25: Original 51-issue plan approved (Decision #003)
- 2026-11-25: Issues #015-#016 completed
- 2026-11-26: Identified PM overhead problem
- 2026-11-26: Revised to 15-issue streamlined approach (Decision #004 - Approved)
- 2026-11-26: Original plan archived, revised plan active
