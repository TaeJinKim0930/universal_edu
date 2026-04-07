# AI Coach MVP for Mid-Career Reskilling (Contest Plan)

## TL;DR

> **Quick Summary**: Build a competition-ready AI coaching web app that helps employed/mid-career learners diagnose skill gaps, get a personalized weekly learning path, and receive progress feedback.
>
> **Deliverables**:
> - Next.js + Supabase MVP (auth, diagnosis, coaching path, weekly feedback)
> - WSL-first developer workflow and verification scripts
> - Contest-ready AI report evidence artifacts and QA captures
>
> **Estimated Effort**: Medium
> **Parallel Execution**: YES — 3 waves + final verification wave
> **Critical Path**: T1 → T4 → T8 → T10 → T12 → F1-F4

---

## Context

### Original Request
Create an AI education agent idea and delivery strategy for KIT 바이브코딩 공모전, focused on reducing AI-era education gaps. Prioritize practical adoption for employed/mid-career learners first, with future expansion to broader age groups and senior-friendly UX.

### Interview Summary
**Key Discussions**:
- Target strategy fixed: **all-age applicability + senior-friendly UX**, but MVP primary persona is **mid-career workers**.
- Product scope fixed: **AI coaching web app** (diagnosis → personalized path → weekly feedback).
- Stack fixed: **Next.js + Supabase**.
- Validation fixed: **tests-after implementation** (not full TDD), but with mandatory agent-executed QA scenarios.
- Operating model fixed: **Windows host + WSL-centered execution**.
- gstack usage fixed: **adapt operational patterns only**, no full fork.

**Research Findings**:
- `garrytan/gstack` is public and usable as a process reference.
- Contest scoring rewards AI collaboration evidence and practical field applicability.

### Metis Review (incorporated)
**Identified Gaps (addressed)**:
- Scope creep risk → strict MVP + explicit Must-NOT-Have boundaries added.
- Accessibility ambiguity → measurable senior-friendly UX checks added.
- Tests ambiguity → interpreted as "implement slice, then immediately run automated tests/QA per slice".
- gstack drift risk → explicit allowed-borrow list added.

---

## Work Objectives

### Core Objective
Ship a demonstrable AI coaching MVP that improves re-skilling execution quality for employed/mid-career learners, while proving accessibility-aware design and operational rigor for contest judging.

### Concrete Deliverables
- Web app routes: onboarding, diagnosis, learning plan, weekly feedback, progress view
- Supabase-backed auth and per-user data isolation (RLS)
- AI coaching pipeline with safe fallback behavior
- Evidence package under `.sisyphus/evidence/` for contest/report reuse

### Definition of Done
- [ ] User can sign in, complete diagnosis, receive personalized weekly plan, and log weekly progress
- [ ] Each flow has automated verification output (Playwright/API/tests)
- [ ] No cross-user data visibility in DB policy checks
- [ ] Build/typecheck/test commands pass in WSL

### Must Have
- Clear mid-career persona onboarding
- Personalized weekly plan generation (deterministic fallback included)
- Weekly feedback loop with progress delta display
- Accessibility baseline checks (keyboard flow + zoom + readable UI copy)
- MVP data strategy: user-entered profile/diagnosis + seeded sample dataset (no heavy external data ingestion)

### Must NOT Have (Guardrails)
- No full LMS build (courses, classroom management, complex content authoring)
- No job-placement/recruiting guarantees or legal/medical advice behavior
- No full gstack framework recreation or vendoring
- No extra infra (queues/vector DB/microservices) unless explicitly required by blocker
- No resume file parsing pipeline in MVP v1

---

## Verification Strategy (MANDATORY)

> **ZERO HUMAN INTERVENTION** — all verification is agent-executed.

### Test Decision
- **Infrastructure exists**: NO (assume bootstrap needed)
- **Automated tests**: Tests-after (per-slice immediate verification)
- **Framework**: Vitest + Playwright

### QA Policy
- **Frontend/UI**: Playwright flows + screenshots
- **API/Backend**: curl checks + JSON assertions
- **DB/Security**: Supabase policy verification scripts
- **Evidence**: Save artifacts to `.sisyphus/evidence/task-{N}-{slug}.*`

---

