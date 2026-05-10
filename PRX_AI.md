# AI Workflow Setup — Bootstrap Instructions

> **What this file does:** When executed by GitHub Copilot, it analyzes your complete repository and installs a 3-phase AI-controlled software delivery workflow (Plan → Review → Execute) tailored to your specific codebase.
>
> **How to trigger it:** Add this file to the root of any repository, then tell Copilot:
> `"Execute PRX_AI.md"`
>
> **After setup is complete**, run any task through the workflow with:
> `"[Your task description], follow workflow-instructions.md"`

---

## What Gets Created

| File | Purpose |
|------|---------|
| `COPILOT_RULEBOOK.md` | Your project's source of truth — architecture, patterns, naming conventions, absolute rules. Generated from live codebase analysis. |
| `docs/ai-workflow/workflow-instructions.md` | The governing workflow document. Defines the auto-execution protocol and all phase contracts. |
| `docs/ai-workflow/phase-1-architect.md` | Phase 1 role: Senior Architect. Plans every change before any code is written. |
| `docs/ai-workflow/phase-2-reviewer.md` | Phase 2 role: Senior Reviewer. Approves or rejects the plan before implementation begins. |
| `docs/ai-workflow/phase-3-executor.md` | Phase 3 role: Senior Executor. Implements exactly what Phase 2 approved. Nothing more. |

---

---

# ══════════════════════════════════════════════════════
# EXECUTION INSTRUCTIONS FOR COPILOT
# ══════════════════════════════════════════════════════

> **Copilot: When you read this file, execute every step below sequentially and automatically.**
> Do not ask for confirmation between steps. Do not skip any step.
> You are the agent executing this bootstrap. Complete all 6 steps and report when done.

---

## STEP 1 — Deep Repository Analysis

Perform a complete, systematic analysis of this repository. Read every file that is relevant to architecture, patterns, and conventions. The goal is to extract the ground truth of how this codebase works so that `COPILOT_RULEBOOK.md` reflects the actual code — not assumptions.

### 1.1 — Identify the Project Type and Tech Stack

Inspect the following files (if they exist):
- `package.json` / `package-lock.json` / `yarn.lock` / `pnpm-lock.yaml` / `Pipfile` / `pyproject.toml` / `go.mod` / `pom.xml` / `build.gradle` / `Gemfile` / `Cargo.toml`
- `Dockerfile` / `docker-compose.yml` / `docker-compose.*.yml`
- `*.config.js` / `*.config.ts` / `vite.config.*` / `webpack.config.*` / `tsconfig.json` / `jsconfig.json`
- `README.md` / `CONTRIBUTING.md`

Extract and document:
- Programming language(s) and version(s)
- Runtime(s) (Node.js, Python, JVM, Go, etc.)
- Framework(s) (Express, FastAPI, Spring, Rails, Next.js, NestJS, etc.)
- Module system (CommonJS `require`, ES Modules `import`, Python modules, etc.)
- Database(s) (MongoDB, PostgreSQL, MySQL, Redis, etc.) and ORM/ODM (Mongoose, Prisma, SQLAlchemy, etc.)
- Frontend framework (React, Vue, Angular, Svelte, etc.) and version
- State management (Redux, Zustand, MobX, Pinia, NgRx, Context API, etc.)
- HTTP client library (axios, fetch, superagent, requests, etc.)
- Testing framework(s) (Jest, Vitest, pytest, JUnit, RSpec, etc.)
- Build tooling (Vite, webpack, Rollup, esbuild, Maven, Gradle, etc.)
- Containerisation / deployment infrastructure
- Styling system (CSS Modules, SCSS, Tailwind, Styled-Components, etc.)

### 1.2 — Map the Repository Structure

List every top-level directory and its purpose. For each key directory, list its contents and the layer it belongs to (entry point, routes, controllers, services, models, middleware, utils, tests, pages, components, features, store, etc.).

Identify:
- Where does the application start? (entry point file)
- Where are routes/endpoints defined?
- Where does business logic live?
- Where are data models/schemas?
- Where are tests?
- Where is configuration loaded?
- Where are utilities/helpers?
- What is the module boundary between concerns?

### 1.3 — Extract Architecture Patterns

Read the actual source files to extract the real patterns used. For each of the following, find 2–3 concrete examples from the codebase:

**Server/Backend patterns (if applicable):**
- How are routes registered? (file names, router creation, middleware application)
- How are request handlers structured? (is there a decorator/wrapper pattern? controller delegation?)
- How is business logic separated from route handling? (controllers vs services vs handlers)
- How are errors thrown and handled? (custom error classes? error middleware? try/catch? Result types?)
- How is logging done? (which library, what format, where is the logger imported)
- How is validation done? (which library, where are validators, how are they applied)
- How is authentication enforced? (middleware, guards, decorators, interceptors)
- How is the database accessed? (ORM/query builder pattern, repository pattern, direct model access)
- How are environment variables loaded and used? (central config file, direct `process.env`, etc.)
- How is the response shaped? (standard envelope? plain data? custom transformers?)

**Frontend/Client patterns (if applicable):**
- How are pages/views structured? (routing, lazy loading, code splitting)
- How are components organized? (flat, feature-based, atomic design, etc.)
- How is state managed? (Redux slices, Zustand stores, Pinia stores, context providers)
- How are API calls made? (shared axios/fetch instance? interceptors? service layer? hooks?)
- How is async logic handled? (Redux-Saga, Thunks, React Query, SWR, hooks with useEffect?)
- How is styling applied? (co-located modules, global sheets, utility classes, theme system?)
- How are hooks organized? (custom hooks directory, co-located hooks, naming convention?)
- How are forms handled? (React Hook Form, Formik, manual controlled state?)

### 1.4 — Extract Naming Conventions

Read files across different directories to extract exact naming conventions. Do not assume — read real examples.

Document:
- File naming: kebab-case, camelCase, PascalCase, snake_case, what suffixes are used
- Directory/folder naming: what casing, what structure
- Variable naming: camelCase, snake_case, SCREAMING_SNAKE_CASE for what
- Function/method naming: naming prefixes (handle, get, fetch, on, create), verb conventions
- Class naming: PascalCase? what suffixes (Controller, Service, Manager, Handler)?
- Test file naming: `*.test.js`, `*.spec.ts`, `test_*.py`, `*_test.go`
- Export patterns: named exports vs default exports, module.exports style
- CSS/SCSS class naming: BEM, camelCase, utility-first?
- Redux/State action naming: `feature/ACTION_VERB`, `SCREAMING_SNAKE_CASE`?

### 1.5 — Extract Security Patterns

Read middleware, auth-related files, and validators to document:
- How is auth implemented? (session, JWT, OAuth, API key, cookie?)
- What security headers are applied? (helmet, custom headers)
- How is rate limiting applied? (which middleware, where, limits)
- How is input validation enforced? (which library, which files, scope)
- How are secrets managed? (env vars, config files, vault)
- How are sensitive fields protected? (field exclusion in queries, serialisation transforms)
- What CORS policy is applied?
- How are errors surfaced to clients? (generic messages, structured errors, stack traces?)

### 1.6 — Extract Test Patterns

Read test files to document:
- Test structure: describe/it, class-based, flat functions
- Setup pattern: beforeAll, beforeEach, fixtures, factories
- Mocking pattern: how are modules/dependencies mocked?
- Assertion library: jest expect, chai, pytest assert, etc.
- Integration test pattern: supertest, TestClient, Rack::Test?
- What is NOT tested (test coverage gaps to note)
- Test file location convention

### 1.7 — Identify Critical Integration Points

Document:
- External APIs or downstream services being called (from where?)
- Message queues, event buses (if any)
- Shared utilities used across many modules
- The request lifecycle from entry to response (middleware order matters)
- Session/token lifecycle

---

## STEP 2 — Generate `COPILOT_RULEBOOK.md`

Using everything discovered in Step 1, create `COPILOT_RULEBOOK.md` in the **root of the repository**.

This file is the **single source of truth** for the AI workflow. Every phase (1, 2, 3) reads it before doing anything. It must be derived from the actual codebase — not invented.

Use the following structure. Fill every section with real findings from Step 1. Where a section does not apply to this project, keep the heading and write "N/A — [reason]".

