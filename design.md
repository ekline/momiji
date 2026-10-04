# Momiji design checkpoint

Initial discussion checkpoint: 2026-10-04.

This records requirements and design directions before implementation. Proposed
details below remain subject to worked examples and schema review.

## Purpose and workflows

Support self-improvement and time management across one person's responsibilities.
Missed habits should not automatically become an accumulating debt.

The three principal workflows are:

1. Capture a task or structured set quickly, without requiring immediate categorization.
2. Review each morning, including milestones this week and this month, and choose
   a manageable daily plan.
3. Periodically look ahead, reorganize, and prune commitments.

The daily plan is distinct from the full set of commitments. Selecting something
for today does not invent a deadline. Plain-text output and compact shorthand
must support pen-and-paper use.

## Independent dimensions and identity

Completion policy, persona, source, access identity, ownership, attention, and
daily selection describe different aspects of a task.

A source may contain several personae; one persona may span multiple sources.
Several approved login accounts must be able to access the same personal planning
workspace. The login account does not implicitly select a persona. Source access
credentials are a separate concern from application login.

Use UUIDs for independently referenced entities. Names, abbreviations, hierarchy
paths, and Git remote URLs must not be reference identities. Entity boundaries,
cross-repository references, and identity mappings still need schema design.

## Completion and recurrence

Distinguish a task or persistent commitment from records of performing it.
Completing a one-time task finishes that task. Completing a recurring commitment
records an occurrence without retiring the commitment.

Support daily recurrence and targets such as gym visits 2–3 times per week.
A weekly target need not imply particular weekdays. Support both missed periods
that lapse and obligations that carry forward; their exact accumulation and
completion-allocation rules remain open. The minimum and optional upper target
must be considered explicitly rather than silently reducing a range to one number.

Retirement, deletion, skipping, completion correction, and recurrence-policy
changes require distinct, explicit semantics. Time-zone, week-boundary, travel,
and daylight-saving behavior remain open.

## Shared sources and private planning

Default planning primarily includes tasks assigned to the user or highlighted for
their attention. Full-source browsing remains available within source permissions.
Following an item for monitoring is a proposed additional relationship.

An unassigned task can request attention from several people so one can claim it.
Claiming changes shared ownership. Dismissing a request from one's own view must
not dismiss it for everyone. Consideration, accepted responsibility, and today's
selection are separate decisions.

Keep personal planning annotations separate from shared task content: choosing
today's focus must not automatically alter a team's priority. Imported tasks need
stable source associations, and supported shared edits should write back to their
source. Specific external integrations are not yet selected.

## Hierarchy and presentation

Support flexible-depth organizational paths and optional abbreviations, including:

- `SELF : FIT : stretch` — a personal fitness routine.
- `COMM : NEWS : publish issue` — a community newsletter milestone.

Do not fix the semantic meaning of each path position. Proposed organizational
nodes have stable IDs, names, abbreviations, and parent references. Persona
inheritance from explicitly configured nodes is a proposal, not yet a settled rule.

Organizational containment and task decomposition are different relationships.
Moving an item between categories must not implicitly change completion behavior.

The proposed card renderer uses abbreviated paths and optional short titles,
supports grouping and enough context to resolve ambiguity, and preserves the
user's daily order. The name Momiji suggests a maple leaf as a handwritten note;
seasonal visual variations should not imply failure or overdue status.

## Storage and versioning

The public software repository holds source, protobuf definitions, validators,
migrations, and hook adapters. Users maintain separate task repositories.
Protobuf Text Format (`.txtpb`) is the intended readable storage format.

Support model-generation directories such as `v0/`, `v1/`, and `v2/` in the
software/schema structure. A unified view must support, for example, personal
v2 data alongside work v0 data. Reading a repository must not migrate it; edits
must use its supported stored version. Migration is an explicit operation.

Keep application release, data-model generation, and validation policy version
conceptually distinct. Define compatibility before promising safe mixed-version
editing. Unsupported features must be explained; personal overlays may represent
some preferences, but cannot pretend to update an incapable shared source.

One file per task/commitment and separate completion records is the current layout
proposal. Decide formatting, comments, unknown-field preservation, reference
integrity, deletion, and file granularity before freezing the format.

## Git synchronization and acceptance

For a Git-backed source, `main` is authoritative and branches propose changes.
A shared claim is pending until accepted. Competing claims are arbitrated by
acceptance into `main`; claiming an unassigned task and reassigning an owned task
are different transitions.

Validation must check semantics against the exact old and proposed authoritative
states. A clean textual merge alone is insufficient. Serialize acceptance or use
an equivalent atomic update against the validated base, then retry stale proposals.

Local hooks provide early feedback. Where supported, server receive hooks enforce
acceptance; hosted providers may require equivalent checks. Hook wrappers invoke
the same Rust validation implementation as the application.

Support installed tooling and investigate pinned submodules for repeatable updates.
Hooks require explicit setup; shipping a submodule does not activate them.
Server enforcement uses server-selected tooling, not arbitrary code selected by
an incoming proposal. Tool upgrades and any migrations should be reviewable.

## Proposed Rust boundaries

| Crate | Responsibility |
|---|---|
| `momiji-model` | Versioned protobuf representations, txtpb I/O, migrations |
| `momiji-core` | Semantics, planning views, recurrence, transition validation |
| `momiji-git` | Checkouts, branches, synchronization, hook integration |
| `momiji-cli` | Capture, review, editing, validation, setup, migration |
| `momiji-web` | Later HTTP API and browser interface |

Keep core behavior independent of Git, HTTP, and CLI presentation. Initially build
only the crates needed to exercise the model. Generated types, semantic adapters,
and migration dependencies need a concrete dependency review before scaffolding.

## Deployment direction

A local Rust process can serve a browser interface and share its core with the CLI.
A hosted deployment can use persistent server storage for repositories and map
approved accounts to the same planning workspace, with per-user/source isolation.
A dedicated mobile application is not required. Git need not run in the browser.

Proposed Linux storage follows XDG:

| Base directory | Contents |
|---|---|
| `$XDG_CONFIG_HOME/momiji/` | Local configuration and source registrations |
| `$XDG_DATA_HOME/momiji/repos/` | Managed checkouts and unpublished work |
| `$XDG_STATE_HOME/momiji/` | Logs and local session state |
| `$XDG_CACHE_HOME/momiji/` | Rebuildable indexes and previews |

Allow existing user-managed checkouts. Unsynchronized changes are durable data,
not disposable cache. Separate local preferences from portable personal planning
records that belong in a private repository.

## Licensing

Apache-2.0 with a short contribution policy. No separate CLA or enforced DCO
process initially. The existing repository LICENSE is authoritative.

## Reference material

- [Git hooks](https://git-scm.com/docs/githooks)
- [Git submodules](https://git-scm.com/docs/gitsubmodules)
- [XDG Base Directory Specification](https://specifications.freedesktop.org/basedir/0.8/)
- [Apache License 2.0](https://www.apache.org/licenses/LICENSE-2.0)

See [the backlog](backlog.md) for the model-first implementation sequence.
