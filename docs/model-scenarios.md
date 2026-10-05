# Momiji model scenarios

Tranche 1 worked scenarios, prepared from the design discussion of 2026-10-04.

These scenarios exercise the [candidate data model](data-model.md). Each one shows
its assumptions, identities, relevant fields, ordered user actions, the records
created or changed, derived assessments before and after, and what the user
ordinarily sees.

**Record notation.** The record sketches are conceptual. They are not protobuf Text
Format and have not been validated against any schema. Field names are discussion
vocabulary from the data model. Status labels (**Agreed**, **Proposed**, **Open**,
Q1–Q10) follow the data model.

## Conventions

### Clock and calendar assumptions

These apply to these examples only; they are not global defaults (Q3).

- All times are local in a fixed UTC−05:00 offset, written `-05:00`. No
  daylight-saving change occurs within any example.
- Days run from local midnight to local midnight. Weeks are ISO weeks that start on
  Monday, so `2026-W41` is Monday 5 October to Sunday 11 October 2026.
- Periods are half-open, `[start, end)`.
- Each assessment states its evaluation time, written `E` followed by a date and time.
- One user, `A-USER`, records everything in one personal source. In scenario C, the
  household tasks are in a second source.

### Identity legend

The example UUIDs follow a readable pattern: the first four hex digits of the last
group encode the kind of entity. Real UUIDs are random. Occurrence UUIDs are
illustrative only, because the addressing scheme for computed occurrences is open
(Q1). Sketches use aliases; each alias always stands for the UUID below.

| Alias | UUID | Meaning |
|---|---|---|
| `N-SELF` | `00000000-0000-4000-8000-000100000001` | Organizational node `SELF` |
| `N-FIT` | `00000000-0000-4000-8000-000100000002` | Node `FIT`, child of `SELF` |
| `N-HOME` | `00000000-0000-4000-8000-000100000003` | Node `HOME` |
| `N-MONEY` | `00000000-0000-4000-8000-000100000004` | Node `MONEY` |
| `T-STRETCH-WK` | `00000000-0000-4000-8000-000200000001` | Stretch three times weekly; predecessor |
| `T-STRETCH` | `00000000-0000-4000-8000-000200000002` | Stretch daily; successor |
| `T-GYM` | `00000000-0000-4000-8000-000200000003` | Gym, minimum 2 and preferred 3 per week |
| `T-MARA-STR` | `00000000-0000-4000-8000-000200000004` | Marathon plan: weekly strength session |
| `T-HOUSE` | `00000000-0000-4000-8000-000200000005` | Maintain the house; ongoing |
| `T-FIN` | `00000000-0000-4000-8000-000200000006` | Manage finances; ongoing |
| `T-GARBAGE` | `00000000-0000-4000-8000-000200000007` | Take out garbage weekly |
| `T-RECYCLE` | `00000000-0000-4000-8000-000200000008` | Take out recycling weekly |
| `T-INS-PAY` | `00000000-0000-4000-8000-000200000009` | Pay home insurance premium for each cycle |
| `T-INS-BOOK` | `00000000-0000-4000-8000-00020000000a` | Record each insurance payment in bookkeeping |
| `T-BUDGET-YR` | `00000000-0000-4000-8000-00020000000b` | Review household budget annually |
| `T-BUDGET-WK` | `00000000-0000-4000-8000-00020000000c` | Review household budget weekly; successor |
| `T-APPT` | `00000000-0000-4000-8000-00020000000d` | Make a dentist appointment; undated, one-time |
| `T-REPORT` | `00000000-0000-4000-8000-00020000000e` | Submit monthly expense report |
| `O-SWK-W39` | `00000000-0000-4000-8000-000300000001` | `T-STRETCH-WK`, week `2026-W39` |
| `O-SWK-W40` | `00000000-0000-4000-8000-000300000002` | `T-STRETCH-WK`, week `2026-W40` |
| `O-S-1001` | `00000000-0000-4000-8000-000300000003` | `T-STRETCH`, day `2026-10-01` |
| `O-S-1002` | `00000000-0000-4000-8000-000300000004` | `T-STRETCH`, day `2026-10-02` |
| `O-S-1003` | `00000000-0000-4000-8000-000300000005` | `T-STRETCH`, day `2026-10-03` |
| `O-S-1004` | `00000000-0000-4000-8000-000300000006` | `T-STRETCH`, day `2026-10-04` |
| `O-GYM-W41` | `00000000-0000-4000-8000-000300000008` | `T-GYM`, week `2026-W41` |
| `O-MARA-W41` | `00000000-0000-4000-8000-000300000009` | `T-MARA-STR`, week `2026-W41` |
| `O-PAY-C2605` | `00000000-0000-4000-8000-00030000000a` | `T-INS-PAY`, cycle starting 2026-05-01 |
| `O-PAY-C2611` | `00000000-0000-4000-8000-00030000000b` | `T-INS-PAY`, cycle starting 2026-11-01 |
| `O-BOOK-C2605` | `00000000-0000-4000-8000-00030000000c` | `T-INS-BOOK`, cycle starting 2026-05-01 |
| `O-BOOK-C2611` | `00000000-0000-4000-8000-00030000000d` | `T-INS-BOOK`, cycle starting 2026-11-01 |
| `O-BUDGET-2026` | `00000000-0000-4000-8000-00030000000e` | `T-BUDGET-YR`, year 2026 |
| `O-APPT` | `00000000-0000-4000-8000-00030000000f` | `T-APPT`, its single occurrence |
| `O-REPORT-2603` | `00000000-0000-4000-8000-000300000010` | `T-REPORT`, month `2026-03` |
| `AC-NN` | `00000000-0000-4000-8000-0004000000NN` | Activity number `NN` |
| `CN-NN` | `00000000-0000-4000-8000-0005000000NN` | Contribution number `NN` |
| `R-NN` | `00000000-0000-4000-8000-0006000000NN` | Transitions, skips, removals, deadline changes, retractions |
| `PE-NN` | `00000000-0000-4000-8000-0008000000NN` | Personal planning entries |
| `A-USER` | `00000000-0000-4000-8000-000700000001` | The example user |
| `W-PLAN` | `00000000-0000-4000-8000-000700000002` | The user's planning workspace |