```markdown
# [Project Name] — Copilot Rulebook & Codebase Analysis

> Generated: [Date]
> Scope: Complete repository audit — [describe what was analyzed]
> Source of truth: Live codebase analysis. All rules are derived from the actual code.

---

# 1. Executive Summary

[One paragraph: what is this application? What is the architecture pattern (MVC, BFF, microservices, monolith, API, CLI, etc.)? What is the core invariant that must never be violated? What does the application do?]

---

# 2. Tech Stack

## Backend / Server

| Dimension | Value | Evidence |
|-----------|-------|---------|
| Runtime | | |
| Framework | | |
| Language | | |
| Module system | | |
| Database | | |
| ORM / Query builder | | |
| Auth mechanism | | |
| Password hashing (if any) | | |
| Logging | | |
| Validation library | | |
| Security middleware | | |
| HTTP client (for downstream calls) | | |
| Testing | | |

## Frontend / Client (if applicable)

| Dimension | Value | Evidence |
|-----------|-------|---------|
| Framework | | |
| Language | | |
| Module system | | |
| Build tool | | |
| State management | | |
| Routing | | |
| HTTP client | | |
| Styling system | | |
| Testing | | |

## Infrastructure

| Dimension | Value | Evidence |
|-----------|-------|---------|
| Containerisation | | |
| Environments | | |

---

# 3. Repository Structure

[Complete annotated directory tree showing every significant file and its purpose]

---

# 4. Architecture

## 4.1 System Diagram

[ASCII diagram showing the full request/data flow from client through layers to database/external services]

## 4.2 Layer Responsibilities

[Table: Layer | File(s) | Responsibility | Boundary — what this layer must NOT do]

## 4.3 Request Data Flow

[Step-by-step walkthrough of a typical request from entry to response for the most representative use case]

## 4.4 Module/Package Dependency Rules

[Which layers can depend on which — dependency direction constraints]

---

# 5. API Endpoints / Routes (if applicable)

[Complete list of all endpoints: method, path, auth required, validation, description, response shape]

## Standard Response Envelope (if applicable)

[The exact shape of success and error responses — must never be invented or varied]

---

# 6. Data Models / Schemas (if applicable)

[Every model/schema with all fields, types, constraints, sensitive field rules, and key behaviors]

---

# 7. State Management (if applicable)

[Store shape, slice/feature structure, action naming convention, async effect pattern]

---

# 8. Middleware / Request Pipeline

[The complete middleware/interceptor chain in execution order — do not reorder without full understanding]

---

# 9. Key Patterns — Complete Code Examples

## Pattern: [Most important server/backend pattern]

[Complete, real code example from the codebase or exact pattern to follow]

## Pattern: [Route/endpoint registration]

[Complete example]

## Pattern: [Error throwing in business logic]

[Complete example]

## Pattern: [Logging]

[Complete example]

## Pattern: [Validation]

[Complete example]

## Pattern: [Client/Frontend state action]

[Complete example — if applicable]

## Pattern: [Client/Frontend async effect]

[Complete example — if applicable]

## Pattern: [Client/Frontend API service call]

[Complete example — if applicable]

---

# 10. Error Handling

## Error Class Hierarchy

[Diagram of error classes and their inheritance]

## Error Flow

[How errors travel from where they are thrown to the client response]

## Error Code Reference

[Table: Error code | HTTP status | When used]

---

# 11. Security Implementation

## Implemented Controls

[Table: Control | Implementation | Location]

## Non-Negotiable Security Rules

[Numbered list of security rules that must never be violated in this project]

---

# 12. Logging

[Logger configuration, log levels, transports, request ID propagation, redacted fields]

---

# 13. Environment Variables / Configuration

[Table: Variable | Required | Default | Description]

---

# 14. Testing

## Test Patterns

[Framework, test file location, describe/it structure, mocking approach, integration test setup]

## Standard Server/Backend Test Pattern

[Complete code example of the standard test pattern]

## Standard Frontend Test Pattern (if applicable)

[Complete code example]

## Test Requirements Per Change Type

| Change type | Required test | Location |
|-------------|-------------|----------|
| New endpoint/route | | |
| New component | | |
| New reducer/store slice | | |
| New async effect | | |
| New service | | |

---

# 15. Styling System (if applicable)

[SCSS/CSS architecture, design tokens, naming conventions, co-location rules]

---

# 16. Naming Conventions

## Backend / Server

| Entity | Convention | Example |
|--------|-----------|---------|
| Files | | |
| Variables / functions | | |
| Classes | | |
| Constants | | |
| Export style | | |

## Frontend / Client (if applicable)

| Entity | Convention | Example |
|--------|-----------|---------|
| Component files | | |
| Component folders | | |
| Feature folders | | |
| Service files | | |
| Hook files | | |
| Style module files | | |
| Action type constants | | |
| Action creators | | |
| Event handlers | | |

---

# 17. Copilot Rulebook

## A. Absolute Rules (Never Violate)

[Derived from the architecture. These are the hard constraints — violating any of them produces broken or insecure code. Examples:]
1. [BFF/boundary rule if applicable]
2. [Error handling rule — must use typed error classes, not raw Error()]
3. [Logging rule — must use shared logger, not console.log]
4. [Async rule — must use established async pattern, not ad-hoc]
5. [Validation rule — all input must be validated server-side]
6. [Auth rule — protected routes must use auth middleware]
7. [Module system rule — never mix module systems]
8. [Secrets rule — never commit secrets]
9. [Response envelope rule — never bypass the standard response shape]
10. [Stack trace rule — never expose err.stack in API responses]

## B. File Placement Rules

### Backend

| Code type | Location |
|-----------|---------|
| New route/endpoint file | |
| New controller/handler | |
| New service/business logic | |
| New data access layer | |
| New validator | |
| New error type | |
| New constant | |
| New config value | |
| New model/schema | |
| New middleware | |
| New test | |

### Frontend (if applicable)

| Code type | Location |
|-----------|---------|
| New page/view | |
| New shared component | |
| New state feature/slice | |
| New API service | |
| New hook | |
| New utility | |
| New style file | |
| New test | |

## C. Complete Code Patterns

[Reproduce the exact patterns from Section 9 here for easy reference during implementation]

## D. Anti-Patterns — What NOT to Do

| Anti-pattern | Correct approach |
|-------------|-----------------|
| [Pattern from codebase] | |
| [Pattern from codebase] | |
| [Pattern from codebase] | |
| [Pattern from codebase] | |
| [Pattern from codebase] | |

## E. Architecture Rules

### Backend
[Numbered list of architecture rules derived from how the codebase is structured]

### Frontend (if applicable)
[Numbered list of architecture rules derived from how the codebase is structured]

## F. Security Rules (Non-Negotiable)

[Numbered list of security rules specific to this codebase's implementation]

## G. Documentation Rules

When any of the following change, update the corresponding doc:

| Change | Document to update |
|--------|-------------------|
| New or changed endpoint | |
| New env variable | |
| New file added | |
| Significant architectural decision | |

---

# 18. Development Workflows

[How to run the project locally, how to run tests, how to build for production, relevant commands]

---

# 19. Known Design Notes and Limitations

[Any important context, technical debt, legacy decisions, or non-obvious behaviors that an implementor must know]
```

---

## STEP 3 — Create `docs/ai-workflow/workflow-instructions.md`

Create the directory `docs/ai-workflow/` if it does not exist.

Create `docs/ai-workflow/workflow-instructions.md` with the following exact content (replace `[PROJECT NAME]` with the actual project name from Step 1):

```markdown
# Workflow Instructions

> **Purpose:** Define the complete operating workflow for the 3-phase software delivery system used in the [PROJECT NAME] repository.
> **Governing Document:** `COPILOT_RULEBOOK.md` — **mandatory compliance in every phase, every handoff, every decision. This workflow exists to enforce it.**

---

## 0. Foundational Mandate

This workflow exists for one reason: **to guarantee that every code change in this repository is planned before it is reviewed, reviewed before it is implemented, and always consistent with `COPILOT_RULEBOOK.md`.**

No exceptions. No shortcuts. No phase may be skipped.

Every participant in this workflow — whether human or AI agent — must read `COPILOT_RULEBOOK.md` before executing their phase. It is not a background document. It is the governing law.

---

## 0A. Auto-Execution Protocol

> **This section governs automated workflow execution. When the trigger format is detected, the AI agent must execute all three phases sequentially and automatically — without waiting for human prompting between phases.**

### Trigger Format

Automated execution begins when a message matches this pattern:

```
[Task description or ticket content], follow workflow-instructions.md
```

**Examples:**
- `"Add input validation to the user endpoint", follow workflow-instructions.md`
- `"TASK-042: Implement password reset flow", follow workflow-instructions.md`
- `"Bug: Session not expiring on logout, follow workflow-instructions.md"`

On trigger detection: **do not ask any clarifying questions about the workflow. Begin Phase 1 immediately.**

---

### Auto-Execution Chain

```
TRIGGER DETECTED
        |
        v
+-------------------------------------------------------+
|  PHASE 1 — Read phase-1-architect.md                 |
|  Inspect codebase, produce Implementation Plan       |
|  Self-verify compliance checklist (Section 11)       |
+-------------------------------------------------------+
        |
        v
  Output header:
  "## PHASE 1 COMPLETE — Automatically starting Phase 2 Review..."
        |
        v
+-------------------------------------------------------+
|  PHASE 2 — Read phase-2-reviewer.md                  |
|  Review Phase 1 plan against COPILOT_RULEBOOK.md     |
|  Execute all review steps                            |
+-------------------------------------------------------+
        |
   +----+--------------------+
   |                         |
REJECTED                 APPROVED
   |                         |
   v                         v
Output header:          Output header:
"## PHASE 2 REJECTED    "## PHASE 2 APPROVED —
 — Revising Plan        Automatically starting
 (Revision vN)..."      Phase 3 Implementation..."
   |                         |
   v                         v
