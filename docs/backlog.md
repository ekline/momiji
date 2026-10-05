# Momiji's TODO list

The first milestone is a reviewed data model, not a finished UI. Items below are
design and implementation work; documenting a requirement does not complete it.

## 1. Model and behavioral examples

Tranche 1 documentation is in [the data model](data-model.md) and
[the model scenarios](model-scenarios.md). The items checked below are
documentation only. The model itself has not been reviewed or frozen.

- [x] Document candidate tasks, occurrences, activities, contributions, assessments,
      planning entries, transitions, and deadline changes, with their invariants.
- [x] Walk the daily stretch, gym/marathon, and insurance/bookkeeping scenarios,
      plus the additional review checks.
- [x] Document the lapse-strategy compatibility matrix, successor-task supersession,
      and same-identity deadline changes.
- [ ] Review tranche 1 and settle the decisions that block an initial schema draft:
      occurrence identity (Q1), the date and time-zone model (Q3), and record shapes
      for corrections, skips, transitions, and attribution (Q5).
- [ ] Settle or explicitly defer keep-one semantics and credit allocation (Q2),
      deadline/period interaction (Q4), contribution measures (Q6), supersession
      graph and follow policies (Q7), and parent cardinality and dependency
      enforcement (Q8).
- [ ] Define entity boundaries, UUID references, and cross-source identity rules (Q9).
- [ ] Separate access identities, personae, source membership, and credentials.
- [ ] Define ownership, attention requests, claiming, dismissal, and personal overlays.
- [ ] Define organizational hierarchy separately from task decomposition/dependencies.
- [ ] Finalize one-time completion, recurring commitments, and correction/retraction.
- [ ] Finalize recurrence strategies, target ranges, and lapse strategies.
- [ ] Define planning horizons, deadlines, milestones, and daily selection separately.
- [ ] Resolve time zones, week boundaries, travel, and daylight-saving transitions.
- [ ] Define retirement/deletion and the treatment of historical references.
- [ ] Walk the examples below through all relevant transitions before freezing schemas.

| Example | Behavior to demonstrate |
|---|---|
| `SELF : FIT : stretch` | Drafted in scenario A: late recording, skip, three lapse strategies, weekly-to-daily successor |
| Gym 2–3 times weekly | Drafted in scenario B: minimum versus preferred, one session serving two objectives, duplicate credit |
| Insurance payment and bookkeeping | Drafted in scenario C: occurrence dependency, deadline extension, ongoing parents |
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
- [ ] Decide file granularity and layout for tasks, nodes, activities, contributions,
      and overlays (Q10; individual activity files are the current leaning).
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
