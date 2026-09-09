# Skills

## API Integrator skill

### Scripts

### Skill description

```markdown
---
name: API Agent
---
# ⚡ Skill: API Agent

> ID: api-agent
> Category: arch
> Slash Command: /api-agent

## Core responsibilities
- Implement ports + use-cases + adapters + OpenAPI-friendly DTOs. Repositories (SQLite + DrizzleORM). SSE endpoint, authentication, pagination, outbox event store + dispatcher.
- DTOs are explicit and versioned (v1).
- Service layer rejects access if scenario/landscape/app not in tenant.
- HTTP routes (adapter) -> Application services -> Ports -> Repos/Eventing/Auth adapters.
```

## Architecting skill

### Scripts

### Skill description

```markdown
 ---
name: architect_agent
description: Validate architecture structure, hexagonal compliance and make recommendations about possible architectural consideration to developers or re
commendations to reviewers.
---
# ⚡ Skill: Architect Agent
> ID: arch-agent
> Category: arch
> Slash Command: /architect

## When to use
Use this skill when the user asks to:

## Core responsibilities
- Favor monorepo layout with small modules, descriptive names, and predictable folder structure.
- Hexagonal architecture on backend AND frontend (ports/adapters, DTO boundaries). Layered inside each hexagon: Domain -> Application -> Adapters.
- No “magic numbers”: constants in config or domain defaults.
- Ports = interfaces; adapters depend on ports; application depends on ports;domain depends on nothing.
- Owns folder structure, boundaries, DTOs, and event taxonomy.
- Validates hexagonal compliance (no adapter leakage into core).
- Events emitted/consumed.
- All writes create domain events that go to Outbox (transactionally)
- Validates Hexagonal boundaries, ports, adapters, layers, patterns (observer, strategy, registry, outbox).
```

## CLI Building skill

### Scripts

### Skill description

```md
---
name: cli-builder
description: Plan the tasks to be performed for building a new C
tags:
 - acceptance criteria
 ---
  
  # ⚡ Skill: CLI Builder
  
  ## Skill Overview
  - A compact, authoritative skill that encapsulates best practices for building fast, interactive, TypeScript-based CLI tools using Bun. Useful for specialized agents that need to generate, review, or refactor CLI code following the repository's standards.
  
  ## When to use
  
  Use this skill when the user asks to:
  
  - Group the patterns, configuration, and build/packaging conventions required to produce high-quality Bun/TypeScript CLIs that are: fast to start, memory-efficient, well-typed, test-covered, and easily distributable as a compiled Bun binary.
  - Generate consistent per-package manifests for Turborepo-managed projects.
  - Keep runtime and devDependencies separated; for binaries, only include runtime deps used at execution.
  - Creating new Bun TypeScript CLI packages, converting to Bun binary distribution.
```

## Dashboard Designing skill

### Scripts

### Skill description

```md
---
name: dashboard-designer
---

# Dashboard Designer

> ID: dashboard-designer
> Category: analytics
> Slash Command: /create-dashboard

## Skill Overview

```

## Delta Planning skill

### Scripts

### Skill description

```md
---
name: Delta Planner
description: Delta planner
---
  
# ⚡ Skill: Delta Planner
  
> ID: feature-creator
> Category: spec
> Slash Command: /create-delta
  
  
## Skill Overview
  
- All new delta should have a sequential id (DLT-001,DLT-002,), target and existing feature and a short title.
- Records add/change/deprecate/remove/fix, version bump, notes and verification
```

## Feature Creating skill

### Scripts

### Skill description

```md
---
name: Feature creation
description: Feature creation forges a feature using a canonical spec for a capability.
 tags:
   - acceptance criteria
---
> ID: feature-creator
> Category: spec
> Slash Command: /create-feature

## Skill Overview

- All new features should have a sequential id (FET-001) and a short title.
- Records version bump, notes, verification, acceptance criteria, owner, metrics.
- Fill Out Specification `SPEC.md` to define:

- Problem statement and goals
- User scenarios (Gherkin):
- Write Gherkin scenarios in `*.feature` file. This creates `tests/features/api_server/api_server.feature`.
- Implement step definitions in `*.steps.ts` file. This creates `tests/features/api_server/steps/api_server.steps.ts`.
- `tests/features/api_server/fixtures/` (directory)
- Functional and non-functional requirements
- Implementation touchpoints

## Examples
File: single_scenario.feature
Feature: Quality stats summary warnings
Report skipped analytics as structured warnings for operators.

  Scenario: Skipped analytics are recorded in summary.json
  Given analytics "api_extraction" is disabled
  When the analysis completes
  Then summary.json includes a warning with code "ANALYTICS_SKIPPED"
  And the warning severity is "warning"

```

