# PRX — AI-Controlled Software Delivery

> **Drop one file into any repository. Copilot analyzes your entire codebase, generates a project-specific rulebook, and installs a gate-controlled 3-phase delivery workflow — automatically.**

---

## The Problem It Solves

AI coding assistants (like GitHub Copilot) are powerful, but they have no memory of your codebase, no understanding of your team's architecture, and no guardrails against overengineering or convention drift. Every prompt starts from scratch. The AI guesses your patterns, violates your layer boundaries, and ships code that "works" but doesn't belong in your project.

PRX fixes this by giving the AI a structured, self-generated understanding of your specific codebase before it ever writes a line of code — and enforcing a strict 3-phase gate before any implementation happens.

---

## How It Solves It

### One Command Installs Everything

Add `PRX_AI.md` to your repository root, then say:

```
"Execute PRX_AI.md"
```

Copilot automatically runs 7 sequential steps and creates 5 files:

| Step | What Happens |
|------|-------------|
| 1 | Deep repository analysis — reads your actual source files |
| 2 | Generates `COPILOT_RULEBOOK.md` — a 19-section source of truth |
| 3 | Creates `workflow-instructions.md` — auto-execution protocol |
| 4 | Creates `phase-1-architect.md` — planning role definition |
| 5 | Creates `phase-2-reviewer.md` — adversarial review gate |
| 6 | Creates `phase-3-executor.md` — implementation role definition |
| 7 | Outputs a setup report |

No external tools. No scripts. No configuration. Pure markdown executed by Copilot.

---

### The 3-Phase Workflow

Every future task runs through three mandatory phases in strict order. No phase can be skipped.

```
Phase 1 — Architect  →  Phase 2 — Reviewer  →  Phase 3 — Executor
       Plan                  Gate                  Implement
```

#### Phase 1 — Senior Architect
- Inspects the codebase before proposing anything
- Produces a complete implementation plan with no code written
- Completes a 31-item self-compliance checklist
- Gate: must output `"Ready: YES"` to proceed

#### Phase 2 — Senior Reviewer
- Runs 14 adversarial review steps against `COPILOT_RULEBOOK.md`
- Zero bias toward approval — acts as an adversarial reviewer
- Classifies every issue as **blocking** or **non-blocking**
- Gate: must output `"Decision: APPROVED"` to proceed
- If rejected, Phase 1 is automatically sent back for revision (up to 3 cycles)

#### Phase 3 — Senior Executor
- Implements **exactly** what Phase 2 approved — nothing more
- Architecture frozen; zero overengineering permitted
- Reads every file before editing it
- Completes a 19-item compliance spot check after implementation
- Produces a full Implementation Summary documenting every file touched

---

### COPILOT_RULEBOOK.md — The 19-Section Source of Truth

The rulebook is generated from your actual source files — not generic best practices. Every phase reads it before doing anything.

| # | Section | What It Captures |
|---|---------|-----------------|
| 01 | Executive Summary | High-level project overview |
| 02 | Tech Stack | Runtime, framework, language, versions |
| 03 | Repository Structure | Directory layout and responsibilities |
| 04 | Architecture | Layer map, dependency direction, boundaries |
| 05 | API Endpoints | Route patterns and conventions |
| 06 | Data Models | Schema patterns and naming |
| 07 | State Management | Async mechanism actually used |
| 08 | Middleware Pipeline | Middleware order and responsibilities |
| 09 | Code Patterns | Real examples from your codebase |
| 10 | Error Handling | Error class hierarchy and usage |
| 11 | Security | Auth mechanism, rate limiting, validation |
| 12 | Logging | Logger instance and usage patterns |
| 13 | Environment Variables | Env keys and access patterns |
| 14 | Testing | Framework, structure, mocking approach |
| 15 | Styling System | CSS approach, naming, component patterns |
| 16 | Naming Conventions | File, folder, variable, function casing |
| 17 | Absolute Rules | Non-negotiable hard constraints (A–D) |
| 18 | Dev Workflows | Git, PR, branch, and deploy conventions |
| 19 | Design Notes | Architecture decisions and rationale |

