# Recommendation for Thursday: fix the migration ordering as this quarter's checkout work

**Decision for you at Thursday's planning meeting:** which one piece of checkout work the platform team takes on this quarter. I recommend fixing the migration ordering: make migrations run in a fixed order behind a lock, so that deploys stop failing on the first attempt.

I scored the three options against five things that are true of checkout today, using strategy-grid (run `2026-09-25-checkout-q4-r2`). On the grid's read, the migration fix is the only option that changes two of the five problems (the deploy race and, less firmly, slow rollbacks). The other two options change one each. The strongest point against it is that the grid's other ranking, by how far today's conditions favour each option, puts the test-suite split first and the migration fix second, and that picking the migration fix leaves the 38-minute test suite as it is.

The five conditions are worded as I gave them to the grid. Every score is a model's judgment of one option against one condition, not a measurement.

## The three options, and what the grid judged each to change

- **Fix migration ordering (recommended).** Make migrations run in a fixed order behind a lock: a migration runner that takes a lock and applies migrations in sequence.
- **Split the test suite.** Split the integration tests onto per-branch databases so that the suite runs in parallel: a database per branch, created at the start of each run.
- **Extract a retry library.** Extract retry into one shared library so that every handler backs off the same way: one engineer, replacing the seven copies behind a single interface.

How much the grid judged each option to change each problem (0 is not at all, 3 is the most):

| What is true today | Fix migration ordering | Split the test suite | Extract a retry library |
|---|---|---|---|
| Retry logic is copy-pasted in seven handlers, and three of the copies differ in backoff. | 0 | 0 | 3 |
| The test suite takes 38 minutes, and 31 of those are integration tests that queue on one shared staging database. | 0 | 3 | 0 |
| The pricing module has had no code owner since March, and 9 of its last 12 changes were reverted or hot-fixed. | 0 | 1 | 0 |
| One deploy in six fails on the first attempt because of a migration-ordering race. | 3 | 1 | 0 |
| A failed deploy takes about 50 minutes to roll back, because half-applied migrations are unwound by hand. | 2 | 0 | 0 |
| **Problems changed (scored 2 or 3)** | **2** | **1** | **1** |
| How far the conditions favour it overall (-1 to +1) | 0.0 | +0.175 | -0.185 |
| Effort (not judged by the grid) | [effort: migration fix] | [effort: test-suite split] | [effort: retry library] |

## Why the migration fix

It is the only option the grid judged to act on two problems, and they are the two deploy problems. On the race, the grid's read is "With migrations applied in a fixed order behind a lock, the ordering race cannot occur" (3 of 3, high confidence). On rollbacks it is "Fewer deploys fail part-way, so fewer rollbacks need unwinding; a rollback that does happen is no faster" (2 of 3, medium confidence).

Each alternative acts on one problem. The split has the grid's strongest single pairing: the slow suite favours it (+0.75), because "31 of the 38 minutes are integration tests queuing on one shared database, which is exactly what per-branch databases remove." The retry library is the only option that any condition fights, the slow suite most of all (-0.5), because "A change across seven handlers needs many full runs of a 38-minute suite, so the slow suite makes this option slow and costly to land."

## What cuts against it

**It leaves three problems as they are, and two of those have an option that would change them.** The 38-minute suite stays unless you pick the split, and the retry duplication stays unless you pick the library. The unowned pricing module stays whichever you pick, because no option scores above 1 of 3 on it.

**The grid's other ranking puts it second.** By how far today's conditions favour each option, the split leads (+0.175), the migration fix is neutral (0.0) and the retry library is fought on balance (-0.185). I treat that ranking as secondary because all five conditions here are problems to fix, and it asks whether a condition makes an option easier or more valuable to do, not whether the option fixes it. The grid's reasoning for the zero on the deploy race says as much: "The option exists to remove this condition. The race does not make the fix easier or more valuable to build; it is simply what the fix is for, so it neither powers nor fights the option." The same ranking did credit the other two options for the problem each fixes (+0.75 for the split on the slow suite, +0.5 for the library on the duplication), so the three are not on the same footing there.

The run also scores the three options as a set: 0.245 (band 0.19 to 0.29), against 0.175 for the best single option. It lists the migration fix as adding nothing to that score and slow rollbacks as a condition no option answers. Both listings are built from the same favour scores, and the fix changes its two problems without being favoured by them.

No condition was judged to fight the migration fix. The nearest thing is a point the grid itself called weak: "A slow suite makes the runner slower to verify, a weak negative."

**Its lead is one problem, and that is the less certain one.** The rollback judgment is at medium confidence, its stated basis is "How many rollbacks follow a failed migration", and on the grid's read the fix makes rollbacks rarer but not shorter. Set that judgment aside and all three options change one problem each.

## What the grid did not judge, and what would change the answer

- **Effort.** The grid does not judge cost, so the effort row is not part of its read.
- **Which problem matters most.** No condition was marked as pivotal in the run, so the count of two problems against one treats all five as equal. If the slow suite matters more to you than the two deploy problems, the split is the option the grid judged to change it.
- **Whether the first-attempt failures are all the ordering race.** The grid judged that the fix has no case without that condition ("If deploys did not fail on migration ordering, the option would have no case"), and its verdict gives as its basis "Whether every first-attempt failure traces to migration ordering."
