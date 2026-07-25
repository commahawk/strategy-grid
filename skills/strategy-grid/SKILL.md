---
name: strategy-grid
description: Judge a set of candidate decisions against a set of named conditions as an independent grid, then rank them and score the set as a whole. Use when a caller has several options to weigh against facts it can name — a repo refactor against what's true of the codebase, a business plan against what's true of the market — and wants each pairing judged with a written case, not a single vibe. Judging runs one blind subagent per cell on the caller's own spend; nothing is shared between cells. Trigger on "score these options against these conditions", "which of these decisions fits our situation", "check this plan against our diagnosis", "is this a coherent strategy", or an explicit /strategy-grid.
---

# Judge grid

Score N options against M conditions as an M×N grid, one blind judgment per
cell, then read the grid twice: **by rows** to rank the options, **by columns**
to score the set as a strategy. The independence of the cells is the whole
point — a verdict that leaks between cells is worthless — so this skill spends
its discipline keeping them apart.

Two vocabulary words, and they are the only ones the caller needs:

- a **condition** is something true about the world the operator does not control
- an **option** is something the operator might do

For a repo question, "the world" is the codebase and its constraints. For a
business question it is the market. The kernel does not care which.

## 1. Shape the inputs

The interface is deliberately two required fields. Refuse to proceed until each
item meets the shape:

```
context       one paragraph, optional — who is deciding and what they're doing.
              Framing for the judge; it is not an input to any score.

conditions[]  name          short handle
              what_is_true  states what IS, never what to do        (required)
              pivotal       true for the one or two that decide it  (optional)

options[]     name          short handle
              we_will       a directive sentence: "We will X so that Y"  (required)
              how           the mechanism, if known                 (optional)
```

Two shaping rules, both of which change results:

**A condition states what is true, not what to do.** A condition that smuggles
in a remedy ("we should extract payments") pre-decides the grid. Rewrite it as
the fact underneath ("payments logic is duplicated in three places").

**A fact about the operator is context, not a condition.** "We have no SRE",
"two contractors join in March", "our budget is $40k" — these describe the
operator, not the world. Put them in `context`. If you'd phrase it *we have* or
*we don't have*, it is context. This one is easy to get wrong and it corrupts
the grid when you do: capacity facts masquerading as conditions let the same
information score twice.

If the caller supplied prose or a file, extract these fields and echo them back.

## 2. Confirm before spending

Every cell is a subagent turn billed to the caller. Before fanning out, show the
shaped grid and the bill:

```
5 conditions × 4 options = 20 cells
  ~20 small-model turns, plus a decisive few escalated
  est. ~2 min

conditions  C1 …  C2 …  C3 …  C4 …  C5 …        (C3 marked pivotal)
options     O1 …  O2 …  O3 …  O4 …
dropped     "C6 states a remedy, not a fact — rewrite as what IS"
            "C7 is about your team, not the world — moved to context"

Proceed?
```

One confirmation, not one per cell. This is also the natural moment for the
caller to prune the grid, which is the cheapest optimization available.

## 3. Fan out one blind cell-judge per cell

For each `(condition, option)` pairing, invoke the **cell-judge** subagent
exactly once. Its prompt is ONLY that pairing's payload:

```
{ "context":   "...",
  "condition": { ...the one condition, full... },
  "option":    { ...the one option, full... } }
```

Independence rules — these are not conventions, they are the product:

- **One pairing per subagent.** Never ask a cell-judge to score several;
  batching lets one judgment anchor the next.
- **Never thread verdicts.** No other cell's score, ranking, or reasoning enters
  a cell-judge's prompt. Each cell is judged as if it were the only one.
- **No shared scratch.** Cells write nothing the others read. The cell-judge has
  no tools and `memory: local` for exactly this reason.
- Spawn in parallel wherever the host allows — they cannot affect each other.

Optional cheap pre-pass: if a pairing is obviously unrelated, you may screen it
to `push 0.0 / grip 0 / stake 0` at low confidence without spawning a judge.
Screen conservatively — only skip pairings no reasonable reading would score.
Measured caveat: on the one grid where this was checked against a full judgment,
8 of 14 screened cells would have scored |push| ≥ 0.5. Screen sparingly, or not
at all when the grid is small enough to judge whole.

