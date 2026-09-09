---
name: QATesting Agent
description: Create high-quality Gherkin BDD feature files compatible with Cucumber, including structure, wording, and best practices for agent-friendly scenarios.
---
# ⚡ Skill: QA Testing Agent

> ID: qa-agent
> Category: arch
> Slash Command: /qa-agent

## When to use

Use this skill when user asked to:
- Write or improve Gherkin feature files, especially for Cucumber. It teaches agents how to produce clear, reusable scenarios and avoid common pitfalls.
- Writing new `.feature` files.
- Refactoring existing BDD scenarios for clarity or reuse.
- Designing acceptance criteria as executable specs.

### Quick workflow for agents
1) Read domain vocabulary and existing features in `tests/features/`.
2) Inspect step definitions in `tests/steps/` to reuse existing phrases.
3) Identify the behavior under test: actor, goal, preconditions, action, and outcomes.
4) Draft the feature in domain language (not UI or implementation language).
5) Validate for Cucumber compatibility and ambiguity.

## Core responsibilities
- Measure test coverage for view specificiation rendering
- Review API contract tests
- Make sure that simulation determinism testing

## Step writing rules
- Use **Given** for state, **When** for the action, **Then** for outcomes.
- Keep **one primary When** per scenario; multiple Thens are ok if they verify a single outcome.
- Use **And/But** to extend the previous step type; avoid mixing step types with And.
- Prefer **domain language**: talk about users, entities, and rules, not UI widgets or function names.
- Avoid logic in steps. The step text is a contract, not code.
- Make steps reusable: avoid embedding one-off specifics in text; use parameters instead.

## Scenario quality checklist
- One behavior per scenario.
- Preconditions are explicit and minimal.
- The outcome is observable and testable.
- No step depends on hidden side effects.
- Each step has an obvious step definition match (avoid ambiguity).

## Scenario Parameterization
Use Scenario Outline with Examples when:
- The same behavior repeats across inputs or roles.
- You want a compact, readable matrix of inputs/expected results.

Use Data Tables when:
- The step needs structured input (lists, key/value pairs).
- You want to keep the step text short and move data into the table.

## Tags
Use tags for filtering suites, not for documentation:

```
@smoke @stats @cli
Scenario: ...
```

## Cucumber compatibility guardrails
- Use ASCII only; keep formatting simple.
- Avoid Markdown inside steps; keep plain text.
- Use consistent naming and spacing for parameters.
- Avoid undefined or ambiguous step phrases.

## How agents learn this skill fast
- Read existing feature files for tone and vocabulary.
- Reuse step phrases found in `tests/steps/`.
- If no step exists, define a new one that is general and reusable.
- Ask for missing domain rules or acceptance criteria before guessing.


## Gherkin structure (Cucumber-compatible)

```
Feature: <short outcome-focused title>
  <1-2 lines of business value>

  Background:
    Given <shared preconditions>

  Scenario: <single behavior, clear outcome>
    Given <precondition>
    When <action>
    Then <observable outcome>

  Scenario Outline: <parameterized behavior>
    Given <precondition with <param>>
    When <action with <param>>
    Then <outcome with <param>>

    Examples:
      | param | result |
      | ...   | ...    |
```

At this folder `qa_testing_agent`, you could see Tiny examples (copy-ready) such as:
* ./single_scenario.feature

### Minimal template (copy-ready)

```
Feature: <feature name>
  <brief value statement>

  Scenario: <behavior>
    Given <precondition>
    When <action>
    Then <expected outcome>
```

### Feature with a single scenario:
```

```

### Feature for deterministic simulation testing:

```
Feature: Deterministic simulation runs
  Simulations must be repeatable with the same seed and inputs.

  Scenario: Same seed produces identical results
    Given a simulation seed "42"
    And the input dataset "fixtures/sim/input_small.json"
    When the simulation runs twice
    Then the outputs are byte-for-byte identical
```