A computed occurrence that has not been stored is written as task plus period, such
as `(T-STRETCH, 2026-10-05)`.

---

## A. Daily stretching

### Assumptions

- The task is `SELF : FIT : stretch`.
- Before October the user stretched three times a week. On 30 September they switch
  to stretching every day. This is a substantive change, so it creates a successor
  task.
- The daily task has one occurrence per local calendar day, with a minimum of 1 and
  no preferred target.
- The daily run is shown three times, once under each lapse strategy. The three runs
  are **alternative histories**, not policies active at the same time. In each run,
  `T-STRETCH` has the same UUID and only its lapse strategy differs.

### A1. Weekly-to-daily successor

#### Task definitions

```text
# Conceptual sketch, not protobuf Text Format
task T-STRETCH-WK                      # 00000000-0000-4000-8000-000200000001
  title: "stretch"
  node: N-FIT                          # displayed as SELF : FIT : stretch
  expectation:
    mode: recurring
    generation: calendar periods, ISO week, Monday start, zone UTC-05:00
    satisfaction: minimum 3
    lapse: lapse

task T-STRETCH                         # 00000000-0000-4000-8000-000200000002
  title: "stretch"                     # metadata copied from T-STRETCH-WK
  node: N-FIT
  supersedes: T-STRETCH-WK
  expectation:
    mode: recurring
    generation: calendar periods, local day, zone UTC-05:00
    satisfaction: minimum 1
    lapse: <lapse | keep one outstanding | accumulate>   # differs per run below
```

#### History before the change

| Activity | Occurred | Contribution | Target |
|---|---|---|---|
| `AC-01` | Tue 2026-09-22 07:00 | `CN-01` | `O-SWK-W39` |
| `AC-02` | Thu 2026-09-24 07:00 | `CN-02` | `O-SWK-W39` |
| `AC-03` | Sat 2026-09-26 08:00 | `CN-03` | `O-SWK-W39` |
| `AC-04` | Mon 2026-09-28 07:00 | `CN-04` | `O-SWK-W40` |