**Section 17** is the most critical — it has four sub-sections:
- **17A — Absolute Rules**: 10 hard constraints no phase may ever violate (BFF boundary, error handling, logging, async patterns, validation, auth, module system, secrets, response shape)
- **17B — File Placement Rules**: Exact destination for every type of new file
- **17C — Code Patterns**: Complete, copy-ready code examples for every layer
- **17D — Anti-Patterns**: What NOT to do, with the correct alternative and reasoning

---

### Built-In Compliance Gates

Each phase enforces its own compliance mechanism before passing work forward.

| Phase | Mechanism | Checks |
|-------|-----------|--------|
| Phase 1 | 31-item self-checklist | Architecture boundaries, file placement, handler wrapping, input validation, auth guards, error classes, logging, naming, module system, secrets |
| Phase 2 | 14 adversarial review steps | Completeness, scope, codebase inspection evidence, layer placement, naming, module system, async patterns, response shape, security, pattern drift |
| Phase 3 | 19-item compliance spot check | All Phase 1 items re-verified on implemented code |

---

### Hard Safeguards

The workflow has 8 explicit hard stops that can never be bypassed:

1. **Approval Gate** — Phase 3 cannot start without an explicit `APPROVED` from Phase 2
2. **Plan-Reality Compatibility Gate** — checks for file and route conflicts before any code is written
3. **Architecture Freeze** — no redesigning or refactoring during implementation
4. **Deviation Protocol** — structural deviations require a full stop and report
5. **Read-Before-Edit Rule** — every file must be read before being modified
6. **Zero Overengineering Rule** — stop immediately when about to create anything not in the approved plan
7. **Maximum Revision Safeguard** — after 3 rejections, pauses and surfaces all blockers to the user
8. **Non-Interruption Enforcement** — only 3 valid pause points; all other pauses are workflow violations

---

### Full Traceability

Every decision across all three phases is traceable via:

- **Task ID** carried through every document (Plan, Review, Summary)
- **Version numbers** on plans across revision cycles (v1, v2, v3)
- **Full context handoff** — complete documents passed between phases, never summaries
- **Implementation Summary** listing every file touched, every deviation, and a final compliance statement

---

## Works With Any Codebase

The workflow files contain zero hardcoded language, framework, or architecture assumptions. All project-specific rules live entirely in `COPILOT_RULEBOOK.md`.

| Dimension | Examples |
|-----------|---------|
| Languages | Node.js, Python, Go, Java, Ruby, Rust, C# |
| Frameworks | Express, FastAPI, Spring, Rails, NestJS, Next.js, Django |
| Architectures | REST API, BFF, Microservices, Monolith, Full-Stack, CLI |

Swap the project — the workflow adapts automatically on next setup run.

---

## The 5 Files Created

```
your-repo/
├── COPILOT_RULEBOOK.md                        ← Generated from your codebase
└── docs/
    └── ai-workflow/
        ├── workflow-instructions.md           ← Auto-execution protocol
        ├── phase-1-architect.md               ← Planning role
        ├── phase-2-reviewer.md                ← Review gate role
        └── phase-3-executor.md                ← Implementation role
```

---

## Key Metrics

| Metric | Value |
|--------|-------|
| Files created per setup | 5 |
| Workflow phases | 3 |
| Rulebook sections | 19 |
| Human prompts between phases | 0 |
| Compliance checks per plan | 31 (Phase 1) + 14 (Phase 2) + 19 (Phase 3) |
| Max auto-revision cycles | 3 |

---

## Author

**Ajay Kumar Sharma** — Developer & Author  
✉ ajaykumarsharma.20196@gmail.com  
📞 +91 8077651669  
🔗 [linkedin.com/in/ajay-sharma-219b53141](https://www.linkedin.com/in/ajay-sharma-219b53141/)

---

© 2026 Ajay Kumar Sharma · All rights reserved