## 4. Coverage check

The fan-out is model-driven, so verify it. List every expected
`(condition, option)` pairing and confirm each has a parseable verdict. Re-spawn
anything missing or malformed. **Do not aggregate a grid with holes** — a
missing cell silently changes both the ranking and the strategy score.

## 5. Escalate the decisive cells

The cheap judge is well calibrated across the bulk of a grid but compresses
strong scores toward the middle — measured at −0.34 mean magnitude on cells
where the reference judge scored |push| ≥ 0.75. That is exactly where the result
is most sensitive. Re-judge a small decisive subset with **cell-judge-pro**
(identical rubric, stronger model, same one-pairing-per-call isolation) on the
*same* payload, and let its verdict replace the cheap one for those cells.

Escalate a cell if ANY of:

- **Extreme** — cheap `|push| ≥ 0.75`. The band the cheap judge gets wrong most.
- **Argmin per option** — the single lowest-push cell for each option. The
  `min()` term rides entirely on it.
- **Argmax per condition** — the cell that is a condition's best answer. The
  strategy score rides entirely on these.
- **Near-tie separator** — when two options' provisional rolls sit within ~0.05,
  escalate the cells that differ between them.
- **Low confidence** — any cell the cheap judge returned as `confidence: low`.

Keep escalation blind: `cell-judge-pro` sees only its one payload, never the
cheap verdict and never another cell. Record which tier produced each final
verdict. If nearly every cell escalates, the grid is genuinely contested — say
so rather than hiding it behind an average.

## 6. Aggregate

Read the grid twice.

### By rows — each option

```
push_j     = 0.7 · mean_i(push[i][j])  +  0.3 · min_i(push[i][j])
changes_j  = #{ i : grip[i][j]  ≥ 2 }   how many conditions it actually changes
depends_j  = #{ i : stake[i][j] = 3 }   how many single points of failure it has
```

The `min` term lets one badly-fought condition sink an option. Do not drop it —
it is the difference between "on average fine" and "has a fatal objection."
`changes` and `depends` are reported, never folded into the rank.

**Do not use `max` for grip or stake.** It was the obvious choice and it is
wrong: over a dozen conditions, `max` saturates. Measured on a 12×10 grid,
`max(stake)` returned 3 for **all ten** options — a column with one distinct
value across the whole strategy. Counts survive because they scale with M
instead of collapsing to it. This holds regardless of judge quality: even a
perfectly calibrated judge, asked twelve times whether an option depends on
something, will say yes at least once.

### By columns — the set as a strategy

```
r_i = max_j(push[i][j])               the best answer the set has to condition i
S   = 0.7 · mean_i(r_i)  +  0.3 · min_i(r_i)
```

Same shape, transposed. The `min` term is "failure to face the challenge" made
arithmetic: one condition nothing answers drags the whole strategy.

**Pivotal conditions**, when the caller marked any:

```
mean_i  → weighted mean, pivotal conditions at weight 2
min_i   → 0.5 · min(all conditions)  +  0.5 · min(pivotal conditions)
```

If nothing is marked, everything is equal and the report says so.

### Anchors — S is never reported alone

```
floor        0.00   do nothing: no option, every r_i = 0
best single  max_j(push_j)   the strongest single option's own roll-up
ceiling      1.00   every condition answered at +1.00
```

`best single` is computed from the caller's own grid, and it is the anchor that
matters: **a set that does not clear its own best member is not a strategy.**

### Band — measured, never asserted

Take the escalated cells, where both a cheap and a strong verdict exist, and
collect the residuals `push_pro − push_cheap`. Bootstrap: 200 resamples in which
every un-escalated cell's push is perturbed by a residual drawn from that set;
recompute S each time; report the 10th–90th percentile.

If no cells escalated there are no residuals — report **"band unavailable"** and
say why. Never substitute a guessed interval.

### Free diagnostics — no extra judgments

- **Answered** — conditions with `r_i ≥ 0.25`. The rest are left standing.
- **Carriers / passengers** — an option is a carrier if it is `argmax_j` for at
  least one condition. An option that is argmax for nothing contributes nothing
  the rest doesn't already do.