#### Action: Wed 2026-09-30 20:00, the user switches to daily stretching

The user picks an effective time of Thursday at midnight, so the daily schedule
starts with a full day. The UI default would have been now. For the unfinished week
`O-SWK-W40`, the user accepts closing it as superseded.

Records created, in one repository change:

```text
task T-STRETCH (as above)

transition R-01
  predecessor: T-STRETCH-WK
  successor: T-STRETCH
  effective_at: 2026-10-01T00:00-05:00
  open_occurrences: close as superseded      # applies to O-SWK-W40
  successor_start: first full period, day 2026-10-01
  recorded_at: 2026-09-30T20:00-05:00
  reason: "Switching to a short daily routine"

occurrence O-SWK-W40                         # stored now, because R-01 decides its fate
  task: T-STRETCH-WK
  period: [2026-09-28T00:00-05:00, 2026-10-05T00:00-05:00)
```

`T-STRETCH-WK` itself is not edited. Its expectation stays readable in the working
tree, so its old occurrences can still be interpreted.

#### Assessments

| Occurrence | Before `R-01` (E 2026-09-30 19:00) | After `R-01` (E 2026-10-05 09:00) |
|---|---|---|
| `O-SWK-W39` | Satisfied, 3 of 3 | Satisfied, 3 of 3. Unchanged; still interpreted by the weekly rule |
| `O-SWK-W40` | Open, 1 of 3 | Closed as superseded, 1 of 3. Not unsatisfied, not prorated, not a failure |
| `(T-STRETCH-WK, 2026-W41)` | Not yet generated | Never generated |
| `T-STRETCH` daily | Does not exist | Generates from day 2026-10-01 |

`AC-04` remains credited only to `O-SWK-W40`. The successor gets no credit for it.

**Alternatives that were not chosen:**

- **Keep outstanding.** `O-SWK-W40` would stay actionable under the weekly rule. It
  would need 2 more sessions by Sunday, while the daily task runs alongside it. A
  single stretch counts toward both only if the user creates both contributions.
- **Delay to boundary.** `effective_at` would be Monday 2026-10-05, and `W40` would
  finish under the weekly rule.

#### What the user sees

The history view groups the chain under one heading, without exposing UUIDs:

```text
SELF : FIT : stretch
  daily since Thu 1 Oct       (replaced 3x weekly)
  W40  1/3  replaced mid-week
  W39  3/3  ✓
```

### A2. Daily run: actions common to all three lapse strategies

| # | When recorded | Action | Records created |
|---|---|---|---|
| 1 | Thu 10-01 07:12 | Taps the checkbox after stretching at 07:10 | `O-S-1001` (materialized), `AC-05` occurred 10-01 07:10, `CN-05` → `O-S-1001` |
| 2 | — | Does not stretch on Fri 10-02 | Nothing |
| 3 | Sun 10-04 08:00 | Stretched Sat 10-03 at 21:30, but records it Sunday morning and selects "yesterday" | `O-S-1003`, `AC-06` occurred 10-03 21:30 and recorded 10-04 08:00, `CN-06` → `O-S-1003` |
| 4 | Sun 10-04 08:01 | Explicitly skips Sunday: "rest day" | `O-S-1004`, skip `R-02` → `O-S-1004` |

Sketch of the records from action 3:

```text
activity AC-06
  actors: [A-USER]
  description: "stretch"
  occurred: 2026-10-03T21:30-05:00
  recorded_at: 2026-10-04T08:00-05:00

contribution CN-06
  activity: AC-06
  occurrence: O-S-1003     # explicit target. The UI defaulted to the activity's own day
  credit: 1 session
  recorded_at: 2026-10-04T08:00-05:00
```

**Stored versus derived.** `O-S-1002` is never stored, and no "missed" record exists
for Friday. Friday's outcome below is derived from the task's expectation, the
absence of contributions, and the evaluation time. This is an **inferred** lack of
satisfaction. Sunday has an explicit skip record, `R-02`, which is a **user
decision**. The two are reported differently.