+-------------------+   +-------------------------------------------------------+
|  PHASE 1 REVISION |   |  PHASE 3 — Read phase-3-executor.md                  |
|  Address every    |   |  Implement the Final Approved Implementation Plan     |
|  blocking issue   |   |  Produce Implementation Summary                       |
|  from Phase 2     |   +-------------------------------------------------------+
|  Mark plan as vN  |                    |
+-------------------+                    v
        |                    Output header:
        v                    "## PHASE 3 COMPLETE — Implementation finished."
  Loop to Phase 2
  (auto-review revised plan)
```

---

### Phase Transition Headers

At every automatic transition, output a clearly visible header **before** starting the next phase.

| Transition | Header to output |
|---|---|
| Phase 1 complete → Phase 2 | `## PHASE 1 COMPLETE — Automatically starting Phase 2 Review...` |
| Phase 2 rejected → Phase 1 revision | `## PHASE 2 REJECTED — Automatically revising plan (Revision vN)...` |
| Phase 2 approved → Phase 3 | `## PHASE 2 APPROVED — Automatically starting Phase 3 Implementation...` |
| Phase 3 complete | `## PHASE 3 COMPLETE — Implementation finished.` |

---

### Revision Cycle Tracking

- Initial plan = **v1**
- First revision after rejection = **v2**
- Maximum automatic revisions = **3**

If Phase 2 rejects the v3 plan, **stop the automatic loop** and output:

```
## AUTO-EXECUTION PAUSED — Maximum revision cycles reached.

Phase 2 has rejected 3 consecutive plan versions.
Automatic execution cannot continue without human review.

Please review the Phase 2 blocking issues below and provide guidance:

[List all current Phase 2 blocking issues here]
```

---

### Non-Interruption Rule

Once auto-execution is triggered, the agent must **not** pause between phases to ask:
- "Should I proceed to Phase 2?"
- "Do you want me to start the review?"
- "Should I implement this now?"
- "Do you approve of this plan?"

**These questions are forbidden during auto-execution.**

**The only permitted pause points are:**
1. Maximum revision cycles reached (3 rejections)
2. A structural deviation found during Phase 3 that the approved plan did not account for
3. An ambiguity in the approved plan that cannot be resolved without human input (Phase 3 only)

---

### Context Carrying Between Phases

| Handoff | What is carried |
|---|---|
| Phase 1 → Phase 2 | The complete Phase 1 Implementation Plan (all sections) |
| Phase 2 (rejected) → Phase 1 revision | The complete Phase 2 rejection document with all blocking issues |
| Phase 2 (approved) → Phase 3 | The complete Final Approved Implementation Plan from Phase 2 Section 5 |
| Phase 3 (deviation found) → human | The deviation report |

Do not summarize or truncate any document when passing it to the next phase.

---

## 1. Workflow Overview

This is a **3-phase, gate-controlled software delivery workflow**.

```
[Task / Feature Request]
        |
        v
+-------------------------------------------------------+
|  PHASE 1 - Senior Architect (phase-1-architect.md)    |
|  Input:  Task description + COPILOT_RULEBOOK.md       |
|  Output: Complete Implementation Plan                 |
|  Gate:   Self-verification checklist ("Ready: YES")   |
+-------------------------------------------------------+
        |
        v
[Phase 1 Output -> Phase 2 Input]
        |
        v
+-------------------------------------------------------+
|  PHASE 2 - Senior Reviewer (phase-2-reviewer.md)      |
|  Input:  Phase 1 plan + COPILOT_RULEBOOK.md           |
|  Output: Review document with APPROVED or REJECTED    |
|  Gate:   Explicit APPROVED decision required          |
+-------------------------------------------------------+
        |
   +----+----------+
   |               |
APPROVED        REJECTED
   |               |
   v               v
[Phase 2     [Return to Phase 1
 Final Plan   for revision]
 -> Phase 3]
   |
   v
+-------------------------------------------------------+
|  PHASE 3 - Senior Executor (phase-3-executor.md)      |
|  Input:  Phase 2 Final Approved Plan + COPILOT_RULEBOOK|
|  Output: Implemented code + Implementation Summary   |
|  Gate:   Compliance statement (all YES)               |
+-------------------------------------------------------+
        |
        v
[Implementation Complete]
```

---

## 2. Execution Order

1. Phase 1 — Senior Architect
2. Phase 2 — Senior Reviewer
3. Phase 3 — Senior Executor

**No phase may execute out of order. No phase may be skipped. No phase may run concurrently.**

---

## 3. Inputs and Outputs for Each Phase

### Phase 1 — Architect

| | Detail |
|---|---|
| **Required inputs** | Task description + repository access + `COPILOT_RULEBOOK.md` |
| **Mandatory reading** | `COPILOT_RULEBOOK.md` — full read before planning begins |
| **Governing file** | `phase-1-architect.md` |
| **Output** | Complete Implementation Plan |
| **Output gate** | Last section must say "Ready for Phase 2 Review: YES" |
| **Output format** | `Implementation Plan — [Task ID]: [Title]` |

### Phase 2 — Reviewer

| | Detail |
|---|---|
| **Required inputs** | Complete Phase 1 plan + repository access + `COPILOT_RULEBOOK.md` |
| **Mandatory reading** | `COPILOT_RULEBOOK.md` — full read before review begins |
| **Governing file** | `phase-2-reviewer.md` |
| **Output** | Review document with APPROVED or REJECTED |
| **If APPROVED** | Also outputs the `Final Approved Implementation Plan` in Section 5 |
| **If REJECTED** | Lists all blocking issues by ID with exact correction requirements |

### Phase 3 — Executor

| | Detail |
|---|---|
| **Required inputs** | Phase 2 approved plan + repository access + `COPILOT_RULEBOOK.md` |
| **Mandatory reading** | `COPILOT_RULEBOOK.md` — full read before implementation begins |
| **Governing file** | `phase-3-executor.md` |
| **Output** | All implemented code changes + Implementation Summary document |
| **Output gate** | Implementation Summary compliance statement must have all items YES |

---

## 4. Handoff Contract

### Handoff 1: Phase 1 → Phase 2

**Phase 1 must deliver:**
- A complete implementation plan with all required sections
- The "Ready for Phase 2 Review: YES" statement
- A completed compliance checklist
- No implementation code anywhere in the document

**Phase 2 must verify before beginning:**
- All required sections are present
- The "Ready" flag is explicitly YES

### Handoff 2: Phase 2 → Phase 3

**Phase 2 must deliver (if APPROVED):**
- An explicit `Decision: APPROVED` statement
- A `Final Approved Implementation Plan` reproduced in full in Section 5
- A `Final Recommendation for Phase 3` section

**Phase 2 must deliver (if REJECTED):**
- An explicit `Decision: REJECTED` statement
- Every blocking issue listed with ID, rule violated, and exact correction required

---

## 5. The Approval Gate

Phase 3 MAY proceed only when ALL of the following are true:

| Condition | Verification |
|---|---|
| Phase 2 review document exists | Present and complete |
| Decision is APPROVED | "APPROVED" appears explicitly |
| No blocking issues are listed | Section is empty or states "None identified" |
| Final Approved Implementation Plan is present | Section 5 of Phase 2 review |

Phase 3 must NOT proceed if ANY of the following are true:

| Failure Condition | Required Action |
|---|---|
| Phase 2 review does not exist | Stop. Wait for Phase 2 completion. |
| Decision is REJECTED | Stop. Return to Phase 1 for revision. |
| Decision is absent or ambiguous | Stop. Treat as REJECTED. |
| Blocking issues remain | Stop. Return to Phase 1 for revision. |

---

## 6. The Rejection Loop

When Phase 2 rejects:

1. Phase 1 receives the rejection with all blocking issues (BLK-1, BLK-2, ...)
2. Phase 1 revises the plan — addresses every blocking issue — marks as v2, v3, etc.
3. Phase 1 re-runs the compliance checklist
4. Phase 2 re-reviews the full plan — not just the previously failed sections
5. Loop repeats until APPROVED or maximum revisions reached

**Rules:**
- Phase 1 must address every blocking issue. Resolving 3 of 4 is still rejected.
- Phase 1 must not silently change unrelated sections.
- Phase 2 must run the full review on every resubmission.
- Phase 3 must not start during a rejection loop.

---

## 7. Mandatory Rules Across All Phases

### Rule 1 — COPILOT_RULEBOOK.md is the supreme governing document
No decision in Phase 1, 2, or 3 may contradict it.

### Rule 2 — No phase may be skipped
Phase 1 → Phase 2 → Phase 3. Always. No exceptions.

### Rule 3 — No phase may implement code
Only Phase 3 implements code. Phase 1 plans. Phase 2 reviews. Phase 3 executes.

### Rule 4 — Approvals must be explicit
The word "APPROVED" must appear explicitly in the Phase 2 decision.

### Rule 5 — Rejections must be actionable
Each rejection must list every blocking issue with ID, rule violated, and what must change.

### Rule 6 — Codebase inspection is mandatory in all phases
Every phase must inspect existing patterns before proposing or approving changes.

### Rule 7 — All documents must be complete
A plan with missing sections is not a plan. A review with no decision is not a review.

### Rule 8 — Pattern extension beats pattern creation
Prefer extending existing patterns. A new pattern requires justification in Phase 1 and approval in Phase 2.

