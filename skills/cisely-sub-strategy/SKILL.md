---
name: cisely-sub-strategy
description: >-
  Author Cisely sub-strategies — your own custom strategy kinds (a technology strategy, a go-to-market
  strategy, a people-&-culture strategy, etc.). Use when the user wants to define a company-wide
  template/schema for a kind of strategy and then fill in instances of it, or mentions sub-strategies,
  strategy types/kinds, a technology/GTM/people strategy, or wants to standardize what a given kind of
  strategy should contain. Covers defining the schema, publishing it, authoring records against it, and
  nesting them under a core strategy.
---

# Authoring sub-strategies in Cisely

Where Cisely's **core strategy** has a fixed shape the platform defines for you (winning aspiration,
where to play, how to win), **sub-strategies** are the *flexible* surface: you define your **own
strategy kinds** and shape them however your organization needs.

It works in two moves:

1. **Definition = the taxonomy.** You declare, once and company-wide, what a given kind of strategy
   (say a *Technology Strategy*) should contain — its fields, as a JSON Schema. This sets the standard.
2. **Record = the instance.** Different parts of the business then author records of that kind, each
   carrying actual values validated against the definition's schema. So you set the bar once, and each
   business unit fills it in.

A record **refines** a core strategy (or another record), connecting a specialized plan to the overall
direction so the whole tree is traversable.

## When to reach for this

The user wants a *kind* of strategy Cisely doesn't model natively — a technology strategy, a GTM
strategy, an internal people-&-culture strategy, an ESG strategy — and ideally wants it consistent
across the company. If they just want a single strategic posture, that's the **core strategy** (use the
`cisely-agency-model` skill instead).

## The flow

### A. Define the kind (the schema)

1. `CreateSubStrategyDefinition(key, display_name, description, validation_schema)` — creates a **DRAFT**
   definition. `key` is the stable identifier records will bind to (e.g. `technology-strategy`).
   `validation_schema` is a **JSON Schema (2020-12)** document describing the fields every record of
   this kind must have.
2. Iterate while DRAFT: `UpdateSubStrategyDefinitionSchema(id, validation_schema)` (allowed **only**
   while DRAFT).
3. `PublishSubStrategyDefinition(id)` — DRAFT → **PUBLISHED**. This **freezes the schema** and bumps its
   version. Records can now be authored against a stable contract. (You can't edit a published schema —
   publish a new version-bumped definition if the standard needs to change.)
4. `ArchiveSubStrategyDefinition(id)` — retire a kind; existing records remain, new ones are blocked.
5. Read: `GetSubStrategyDefinitionByKey(key)`, `ListSubStrategyDefinitions`.

**Designing the schema well:** keep it to the fields that genuinely define this kind of strategy at a
company level — the things you want every unit's version to address. Favor a small set of clear,
required fields over an exhaustive one. Use enums for controlled vocabularies, and descriptions on each
property (they guide the people filling it in).

### B. Author an instance (a record)

1. `CreateSubStrategyRecord(definition_key, slug, display_name, data)` — `data` is the JSON content,
   **validated against the bound definition's published schema** before it persists (a violation → 400,
   with the validation error). Fix `data` to satisfy the schema and retry.
2. `ReviseSubStrategyRecord(id, data)` — replace the data wholesale; re-validated against the schema.
3. Read: `GetSubStrategyRecord(id)`, `ListSubStrategyRecords`.

### C. Connect it to the strategy tree (REFINES)

A record should hang off the broader strategy it elaborates:

- `LinkSubStrategyParent(record_id, strategy_id)` — the record **refines a core strategy**.
- `LinkSubStrategyToSubStrategy(child_id, parent_id)` — nest a record under **another record** (e.g. a
  divisional technology strategy refining the company-wide one).
- A record has **at most one parent** of either kind, and the graph stays acyclic. Unlink first to move
  it (`UnlinkSubStrategyParent` / `UnlinkSubStrategyToSubStrategy`).

## Working with the user

- **Confirm the schema before publishing** — publishing freezes it. Show the user the JSON Schema and
  walk the fields; once instances exist against it, the contract should be stable.
- **Separate the two jobs explicitly:** "Let's first agree what a `<kind>` strategy should always
  contain (the template), then you (or each team) fill in your actual one (the record)."
- **Validate by attempting** — if a record's `data` 400s, read the validation error, explain what's
  missing/mismatched, and correct it.
- **Echo the UI link** after changes: records show at `https://app.cisely.dev/concise/sub-strategies/{id}`
  and beneath their strategy on the **Purpose Board** (`https://app.cisely.dev/concise/purpose`).

> Note: each sub-strategy tool's own MCP description now carries the when-to-use guidance and a
> "View in Cisely" link; this skill is the deeper guide to the end-to-end workflow (schema → publish →
> records → nest).