Two evaluation times are shown below:

- E1, Sun 2026-10-04 07:00: before the late recording and the skip.
- E2, Mon 2026-10-05 09:00: after both.

### A3. Run 1: Lapse

| Occurrence | E1 | E2 |
|---|---|---|
| `O-S-1001` | Satisfied, 1 of 1 | Satisfied, 1 of 1 |
| `(T-STRETCH, 2026-10-02)` | Closed unsatisfied | Closed unsatisfied |
| `O-S-1003` | Closed unsatisfied, provisionally | **Satisfied**, 1 of 1. Recomputed after the late recording |
| `O-S-1004` | Open, today | Skipped |
| `(T-STRETCH, 2026-10-05)` | Not yet started | Open, today |

Saturday's verdict changes because the activity happened within Saturday. Recording
it late does not matter (**Agreed**). Under the **Proposed** rule, an activity that
happened on Monday could not be credited to Friday.

What the user sees at E2:

```text
Today, Mon 5 Oct
  [ ] stretch

History   Thu ✓   Fri ·   Sat ✓   Sun skipped
```

Nothing is owed. Friday appears as a plain gap, not an overdue item. The task stays
active.

### A4. Run 2: Accumulate

| Occurrence | E1 | E2 |
|---|---|---|
| `O-S-1001` | Satisfied | Satisfied |
| `(T-STRETCH, 2026-10-02)` | Open and owed, 0 of 1; its period has ended | Open and owed, 0 of 1 |
| `O-S-1003` | Open and owed, 0 of 1 | Satisfied, 1 of 1 |
| `O-S-1004` | Open, today | Skipped. Not owed |
| `(T-STRETCH, 2026-10-05)` | Not yet started | Open, today |

Each owed day remains its own occurrence with its own remaining requirement. There
is no debt counter (**Agreed** preference).

Run-specific action, Mon 10-05 18:30: the user does an extra session and chooses to
apply it to Friday.

```text
occurrence O-S-1002          # materialized now, because a contribution references it
  task: T-STRETCH
  period: [2026-10-02T00:00-05:00, 2026-10-03T00:00-05:00)

activity AC-07
  occurred: 2026-10-05T18:30-05:00     # the true time is kept
  recorded_at: 2026-10-05T18:35-05:00

contribution CN-07
  activity: AC-07
  occurrence: O-S-1002                 # explicit target, not allocated by timestamp
```

After this action, `O-S-1002` is satisfied by late work, and today's occurrence
`(T-STRETCH, 2026-10-05)` is still open. Under Lapse, `CN-07` would be refused under
the **Proposed** rule.

What the user sees at E2, before the make-up session:

```text
Today, Mon 5 Oct
  [ ] stretch
  [ ] stretch   owed from Fri 2 Oct
```

### A5. Run 3: Keep one outstanding

The exact semantics are **Open** (Q2). Both candidates are shown, and neither is
chosen here.

**Candidate K-A: one carried obligation plus the current one.** In this version the
oldest obligation is carried; whether the oldest or the newest is carried is part
of Q2.

| Occurrence | E1 | E2 |
|---|---|---|
| `O-S-1001` | Satisfied | Satisfied |
| `(T-STRETCH, 2026-10-02)` | Outstanding; the carried obligation | Outstanding; the carried obligation |
| `O-S-1003` | Coalesced into Friday | Satisfied. No longer coalesced, after recomputation |
| `O-S-1004` | Open, today | Skipped |
| `(T-STRETCH, 2026-10-05)` | — | Open, today |

Consequences of K-A:

- At most one extra item is ever shown, however many days are missed.
- A late recording can change which day is reported as carried. This is harmless,
  because coalescing is derived, but the label can shift.
- If the newest obligation were carried instead, Saturday would be carried at E1.
  After the late recording, the carried label would move back to Friday.
- One make-up session, as in Run 2, clears the carried obligation.

What the user sees at E2:

```text
Today, Mon 5 Oct
  [ ] stretch
  [ ] stretch   1 carried (from Fri 2 Oct)
```