### Rule 9 — Auto-execution is the default mode
When the trigger format from Section 0A is detected, execute all phases automatically without prompting.

---

## 8. Anti-Patterns to Avoid

| Anti-Pattern | Why It Is Forbidden |
|---|---|
| Skipping Phase 1 and going directly to Phase 3 | Unplanned, unreviewed, inconsistent code |
| Skipping Phase 2 and going directly to Phase 3 | Removes the gatekeeper; defects reach the codebase |
| Phase 3 implementing anything not in the approved plan | Introduces unreviewed code |
| Phase 2 issuing conditional approvals | Allows defective plans to reach Phase 3 |
| Phase 1 marking a plan "Ready" with open blockers | Wastes review cycles |
| Any phase assuming COPILOT_RULEBOOK.md is already known | It must be re-read at the start of every phase |
| Agent pausing between phases to ask "should I proceed?" | Violates Non-Interruption Rule |
| Phase 2 approving without reproducing the final plan | Leaves Phase 3 without a clear authoritative document |

---

## 9. Quick Reference

### Workflow Phases

| Phase | Role | Key Output | Gate |
|---|---|---|---|
| 1 | Senior Architect | Implementation Plan | "Ready: YES" |
| 2 | Senior Reviewer | Review + Decision | Explicit "APPROVED" or "REJECTED" |
| 3 | Senior Executor | Code + Implementation Summary | All compliance YES |

### Auto-Execution

| Item | Value |
|---|---|
| Trigger format | `[task description], follow workflow-instructions.md` |
| Phase transitions | Automatic — no human prompting |
| Rejection handling | Auto-revise up to 3 times, then pause for human |
| Pause conditions | Max revisions / Phase 3 structural deviation / Phase 3 genuine ambiguity |
```

---

## STEP 4 — Create `docs/ai-workflow/phase-1-architect.md`

Create `docs/ai-workflow/phase-1-architect.md` with the following exact content (replace `[PROJECT NAME]` and `[STACK SUMMARY]` with values from your Step 1 analysis):

```markdown
# Phase 1 — Senior Architect

> **Role:** Senior Software Architect
> **Mission:** Analyze the incoming task and produce a complete, reviewed-ready implementation plan. Do not write any implementation code.
> **Governing Document:** `COPILOT_RULEBOOK.md` — **mandatory compliance. Every decision in this phase must be verified against it.**

---

## 0. Pre-Flight Mandate

Before reading the task description, before thinking about solutions, before doing anything else:

1. **Open and read `COPILOT_RULEBOOK.md` in full.** This document is the single source of truth for this repository's architecture, naming conventions, folder structure, patterns, and code quality standards.
2. **Treat every rule in `COPILOT_RULEBOOK.md` as a hard constraint, not a guideline.** No proposed approach may violate it.
3. **Do not begin planning until you have confirmed you understand the rulebook's contents.**

---

## 1. Role Definition

You are acting as a **Senior Architect** for the [PROJECT NAME] project — [STACK SUMMARY]. Your sole responsibility in this phase is to:

- Deeply understand the problem
- Deeply understand the existing codebase
- Produce a precise, reviewable, implementation-ready plan
- **Never write implementation code**
- **Never make assumptions without stating them explicitly**
- **Never propose patterns that do not exist in the codebase** unless the existing patterns genuinely cannot accommodate the requirement

---

## 1B. Phase 1 — Rules and Goals

### Goals of Phase 1

| # | Goal | Success Condition |
|---|------|-------------------|
| G1 | Fully understand the requirement | Describe the task in one precise sentence with zero ambiguity |
| G2 | Map the requirement to the existing architecture | Every proposed change is anchored to an existing pattern in the codebase |
| G3 | Identify all affected files and layers | No file that will be touched is missing from the plan |
| G4 | Produce a reviewable, implementable plan | Phase 2 can evaluate it without needing to ask clarifying questions |
| G5 | Comply with `COPILOT_RULEBOOK.md` at every decision point | Plan passes the compliance checklist with no violations |
| G6 | Identify all risks, edge cases, and open questions | Nothing is hidden — if it is uncertain, it is stated explicitly |

### Absolute Rules — violation causes auto-rejection by Phase 2

1. **No implementation code.** You produce plans, not code. Pseudocode is permitted only if explicitly labeled.
2. **No invention of new patterns.** Every proposed pattern must already exist in the codebase. If no pattern exists, state why a new one is necessary.
3. **No assumption without declaration.** Every assumption must be explicitly listed.
4. **No skipping the codebase inspection.** You must inspect actual files before proposing changes.
5. **Never propose patterns that violate `COPILOT_RULEBOOK.md` Section 17A (Absolute Rules).** If you are unsure whether a proposed approach complies, re-read that section.
6. **You must complete the compliance checklist (Section 11) before submitting.** If any item fails, fix the plan.

### Behavioral Rules

7. **Challenge the requirement before you plan the solution.** If the task is ambiguous, flag it.
8. **Be explicit over implicit.** Say exactly which file, which function, which route path, which action type.
9. **State what you are NOT changing.** If you inspected a file and decided no change is needed, say so.
10. **Scope is a constraint, not a suggestion.** Do not add "nice to have" improvements.
11. **One plan per submission.** Make the architectural decision yourself and justify it. Presenting options is abdication of responsibility.

---

## 2. Responsibilities

- Thoroughly read and understand the task description
- Identify every ambiguity and state it explicitly
- Inspect the relevant sections of the codebase before proposing anything
- Map the requirement to the existing architecture per `COPILOT_RULEBOOK.md`
- Identify all files, modules, services, and routes that will be affected
- Propose a file-level implementation plan that strictly conforms to `COPILOT_RULEBOOK.md`
- Identify all risks, dependencies, edge cases, and open questions
- Produce a structured plan document that Phase 2 can evaluate and approve or reject

---

## 3. Mandatory Compliance with COPILOT_RULEBOOK.md

Every proposed change must be verified against the following sections of `COPILOT_RULEBOOK.md`:

| Rulebook Section | What to Verify |
|---|---|
| Absolute Rules (Section 17A) | All hard constraints for this project |
| File Placement Rules (Section 17B) | Correct folder, layer, and file suffix |
| Code Patterns (Section 17C) | The exact patterns to mirror |
| API Endpoints (Section 5, if applicable) | Envelope shape, validator presence, auth presence |
| Data Models (Section 6, if applicable) | Schema rules, sensitive field handling |
| State Management (Section 7, if applicable) | Store slices, action types, async effect registration |
| Security Rules (Section 17F) | Validators, rate limiters, auth middleware, sensitive fields |
| Naming Conventions (Section 16) | File naming, folder naming, variable naming, constants |
| Documentation Rules (Section 17G) | Which docs to update |

---

## 4. Task Analysis Protocol

### Step 4.1 — Read the Task Completely
- Read the full description and acceptance criteria
- Identify what the task is asking for in one precise sentence

### Step 4.2 — Identify Ambiguities
For each ambiguity:
- What is unclear
- What assumption you are making to proceed
- The risk if the assumption is wrong

### Step 4.3 — Classify the Work

**Backend/Server:**
- New endpoint / route
- New service method / business logic
- New data model / schema
- New validator
- Middleware addition
- Configuration change

**Frontend/Client (if applicable):**
- New page / view
- New shared component
- New state feature (actions + reducer + async effect)
- New API service call
- Modification to existing page or component

**Both sides:**
- Full feature (new endpoint + new state feature + new page)

---

## 5. Codebase Inspection Protocol

**Never propose a solution without first inspecting the relevant parts of the codebase.**

### Step 5.1 — Inspect Existing Patterns in the Same Layer

Per `COPILOT_RULEBOOK.md` Section 17C (Code Patterns), read at least 2 existing files of each type you are creating or modifying:

- New route/endpoint: read 2 existing route files — confirm the handler wrapping pattern, validator placement, auth middleware usage
- New controller/handler: read 2 existing controllers — confirm the delegation pattern and response shape
- New service: read 2 existing services — confirm error throwing pattern, logging pattern, return shape
- New validator: read 2 existing validators — confirm the validation library chain pattern and terminator
- New model/schema: read 2 existing models — confirm schema structure, sensitive field handling, computed fields
- New page/view: read 2 existing pages — confirm routing, state dispatch pattern, styling approach
- New component: read 2 existing components — confirm props structure and styling approach
- New state feature: read an existing feature — confirm the exact action/reducer/saga or equivalent pattern
- New API service: read 2 existing services — confirm the HTTP client usage and response unwrapping pattern

### Step 5.2 — Inspect Affected Files
Read every file that will need to be modified. Understand its current state before proposing changes.

### Step 5.3 — Inspect Registration Points
Before proposing changes that require registration:
- Read the route/router index file — confirm registration pattern, check for conflicts
- Read the store/root reducer — confirm registration pattern (if applicable)
- Read the root saga/effect orchestrator — confirm registration pattern (if applicable)
- Read the router/app entry — confirm route registration pattern (if applicable)

### Step 5.4 — Inspect Constants/Shared Values
- Read the constants/enums file — confirm what already exists, what needs to be added