## Execution Strategy

### gstack Pattern Adoption (Selected for this project)

> Full fork is excluded. The following are **pattern transplants** only.

1. **Workflow Command Set (office-hours style)**
   - Define project-local prompt commands for: discovery, plan review, QA, release check.
   - Command intent examples:
     - `office-hours`: problem framing + scope guardrail questions
     - `plan-review`: architecture/risk checklist against current TODO wave
     - `qa-run`: execute mandatory scenario bundle and capture evidence
     - `ship-check`: pre-submit audit (tests, secrets, deadline-safe commits)

2. **WSL Agent Ops Pack**
   - Standardize WSL-only execution rules, tmux session naming, and artifact paths.
   - Require all verification commands to run inside WSL and save logs under `.sisyphus/evidence/`.

3. **Verification/Release Checklist**
   - Add a single pre-submit checklist for contest constraints:
     - public repo visibility
     - no API key leakage
     - deadline cutoff commit audit
     - live URL health check
     - AI report completeness

4. **Document Template Bundle**
   - Prepare reusable templates for:
     - AI collaboration report
     - demo script (3-minute + fallback script)
     - judging criteria mapping table (technical completeness, AI efficiency, planning, creativity)

### Parallel Execution Waves

Wave 1 (Foundation — start immediately):
- T1. Project scaffold + WSL runbook
- T2. Design tokens + senior-friendly UI baseline
- T3. Data model + Supabase schema + RLS baseline
- T4. Auth + session foundation
- T5. Test/bootstrap setup (Vitest + Playwright + evidence dirs)

Wave 2 (Core features — max parallel after Wave 1):
- T6. Onboarding + diagnosis flow UI
- T7. AI coaching service contract + fallback handling
- T8. Personalized weekly plan engine
- T9. Weekly feedback capture + progress timeline
- T10. API endpoints + server actions integration

Wave 3 (Integration + contest readiness):
- T11. End-to-end stitching + UX hardening
- T12. Contest artifact pack (AI report inputs, screenshots, demo script)
- T13. Workflow command templates (office-hours/plan-review/qa-run/ship-check)
- T14. WSL ops standard (tmux/session/logging rules)
- T15. Pre-submit release checklist enforcement
- T16. Document template bundle finalization

Wave FINAL (parallel review gate):
- F1 Plan compliance audit
- F2 Code quality review
- F3 Real QA execution
- F4 Scope fidelity check

Critical Path: T1 → T4 → T8 → T10 → T11 → T12 → F1-F4
Max Concurrent: 5

### Dependency Matrix (full)
- T1: Blocked By none | Blocks T6,T10,T11,T12
- T2: Blocked By none | Blocks T6,T11
- T3: Blocked By none | Blocks T8,T9,T10
- T4: Blocked By T1,T3 | Blocks T6,T10,T11
- T5: Blocked By T1 | Blocks T11,T12,F2,F3
- T6: Blocked By T2,T4 | Blocks T11
- T7: Blocked By T1 | Blocks T8,T10,T11
- T8: Blocked By T3,T7 | Blocks T10,T11
- T9: Blocked By T3 | Blocks T10,T11
- T10: Blocked By T1,T3,T4,T7,T8,T9 | Blocks T11,T12
- T11: Blocked By T2,T4,T5,T6,T7,T8,T9,T10 | Blocks T12,F1,F2,F3,F4
- T12: Blocked By T1,T5,T10,T11 | Blocks F1,F3
- T13: Blocked By T1,T5 | Blocks T15,T16,F1
- T14: Blocked By T1 | Blocks T15,F2,F3
- T15: Blocked By T5,T10,T12,T13,T14 | Blocks F1,F3
- T16: Blocked By T12,T13 | Blocks F1

### Agent Dispatch Summary
- Wave 1: T1/T5=`quick`, T2=`visual-engineering`, T3/T4=`unspecified-high`
- Wave 2: T6=`visual-engineering`, T7/T8=`deep`, T9/T10=`unspecified-high`
- Wave 3: T11=`deep`, T12=`writing`
- Final: F1=`oracle`, F2=`unspecified-high`, F3=`unspecified-high`(+playwright), F4=`deep`

---

## TODOs

