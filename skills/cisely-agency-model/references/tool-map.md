# Cisely MCP tool map

> Which tools build which concept, the order, the edges that form, and the gotchas. Tools are exposed
> as `mcp__cisely__<Name>`. Every tool's own description carries detailed when-to-use guidance and a
> "View in Cisely" link — read it when in doubt.

## The universal lifecycle

Every node follows `Create → Revise → Activate → (Archive)`:

- **Create** makes an empty **DRAFT** and returns its `id`.
- **Revise** replaces the **whole declaration** (not a delta — always send the full content). Each
  revision is snapshotted to history.
- **Activate** commits DRAFT → ACTIVE, but only if the declaration carries the required content
  (else a `*_CONTENT_REQUIRED` 409 tells you exactly what's missing). **Only build edges onto
  things you intend to activate.**
- **Archive** retires ACTIVE → ARCHIVED, keeping history. Use it for *declined* / retired concepts —
  record the rationale.

`Get<X>` / `List<X>` read; `List<X>Revisions` shows history.

## Response side

| Concept | Create | Activate needs | Born-linked edge | Discovered-later links |
|---|---|---|---|---|
| **Mission** (singleton) | `EstablishMission` (no input) | `purpose` | — | — |
| **Strategy** | `CreateStrategy(slug, display_name)` — **requires an active mission** (409 `MISSION_REQUIRED`) | `winning_aspiration` + `where_to_play` + `how_to_win` | →PURSUES→ Mission (auto) | `LinkStrategyRootedInBelief`; `LinkStrategyParent` (nest) |
| **Initiative** | `CreateInitiative(slug, display_name, strategy_id)` | core content **and** a falsifiable hypothesis (`statement` + `falsification_criterion`) | →EXECUTES→ Strategy (auto) | measured by metrics |
| **Metric** | `CreateMetric(slug, display_name)` (free-floating) | `unit` + `direction` + `target_value` | — | `LinkMetricMeasuresStrategy` / `…Initiative` / `…Expectation`; `RecordObservation` |

- **Mission flow:** `EstablishMission` → `ReviseMission(declaration: purpose, just_cause, vision, core_values)` → `ActivateMission`. A second establish 409s (singleton).
- **Strategy** auto-pursues the mission at create. Root it in a belief and, where it's really a
  sub-plan of a broader strategy, nest with `LinkStrategyParent` (one parent; no cycles).
- **Initiative** can't activate without a hypothesis you could be proven wrong about.
- **Metric**: prefer linking to an **expectation** — that closes the demand↔response loop. Feed
  readings with `RecordObservation(metric_id, value, observed_at)` (metric must be ACTIVE).

## Demand side

| Concept | Create | Activate needs | Born-linked edge | Discovered-later links |
|---|---|---|---|---|
| **Persona** | `CreatePersona(slug, display_name, nature)` (free-floating) | `characterization` + the nature-matching profile | — | holds goals; situated as stakeholders |
| **Goal** | `CreateGoal(slug, display_name, persona_id)` | `statement` | Persona →HOLDS→ Goal (auto) | `LinkGoalRootedInBelief`; proxied by expectations |
| **Operating Context** | `CreateOperatingContext(slug, display_name)` (free-floating) | `purpose` + `category` + `cadence` | — | stakeholders situate into it |
| **Stakeholder** | `SituateStakeholder(persona_id, operating_context_id, display_name, declaration)` — **needs both parents**; declaration **must** carry `principal_hood` | `principal_hood` + `influence` + `interest` | →OF_PERSONA→ Persona and →IN_CONTEXT→ Context (auto) | holds expectations |
| **Expectation** | `CreateExpectation(slug, display_name, stakeholder_id)` | `statement` + `valence` + `articulation` | Expectation →HELD_BY→ Stakeholder (auto) | `LinkExpectationProxiesForGoal`; measured by metrics |
| **Belief** | `CreateBelief(slug, display_name)` (free-standing) | `statement` + `conviction` | — | goals & strategies root into it |

- **`nature` is immutable** and dictates the profile the declaration must carry (HUMAN→human
  profile, etc.). Pick it deliberately at create.
- **Stakeholder has no "create" verb** — it is **situated**. It is born carrying `principal_hood`
  (`FREE_STANDING` or `DELEGATED`); omitting it 409s `STAKEHOLDER_CARGO_REQUIRED`. One stakeholder per
  (persona, context) pair.
- **`LinkExpectationProxiesForGoal` is guarded:** the goal's holder-persona must equal the
  expectation's stakeholder's persona, else 409 `EXPECTATION_GOAL_HOLDER_MISMATCH`. (You can only
  proxy a goal held by the same identity that holds the expectation.)

## Belief rooting & alignment

`LinkGoalRootedInBelief` and `LinkStrategyRootedInBelief` are **deliberately unguarded** — the whole
point is that a goal and a strategy can converge on the **same belief instance**. When you see that a
stakeholder's goal and a business strategy spring from the same conviction, root them in **one**
belief. That convergence is the measurable alignment signal — actively look for it.

## Recommended session order

1. `GetMission` / `ListPersonas` / `ListStrategies` … — see what exists first.
2. Mission: establish → revise → activate.
3. Beliefs: create → revise → activate (capture the company's convictions early).
4. Per persona: create → revise → activate; then its goals (create → revise → activate → root in
   belief).
5. Operating contexts: create → revise → activate.
6. Situate stakeholders (persona × context); then their expectations (create → revise → activate →
   proxy-for-goal).
7. Strategies: create → revise → activate → root in belief (aim to share a belief with a goal).
8. Initiatives (with hypothesis) → activate. Metrics → activate → link (prefer expectations) →
   observe.
9. Synthesize JTBD / VPC as views (`/cisely:canvas`).

## Edge reference (relationship → tool)

| Edge | Tool | Auto at create? | Guard |
|---|---|---|---|
| Persona →HOLDS→ Goal | `CreateGoal` | yes | persona must exist |
| Expectation →HELD_BY→ Stakeholder | `CreateExpectation` | yes | stakeholder must exist |
| Stakeholder →OF_PERSONA / IN_CONTEXT | `SituateStakeholder` | yes | both parents exist; principal-hood present |
| Strategy →PURSUES→ Mission | `CreateStrategy` | yes | active mission exists |
| Initiative →EXECUTES→ Strategy | `CreateInitiative` | yes | strategy exists |
| Expectation →PROXIES_FOR→ Goal | `LinkExpectationProxiesForGoal` | no | same holder-persona |
| Goal →ROOTED_IN→ Belief | `LinkGoalRootedInBelief` | no | unguarded (alignment) |
| Strategy →ROOTED_IN→ Belief | `LinkStrategyRootedInBelief` | no | unguarded (alignment) |
| Metric →MEASURES→ Strategy/Initiative/Expectation | `LinkMetricMeasures*` | no | — |
| Strategy →PARENT_OF→ Strategy | `LinkStrategyParent` | no | one parent, acyclic |
| SubStrategy →REFINES→ Strategy/SubStrategy | `LinkSubStrategyParent` / `…ToSubStrategy` | no | one parent, acyclic — see `cisely-sub-strategy` skill |
