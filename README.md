# strategy-grid

Judge a set of **options** (things you might do) against a set of **conditions**
(things that are true), one blind subagent per pairing, then read the resulting
grid twice — by rows to rank the options, by columns to score the set as a
strategy.

It works the same on a repo refactor and on a business plan, because the only
ontology is *facts you don't control* and *things you might do*.

There is no server and no API key. The schema discipline and the rubric travel in
the plugin; the inference runs in your own subagents, on your own spend.

The plugin ships a second skill, **strategy-comms**, for the step after the
grid: telling whoever decides. It writes a run up as a recommendation, and it
reworks any draft you already have so a senior reader can act on it. See
[strategy-comms](#strategy-comms-tell-whoever-decides) below.

## Install

```
/plugin marketplace add commahawk/strategy-grid
```
```
/plugin install strategy-grid@strategy-grid
```

Then:

```
/strategy-grid
```

or just describe the problem — *"score these options against these conditions"*,
*"is this a coherent strategy"*, *"check this plan against what we know"*.

## What you provide

Two required fields. That is the whole interface.

```
context       one paragraph, optional — who is deciding and what they're doing

conditions[]  name  ·  what_is_true   (+ pivotal, optional)
options[]     name  ·  we_will        (+ how, optional)
```

A **condition** is something true about the world you do not control. For a repo
question the "world" is the codebase; for a business question it is the market.
A fact about *you* — your team, budget, calendar — is context, not a condition,
and the skill will move it for you.

## What comes back

A strategy score placed between two anchors it did not choose, a ranked option
table, and the structural read of the set.

```
STRATEGY STRENGTH                            0.57    band 0.48 – 0.65

  ├────────────┼──────────────────────────────┼─────────────────────┤
 0.00        0.25                           0.57                  1.00
 nothing   your strongest                                   every condition
 changes   single option                                    fully answered

OPTIONS                                   push      changes        depends on
  1  Parallelize CI first                 +.42   2 of 5 conditions   —
  2  Freeze the schema + compat           +.28   0 of 5              C4
  3  Extract payments to a package        −.05   2 of 5              C2

THE SET
  ANSWERED       4 of 5 conditions have an option pushing back
  CARRIED BY     every option carries something; no passengers
  PULLING APART  two options pull opposite ways on C1 — they are not additive
  CONCENTRATED   2 of 3 options have no case without C3
```

`best single option` is computed from your own grid, and it is the anchor that
matters: **a set that does not clear its own best member is not a strategy.**

## The three relations

Every cell scores the same three things, and each is readable from exactly two
sentences — what the condition says is true, and what the option says it will do.
That constraint is the design rule: a dimension needing outside knowledge (what
things cost, what's standard practice, what you can afford) is one the judge
would be guessing at, so it isn't here.

```
        push  ──────────>          the condition acts on the option   (−1 … +1)
   CONDITION              OPTION
        <──────────  grip          the option acts on the condition   (0 … 3)
        ═══ stake ═══              how much the option rests on it    (0 … 3)
```

**push** is the ranked dimension. **grip** separates riding a favourable
condition from changing it — high push with grip 0 is surfing, a real strategy
but a different one. **stake** is the counterfactual: if this condition stopped
being true, does the option still make sense? It asks the judge to run that
counterfactual rather than asking you to write a counter-argument in advance.

## Why the cells are blind

Each pairing goes to one subagent that sees that pairing and nothing else — no
tools, `memory: local`, one turn. No verdict is ever threaded into another cell's
prompt. Subagent isolation is the default in Claude Code, so this costs nothing
to enforce and it is the whole reason the grid means anything: a judgment that
leaked from the cell next door would just be an average wearing a number.

The trade is that the rubric is re-sent per cell, so keep M and N tight.

## Composition — one motion, used again

The skill's motion is more general than one report: it **materializes a relation
between two lists as a judged matrix**, and every run writes that matrix to
`.strategy-grid/runs/<slug>.json`. Ranking and the strategy score are arithmetic
applied afterward — so complex goals are chains of the same motion with plain
computation between:

| goal | chain |
|---|---|
| refine a strategy | grid → edit what the objections point at → grid → **diff the run files** = what moved |
| wide funnel | coarse pass over many options → full grid on the survivors |
| zoom | grid the strategies → take the winner → grid its first-actions against the same conditions |
| sensitivity | grid once → recompute the ranking with each condition deleted — **no new inference** |
| write it up | grid → **strategy-comms** on the run file: your pick, the case against it, a marked blank for cost — **no new judging** |

Two rules keep chains cheap and honest. Between motions, *compute* — never
re-judge what arithmetic over an existing run file can answer, and inside a
refine loop only cells whose text changed get new judgments. And the rubric
judges one relation — directed fact → action; a chain step needing a different
relation (say, symmetric affinity within one list) needs its own calibrated
rubric, not an improvised one.

## strategy-comms: tell whoever decides

A grid says which options the conditions favour. Someone still has to read
that and decide, and they have not seen the grid. `strategy-comms` writes it
up for them:

```
/strategy-grid:strategy-comms .strategy-grid/runs/<slug>.json
```

or *"write this run up for our COO"*. It leads with your pick, because the
grid ranks and does not choose, and takes the case against that pick from the
run and its traces. Your conditions go in as your facts, unchanged. Scores go
in as a model's judgment, never as findings. Cost gets a marked blank for each
option, because the grid never judged it (limit 3 below).

It also works with no grid at all, on any draft you have already written:
*"make this exec-ready for my VP"*, *"what will my director poke holes in?"*
It moves the main point and the ask to the opening, keeps facts apart from
your read of them, puts the biggest risk in a sentence of its own, and adds no
facts of its own. The sentences that are its synthesis come back listed for
you to confirm, and where the argument needs something only you can supply it
leaves a short bracketed placeholder instead of a guess.

Its test record, known limits, examples and sources are in
[`skills/strategy-comms/README.md`](skills/strategy-comms/README.md).

## What has been measured

Three grids, 320 blind judgments, judged by a small model with a decisive
fraction escalated to a stronger one.

**The aggregation holds.** Holding labels fixed and swapping only the formula
reproduced a recorded prior ranking at Spearman **+0.929**, top-3 Jaccard 1.00.

**The rubric took two passes to calibrate.** The first draft scored +0.214 and
+0.243 against the priors on two grids. The runs found three defects — a ±0.5
evidence gate that had been weakened, `grip` and `stake` shipped without the
near-zero prior `push` has, and `max()` saturating as an aggregator (it returned
the top level for *all ten* options on a 12 × 10 grid, which is true regardless
of judge quality). After fixing all three:

| | reference | before | after |
|---|---|---|---|
| share in the near-zero band | 55% | 33% | **56%** |
| share at \|push\| ≥ 0.5 | 45% | 67% | **44%** |
| bias vs reference | — | +0.196 | **+0.036** |
| Spearman vs the prior | — | +0.214 | **+0.857** |

**Two tiers, because the cheap judge compresses.** On cells a stronger reference
scored `|push| ≥ 0.75`, the small model came in **0.341** lower in magnitude.
Escalating ~14% of cells — extremes, per-option argmins, per-condition argmaxes,
near-ties, low-confidence — recovers most of that. `cell-judge-pro` runs the
byte-identical rubric on the same payload, never a review of the cheap verdict.

**strategy-comms has its own, smaller record.** In its latest round, three
blind judges compared its output with plain Claude's on each of seven inputs.
All three preferred the skill's on six and called the seventh a tie, mostly by
slight margins. The judges scored it higher on inventing nothing and lower on
form. After a review of the hand-off from a run, two rules were tightened and
re-tested on two grid runs, one built to reproduce the failures the review
found; all three judges preferred the skill's on both. The detail, including
how earlier rounds fell short of blind, is in
[its README](skills/strategy-comms/README.md#what-has-been-measured).

## Known limits

Printed in the output, not buried here.

1. **The strategy score is monotone in option count.** `max` cannot fall when an
   option is added, so padding the set never lowers it. Read it with the carriers
   list, the only thing that exposes an option contributing nothing.
2. **Conditions are unweighted unless marked `pivotal`.**
3. **Nothing here is about cost.** The grid says which options the conditions
   favour. It has no basis to say what you can afford, and does not try.
4. **Ranking by push favours riding a condition over fixing one.** An option that
   exists to *remedy* a condition is not powered by it, so it can score 0 on push
   and 2–3 on grip. On a cold repo grid the option that resolved a verified
   problem ranked last, with a `case_for` that said in words that it solved the
   problem. Business conditions are usually trends you ride, so push works;
   codebase conditions are usually problems you fix, so it misses. **When your
   conditions are problems, read `changes` as the primary result.**

## Layout

```
.claude-plugin/plugin.json
.claude-plugin/marketplace.json    the repo is its own marketplace
agents/cell-judge.md               small model · effort low · 1 turn · no tools
agents/cell-judge-pro.md           stronger model — identical rubric body
skills/strategy-grid/SKILL.md      shape → confirm → fan out → verify →
                                   escalate → aggregate → report
skills/strategy-comms/             the write-up skill: SKILL.md, references,
                                   test inputs, examples, its own README
```

The grid is four markdown files. strategy-comms adds about seventy-five, of which
Claude reads three (its SKILL.md and two reference files); the rest are its
README, examples and test inputs. No dependencies, nothing to build, nothing
to run.

## License

MIT.
