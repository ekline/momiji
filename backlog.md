# Momiji's TODO list

The first milestone is a reviewed data model, not a finished UI. Items below are
design and implementation work; documenting a requirement does not complete it.

## 1. Model and behavioral examples

- [ ] Define entity boundaries, UUID references, and cross-source identity rules.
- [ ] Separate access identities, personae, source membership, and credentials.
- [ ] Define ownership, attention requests, claiming, dismissal, and personal overlays.
- [ ] Define organizational hierarchy separately from task decomposition/dependencies.
- [ ] Define one-time completion, recurring commitments, and completion corrections.
- [ ] Define daily and weekly recurrence, target ranges, and missed-period policies.
- [ ] Define planning horizons, deadlines, milestones, and daily selection separately.
- [ ] Resolve time zones, week boundaries, travel, and daylight-saving transitions.
- [ ] Define retirement/deletion and the treatment of historical references.
- [ ] Walk the examples below through all relevant transitions before freezing schemas.

| Example | Behavior to demonstrate |
|---|---|
| `SELF : FIT : stretch` | Daily completion persists in history; commitment remains; exercise both missed-period policies |
| Gym 2–3 times weekly | Flexible visit dates; distinguish minimum and upper target; define period rollover |
| Make an appointment | One-time completion; distinguish booking from a subsequent appointment task |
| `COMM : NEWS : publish issue` | Milestone, supporting tasks, compact title, reorganization without changing identity |
| Shared family appointment | Several attention recipients; one claimant; losing claim does not become reassignment |
| Work team task | Shared ownership/status with private daily selection; full-source browsing remains possible |
| Personal v2 plus work v0 | One view; version-correct edits; no implicit migration or loss of unsupported data |
| Two offline edits | Pending versus accepted changes; deterministic handling of conflict and retry |
| Paper daily card | Selected order and meaningful shorthand without exposing UUIDs or inventing deadlines |

## 2. Schemas and repository format

- [ ] Draft the initial protobuf generation and worked `.txtpb` fixtures.
- [ ] Choose proto syntax/edition and Rust tooling after checking txtpb support.
- [ ] Decide file granularity and layout for tasks, nodes, completions, and overlays.
- [ ] Define deterministic formatting and comment/unknown-field round-trip behavior.
- [ ] Define model-version discovery, client compatibility, and refusal of unsafe writes.
- [ ] Specify explicit migrations and how they are reviewed and recovered.
- [ ] Review crate dependencies and create the minimum Rust workspace.

## 3. Local vertical slice

- [ ] Implement reading, validation, capture, editing, and completion for the examples.
- [ ] Produce a mixed-persona daily view and compact plain-text card.
- [ ] Implement forward planning and pruning/retirement.
- [ ] Register existing checkouts and support managed XDG storage.
- [ ] Test semantic invariants and representative transitions using the agreed fixtures.

## 4. Shared Git workflow

- [ ] Implement proposed branches and accepted authoritative state.
- [ ] Validate transitions against the exact accepted base; handle concurrent claims.
- [ ] Expose validation through the CLI and thin hook wrappers.
- [ ] Specify setup, installed-release and pinned-submodule workflows, and upgrades.
- [ ] Verify enforcement options for the selected Git host.
- [ ] Exercise offline edits, rejected changes, retries, and recoverable unpublished work.

## 5. Web access and additional sources

- [ ] Serve the browser interface locally using the shared core.
- [ ] Support hosted persistence and multiple approved logins for one workspace.
- [ ] Define source permissions, credential handling, and private/shared data boundaries.
- [ ] Select initial external-source adapters and specify supported write-back behavior.
- [ ] Develop Momiji's maple-leaf branding and optional seasonal themes.

A native mobile app is not an initial requirement. Model review precedes interface
polish and integrations.