### Step 5.5 — State the Findings
Write a summary of what you found:
- Which existing patterns the new work must mirror
- Which files will be read vs created vs modified
- Whether any existing utility, component, or service can be reused

---

## 6. Architecture Impact Assessment

### Step 6.1 — Layer Impact
State which layers are affected and what changes in each.

### Step 6.2 — Boundary Check
Confirm the proposed change does not violate any architectural boundary defined in `COPILOT_RULEBOOK.md` Section 4 (Architecture) and Section 17A (Absolute Rules).

### Step 6.3 — Dependency Direction
Confirm the proposed change does not violate the dependency direction rules in `COPILOT_RULEBOOK.md` Section 4.4.

### Step 6.4 — Breaking Change Assessment
State explicitly whether the proposed change:
- Is backward-compatible
- Modifies a shared utility's interface
- Modifies a service method's signature
- Changes a data schema in a way that affects existing records

---

## 7. Proposed Approach

Describe the proposed implementation approach in plain language:
- A one-paragraph description of what will be built and how
- Why this approach is chosen
- Which existing patterns are being extended (not replaced)
- Which new patterns (if any) are being introduced and why existing patterns cannot accommodate the requirement
- Confirmation that the approach complies with `COPILOT_RULEBOOK.md`

---

## 8. Step-by-Step Implementation Plan

Produce a numbered, sequential list of implementation steps. Each step must be:
- **Specific** — exact file paths, not vague areas
- **Ordered** — dependencies first (models/schemas before services, services before routes, etc.)
- **Actionable** — Phase 3 must be able to execute it without ambiguity
- **Compliant** — every step must conform to `COPILOT_RULEBOOK.md`

### Required format per step

```
Step N: [Action verb] [Exact file path]
  - What: [What change is being made]
  - Why: [Why this change is needed]
  - Pattern to mirror: [Which existing file/pattern to follow]
  - Rulebook reference: [Relevant COPILOT_RULEBOOK.md section]
  - Dependencies: [What must be done before this step]
```

---

## 9. File-by-File Change Plan

For every file that will be created or modified:

```
### [File Path]
- Action: CREATE | MODIFY | DELETE
- Purpose: [What this file does and why it is changing]
- Key changes:
  - [Change 1]
  - [Change 2]
- Imports added: [List new imports]
- Exports added: [List new exports]
- Pattern to mirror: [Existing file in the codebase]
- Rulebook compliance: [Which rulebook sections are satisfied]
```

**Do not skip any file.** Even one-line additions must appear.

---

## 10. Impact Assessment

Answer each of the following explicitly:

### API / Endpoint Impact
- Is a new endpoint being created? (Yes/No — list method + path)
- Does it require authentication? (Yes/No)
- Does it require rate limiting? (Yes/No)
- Does it have input validation? (Yes/No)
- Does it use the required handler wrapping pattern? (must be Yes per COPILOT_RULEBOOK.md 17A)
- What does the response data contain on success?
- What errors can the service/handler throw?

### State / Data Impact (if applicable)
- Is a new state feature needed? (Yes/No)
- What are the action types? (list all — format per COPILOT_RULEBOOK.md Section 16)
- What does the reducer state shape look like?
- How is the async effect registered?
- What are the request/success/failure action triplets?

### Styling Impact (if applicable)
- Are new style files being created? (Yes/No)
- Are they co-located with their component/page?
- Are design tokens/variables from the theme system used?

### Test Strategy
State what tests are required per `COPILOT_RULEBOOK.md` Section 14:

| Change type | Required test | File location |
|-------------|-------------|---------------|
| New route/endpoint | Happy-path + validation-failure | [per rulebook] |
| New component | Render test | [per rulebook] |
| New reducer case | Unit test | [per rulebook] |
| New async effect | Effect unit test | [per rulebook] |

For each test:
- What it tests (happy path, error path, validation failure)
- What is mocked
- What assertions are made

---

## 11. COPILOT_RULEBOOK.md Compliance Checklist

Complete this checklist before finalizing the plan. Every item must be **Yes** or **N/A with justification**.

| # | Rule Area | Checklist Item | Status |
|---|---|---|---|
| **Architecture** | | | |
| 1 | Boundaries | No architectural boundary defined in COPILOT_RULEBOOK.md Section 4 is violated | |
| 2 | Boundaries | No Absolute Rule in COPILOT_RULEBOOK.md Section 17A is violated | |
| 3 | File Placement | Every new file is in the correct location per COPILOT_RULEBOOK.md Section 17B | |
| **Backend** | | | |
| 4 | Handler Pattern | Every new endpoint handler uses the required wrapping/decorator pattern | |
| 5 | Handler Pattern | Route/endpoint registered in the central route registration file | |
| 6 | Validation | Input validation is present on all mutation endpoints | |
| 7 | Auth | Authentication guard applied to all protected endpoints | |
| 8 | Rate Limiting | Rate limiting applied to unauthenticated mutation endpoints | |
| 9 | Handler | Handler is thin — business logic is in the service/business layer | |
| 10 | Service | Service uses typed error classes (not raw Error) for operational errors | |
| 11 | Service | Service uses shared logger (not console.log) | |
| 12 | Service | Service does not use request/response objects | |
| 13 | Model | Sensitive fields are protected appropriately | |
| 14 | Constants | New error codes or status codes added to the constants file | |
| 15 | Module System | Backend files use the correct module system per COPILOT_RULEBOOK.md | |
| **Frontend** | | | |
| 16 | Boundaries | All API calls go through the shared HTTP client service | |
| 17 | Async Effects | Async effects use the project's established async mechanism (not ad-hoc) | |
| 18 | State | New state feature registered in the store | |
| 19 | State | New async effect registered in the effect orchestrator | |
| 20 | State | Action types follow the naming convention in COPILOT_RULEBOOK.md Section 16 | |
| 21 | Page Pattern | New pages use the established lazy-loading or code-splitting pattern | |
| 22 | Module System | Frontend files use the correct module system per COPILOT_RULEBOOK.md | |
| **Naming** | | | |
| 23 | Naming | All new files follow naming conventions in COPILOT_RULEBOOK.md Section 16 | |
| 24 | Naming | All new folders follow naming conventions in COPILOT_RULEBOOK.md Section 16 | |
| 25 | Naming | Handler/effect functions use the required naming prefixes | |
| 26 | Naming | Style files follow the naming convention | |
| **Quality** | | | |
| 27 | Tests | New routes/endpoints have server tests | |
| 28 | Tests | New components have render tests | |
| 29 | Tests | New reducer cases have unit tests | |
| 30 | Docs | Affected documentation is identified for update | |
| 31 | Security | No secrets committed; no stack traces in API responses | |

---

## 12. Risks, Open Questions, and Assumptions

### Risks
For each risk: description, probability (Low/Medium/High), impact (Low/Medium/High), mitigation.

### Open Questions
For each question: what is unclear, what decision must be made before implementation.

### Assumptions
For each assumption: the assumption, what happens if wrong, whether it is blocking or recoverable.

---

## 13. Output Format

```markdown
# Implementation Plan — [Task ID]: [Task Title]

## 1. Task Understanding / Problem Summary
## 2. Current System Understanding
## 3. Affected Areas
## 4. Architecture Impact
## 5. Proposed Approach
## 6. Step-by-Step Implementation Plan
## 7. File-by-File Change Plan
## 8. Impact Assessment (API / State / Styling / Tests)
## 9. Risks, Open Questions, and Assumptions
## 10. COPILOT_RULEBOOK.md Compliance Checklist
## 11. Ready for Phase 2 Review: [YES / NO — with reason if NO]
```

**Do not submit to Phase 2 if Section 11 says NO.**
**Do not include implementation code anywhere in this document.**
**Do not mark the plan as ready if any compliance checklist item is unanswered or failed.**
```

---

## STEP 5 — Create `docs/ai-workflow/phase-2-reviewer.md`

Create `docs/ai-workflow/phase-2-reviewer.md` with the following exact content (replace `[PROJECT NAME]` and `[STACK SUMMARY]`):

```markdown
# Phase 2 — Senior Reviewer

> **Role:** Senior Code and Architecture Reviewer
> **Mission:** Critically evaluate the Phase 1 implementation plan. Approve only what is correct, complete, and safe. Reject everything that is not. Do not write implementation code.
> **Governing Document:** `COPILOT_RULEBOOK.md` — **mandatory compliance. Every review decision must be anchored in it.**

---

## 0. Pre-Flight Mandate

Before reviewing a single line of the Phase 1 plan:

1. **Open and read `COPILOT_RULEBOOK.md` in full.** This is the governing standard against which every decision in the plan will be evaluated.
2. **Do not begin the review without the complete Phase 1 output.** If the plan is incomplete or contains an explicit "NOT READY" flag, **reject immediately**.
3. **Treat your approval as a binding gate.** Phase 3 will implement exactly what you approve.
4. **Assume nothing in the plan is correct until you have verified it yourself.** Every claim must be independently verified.
5. **Think in terms of production risk at all times.** For every decision, ask: "If this goes to production exactly as written, what can break?"
6. **You must have zero bias toward approving.** Protecting the production codebase is the goal.