## QA Testing skill

### Scripts

### Skill description

```md
---
name: QA Testing
description: Quality Assurance Testing
---

TDD is the red → green loop. This skill is the reference that makes that loop produce tests worth keeping: what a good test is, where tests go, the anti-patterns, and the rules of the loop. Every section applies on every cycle: consult them before and during the loop, not after.

When exploring the codebase, read `CONTEXT.md` (if it exists) so test names and interface vocabulary match the project's domain language, and respect ADRs in the area you're touching.

## What a good test is

Tests verify behavior through public interfaces, not implementation details. Code can change entirely; tests shouldn't. A good test reads like a specification: "user can checkout with valid cart" tells you exactly what capability exists, and it survives refactors because it doesn't care about internal structure.

See [tests.md](tests.md) for examples and [mocking.md](mocking.md) for mocking guidelines.

## Seams: where tests go

A **seam** is the public boundary you test at: the interface where you observe behavior without reaching inside. Tests live at seams, never against internals.

**Test only at pre-agreed seams.** Before writing any test, write down the seams under test and confirm them with the user. No test is written at an unconfirmed seam. You can't test everything, so agreeing the seams up front is how testing effort lands on the critical paths and complex logic instead of every edge case.

Ask: "What's the public interface, and which seams should we test?"

When the shape of that interface is itself in question (how deep the module is, where the seam belongs, what the interface should expose), call the Skill tool with "codebase-design" for the vocabulary. It is the shared source of the module, interface, depth, seam, adapter, leverage and locality terms, and it is a reference to consult, not a session to run.

## Anti-patterns

- **Implementation-coupled**: mocks internal collaborators, tests private methods, or verifies through a side channel (querying the database instead of using the interface). The tell: the test breaks when you refactor but behavior hasn't changed.
- **Tautological**: the assertion recomputes the expected value the way the code does (`expect(add(a, b)).toBe(a + b)`, a snapshot derived by hand the same way, a constant asserted equal to itself), so it passes by construction and can never disagree with the code. Expected values must come from an independent source of truth: a known-good literal, a worked example, the spec.
- **Horizontal slicing**: writing all tests first, then all implementation. Bulk tests verify _imagined_ behavior: you test the _shape_ of things rather than user-facing behavior, the tests go insensitive to real changes, and you commit to test structure before understanding the implementation. Work in **vertical slices** instead: one test → one implementation → repeat, each test a **tracer bullet** that responds to what the last cycle taught you.

## Rules of the loop

- **Red before green.** Write the failing test first, then only enough code to pass it. Don't anticipate future tests or add speculative features.
- **One slice at a time.** One seam, one test, one minimal implementation per cycle.
- **Refactoring is not part of the loop.** It belongs to the review stage (see the `code-review` skill), not the red → green implementation cycle.
```

### References

```md
# When to Mock

Mock at **system boundaries** only:

- External APIs (payment, email, etc.)
- Databases (sometimes - prefer test DB)
- Time/randomness
- File system (sometimes)

Don't mock:

- Your own classes/modules
- Internal collaborators
- Anything you control

## Designing for Mockability

At system boundaries, design interfaces that are easy to mock:

**1. Use dependency injection**

Pass external dependencies in rather than creating them internally:

```typescript
// Easy to mock
function processPayment(order, paymentClient) {
  return paymentClient.charge(order.total);
}

// Hard to mock
function processPayment(order) {
  const client = new StripeClient(process.env.STRIPE_KEY);
  return client.charge(order.total);
}


**2. Prefer SDK-style interfaces over generic fetchers**

Create specific functions for each external operation instead of one generic function with conditional logic:

```typescript
// GOOD: Each function is independently mockable
const api = {
  getUser: (id) => fetch(`/users/${id}`),
  getOrders: (userId) => fetch(`/users/${userId}/orders`),
  createOrder: (data) => fetch('/orders', { method: 'POST', body: data }),
};

// BAD: Mocking requires conditional logic inside the mock
const api = {
  fetch: (endpoint, options) => fetch(endpoint, options),
};

The SDK approach means:
- Each mock returns one specific shape
- No conditional logic in test setup
- Easier to see which endpoints a test exercises
- Type safety per endpoint
```