**Candidate K-B: merge into the next period.** At E1, Friday's unmet requirement
merges into Saturday. Saturday then closes unmet, and the combined requirement
merges into Sunday. The amount Sunday then requires is **undefined**. The candidate
merge rules give different answers:

| Merge rule | Sunday requires at E1 | At E2, after the late Saturday recording |
|---|---|---|
| Maximum of the merged requirements | 1 | Saturday needs 1 and has 1, so it is satisfied and the carried work is gone. The visible behavior matches Lapse, but the history says "merged" rather than "closed" |
| Sum of the merged requirements | 3 | Saturday needs 2 and has 1, so it merges into Sunday. Sunday is **skipped**. Whether a skip also excuses the carried work is **undefined** |

K-B is therefore not recommended as the initial interpretation. See Q2 in the
[data model](data-model.md#open-questions).

### A6. Facts that hold in all runs

- `T-STRETCH` stays active throughout. Individual periods end; the task does not.
- Persisted records are only activities, contributions, the skip, the transition,
  and occurrences that something references. No record exists for an inactive day.
- Saturday's activity keeps both its time of occurrence and its recording time.

---

## B. One training session, two objectives

### Assumptions

- Week `2026-W41` runs from Mon 2026-10-05 00:00 to Mon 2026-10-12 00:00, in
  UTC−05:00.
- `SELF : FIT : gym` (`T-GYM`) is a recurring weekly task with a minimum of 2, a
  preferred target of 3, and lapse strategy Lapse. "Gym 2–3 times weekly" means 2
  is the minimum and 3 is preferred; 3 is not a maximum.
- `SELF : FIT : marathon strength` (`T-MARA-STR`) is a recurring weekly task with a
  minimum of 1 and lapse strategy Lapse. Its occurrence this week is `O-MARA-W41`.
- Marathon activities are not gym visits automatically, and gym visits are not
  marathon training automatically. The user decides which expectations each session
  serves.
- Both occurrences are materialized when they are first referenced.

### Actions and records

| # | When recorded | Action | Records |
|---|---|---|---|
| 1 | Mon 10-05 19:05 | Checks "gym" for an 18:00–19:00 leg-strength circuit. When prompted, also ticks "marathon strength" | `AC-08`, `CN-08` → `O-GYM-W41`, `CN-09` → `O-MARA-W41` |
| 2 | Mon 10-05 19:06 | Taps "gym" again by mistake | No new record. `AC-08` → `O-GYM-W41` already exists, so a second link is refused |
| 2b | Mon 10-05 (merge) | A second device had also created a link while offline: `CN-15`, the same activity and occurrence. It arrives in a sync | `CN-15` is kept, counted once, and flagged as a duplicate for cleanup (**Proposed**) |
| 3 | Wed 10-07 13:10 | Quick capture: "lunchtime swim at the gym", 12:15–13:00, with no link | `AC-10` only |
| 4 | Wed 10-07 20:00 | Links the swim to the gym week, but not to marathon strength | `CN-10` → `O-GYM-W41` |
| 5 | Thu 10-08 07:20 | Records "easy 8 km run", 06:30 | `AC-11`. Not linked to the gym, and not to marathon strength because it is not a strength session |
| 6 | Sat 10-10 10:05 | Checks "gym" for a 09:00 upper-body session | `AC-12`, `CN-12` → `O-GYM-W41` |
| 7 | Sat 10-10 11:00 | Decides Monday's circuit should not count as marathon strength after all | Removal `R-03` of `CN-09` |
| 8 | Sun 10-11 17:00 | Checks "gym" for a 16:00 marathon strength routine and ticks both | `AC-13`, `CN-13` → `O-GYM-W41`, `CN-14` → `O-MARA-W41` |

Sketch of action 1. One activity has two contributions:

```text
activity AC-08
  actors: [A-USER]
  description: "Gym: leg-strength circuit"
  occurred: [2026-10-05T18:00-05:00, 2026-10-05T19:00-05:00)
  recorded_at: 2026-10-05T19:05-05:00

contribution CN-08
  activity: AC-08
  occurrence: O-GYM-W41
  credit: 1 session

contribution CN-09
  activity: AC-08
  occurrence: O-MARA-W41
  credit: 1 session
```

Sketch of action 7. Only the contribution is removed:

```text
contribution removal R-03       # record shape is Open (Q5)
  contribution: CN-09
  recorded_at: 2026-10-10T11:00-05:00
  reason: "Upper-body circuit, not the plan's strength routine"
```

### Derived assessments after each action

Progress counts distinct non-retracted root activities with a live contribution.

| After | `O-GYM-W41` | `O-MARA-W41` |
|---|---|---|
| 1 | 1 of 2. Partial | 1 of 1. Satisfied |
| 2 and 2b | **1 of 2**. `CN-15` duplicates `CN-08`, so it adds nothing | 1 of 1. Satisfied |
| 3 | 1 of 2. `AC-10` is not yet linked | 1 of 1 |
| 4 | 2 of 2. **Minimum met** | 1 of 1 |
| 5 | 2 of 2. The run is not a gym visit | 1 of 1 |
| 6 | 3 of 3. **Preferred met** | 1 of 1 |
| 7 | 3 of 3. Unchanged; `CN-08` and `AC-08` are intact | **0 of 1. Open** |
| 8 | 4 visits. Preferred met; a fourth visit is allowed | 1 of 1. Satisfied |

At E Mon 2026-10-12 00:00, the period closes. `O-GYM-W41` is satisfied with the
preferred target met, at 4 visits. `O-MARA-W41` is satisfied.

**Nested activity.** If the user also logged exercises within the 16:00 session,
such as `AC-14` for squats with `part_of: AC-13`, the gym count would still go up
by one. The **Proposed** rule counts the root activity. General measures are Q6.

### What the user sees

After action 1:

```text
SELF : FIT
  gym                 1/2 this week   (aim 3)
  marathon strength   ✓ this week
```

After action 3, before linking:

```text
Unlinked activity
  Wed 12:15  lunchtime swim at the gym      [count toward…]
```

After action 7:

```text
SELF : FIT
  gym                 3/3 this week ✓ (aim reached)
  marathon strength   0/1 this week
```

The checkbox is the ordinary path. Contribution records appear only in the
activity's detail view, such as "counts toward: gym W41, marathon strength W41".

---

## C. Insurance payment and bookkeeping

### Assumptions

- `T-HOUSE` ("Maintain the house") is an ongoing task with no occurrences, under
  `N-HOME`. `T-FIN` ("Manage finances") is an ongoing task under `N-MONEY`.
- These household tasks are in a household source, separate from the personal
  source used in scenarios A and B. Planning entries stay in the personal workspace,
  `W-PLAN`.
- Children of `T-HOUSE`:
  - `T-GARBAGE` and `T-RECYCLE`: weekly, minimum 1, lapse strategy Lapse.
  - `T-INS-PAY`: pay the home insurance premium.
- Child of `T-FIN`: `T-INS-BOOK`, record the payment in the household's bookkeeping
  ledger.
- Insurance coverage runs in six-month cycles starting on 1 May and 1 November. Both
  insurance tasks generate calendar periods matching these cycles. Both have a
  minimum of 1 and lapse strategy Accumulate, because an unpaid or unrecorded cycle
  stays owed.
- **What counts as completion** (`completion_meaning`):
  - `T-INS-PAY`: "Payment submitted through the insurer's portal and a confirmation
    number received. The bank clearing the payment is not required."
  - `T-INS-BOOK`: "Payment entered and categorized in the household ledger."
- Recording the payment in Momiji is **not** the bookkeeping task. The bookkeeping
  happens in another system and is a separate activity.
- Due dates are entered from the insurer's notice. Relative deadline rules are a
  future option.

### Prior history (already stored)

| Occurrence | Period | Satisfied by |
|---|---|---|
| `O-PAY-C2605` | [2026-05-01, 2026-11-01) | `AC-20`, paid 2026-04-20, via `CN-20` |
| `O-BOOK-C2605` | [2026-05-01, 2026-11-01) | `AC-21`, ledger entry 2026-04-25, via `CN-21` |

### Actions and records

| # | When | Action | Records |
|---|---|---|---|
| 1 | Sun 09-20 10:00 | The renewal notice arrives. The user records the payment due date for the November cycle and links the bookkeeping for that cycle to this payment | `O-PAY-C2611` with `due: 2026-10-15`; `O-BOOK-C2611` with `depends_on: O-PAY-C2611` |
| 2 | Sat 10-10 09:00 | The insurer agrees to accept payment until 29 October. The user moves the deadline two weeks later | Deadline change `R-04` on `O-PAY-C2611` |
| 3 | Tue 10-27 21:05 | Pays at 21:00 and checks the box | `AC-22`, `CN-22` → `O-PAY-C2611` |
| 4 | Mon 11-02 19:10 | Enters the payment in the ledger at 19:00 and checks the box | `AC-23`, `CN-23` → `O-BOOK-C2611` |

Sketches:

```text
occurrence O-PAY-C2611
  task: T-INS-PAY
  period: [2026-11-01T00:00-05:00, 2027-05-01T00:00-05:00)   # coverage cycle
  available_from: 2026-09-20
  due: 2026-10-15                                             # original value

occurrence O-BOOK-C2611
  task: T-INS-BOOK
  period: [2026-11-01T00:00-05:00, 2027-05-01T00:00-05:00)
  depends_on: [O-PAY-C2611]      # this cycle's payment only, not "any payment"

deadline change R-04
  occurrence: O-PAY-C2611         # same occurrence, same task, no successor
  previous_due: 2026-10-15
  new_due: 2026-10-29
  recorded_at: 2026-10-10T09:00-05:00
  explanation: "Insurer confirmed payment accepted until 29 October"

activity AC-22
  description: "Paid home insurance premium, cycle from 1 Nov; confirmation number noted"
  occurred: 2026-10-27T21:00-05:00
  recorded_at: 2026-10-27T21:05-05:00

contribution CN-22
  activity: AC-22
  occurrence: O-PAY-C2611
```

**What does not change in `R-04`:**

- No successor task is created.
- `O-PAY-C2611` keeps its UUID and its coverage period.
- The original due date stays in history.
- The next cycle, `(T-INS-PAY, cycle 2027-05-01)`, is still computed from the
  generation rule. Its period is unchanged, and it has no due date until a notice is
  entered.

### Derived assessments

| Evaluation | `O-PAY-C2611` | `O-BOOK-C2611` |
|---|---|---|
| E 10-01 | Open. Due Thu 15 Oct | Open. Prerequisite `O-PAY-C2611` is unmet |
| E 10-16, without `R-04` | Open and **overdue** since 15 Oct. Still owed, not closed | Open. Prerequisite unmet |
| E 10-16, with `R-04` | Open. Due Thu 29 Oct; **not overdue** | Open. Prerequisite unmet |
| E 10-28 | **Satisfied**, by `AC-22` | Open. Prerequisite **met** |
| E 11-03 | Satisfied | **Satisfied**, by `AC-23` |

`O-PAY-C2605` was satisfied in April, but it never satisfies the prerequisite of
`O-BOOK-C2611`. The dependency names a specific occurrence.

**Blocking versus advisory (Open, Q8).** The recommended behavior is advisory. If
the user recorded the ledger entry before paying, Momiji would record it and show a
warning: "Prerequisite not yet met: insurance payment, cycle from 1 Nov". Under a
blocking policy, `CN-23` would be refused until `CN-22` exists. The real ledger entry
would still exist, but Momiji would not reflect it.

### Parent views

At E Tue 2026-10-20 12:00, in week `2026-W43`:

```text
HOME : Maintain the house                    ongoing
  garbage              this week   [ ]
  recycling            this week   ✓
  insurance payment    cycle from 1 Nov, due Thu 29 Oct (moved from 15 Oct)

MONEY : Manage finances                      ongoing
  insurance bookkeeping   cycle from 1 Nov, waiting on: insurance payment
```

The dates shown under each parent belong to the children. Neither parent has a due
date, and neither inherits the earliest child date.

At E Tue 2026-11-03 12:00, after both insurance items are satisfied:

```text
HOME : Maintain the house                    ongoing
  garbage              this week   [ ]
  recycling            this week   [ ]
  insurance payment    next cycle from 1 May 2027

MONEY : Manage finances                      ongoing
  (nothing actionable)
```

Both parents remain active. Satisfying every currently actionable child does not
complete an ongoing parent. Retiring or rescheduling `T-HOUSE` would not affect its
children unless the user chose an explicit subtree operation.

---

## Additional review checks

| Check | Example | Outcome |
|---|---|---|
| Renaming or moving a category does not change references or assessments | Rename `N-FIT` from `FIT` to `FITNESS`, or move it under a new `HEALTH` node | Only the node record changes. `T-GYM` still references `N-FIT`, `O-GYM-W41` is still at 4 visits, and cards show `SELF : FITNESS : gym` |
| An annual task can switch to a weekly successor immediately | See R1 below | `O-BUDGET-2026` is closed as superseded. No annual failure is created, and nothing is prorated |
| A superseded task can keep an outstanding obligation | See R1, alternative | `O-BUDGET-2026` stays actionable under the annual rule, and no 2027 occurrence is generated |
| An undated one-time task completes; an ongoing parent does not | See R2 below | `T-APPT` completes. `T-HOUSE` stays active (scenario C) |
| Late work satisfies an older occurrence and keeps its actual time | See R3 below | `O-REPORT-2603` is satisfied by an April activity |
| Retracting a shared activity affects every assessment that uses it | Retract `AC-13` from scenario B with retraction `R-05`, which records the reason and time | Gym drops from 4 to 3 visits (still preferred met). Marathon drops from 1 of 1 to 0 of 1 (open) |
| Unlinking one contribution affects only its target | Remove `CN-14` instead of retracting | Marathon drops to 0 of 1. Gym stays at 4 visits. `AC-13` is intact |
| Shared task content is distinct from private planning | Planning entry `PE-01` in `W-PLAN` selects `O-PAY-C2611` for Tue 10-27, first in order, with the note "after dinner" | `T-INS-PAY` and `O-PAY-C2611` are unchanged, and their due date stays 29 Oct. Other household members see no change |
| Mixed model generations appear together without implicit migration | The personal source is stored as a newer generation, and the household source as an older one that, as an illustrative assumption, cannot represent `preferred` | One view shows both. Reading migrates neither source. Household edits are written in the household source's own generation. Setting a preferred target there is refused with an explanation, and is not silently dropped |

**R1. Annual to weekly.** `T-BUDGET-YR` is annual, with a minimum of 1 and lapse
strategy Lapse. On Sun 2026-10-04 at 12:00, the user creates `T-BUDGET-WK`, which is
weekly with a minimum of 1. The transition `R-06` takes effect now, the UI default.
It closes the open `O-BUDGET-2026` as superseded. For the successor's start, the
user chooses the first full week, `2026-W41`, instead of an initial partial week
consisting only of Sunday. The 2025 annual occurrence stays satisfied. The 2026
annual occurrence shows "replaced in October", not "missed", and no fractional
requirement such as 0.75 is invented.
*Alternative:* with keep outstanding, `O-BUDGET-2026` remains owed under the annual
rule. Because of its Lapse strategy, it closes unsatisfied at 2027-01-01 if no
contribution arrives.

**R2. Undated one-time task.** `T-APPT` ("Make a dentist appointment") has no due
date. On Thu 2026-10-08 the user calls and books the appointment, and records it as
`AC-30` with `CN-30` → `O-APPT`. The task is now completed. Attending the
appointment is a separate task, not this one.

**R3. Late monthly report.** `T-REPORT` is monthly with a minimum of 1 and lapse
strategy Accumulate. On 2026-04-03 the user submits the March report and records
`AC-31`, which occurred 2026-04-03T16:00. Its contribution `CN-31` explicitly targets
`O-REPORT-2603`, which satisfies March. April's occurrence stays open. The activity
keeps its April time.
