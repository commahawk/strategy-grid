# Checkout work for this quarter: recommendation

For Thursday's planning meeting, where you pick one of three pieces of work on the checkout service for the platform team.

**I recommend we fix migration ordering:** build a migration runner that takes a lock and applies migrations in a fixed sequence. One deploy in six fails on its first attempt because of a migration-ordering race, and the runner removes that race. It is also the only one of the three options that our strategy-grid run found to change two of our five known problems. The other two options change one each.

The grid's summary score ranks a different option first: splitting the test suite. The two options were not scored on the same basis, which I explain below. The choice between them comes down to whether failed deploys or slow CI costs us more, and the grid does not measure that.

## What the migration fix does

- It removes the deploy race. The grid's verdict, at high confidence, is that with migrations applied in a fixed order behind a lock, the race cannot occur.
- It means fewer manual rollbacks. A failed deploy takes about 50 minutes to roll back, because half-applied migrations are unwound by hand. Fewer deploys would fail part-way. A rollback that does happen would be no faster.

An earlier version of this option was a checklist step in the release runbook. The grid judged that a checklist depends on someone remembering it, so the option is now a runner that enforces the order.

## How the three options scored

The run (2026-09-25-checkout-q4-r2) judged each option against five known problems with the checkout service: duplicated retry logic, a 38-minute test suite, an unowned pricing module, the deploy race, and slow rollbacks. Each pairing of an option with a problem is judged separately, with written reasoning. The figures in this note are the ones recorded in that run.

| Option | Grid score | Problems it changes |
|---|---|---|
| Split the integration tests onto per-branch databases | +0.175 | 1 of 5: slow CI |
| Fix migration ordering (recommended) | 0.0 | 2 of 5: the deploy race, and how often rollbacks are needed |
| Extract retry logic into a shared library | -0.185 | 1 of 5: retry duplication |

The grid score is the tool's measure of how strongly the five problems work for or against doing an option. Positive is in favour.

## Why I am not following the score

For the test-suite split and the retry library, the problem each one exists to fix was counted in its favour: +0.75 for the split against slow CI, and +0.5 for the library against duplicated retry code. For the migration fix, the same relationship was scored 0, on the reasoning that the race "is simply what the fix is for". That verdict records the counter-argument itself: a deploy that fails one time in six "argues for a positive score".

So the 0 comes from one judgment reading the question more strictly than the others did. The same judgment found that the fix removes the race outright. Applying either reading to all three options should narrow the gap between the top two. I have not re-scored the grid, so I am not claiming it would rank the migration fix first.

## The case for splitting the test suite instead

It is the strongest alternative. The suite takes 38 minutes, and 31 of those are integration tests queuing on one shared staging database. Per-branch databases remove the queue. Creating a database on every run takes time that has not been measured, and it eats part of the saving. The split would also surface pricing regressions a little earlier, and it might surface the migration race in CI before a deploy. The grid rated that last point at low confidence and noted that surfacing the race is not fixing it.

If the team loses more hours to waiting on CI than to failed deploys and rollbacks, the split is the better pick. The grid has no deploy counts or CI-run counts, so I cannot settle that with numbers. My judgment is that a failed deploy, which leaves half-applied migrations for someone to unwind by hand, is the worse of the two to live with for another quarter.

The retry library is the weakest of the three this quarter. The slow suite makes a change across seven handlers costly to land, the change touches handlers in the unowned pricing module, and the three copies that back off differently may do so on purpose.

## What stays unfixed if you approve this

- CI stays at 38 minutes, and retry logic stays copy-pasted in seven handlers.
- A rollback still takes about 50 minutes when one is needed. None of the three options makes it faster.
- The pricing module still has no code owner. It has had none since March, and 9 of its last 12 changes were reverted or hot-fixed. None of the three options changes that.

## What I don't know yet

- **Whether every first-attempt failure is the migration race.** The recommendation depends on this more than on anything else. If some failures have another cause, the runner will not remove them, and the grid notes that if deploys were not failing on migration ordering, this option would have no case.
- **How many rollbacks follow a failed migration.** This sets how much rollback time the fix saves.
- **Effort.** The grid has no estimate for any of the three options.
