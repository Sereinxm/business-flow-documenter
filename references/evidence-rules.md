# Evidence Rules

Use these rules to keep the final business process document trustworthy.

## Evidence strength

Strong evidence:

- Route definitions, controllers, handlers, resolvers.
- Service, use case, workflow, or transaction code.
- ORM models, database schemas, migrations, SQL files.
- Status enums and state transition code.
- Tests that assert behavior.
- Config files that define integrations, queues, jobs, or feature flags.

Medium evidence:

- README, docs, comments, ADRs.
- Type names, DTOs, interface names.
- UI labels and page routes.

Weak evidence:

- Folder names alone.
- Unused files.
- Comments that conflict with implementation.
- Old docs not supported by current code.

## Claim labels

Use these labels when needed:

- `Verified`: supported by strong evidence.
- `Likely`: supported by medium evidence or partial call chains.
- `Assumption`: plausible but not fully verified.
- `Needs confirmation`: cannot be verified from available code.

## Evidence format

Preferred format:

```markdown
- `src/modules/order/order.controller.ts`
  - `OrderController.createOrder`
  - Notes: order creation entrypoint.
```

For tables:

```markdown
| Claim | Evidence | Confidence |
|---|---|---|
| Order creation starts at POST /orders | `src/modules/order/order.controller.ts`, `createOrder` | Verified |
```

## Verification checklist

Before finalizing, check:

- Each core flow has at least one entrypoint and one service/usecase reference.
- Each state transition has enum or update-code evidence.
- Each data lifecycle claim points to a model, schema, migration, repository, or SQL file.
- Each external integration claim points to a client, config, SDK usage, or request code.
- Each permission claim points to middleware, guard, policy, role checks, or tests.
- Unsupported claims are moved to uncertainty sections.