- [ ] 1. Project scaffold + WSL runbook

  **What to do**:
  - Initialize Next.js app structure and WSL-first command conventions.
  - Add `.sisyphus/evidence/` substructure and command index.

  **Must NOT do**:
  - No infra beyond local + hosted Supabase baseline.

  **Recommended Agent Profile**:
  - Category: `quick` (initial project setup)
  - Skills: `[]`

  **Parallelization**: YES (Wave 1) | Blocked By: None | Blocks: T6,T10,T11,T12

  **References**:
  - `https://github.com/garrytan/gstack` - process inspiration only (no full adoption)
  - `https://nextjs.org/docs` - app scaffolding and runtime patterns

  **Acceptance Criteria**:
  - `npm run dev` in WSL starts app successfully
  - Evidence: `.sisyphus/evidence/task-1-wsl-dev-start.txt`

  **QA Scenarios**:
  - Happy path: Run app in WSL, HTTP 200 on home route, capture terminal log.
  - Failure path: Missing env var triggers clear startup error message.

- [ ] 2. Senior-friendly design baseline

  **What to do**:
  - Define typography scale, spacing, contrast baseline, and simple navigation pattern.
  - Create reusable UI primitives for large tap targets and readable copy.

  **Must NOT do**:
  - No pixel-perfect over-design or non-functional visual scope creep.

  **Recommended Agent Profile**:
  - Category: `visual-engineering`
  - Skills: `[]`

  **Parallelization**: YES (Wave 1) | Blocked By: None | Blocks: T6,T11

  **References**:
  - `https://www.w3.org/WAI/WCAG22/quickref/` - measurable accessibility baseline

  **Acceptance Criteria**:
  - Core screens pass keyboard navigation and visible focus checks.
  - Evidence: `.sisyphus/evidence/task-2-a11y-baseline.png`

  **QA Scenarios**:
  - Happy path: Keyboard-only tab order reaches major controls in sequence.
  - Failure path: Force 200% zoom; verify no critical controls disappear.

- [ ] 3. Supabase schema + RLS baseline

  **What to do**:
  - Define tables for profile, diagnosis, weekly_plan, feedback_log.
  - Apply row-level security policies for strict per-user isolation.

  **Must NOT do**:
  - No shared public write paths for user coaching data.

  **Recommended Agent Profile**:
  - Category: `unspecified-high`
  - Skills: `[]`

  **Parallelization**: YES (Wave 1) | Blocked By: None | Blocks: T8,T9,T10

  **References**:
  - `https://supabase.com/docs/guides/database/postgres/row-level-security`

  **Acceptance Criteria**:
  - Policy test script shows user A cannot read user B rows.
  - Evidence: `.sisyphus/evidence/task-3-rls-check.json`

  **QA Scenarios**:
  - Happy path: Authenticated user reads own records only.
  - Failure path: Cross-user query returns 0 rows/permission denial.

- [ ] 4. Auth/session foundation

  **What to do**:
  - Implement sign-in/out and protected app shell routing.
  - Ensure session expiry handling with re-auth prompt.

  **Must NOT do**:
  - No social login expansion in MVP unless blocking.

  **Recommended Agent Profile**:
  - Category: `unspecified-high`
  - Skills: `[]`

  **Parallelization**: YES (Wave 1) | Blocked By: T1,T3 | Blocks: T6,T10,T11

  **References**:
  - `https://supabase.com/docs/guides/auth`

  **Acceptance Criteria**:
  - Protected routes inaccessible without session.
  - Evidence: `.sisyphus/evidence/task-4-auth-flow.png`

  **QA Scenarios**:
  - Happy path: Login succeeds and redirects to dashboard.
  - Failure path: Expired token forces clean re-login state.

- [ ] 5. Test and evidence harness bootstrap

  **What to do**:
  - Configure Vitest and Playwright.
  - Add standard artifact save paths for CI/local runs.

  **Must NOT do**:
  - No brittle one-off scripts without reusable command wrappers.

  **Recommended Agent Profile**:
  - Category: `quick`
  - Skills: `[]`

  **Parallelization**: YES (Wave 1) | Blocked By: T1 | Blocks: T11,T12,F2,F3

  **References**:
  - `https://playwright.dev/docs/intro`
  - `https://vitest.dev/guide/`

  **Acceptance Criteria**:
  - `npm run test` and `npx playwright test` execute baseline suites.
  - Evidence: `.sisyphus/evidence/task-5-test-bootstrap.txt`

  **QA Scenarios**:
  - Happy path: Test commands pass on clean run.
  - Failure path: Introduced failing spec correctly fails CI command.