- **Pulling apart** — any condition where one option scores `push ≥ +0.5` and
  another scores `push ≤ −0.5`. Those two options are fighting, and no per-option
  ranking would ever show it.
- **Concentrated** — any condition where half or more of the options carry
  `stake = 3`. The set is one bet wearing several hats. Use `= 3`, not `≥ 2`:
  on the 12×10 grid, `≥ 2` flagged 11 of 12 conditions, which is no signal at all.
- **All surf** — if no cell anywhere in the grid reaches `grip ≥ 2`, the set only
  adapts to its conditions and changes none of them.

## 7. Report

```
STRATEGY STRENGTH                            0.57    band 0.48 – 0.65

  ├────────────┼──────────────────────────────┼─────────────────────┤
 0.00        0.25                           0.57                  1.00
 nothing   your strongest                                   every condition
 changes   single option                                    fully answered

  The set scores 2.3× its best single option — the options are doing
  different work, not restating each other.

  band: measured from 4 escalated cells (cheap vs strong), bootstrapped.


OPTIONS                                   push      changes        depends on
  1  Parallelize CI first                 +.42   2 of 5 conditions   —
  2  Freeze the schema + compat           +.28   0 of 5              C4
  3  Extract payments to a package        −.05   2 of 5              C2
  4  Rewrite the client                   −.31   1 of 5              —

  push       do the conditions push it forward or fight it?
             worst single condition counts double — one bad condition can sink an option
  changes    how many conditions this option actually changes, rather than
             adapting to or benefiting from
  depends on the conditions this option has no remaining case without


THE SET
  ANSWERED       4 of 5 conditions have an option pushing back
                 ▸ C5 "…" is left standing — best any option does is +0.25
  CARRIED BY     every option carries something; no passengers
                 Parallelize CI → C1, C4, C5   Extract → C2   Freeze → C3
  PULLING APART  Parallelize CI (+1.00) and Extract payments (−0.75) pull
                 opposite ways on C1. They are not additive.
  CONCENTRATED   2 of 3 options are premised on C3. If it slips, two-thirds
                 of this loses its reason to exist.
```

Quote the `case_against` for each option's worst pairing on request, or inline
when the grid is small. The objection is usually more useful than the rank.

## 8. Print the limits

These are not footnotes; put them in the output.

1. **S is monotone in option count.** `max` cannot fall when an option is added,
   so padding the set never lowers the score. This is why the carriers list is
   mandatory — it is the only thing that exposes an option contributing nothing.
2. **Conditions are unweighted unless marked.** Answering a trivial condition
   scores exactly like answering a pivotal one. Say which conditions were marked,
   or say that none were.
3. **Nothing here is about cost.** The grid says which options the conditions
   favor. It never says what the operator can afford, and it has no basis to.
4. **Ranking by push favours riding a condition over fixing one.** `push` asks
   whether the condition powers the option. An option that exists to *remedy* a
   condition is not powered by it, so it can score 0 on push while scoring 2–3 on
   grip. Measured on a cold repo grid, the option that directly resolved a
   verified problem ranked last and was flagged as the only passenger, with
   `push 0 / grip 2` and a `case_for` that said in words that it solved the
   problem. **When your conditions are problems rather than trends — which is
   typical of a codebase and atypical of a market — read the `changes` column as
   the primary result and treat the push ranking as secondary.** Say which kind
   of grid this is in the report.

## 9. Keep the corpus

Append every returned verdict, with its full `(condition, option)` inputs, tier,
and the run's aggregates, to `.strategy-grid/traces.jsonl` in the working directory —
one JSON object per line. The subagents are ephemeral; this is the only durable
record. It costs nothing and turns repeated use into a labeled corpus the caller
owns.

## Cost

Each cell is one `cell-judge` turn on a small model (`haiku`, `effort: low`,
`maxTurns: 1`), billed to the caller, plus a decisive fraction re-judged by
`cell-judge-pro`. The rubric is re-sent per cell — subagents share no prefix
cache — so keep M and N tight and escalate sparingly. There is no server and no
API key: the discipline ships in this plugin, the inference is the caller's.
