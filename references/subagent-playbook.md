# Subagent Playbook

Use this playbook to coordinate focused analysis passes across a large repository.

## Workspace convention

Write each pass to a separate file under `.ai-analysis/`. Each pass must include:

- Scope inspected.
- Key findings.
- Evidence table.
- Uncertainties.
- Suggested follow-up paths.

## Pass 1: Repo Mapper

Goal: understand the project shape before deep analysis.

Inspect first:

- README and docs index.
- Dependency manifests: package.json, pnpm-lock.yaml, yarn.lock, pom.xml, build.gradle, go.mod, Cargo.toml, requirements.txt, pyproject.toml.
- Runtime files: Dockerfile, docker-compose.yml, Procfile, serverless config, deployment config.
- Source roots: src, app, server, backend, frontend, api, routes, controllers, services, models, repositories, jobs, workers.

Output sections:

```markdown
# Repo Map

## Technology stack
## Runtime and entrypoints
## Core directories
## Major configuration files
## System layering
## Core modules
## Modules needing deeper analysis
## Evidence
## Uncertainties
```

## Pass 2: Business Domain

Goal: extract business language and domain model.

Look for model, entity, schema, enum, constants, domain, types, dto, interface, test descriptions, page names, route names.

Output sections:

```markdown
# Business Domain Model

## Domain overview
## Glossary
| Term | Meaning | Evidence |
|---|---|---|
## Core entities
| Entity | Meaning | Evidence |
|---|---|---|
## Roles
| Role | Meaning | Evidence |
|---|---|---|
## Status enums
| Status | Meaning | Source |
|---|---|---|
## Entity relationships
## Uncertainties
```

## Pass 3: API / Entry Analysis

Goal: map external entrypoints to business actions.

Include HTTP, RPC, GraphQL, webhooks, CLI commands, queue consumers, cron triggers.

Output sections:

```markdown
# API / Entry Analysis

## Entrypoint overview
| Method / Type | Path / Entry | Business action | Handler | Evidence |
|---|---|---|---|---|
## Call chains
## Request input summary
## Response output summary
## Middleware / authorization
## Uncertainties
```

## Pass 4: Service Flow

Goal: reconstruct core business processes from actual call chains.

Inspect service, usecase, workflow, manager, transaction, domain service, error handling.

For each flow include:

```markdown
### Flow: [name]

#### Business goal
#### Trigger
#### Participating modules
| Module | File | Responsibility |
|---|---|---|
#### Execution steps
#### Branches
| Condition | Result | Evidence |
|---|---|---|
#### State changes
| From | To | Trigger | Evidence |
|---|---|---|---|
#### Error handling
| Scenario | Handling | Evidence |
|---|---|---|
#### Call chain
```text
Route -> Handler -> Service -> Repository -> Database
```
#### Uncertainties
```

## Pass 5: Data Model

Goal: map data objects and lifecycle.

Inspect schema, migrations, ORM models, repositories, DAO, SQL, database clients.

Output sections:

```markdown
# Data Model Analysis

## Core tables / models
| Table / Model | Business meaning | Evidence |
|---|---|---|
## Field notes
## Relationships
| Entity A | Relationship | Entity B | Evidence |
|---|---|---|---|
## Read/write scenarios
| Object | Create | Update | Read | Delete |
|---|---|---|---|---|
## Data lifecycle
## Uncertainties
```

## Pass 6: Frontend / UI

Run only if frontend code exists.

Inspect pages, routes, views, components, store, hooks, API clients, forms, tables, dashboards.

Output sections:

```markdown
# Frontend / UI Flow Analysis

## Pages / routes
| Page | Route | Business meaning | Evidence |
|---|---|---|---|
## User operation paths
## Frontend API calls
| Page / Component | API | Business action | Evidence |
|---|---|---|---|
## State management
## Frontend-backend mapping
## Uncertainties
```

## Pass 7: Integrations

Goal: identify system boundaries and third-party dependencies.

Search for SDK clients, HTTP clients, webhooks, MQ, Kafka, RabbitMQ, Redis, S3, OSS, Stripe, payment, email, SMS, notification, search, AI provider, external API.

Output sections:

```markdown
# Integration Analysis

## External dependencies
| System | Purpose | Call location | Evidence |
|---|---|---|---|
## Integration method
## Call timing
## Inputs and outputs
## Retry / fallback logic
## System boundaries
## Uncertainties
```

## Pass 8: Tasks and Messaging

Goal: understand asynchronous behavior.

Inspect cron, schedule, job, worker, queue, consumer, producer, event, listener, subscriber, task.

Output sections:

```markdown
# Tasks and Messaging Analysis

## Scheduled jobs
| Job | Trigger | Business role | Evidence |
|---|---|---|---|
## Async jobs
| Job | Enqueue location | Consumer | Business role | Evidence |
|---|---|---|---|---|
## Message flows
| Message / Event | Producer | Consumer | Business meaning |
|---|---|---|---|
## Retry behavior
## Impact on main flows
## Uncertainties
```

## Pass 9: Permissions and Validation

Goal: identify auth, authorization, validation, and audit rules.

Inspect auth, guard, middleware, permission, role, policy, validator, schema validation, zod, joi, class-validator, audit, log.

Output sections:

```markdown
# Permissions and Validation Analysis

## Authentication
## Authorization model
| Permission / Role | Allowed actions | Evidence |
|---|---|---|
## Input validation
| Entry | Rule | Evidence |
|---|---|---|
## Business rule validation
| Rule | Scenario | Evidence |
|---|---|---|
## Sensitive operations
## Audit logs
## Uncertainties
```

## Pass 10: Tests Verification

Goal: validate business behavior through tests.

Inspect test, tests, spec, e2e, integration test, unit test, fixture, mock, snapshot.

Output sections:

```markdown
# Tests Verification

## Business flows covered by tests
| Test file | Business behavior | Evidence |
|---|---|---|
## Business rules from tests
| Rule | Test location | Notes |
|---|---|---|
## Error and boundary cases
## Mapping to flow analysis
## Gaps found
## Uncertainties
```

## Pass 11: Final Verification

Goal: prevent unsupported conclusions.

Output sections:

```markdown
# Verification Report

## Verified conclusions
## Claims with insufficient evidence
## Conflicts
## Potentially missing flows
## Questions for human confirmation
| Question | Reason | Suggested owner |
|---|---|---|
```