- [ ] 6. Onboarding + diagnosis flow

  **What to do**:
  - Build mid-career onboarding form (role, goal, weekly hours).
  - Add diagnosis questionnaire and score normalization.

  **Must NOT do**:
  - No long survey (>5 minutes) in MVP.

  **Recommended Agent Profile**:
  - Category: `visual-engineering`
  - Skills: `[]`

  **Parallelization**: YES (Wave 2) | Blocked By: T2,T4 | Blocks: T11

  **References**:
  - `## Context > Interview Summary` (this plan file) - persona and scope decisions

  **Acceptance Criteria**:
  - User completes onboarding + diagnosis in <= 5 screens.
  - Evidence: `.sisyphus/evidence/task-6-onboarding-flow.mp4`

  **QA Scenarios**:
  - Happy path: Valid inputs produce diagnosis result page.
  - Failure path: Empty required fields show clear inline errors.

- [ ] 7. AI coaching service contract + fallback

  **What to do**:
  - Define prompt contract for weekly plan generation.
  - Add timeout/empty response fallback templates.

  **Must NOT do**:
  - No opaque black-box output without explainable section labels.

  **Recommended Agent Profile**:
  - Category: `deep`
  - Skills: `[]`

  **Parallelization**: YES (Wave 2) | Blocked By: T1 | Blocks: T8,T10,T11

  **References**:
  - `https://platform.openai.com/docs/guides/prompt-engineering`

  **Acceptance Criteria**:
  - Failed AI call returns safe fallback weekly template.
  - Evidence: `.sisyphus/evidence/task-7-ai-fallback.json`

  **QA Scenarios**:
  - Happy path: AI response parsed into expected structured sections.
  - Failure path: Provider timeout triggers fallback in <5s with user-friendly copy.

- [ ] 8. Personalized weekly plan engine

  **What to do**:
  - Convert diagnosis + goals into weekly plan blocks.
  - Persist plan versions and regenerate logic.

  **Must NOT do**:
  - No fully autonomous irreversible writes without user confirm.

  **Recommended Agent Profile**:
  - Category: `deep`
  - Skills: `[]`

  **Parallelization**: YES (Wave 2) | Blocked By: T3,T7 | Blocks: T10,T11

  **References**:
  - `https://supabase.com/docs/guides/database`

  **Acceptance Criteria**:
  - Regeneration creates new version while preserving old history.
  - Evidence: `.sisyphus/evidence/task-8-plan-versioning.txt`

  **QA Scenarios**:
  - Happy path: User gets generated plan with weekly tasks and rationale.
  - Failure path: Invalid diagnosis payload is rejected with explicit validation error.

- [ ] 9. Weekly feedback and progress timeline

  **What to do**:
  - Build check-in form and timeline visualization of weekly progress.
  - Compute simple trend (improving/stable/at-risk).

  **Must NOT do**:
  - No predictive claims beyond measured user-entered data.

  **Recommended Agent Profile**:
  - Category: `unspecified-high`
  - Skills: `[]`

  **Parallelization**: YES (Wave 2) | Blocked By: T3 | Blocks: T10,T11

  **References**:
  - `## Work Objectives > Must Have` (this plan file) - weekly feedback requirement source

  **Acceptance Criteria**:
  - User can submit weekly status and view timeline changes.
  - Evidence: `.sisyphus/evidence/task-9-weekly-feedback.png`

  **QA Scenarios**:
  - Happy path: New check-in updates timeline immediately.
  - Failure path: Duplicate same-day submission handled with merge/update rule.

