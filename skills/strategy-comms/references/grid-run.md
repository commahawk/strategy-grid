# Writing up a strategy-grid run

How to turn a run file (`.strategy-grid/runs/<slug>.json`) and its traces (`.strategy-grid/traces.jsonl`) into a recommendation for whoever decides. SKILL.md has the four rules that hold throughout. This file has the detail, in the order you need it.

Contents: [Read the run file](#read-the-run-file) · [Say which kind of grid it is](#say-which-kind-of-grid-it-is) · [What counts against an option](#what-counts-against-an-option) · [The parts of the write-up](#the-parts-of-the-write-up) · [Find the verdict for a pairing](#find-the-verdict-for-a-pairing) · [Read a verdict](#read-a-verdict) · [Numbers, names and length](#numbers-names-and-length)

## Read the run file

`conditions[]` holds each condition as the author stated it (`what_is_true`) and whether it was marked `pivotal`. `options[]` holds what each option does (`we_will`, `how`). `context` is what every pairing was judged against. `cells[i][j]` is condition i against option j, and each cell holds:

| Field | Range | What it says |
|---|---|---|
| `push` | -1 to +1 | How far the condition favours or fights the option. |
| `grip` | 0 to 3 | How much the option changes the condition. |
| `stake` | 0 to 3 | How much the option rests on the condition. |
| `confidence` | low, medium, high | The judge's confidence in its own verdict on this pairing. |
| `tier` | | Which judge gave the cell's final verdict: the cheap one, or the stronger one that decisive cells are escalated to. |

Under `aggregates`:

| Field | What it says |
|---|---|
| `options[].push` | The option's rolled-up push: 0.7 of the mean of its pushes plus 0.3 of its lowest, so its worst condition weighs more than any other. |
| `options[].changes` | How many conditions the option changes (grip 2 or 3). |
| `options[].depends_on` | The conditions the option has no case without (stake 3). |
| `S`, `best_single`, `band_10_90` | The strength of the set as a whole (`S`), the best single option's rolled-up push, and the band around `S`. |
| `unanswered` | Conditions the set has no answer to: no option's push on them reaches +0.25. |
| `carriers` | Which options are the best answer to each condition. |
| `passengers` | Options that are the best answer to no condition. |
| `pulling_apart` | Conditions on which one option scores +0.5 or more and another -0.5 or less. |
| `concentrated` | Conditions on which half or more of the options have stake 3. |

If `unanswered`, `pulling_apart`, `concentrated` or `passengers` is missing, work it out from `cells` by the definition in the table. An option is the best answer to a condition when it has the highest push on it and that push is above zero, and options that tie for highest share it. If the run has no band, say so and give none.

## Say which kind of grid it is

Push asks whether a condition powers an option, so it can under-rate an option that exists to fix a condition. A remedy can score zero on push, rank last and be listed as a passenger while changing the very condition it targets, and a condition it changes can still be listed as unanswered. So settle the kind of grid first, because it decides which of the grid's results is primary.

- **A grid of problems** (typical of a codebase): the conditions are things to be fixed. Report as the primary result which conditions each option changes (`changes`), and say so when the options tie on it. Give the push ranking as secondary and say why.
- **A grid of trends** (typical of a market): the conditions are things to ride. The push ranking is the primary result.
- **A mix:** say so in the write-up in a clause, give both readings, and apply the exemption below only to the conditions that are problems.

Take the kind of grid from the author or the grid's own report when you have either. Otherwise judge it from how the conditions are worded and check the cells: grip 2 or 3 with a push of zero is the mark of a remedy that push has under-rated. For that option, report what it changes and apply the exemption, whatever you take the rest of the grid to be. Say in the write-up, in a clause, which kind you took it to be.

Say that push has under-rated an option only where it has: that option has grip 2 or 3 with a push of zero. Name it. Where no option does, the only reason the push ranking comes second is that it measures something else, which is whether the conditions favour an option and not whether the option fixes them.

## What counts against an option

For the recommended option (with no pick, the option the grid favours), look up:

- each condition it has no case without (`depends_on`);
- any pivotal condition another option answers better;
- its worst pairing, when that push is below zero. A negative push counts against an option at any grip, because grip says how deeply the option acts on a condition and not whether that helps;
- any `pulling_apart` or `concentrated` entry it appears in;
- any condition the set leaves unanswered that the pick does not change;
- passenger status, where the exemption below does not cover it;
- in a grid of problems, each problem it leaves unchanged, above all a pivotal one or one another option changes;
- in a grid of problems, a lead on the count that rests on one pairing scored 2 or held at low or medium confidence. Say what the count becomes without that pairing, which is arithmetic.

If the pick's lowest push is zero or above, no condition fights it. Say that once, beside the pick's case, and do not present a zero as a mark against it.

**The exemption for a remedy.** Where an option's grip on a condition is 2 or 3 and its push on that condition is zero, push has under-rated it. Do not count against the option that zero, the rank the zero gives it, or that condition's `unanswered` entry. Passenger status is covered only for an option that has such a pairing. Any other passenger was outscored: on each condition that favours it, another option was judged a better answer, so say that. If no condition favours it, say that.

What the exemption covers is not evidence against the option. It never appears as the strongest point against the pick or under what cuts against it. It goes with the secondary readings, with the reason it is secondary.

**The pairings the recommendation turns on** are the pick's pairings at grip 2 or 3 in a grid of problems (its highest pushes in a grid of trends), each condition in its `depends_on`, and each pairing the list above picks out. The reasoning, the confidence and what would change the answer are all taken from these pairings.

## The parts of the write-up

The shape is the decision memo, with these parts in this order. The part names are for finding your way around, not headings to copy, and a part that comes to a sentence or two joins its neighbour.

- **The pick and the ask.** The author's pick, or `[your pick]`. Then what the grid favours and how firmly: by `changes` in a grid of problems, by push in a grid of trends, by both in a mix. Then the strongest point against the pick, in a sentence of its own: a pivotal condition another option answers better if there is one, and otherwise the first item in the order given under what cuts against it.
- **The conditions it rests on,** in the author's wording, with one plain statement that every score is a model's judgment of one condition against one option.
- **What the grid favours and why.** Set the options side by side in the same terms, with a cost placeholder for each. That is the comparison. Give the reasoning for the pairings the recommendation turns on. Where the exemption applies, and throughout a grid of problems, the secondary readings follow in a short passage of their own, each with the reason it is secondary: the push ranking, the set score when you give it, and whatever the exemption covers. If that ranking did credit another option for the condition it changes, say so there, because the ranking then treated the options differently.
- **What cuts against it.** Each item from the list above, given once, with the grid's reasoning where the verdict has some. The point the opening named comes first, now with the grid's reasoning. Then a condition the pick has no case without, then a pivotal condition it leaves unchanged or another option answers better, then the rest. For a problem the pick leaves unchanged, name the problem and the option that changes it, and give no reasoning.
- **What the grid did not judge.** The grid never judges cost, which is why each option carries a cost placeholder. Say when no condition was marked pivotal.
- **What would change the answer.** The `basis` of the pairings the recommendation turns on. What happens if a condition the pick has no case without fails is already under what cuts against it, so do not repeat it here.

In a chat message, give the pick and the ask, what the grid favours and the strongest point against the pick, and put the rest in your notes.

**The set score.** `S` says how well the options work as a set. Give it when the reader is weighing a set or asks for it: with its band, against the best single option, and naming any passenger, because the set score cannot fall when an option is added and a passenger adds nothing to it. It goes with what the grid favours. In a grid of problems it is a secondary reading for the same reason as the push ranking, since it is built from push. When the reader is choosing one option, leave the set score out of the write-up and give it in your notes.

## Find the verdict for a pairing

These steps find the one verdict you may take reasoning from. `traces.jsonl` is append-only and holds every verdict from every run, including the cheap verdict that an escalated cell's final one replaced. Matching on scores alone is not enough, because the judge scores against the run's context and the same scores can rest on different reasoning.

If no line in the file records a run at all, the steps below do not apply. Use a line only if it is the one line in the file that matches the cell on condition text, option text, tier and scores, and say in your notes that the traces do not record which run it came from.

Otherwise, for each pairing:

1. Keep the lines whose `run` is this run's slug.
2. Among them, take the line whose condition and option text match the run file and whose tier and scores match the cell. If more than one remains, take the last.
3. If no line under this run matches, the cell may have been carried forward, because a refine loop keeps unchanged verdicts from an earlier run file. Only in that case look at earlier runs (the file is append-only, so their lines come before this run's). Use an earlier run's line only if its condition and option text, its tier and its scores all match the cell and that run's file gives the same `context`. If several qualify, take the most recent. Say in your notes that the reasoning was written for the earlier run.
4. Never use a line from a later run, a line from a run whose context differs, or a line that records no run, whatever its scores.
5. If no line qualifies, name the condition and the score and leave the reasoning out.

## Read a verdict

A verdict gives a reason for each score. The judge returns the reasons nested under `push`, `grip`, `stake` and `confidence`, and a trace may store them flat, so go by the field name:

| Field | What it holds | Use it for |
|---|---|---|
| `case_for` | Why the push score was given: why the condition powers or fights the option, or why it has no bearing. | The pick's case. The case against an option where its push is negative. |
| `case_against` | The judge's strongest argument against the push score it gave. It weighed this argument and did not adopt it. | The case against an option where its push is positive. |
| `what_changes` | What the option does to the condition. This is the reason for grip. | What an option changes, or a problem it leaves unchanged. |
| `if_it_were_false` | What happens to the option if the condition fails. This is the reason for stake. | A condition the pick has no case without. |
| `basis` | What would change these scores. It comes with the judge's confidence. | What would change the answer. |

- **Where push is zero,** neither field is reliably a case against the option: `case_for` says why the condition has no bearing and `case_against` argues for some other score, higher or lower, so read both and quote one only for what it says.
- **Introduce a quote as what it is.** `case_for` is the grid's reasoning for its score. `case_against` is the grid's caveat on its own score, so do not put "the grid's reasoning is" in front of it.
- **Confidence is the judge's.** Say whose it is where it first appears. When a pairing the recommendation turns on is at low or medium confidence, say so in plain words beside it. A cell with no verdict line may have been screened without a judge, so give no confidence for it.

## Numbers, names and length

The write-up argues in words. The numbers are there so the reader can check the words.

- Give each measure one plain name where it first appears ("how much it changes the problem", "how far the conditions favour it") and use that name after. In the comparison, put the name in a column header or beside the number, so that no legend has to say what a number's position means.
- Refer to a condition by a short name for what it says ("the help-articles condition", "ticket concentration"), in prose and in the comparison, and never by its number alone.
- A score follows the words it supports, in parentheses, once. Per-pairing scores belong in the comparison. In prose give one only where the reader needs the size: two options compared on the same condition, the pick's worst pairing, or the credit the push ranking gave another option for the condition it changes.
- Give rolled-up scores (each option's, `S` and `best_single`) to two decimals, with a half rounding away from zero, so 0.175 becomes 0.18.
- Lean on the pairings the recommendation turns on. Do not walk through the rest.
- A memo for a reader choosing one option fits on a page: no more than 650 words, not counting the list of conditions or the comparison and its legend. Count before you hand back. When it is over, apply these cuts in order until it fits: anything said twice, then the reasoning for options that are not the pick, then the secondary readings down to a sentence each. Never drop the conditions, the pick's case, what cuts against it or, in a grid of problems, the push ranking with the reason it is secondary.
- In your notes, put what comes from the run in the first item under "Your call", in up to four sentences: the kind of grid you took it to be (first, because the primary result depends on it), the set score if you left it out, any verdict taken from an earlier run, and what else you left out. This item may run past two lines. The others may not.
