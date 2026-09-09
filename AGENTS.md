# IRAKURLE

## Table of Contents

- Patterns
- Architecture elements
- Goals

## Architecture Elements

### Patterns

* Domain layer has no dependency on Hono/React/Drizzle/Zod. 
* Shared package owns DTO + Zod schemas + GraphSpec + RPC typings. 
* Ports & Adapters: Each adapter corresponds to one or more ports.
* Observer: Event bus delivering domain events to listeners (e.g., cache invalidators)

### Frontend

- Bun
- React 
- Vite
- Vitest
- AntV X6
- Ant Design
- MagicUI
- Tailwind CSS
- TanStack Query/Router/Table
- CopilotKit
- Framer Motion
- React Hook Form

### Backend

- Bun
- Hono: Hono views used to serve the frontend shell. API: auth + tenant-scoped services + repos (drizzle), outbox + processor + simulation engine stub.
- Drizzle ORM - Shared schemas + rpc contract
- Better Auth

### Contract API

| Domain | Entities| Request  | Endpoint | Description|
|--------|---------|----------|------------|------|
| Landscape |Tenants   | GET  | /api/v1/tenants | List all tenants |
| Landscape |Tenants   | GET  | /api/v1/tenants/{id}/landscapes | |
| Landscape |Landscape | GET  | /api/v1/landscape?tenant_id= | |
| Landscape | GET | /api/v1/landscapes  | |
| Landscape | GET | /api/v1/applications | |
| Landscape |Landscape | GET  | /api/v1/landscapes/{id}/graph | returns ViewS │ pec.graph DTO |
| Landscape |Landscape | GET  | /api/v1/views/landscape?tenant_id=&landscape_│ id=&scenario_id= | returns full ViewSpec (graph + panels)|
| Analysis  | Scenarios | POST | /api/v1/scenarios | assumptions YAML or structured DTO |
| Analysis  | Scenarios | GET  | /api/v1/scenarios?tenant_id=&landscape_id= |       │ paginated|
| Analysis  | Scenarios | POST | /api/v1/scenarios/{id}:clone ||
| Analysis  | Scenarios | POST | /api/v1/scenarios/{id}:run | emits simulation.requested, returns run id |
| Analysis  | Scenarios | POST | /api/v1/scenarios | |
| Analysis  | Scenarios | POST | /api/v1/scenarios/{id}/runs  | Trigger: user │  requests simulation run Outcome: creates sim_run(status=queued) + enqueues outbox event sim_run.requested |
| Simulation | Simulation Runs | GET  | /api/v1/runs/{id}  | |
| Simulation | Simulation Runs | GET  | /api/v1/runs/{id}/results | |
| Simulation | Simulation Runs | GET  | /api/v1/sim-runs?scenario_id= | (paginated) |

### Contract API Entities

### Contract API Queries

- All queries have the following preconditions: Authenticated user, TenantId derived from auth context.
- TCO one-time + recurring aggregated by year, and total.
- TTM per node and edge estimate in weeks. It works scenario-level "critical path weeks" + "risk buffer weeks" => buffered delivery

### Contract API Invariants

* Costs >= 0
* Weeks >= 0
* Tenant scoping: tenantId must come from auth context
* Run state machine: queued -> running -> completed|failed

### Contract API Events

All domain events persisted to `event_outbox`. OutboxProcessor consumes event run.requested, its Outcome marks run running, computes, stores result snapshot, marks completed. These are core events, shared contracts:
- tenantCreated
- landscapeCreated
- scenarioCreated
- SimRunRequested
- simulationRequested
- SimRunStarted
- simulationStarted
- SimRunCompleted
- simulationCompleted
- simulationFailed

### Ports

* Ports: interfaces used by application (repositories, event bus, clock, auth context, config).

- ScenarioRepositoryPort
- LandscapeRepositoryPort
- AnalyticsRepositoryPort
- EventBusPort
- AuthPort
- ClockPort

### Adapters

* HTTP routes/controllers, DB repositories (Drizzle), auth provider (Better Auth), RPC client (web), UI renderer (X6) and registries.
* **Graph renderer adapter (X6):** Graph renderer adapter rendered via AntV X6 + Ant Design + Tailwind.
* Panel renderer (AntD components) driven by ViewSpec.panels

## Goals

## Read First Agent Instructions

This repository is specification driven. The agents MUST read these files before coding:
- Use .claude/skills such as `feature-creator`, `okf-bundler`,`orchestrator`, `delta-planner`.
- Preserve boundaries and invariants in `/specs/constituion.md`.
- To implement ONLY what is specified in `/specs/features` and `/specs/deltas`.

## Architecture Decision Records
The Architecture decision records is centralized in a single folder `/specs/features`

It contains the non-negotiables instructions, Definition of Ready (per task), Definition of Done (per task).

### Core Principles
- **Hexagonal (ports/adapters)** + event-driven (domain events + outbox). Ports & Adapters & Interfaces: Each adapter corresponds to one or more ports.  Service registry wiring ports to adapters.
- Event-driven workflow for simulations (outbox + worker + status streaming).
- Domain events are emitted via Outbox (DB table).
- OutboxProcessor pulls durable intent, replayable events, and dispatches to handlers (Observer pattern).EventBus subscribe/publish. OutboxProcessor consumes sim_run.requested. The outcome marks run running, computes, stores result, snapshot, marks completed.
- SimulationEngine is triggered asynchronously (in-process for MVP).
- **Specification-Driven**: All work maps to a feature spec in `specs/features
- **Contract-First**: Design by Contract with invariants, preconditions, post-conditions
- **Composable Delivery**: Each feature delivered as atoms → molecules → systems.
- **Local-First**: All scans, bundles, baselines, and caches under `./specs`
- **core:** Pure types, invariants, domain events, entities, value objects, policies; no IO.

### Non-negotiables Instructions

* Tenant scoping is derived from auth context/session and to scope data.
* No SQL in route handlers. Routes call Application Services only.
* Event-driven runs enqueues outbox event. OutboxProcessor performs computation and persists results.

### Definiton of Ready (per task)

1. Feature refers to centralized BDD (Gherkin scenarios) and Unit Tests.
2. Ensure every @scenario_id in Gherkin has at least one test or step definition.
3. End-to-end happy path: create scenario -> run -> results -> render.
4. Unit tests added/updated.
5. BDD scenarios passing (list commands, e.g. `bun test`.
6. User can authenticate (HTTP Basic) and load a landscape and a scenario.
7. User can create/select a scenario (assumptions YAML).
8. 


### Definition of Done (per task)
1. Shared schemas + RPC contract. The schemas exist in packages/shared and are used on both sides.
2. A Gherkin scenario in /specs/behaviors has a matching automated test (api or web).
3. App builds and tests pass
