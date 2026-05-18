# Mermaid Guidelines

Use Mermaid diagrams in `docs/business-flow.md` only when backed by evidence.

## Required diagram families

Try to include these when applicable:

1. Architecture: `flowchart TD`
2. Core process overview: `flowchart TD`
3. Key flow sequence: `sequenceDiagram`
4. State transition: `stateDiagram-v2`
5. Data relationship: `erDiagram`
6. External integration: `flowchart LR`

## Syntax rules

- Always use fenced Markdown code blocks tagged as `mermaid`.
- Use business-language node names, not only function names.
- Avoid special characters that commonly break Mermaid node parsing.
- Prefer short node labels.
- Split large diagrams into multiple smaller diagrams.
- Do not place Markdown tables inside Mermaid blocks.
- For labels with punctuation, prefer simple text or quoted labels where supported.
- Validate mentally that each edge has valid node identifiers.

## Safe examples

Architecture:

```mermaid
flowchart TD
    User[User] --> API[API Layer]
    API --> Service[Business Service]
    Service --> Repo[Repository]
    Repo --> DB[(Database)]
```

Sequence:

```mermaid
sequenceDiagram
    participant User
    participant API
    participant Service
    participant DB
    User->>API: Submit request
    API->>Service: Execute use case
    Service->>DB: Persist data
    DB-->>Service: Return result
    Service-->>API: Return business result
    API-->>User: Return response
```

State:

```mermaid
stateDiagram-v2
    [*] --> Created
    Created --> Processing
    Processing --> Completed
    Processing --> Failed
```

Entity relationship:

```mermaid
erDiagram
    USER ||--o{ ORDER : creates
    ORDER ||--o{ ORDER_ITEM : contains
```