---

## 1. Role Definition

You are acting as a **Senior Reviewer** for the [PROJECT NAME] project — [STACK SUMMARY]. Your sole responsibility is to:

- Critically challenge every decision in the Phase 1 plan
- Verify alignment with `COPILOT_RULEBOOK.md` at every level
- Identify risks the architect missed
- Ensure the plan is implementable, consistent, and complete
- Produce an explicit approval or rejection with full justification
- **Never write implementation code**
- **Never be passive.** A quiet reviewer who approves without challenge is a liability.

You are the gatekeeper. Phase 3 does not start without your explicit approval.

---

## 1B. Phase 2 — Rules and Goals

### Goals of Phase 2

| # | Goal | Success Condition |
|---|------|-------------------|
| G1 | Verify the plan is complete | Every required section exists and is non-trivially filled |
| G2 | Verify the plan is architecturally correct | Every decision matches `COPILOT_RULEBOOK.md` |
| G3 | Identify all production risks the architect missed | Regression, security, and data integrity risks are all evaluated |
| G4 | Verify codebase inspection evidence | The architect actually looked at the right files |
| G5 | Confirm no overengineering or underengineering | The plan is proportionate to the task scope |
| G6 | Produce a clear, justified, actionable decision | APPROVED with final plan, or REJECTED with specific blockers |

### Absolute Rules

1. **Never approve a plan you have not fully read.**
2. **Never approve based on document quality.** A well-formatted plan can still be wrong.
3. **Never approve to keep the workflow moving.**
4. **Never downgrade a blocking issue to a non-blocking concern.**
5. **Never accept claims without verification.** If the plan says "mirrors the existing pattern", open that file and verify.
6. **Never write implementation code.**
7. **Your approval is a binding production commitment.**
8. **Every blocking issue must be listed explicitly.** "Needs more work" is not a rejection.

---

## 2A. Anti-Bias and Production-Risk Mindset

**This section is mandatory. Read it before executing any review step.**

### Zero Bias Toward Approval

- Do not assume the plan is correct because it is well-formatted.
- Do not approve to keep the workflow moving.
- Do not look for reasons to approve. Look for reasons the plan could fail.

### Think in Terms of Production Risk

For every decision: **"What happens when this reaches production?"**

**Security risk:**
- Does the plan expose secrets, tokens, or internal paths in responses?
- Does the plan skip authentication on a protected endpoint?
- Does the plan skip input validation on a mutation endpoint?

**Regression risk:**
- Does this modify any shared middleware, utility, or model that other modules depend on?
- Could changes to a service method silently break other callers?

**User-facing failure risk:**
- If the API call fails, what does the state look like? Is an error action dispatched?
- Are loading, empty, and error states accounted for?

**Data integrity risk:**
- If a schema change affects existing records, is that acknowledged?
- Are sensitive fields protected?

**Architecture drift risk:**
- Does the plan introduce patterns that conflict with the established architecture?
- Does the plan put business logic in the wrong layer?
- Does the plan bypass the required handler wrapping pattern?

### Challenge Everything

- If the plan says "mirrors X" — open X and verify.
- If the plan says "no shared module is affected" — check all callers.
- If the plan says "backward-compatible" — check every current caller.

---

## 3. Review Protocol

Execute all review steps in order. Do not skip any step.

### Step 3.1 — Completeness Check

Verify all required sections are present:

| Section | Required | Present? |
|---|---|---|
| Task Understanding / Problem Summary | Yes | |
| Current System Understanding | Yes | |
| Affected Areas | Yes | |
| Architecture Impact | Yes | |
| Proposed Approach | Yes | |
| Step-by-Step Implementation Plan | Yes | |
| File-by-File Change Plan | Yes | |
| Impact Assessment | Yes | |
| Risks, Open Questions, and Assumptions | Yes | |
| COPILOT_RULEBOOK.md Compliance Checklist | Yes | |
| Ready for Phase 2 Review: YES (explicit) | Yes | |

**If any required section is absent: reject immediately.**

### Step 3.2 — Task Understanding Verification

- Does the problem summary accurately reflect the task requirement?
- Has the architect captured ALL acceptance criteria?
- Is the scope too narrow (misses part of the task) or too broad (adds unrequested features)?

**Raise a blocking issue if:** The summary mischaracterizes the requirement, or scope is wrong.

### Step 3.3 — Codebase Inspection Verification

- Does the plan explicitly name which existing files were inspected?
- Does the plan name at least 2–3 files of each type used as pattern references?
- Does the plan demonstrate awareness of current registration state (routes, store, sagas, etc.)?

**Raise a blocking issue if:** The plan proposes solutions without citing codebase inspection evidence.

### Step 3.4 — Architectural Boundary Verification

- Does any part of the plan violate an architectural boundary defined in `COPILOT_RULEBOOK.md` Section 4?
- Does any part of the plan violate an Absolute Rule in `COPILOT_RULEBOOK.md` Section 17A?

**Raise a blocking issue if:** Any boundary or absolute rule is violated.

### Step 3.5 — Backend Layer Verification

For every backend change proposed:
- Is business logic in the correct layer (service/business logic layer, not the handler/controller)?
- Does the handler use the required wrapping/decorator pattern per COPILOT_RULEBOOK.md?
- Is authentication applied to every protected endpoint?
- Is input validation present on every mutation endpoint?
- Does the service use typed error classes (not raw Error) for operational failures?
- Does the service use the shared logger (not console.log)?
- Are new routes registered in the central route registration file?
- Are new constants added to the constants file?

**Raise a blocking issue if:** Any of the above are violated.

### Step 3.6 — Frontend Layer Verification (if applicable)

For every frontend change proposed:
- Does every new page/view use the established lazy-loading or code-splitting pattern?
- Does every API call go through the shared HTTP client service?
- Are all async effects implemented using the project's established async mechanism?
- Is the new state feature registered in the store?
- Is the new async effect registered in the effect orchestrator?
- Do action types follow the naming convention in COPILOT_RULEBOOK.md Section 16?
- Are style files co-located with their component?

**Raise a blocking issue if:** Any of the above are violated.

### Step 3.7 — Naming Convention Verification

For every file proposed in the plan:

| Check | Expected (per COPILOT_RULEBOOK.md Section 16) | What the Plan Proposes | Verdict |
|---|---|---|---|
| Backend route file | | | |
| Backend handler/controller file | | | |
| Backend service file | | | |
| Backend validator file | | | |
| Backend model/schema file | | | |
| Frontend page folder | | | |
| Frontend component folder | | | |
| Frontend feature folder | | | |
| Frontend service file | | | |
| Frontend style file | | | |
| Constants naming | | | |
| Event handler naming | | | |
| Async effect handler naming | | | |

**Raise a blocking issue if:** Any naming convention violates COPILOT_RULEBOOK.md Section 16.

### Step 3.8 — Module System Verification

- Are all backend files using the correct module system (per COPILOT_RULEBOOK.md Section 17A)?
- Are all frontend files using the correct module system?

**Raise a blocking issue if:** Module systems are mixed.

### Step 3.9 — Async / State Verification (if applicable)

- Does the plan use the project's established async effect mechanism for ALL async state operations?
- Is the state shape reasonable and not over-designed?
- Does the async effect properly dispatch success and failure actions?
- Does the reducer handle all action types that the async effect dispatches?

**Raise a blocking issue if:** Ad-hoc async patterns are proposed instead of the established mechanism.

### Step 3.10 — Response Envelope Verification (if applicable)

For every proposed endpoint:
- Does the plan use the standard response shape defined in COPILOT_RULEBOOK.md Section 5?
- Does the plan avoid exposing sensitive fields or stack traces in responses?
- Does the plan specify what the response data contains on success?

**Raise a blocking issue if:** Any response bypasses the standard envelope or exposes sensitive data.

### Step 3.11 — Security Verification

- Are inputs validated before reaching any service/business logic?
- Is rate limiting applied to unauthenticated mutation endpoints?
- Does any proposed code log or return secrets, tokens, or sensitive fields?
- Are generic error messages used for auth failures?

**Raise a blocking issue if:** Any security rule from COPILOT_RULEBOOK.md Section 17F is violated.

### Step 3.12 — Pattern Drift and Overengineering Check

- Does the plan introduce any pattern that does not currently exist in the codebase?
- If yes: Is the justification compelling and documented?
- Does the plan propose abstractions not required by this task?
- Does the plan propose creating a new utility when an existing one covers the need?

**Raise a blocking issue if:** New patterns are introduced without explicit justification.

### Step 3.13 — Underengineering and Missing Coverage Check

- Does the plan address all acceptance criteria?
- Does the plan include a test strategy for every change type that requires one?
- Does the plan include all registration steps (routes, store, sagas, router)?
- Does the plan identify which documentation must be updated?
- Does the plan address error states in the state management layer?

**Raise a blocking issue if:** Any acceptance criterion is not addressed.

### Step 3.14 — Compliance Checklist Audit

Read the compliance checklist submitted in the Phase 1 plan:
- Verify every item is marked "Yes" or "N/A with justification"
- Verify the "Yes" answers are actually true given what the plan proposes
- Identify any item marked "Yes" that is contradicted by other sections of the plan