- [ ] 10. API/server integration layer

  **What to do**:
  - Connect UI flows to backend actions/endpoints.
  - Ensure auth checks and validation at boundaries.

  **Must NOT do**:
  - No direct client-side privileged DB access.

  **Recommended Agent Profile**:
  - Category: `unspecified-high`
  - Skills: `[]`

  **Parallelization**: YES (Wave 2) | Blocked By: T1,T3,T4,T7,T8,T9 | Blocks: T11,T12

  **References**:
  - `https://nextjs.org/docs/app/building-your-application/data-fetching`

  **Acceptance Criteria**:
  - Protected API calls fail with 401 when unauthenticated.
  - Evidence: `.sisyphus/evidence/task-10-api-auth.txt`

  **QA Scenarios**:
  - Happy path: Authenticated calls return expected schema.
  - Failure path: Malformed payload returns 400 with field-level error.

- [ ] 11. End-to-end integration + UX hardening

  **What to do**:
  - Stitch full journey and remove blocking UX friction.
  - Add accessibility hardening for keyboard, focus, zoom, readable messages.

  **Must NOT do**:
  - No unrelated feature expansion (marketplace, community, enterprise admin).

  **Recommended Agent Profile**:
  - Category: `deep`
  - Skills: `[]`

  **Parallelization**: NO (Wave 3) | Blocked By: T2,T4,T5,T6,T7,T8,T9,T10 | Blocks: T12,F1,F2,F3,F4

  **References**:
  - `https://www.w3.org/WAI/WCAG22/quickref/`

  **Acceptance Criteria**:
  - Full user journey passes in Playwright E2E run.
  - Evidence: `.sisyphus/evidence/task-11-e2e-suite.txt`

  **QA Scenarios**:
  - Happy path: Login → diagnosis → plan → feedback → timeline, all pass.
  - Failure path: Simulated AI timeout shows fallback and keeps workflow recoverable.

- [ ] 12. Contest submission artifact pack

  **What to do**:
  - Prepare AI report inputs, architecture summary, and evidence index.
  - Prepare demo script aligned with judging criteria.

  **Must NOT do**:
  - No unverifiable claims without evidence path.

  **Recommended Agent Profile**:
  - Category: `writing`
  - Skills: `[]`

  **Parallelization**: NO (Wave 3 end) | Blocked By: T1,T5,T10,T11 | Blocks: F1,F3

  **References**:
  - Contest submission requirements from announcement (GitHub URL, Live URL, AI report PDF, signed forms)

  **Acceptance Criteria**:
  - Artifact checklist complete with links and evidence mapping table.
  - Evidence: `.sisyphus/evidence/task-12-submission-checklist.md`

  **QA Scenarios**:
  - Happy path: All required submission items present and accessible.
  - Failure path: Missing item detection script flags incomplete package.

---

## Final Verification Wave (MANDATORY)

- [ ] F1. **Plan Compliance Audit** — `oracle`
  Output: `Must Have [N/N] | Must NOT Have [N/N] | Tasks [N/N] | VERDICT`

- [ ] F2. **Code Quality Review** — `unspecified-high`
  Output: `Build [PASS/FAIL] | Lint [PASS/FAIL] | Tests [N/N] | VERDICT`

- [ ] F3. **Real Manual QA (agent-executed)** — `unspecified-high` (+playwright)
  Output: `Scenarios [N/N pass] | Integration [N/N] | Edge Cases [N] | VERDICT`

- [ ] F4. **Scope Fidelity Check** — `deep`
  Output: `Tasks [N/N compliant] | Contamination [CLEAN/N issues] | VERDICT`

---

## Commit Strategy

- C1: `chore(setup): bootstrap nextjs wsl workflow and evidence structure`
- C2: `feat(auth-db): add supabase schema rls and auth shell`
- C3: `feat(coach): implement diagnosis and weekly plan generation with fallback`
- C4: `feat(progress): add weekly feedback and timeline`
- C5: `test(e2e): integrate playwright flows and accessibility checks`
- C6: `docs(contest): finalize submission artifact mapping`

---

## Success Criteria

### Verification Commands
```bash
npm run lint         # Expected: 0 errors
npm run test         # Expected: all unit/integration tests pass
npx playwright test  # Expected: all e2e scenarios pass
```

### Final Checklist
- [ ] All Must-Have flows demonstrated
- [ ] Must-NOT-Have boundaries respected
- [ ] Accessibility baseline evidence present
- [ ] RLS/user isolation proof present
- [ ] Contest artifact package complete
