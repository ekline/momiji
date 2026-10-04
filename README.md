# Momiji

Momiji is a personal planning application being designed around recurring
commitments, one-time tasks, and a single view across personal, work, and community
responsibilities. Its compact daily view should be easy to transcribe onto a
3×5 card.

The project is in the design stage. There is no runnable application yet.
Getting the data model right is the first milestone.

## Design direction

- Rust libraries and CLI, with a web interface that can be served locally or hosted.
- Human- and agent-readable protobuf Text Format (`.txtpb`) records in Git repositories.
- Stable UUID references, flexible hierarchy, and configurable shorthand.
- Multiple sources and data-model generations in one planning view.
- Separate ownership, attention, persona, and personal planning information.
- Recurring commitments with completion history and explicit missed-period policies.

See the [design checkpoint](docs/design.md) and [project backlog](docs/backlog.md).
The checkpoint distinguishes agreed requirements from implementation proposals
and unresolved questions; it is not a frozen schema.

## Contributing and license

Momiji is licensed under [Apache-2.0](LICENSE).
See [CONTRIBUTING.md](CONTRIBUTING.md) before submitting changes.