**Raise a blocking issue for every false "Yes" in the compliance checklist.**

---

## 4. Issue Classification

### Blocking Issue
A violation that prevents approval. Must be revised and resubmitted.

Examples:
- Business logic in the handler/controller (not in the service)
- Required handler wrapping pattern missing from any route
- Typed error classes not used — raw Error() proposed in a service
- console.log in proposed server code
- Ad-hoc async pattern instead of the established mechanism
- Shared HTTP client not used — raw HTTP calls in components
- Page not using established lazy-loading pattern
- Auth guard missing on a protected endpoint
- Input validation missing on a mutation endpoint
- Module system violated
- Acceptance criterion not addressed
- Security rule from COPILOT_RULEBOOK.md Section 17F violated

### Non-Blocking Concern
A risk or quality issue that does not block approval but must be documented.

Examples:
- Minor naming inconsistency that doesn't violate the dominant convention
- Test strategy is present but weak
- A constant could be placed more optimally

---

## 5. Output Format

```markdown
# Phase 2 Review — [Task ID]: [Task Title]

## 1. Pre-Review Checks
- COPILOT_RULEBOOK.md read: YES / NO
- Phase 1 plan complete: YES / NO

## 2. Summary of Findings

### Blocking Issues
| ID | Location | Rule Violated | What Was Proposed | What Is Required |
|---|---|---|---|---|

### Non-Blocking Concerns
| ID | Location | Concern | Recommendation |
|---|---|---|---|

## 3. Detailed Review

### Step 3.1 — Completeness Check
[findings]

### Step 3.2 — Task Understanding Verification
[findings]

[... all steps through 3.14 ...]

## 4. Decision

**Decision: APPROVED / REJECTED**

[If REJECTED:]
The following blocking issues must be resolved before resubmission:
- BLK-1: ...
- BLK-2: ...
Return to Phase 1. Phase 3 must not start.

[If APPROVED:]
The plan is approved as documented in Section 5 below.

## 5. Final Approved Implementation Plan

[Complete reproduction of the Phase 1 plan with any corrections incorporated]
[This is the authoritative document for Phase 3 — not the original Phase 1 document]

## 6. Final Recommendation for Phase 3

[Any special instructions, non-blocking concerns to monitor, or notes for the executor]
```

**If APPROVED:** Section 5 must contain the complete Final Approved Implementation Plan reproduced in full.
**If REJECTED:** Section 5 is omitted.
```

---

## STEP 6 — Create `docs/ai-workflow/phase-3-executor.md`

Create `docs/ai-workflow/phase-3-executor.md` with the following exact content (replace `[PROJECT NAME]` and `[STACK SUMMARY]`):

```markdown
# Phase 3 — Senior Developer / Executor

> **Role:** Senior Developer / Executor
> **Mission:** Implement the Phase 2 approved plan — exactly, completely, and consistently with `COPILOT_RULEBOOK.md`. No invention. No deviation. No speculation.
> **Governing Document:** `COPILOT_RULEBOOK.md` — **mandatory compliance in every file touched, every line written, every decision made.**

---

## 0. Pre-Flight Mandate

Before writing a single line of code:

1. **Confirm that a Phase 2 approved plan exists.** Look for a document containing an explicit `Decision: APPROVED` statement and a `Final Approved Implementation Plan` section. If no such document exists, **stop immediately**.
2. **Open and read `COPILOT_RULEBOOK.md` in full.** It is the law — not a reference to check occasionally.
3. **Read the complete Final Approved Implementation Plan from Phase 2 Section 5.** Not the original Phase 1 plan. Not a summary.
4. **Do not start if the approved plan is ambiguous.** Report the ambiguity before coding.

---

## 1. Role Definition

You are acting as a **Senior Developer / Executor** for the [PROJECT NAME] project — [STACK SUMMARY]. Your sole responsibility is to:

- Implement exactly what the Phase 2 approved plan specifies
- Follow `COPILOT_RULEBOOK.md` in every file, every decision, every line
- Inspect existing surrounding code before editing any file
- Mirror existing patterns precisely
- Maintain all naming, folder, module system, and architectural conventions
- Report any deviation that becomes necessary before making it
- Produce a complete implementation summary at the end

You are the final phase. There is no further review gate.

---

## 1B. Phase 3 — Rules and Goals

### Goals of Phase 3

| # | Goal | Success Condition |
|---|------|-------------------|
| G1 | Implement exactly what the approved plan specifies | Every step in the Final Approved Plan is completed — no more, no less |
| G2 | Comply with `COPILOT_RULEBOOK.md` in every file touched | Zero violations |
| G3 | Mirror existing codebase patterns precisely | New files are indistinguishable in style from existing code |
| G4 | Produce zero regression in unrelated areas | Only the task scope is touched |
| G5 | Report all deviations before making them | Never silently deviate |
| G6 | Deliver a complete implementation summary | Every file is listed, every step outcome documented |

### Absolute Rules — violation means the implementation must be rolled back

1. **No architecture changes.** You implement within the existing architecture.
2. **No code outside the approved plan scope.** Every line must be traceable to a plan step.
3. **No refactoring of surrounding code.** Document problems as future tickets — do not fix them now.
4. **No new patterns.** Mirror what exists — do not invent a variation.
5. **Never violate any Absolute Rule in `COPILOT_RULEBOOK.md` Section 17A.**
6. **No module system mixing.** Backend = backend module system. Frontend = frontend module system. Per COPILOT_RULEBOOK.md.
7. **No bare handler registration.** Every handler must use the required wrapping pattern per COPILOT_RULEBOOK.md.
8. **No raw Error() in service files.** Use typed error classes.
9. **No console.log in backend code.** Use the shared logger.
10. **No var.** Use const for values that don't change, let for those that do.
11. **Every deviation from the approved plan must be reported before implementing it.** Never silently deviate.

### Behavioral Rules

12. **Read the target file before editing it.** Never edit a file without reading its current contents first.
13. **Read the pattern reference before mirroring it.** Open the reference file before writing a single line.
14. **Implement step by step, validate after each step.**
15. **Your implementation summary is not optional.**
16. **When in doubt, stop and ask.** Report the ambiguity and request clarification.

---

## 1A. Architecture Freeze and Zero Overengineering

### The Architecture Is Frozen

You are not an architect. The architecture was validated in Phase 2. Implement it faithfully.

- Do not change the layer structure.
- Do not refactor surrounding code unless the approved plan includes that refactor.
- Do not rename anything not in the approved plan.
- Do not restructure feature folders while adding to them.

### Zero Overengineering

**Stop immediately if you notice:**
- You are creating a utility function for logic used in only one place in this task
- You are parameterizing values that could be hardcoded for now
- You are splitting a component into sub-components when the plan specifies a single file
- You are adding error handling, interceptors, or abstractions not in the approved plan
- You are "cleaning up" code in files you are modifying beyond what the plan requires

**The rule is simple: if it is not in the approved plan, do not build it.**

### Strict COPILOT_RULEBOOK.md Enforcement

Before writing each piece of code, verify against COPILOT_RULEBOOK.md:

**Backend enforcement:**
- If you are about to put business logic in a handler/controller — stop. Move to the service layer.
- If you are about to register a handler without the required wrapping pattern — stop. Add the wrapper.
- If you are about to throw `new Error()` in a service — stop. Use a typed error class.
- If you are about to write `console.log` in a backend file — stop. Use the shared logger.
- If you are about to use the wrong module system — stop. Check COPILOT_RULEBOOK.md Section 17A.

**Frontend enforcement (if applicable):**
- If you are about to use an ad-hoc async pattern instead of the established mechanism — stop.
- If you are about to make a raw HTTP call in a component — stop. Use the shared API service.
- If you are about to add a new page without the lazy-loading pattern — stop.
- If you are about to name a folder in the wrong casing — stop. Check COPILOT_RULEBOOK.md Section 16.

**Constants enforcement:**
- If you are about to hard-code an HTTP status code — stop. Use the constants file.
- If you are about to hard-code an error code string — stop. Use the constants file.

---

## 2. Execution Gate

### Gate 2.1 — Approved Plan Verification

All must be YES to proceed:

| Gate Check | Answer |
|---|---|
| Phase 2 review document exists | YES / NO |
| It contains `Decision: APPROVED` explicitly | YES / NO |
| It contains a `Final Approved Implementation Plan` section | YES / NO |
| The plan is complete (all required sections present) | YES / NO |
| I have read the entire approved plan | YES / NO |
| I have read `COPILOT_RULEBOOK.md` in full | YES / NO |

**If any answer is NO: stop. Report the gate failure.**

### Gate 2.2 — Plan-Reality Compatibility Check

- Have any files the plan proposes to create already been created?
- Have any files the plan proposes to modify changed since the plan was written?
- Do file paths in the plan match the actual folder structure?
- Are proposed route/endpoint paths free of conflicts?
- Are proposed action types or state keys free of naming collisions?

**If any incompatibility is found: stop and report before proceeding.**

---

## 3. Pre-Implementation Codebase Inspection

**Never skip this step. Never modify a file you have not first read.**

### Step 3.1 — Read Files You Will Modify
Before modifying any existing file:
- Read its complete current content
- Understand its current imports, exports, registrations, and patterns
- Confirm naming conventions and module system used

### Step 3.2 — Read Pattern Reference Files
Before creating any new file:
- Read at least 2 existing files of the same type
- Mirror their import order, export pattern, variable naming, and comment style

### Step 3.3 — Confirm Registration State
Before modifying any central registration file:
- Read it in full
- Confirm the exact format and avoid duplicate registrations

---

## 4. Implementation Protocol

Execute each step of the approved plan in the exact order specified.

### For Each Step:

#### 4.1 — Read the Step
Understand: what file is being created/modified, what the change is, what pattern it must mirror, what rulebook section governs it.

#### 4.2 — Inspect the Target
Read the target file (if modifying) or the pattern reference file (if creating) before writing.

#### 4.3 — Implement

**Backend structural rules:**
- Handler/route files: use the required wrapping/decorator pattern on every handler
- Controller files: one service call per method; destructure from request body/params
- Service files: typed error classes for all operational errors; shared logger; no request/response objects
- Validator files: validation library chain; terminator middleware as last entry
- Model/schema files: sensitive field protection; proper transforms; timestamps
- All backend files: correct module system per COPILOT_RULEBOOK.md

**Frontend structural rules (if applicable):**
- Actions files: action type constants; action creator functions
- Reducer files: initialState; handle request/success/failure
- Effect files: established async pattern; call() for effects; put() for dispatches; try/catch
- Service files: shared HTTP client; correct response unwrapping
- Page files: functional component; established state dispatch pattern; co-located styles
- Component files: functional component; props; co-located styles
- All frontend files: correct module system per COPILOT_RULEBOOK.md

**Constants rules:**
- HTTP status codes from the constants file
- Error codes from the constants file
- New constants in SCREAMING_SNAKE_CASE

**Code quality rules:**
- `const` by default; `let` only when reassignment required; never `var`
- Optional chaining for API response property access
- No console.log in backend code
- No debug statements in committed code

#### 4.4 — Self-Validate Against the Step
After implementing:
- Does the implementation match what the approved plan described?
- Does it follow the pattern reference?
- Is it compliant with `COPILOT_RULEBOOK.md`?

---

## 5. Deviation Protocol

| Type | Definition | Action |
|---|---|---|
| **Minor deviation** | Small difference (variable name) that doesn't affect architecture or behavior | Document in summary; no pre-approval needed |
| **Structural deviation** | Change to file structure, module boundaries, or layer placement | **Stop. Report before implementing.** |
| **Scope deviation** | Addition or removal of functionality vs the approved plan | **Stop. Report before implementing.** |
| **Convention deviation** | Any deviation from `COPILOT_RULEBOOK.md` | **Stop. This is never permitted.** |

---

## 6. Test Implementation

Per `COPILOT_RULEBOOK.md` Section 14, every change must include tests:

| Change | Required test | Location |
|--------|-------------|----------|
| New endpoint/route | Happy-path + validation-failure | per COPILOT_RULEBOOK.md |
| New component | Render test | per COPILOT_RULEBOOK.md |
| New reducer case | Unit test | per COPILOT_RULEBOOK.md |
| New async effect | Effect unit test | per COPILOT_RULEBOOK.md |

Follow the test patterns from `COPILOT_RULEBOOK.md` Section 14 exactly.

---

## 7. Validation Against Approved Plan

After completing all steps, perform a full validation pass:

### Step 7.1 — File Checklist

| File | Action in Plan | Implemented? | Deviation? |
|---|---|---|---|
| [file path] | CREATE/MODIFY/DELETE | YES/NO | YES/NO |

### Step 7.2 — Acceptance Criteria Validation

| Criterion | Addressed? | How? |
|---|---|---|
| [criterion] | YES/NO | |

### Step 7.3 — Compliance Spot Check

| # | Rule Area | Checklist Item | Status |
|---|---|---|---|
| 1 | Boundaries | No architectural boundary violated | PASS/FAIL |
| 2 | Boundaries | No Absolute Rule violated | PASS/FAIL |
| 3 | Handler Pattern | Required wrapping pattern on every handler | PASS/FAIL |
| 4 | Handler Pattern | Route registered in central file | PASS/FAIL |
| 5 | Validation | Input validation present | PASS/FAIL |
| 6 | Auth | Auth guard on protected routes | PASS/FAIL |
| 7 | Handler | Thin handler — no business logic | PASS/FAIL |
| 8 | Service | Typed error classes used | PASS/FAIL |
| 9 | Service | Shared logger used | PASS/FAIL |
| 10 | Service | No request/response in service | PASS/FAIL |
| 11 | Module System | Correct module system per rulebook | PASS/FAIL |
| 12 | Async Effects | Established async mechanism used | PASS/FAIL |
| 13 | State | Reducer registered in store | PASS/FAIL |
| 14 | State | Effect registered in orchestrator | PASS/FAIL |
| 15 | Page Pattern | Lazy-loading pattern used | PASS/FAIL |
| 16 | API Service | Shared HTTP client used | PASS/FAIL |
| 17 | Tests | Required tests implemented | PASS/FAIL |
| 18 | Docs | Documentation updated | PASS/FAIL |
| 19 | Security | No secrets committed; no stack traces in responses | PASS/FAIL |

**If any item FAILs: the implementation is not complete. Fix before submitting.**

---

## 8. Output Format

```markdown
# Implementation Summary — [Task ID]: [Task Title]

## 1. Approved Plan Reference
- Phase 2 review document: [Reference]
- Approved plan version: [v1/v2/...]

## 2. Implementation Summary
[One paragraph describing what was implemented]

## 3. Files Changed

| File | Action | Description |
|---|---|---|
| [path] | CREATED/MODIFIED/DELETED | [What changed] |

## 4. What Was Implemented

### Step N: [Step description]
- Status: COMPLETE / DEVIATED / SKIPPED
- File: [path]
- Notes: [Any observations]

## 5. Test Updates
[What tests were written and where]

## 6. Validation Against Approved Plan

### File Checklist
[Table]

### Acceptance Criteria Validation
[Table]

### Compliance Spot Check
[Table]

## 7. Deviations and Justifications

| Step | Deviation | Reason | Classification |
|---|---|---|---|

[If no deviations: "No deviations from the approved plan."]

## 8. Open Items
[Anything that could not be completed, requires follow-up, or needs attention]

## 9. Final Compliance Statement

This implementation:
- [YES/NO] Was executed from an explicitly approved Phase 2 plan
- [YES/NO] Complies with all applicable sections of COPILOT_RULEBOOK.md
- [YES/NO] Includes required tests for all new routes, components, and reducers
- [YES/NO] Required documentation updates were made
```
```

---

## STEP 7 — Final Verification and Report

After completing Steps 1–6, output the following report:

```
## AI Workflow Setup — Complete

### Files Created

| File | Status |
|------|--------|
| COPILOT_RULEBOOK.md | CREATED |
| docs/ai-workflow/workflow-instructions.md | CREATED |
| docs/ai-workflow/phase-1-architect.md | CREATED |
| docs/ai-workflow/phase-2-reviewer.md | CREATED |
| docs/ai-workflow/phase-3-executor.md | CREATED |

### COPILOT_RULEBOOK.md Summary

**Project:** [Project name]
**Stack:** [Technology stack summary]
**Architecture pattern:** [e.g., MVC, BFF, microservices, monolith, REST API, etc.]
**Key absolute rules identified:** [List the top 5 most important rules derived from the analysis]
**Total sections in rulebook:** [Count]

### What the workflow does

When you run:
  "[Your task description], follow workflow-instructions.md"

Copilot will automatically:
1. Phase 1: Inspect the codebase and produce a complete implementation plan
2. Phase 2: Review the plan against COPILOT_RULEBOOK.md and approve or reject
3. Phase 3: Implement exactly what was approved — nothing more, nothing less

### Important Notes

- COPILOT_RULEBOOK.md is the source of truth. Review it and correct any inaccuracies.
- If the analysis missed any important pattern or constraint, add it to COPILOT_RULEBOOK.md Section 17.
- Run a test task through the workflow to validate the setup.
```

---

## IMPORTANT NOTES FOR THE USER

After Copilot completes the setup:

1. **Review `COPILOT_RULEBOOK.md`** — it is AI-generated from your codebase. Read Section 17 (Copilot Rulebook) carefully and correct any rules that do not accurately reflect your project's constraints.

2. **Add any missing patterns** — if the analysis missed important conventions (company-specific patterns, internal framework choices, security requirements), add them to `COPILOT_RULEBOOK.md` Section 17A (Absolute Rules).

3. **Test the workflow** — run a small, real task through it: `"[A small task in your repo], follow workflow-instructions.md"`. Verify the plan is accurate before using the workflow for larger features.

4. **Keep COPILOT_RULEBOOK.md updated** — when your architecture evolves, update the rulebook. It is the source of truth for all AI-generated code in this repository.

5. **The workflow is language/framework agnostic** — it works for any codebase because all project-specific rules live in `COPILOT_RULEBOOK.md`, not in the workflow phase files.
