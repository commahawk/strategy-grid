# strategy-comms

The step after the grid: telling whoever decides. `strategy-comms` writes a
strategy-grid run up as a recommendation, and it reworks any draft you have
already written (an email, a Slack message, a status update, a memo, a deck
outline, a PR description) so a busy senior reader can act on it.

It ships in the `strategy-grid` plugin. Install that and you have both.

```
/strategy-grid:strategy-comms .strategy-grid/runs/<slug>.json
/strategy-grid:strategy-comms path/to/draft.md
```

or just ask: *"write this run up for our COO"*, *"make this exec-ready for my
VP"*, *"what will my director poke holes in?"*

## From a grid run

A grid says which options the conditions favour. It does not say what to do,
and the person who decides has not seen it. Handed a run file, the skill
drafts the recommendation from the run and its traces and rescores nothing:

- **Your pick leads.** The grid ranks and does not choose. If you have not
  said which option you recommend, it leaves `[your pick]`.
- **Conditions are your facts. Scores are a model's judgment.** Your wording
  and figures go in unchanged. Every score is attributed to the grid and never
  stated as a finding.
- **The case against comes from the run.** The worst pairing for your pick,
  the conditions it has no case without, any pivotal condition another option
  answers better, and whatever the set leaves unanswered. The reasoning is
  taken from that run's own verdicts in `traces.jsonl`, never from another
  run that happens to have the same scores.
- **It says which kind of grid it is.** When the conditions are problems to
  fix, as in a codebase, it leads with which problems each option is judged to
  change and gives the push ranking second, because push under-rates a fix. A
  remedy's zero push or passenger status is not held against it. A negative
  push still is.
- **Cost is a marked blank.** The grid never judges cost, so each option gets
  a cost placeholder and no number is made up.

The opening of one such write-up, from a fictional run:

> **Q2 support funding: recommend fixing the three workflows behind 62% of last quarter's tickets**
>
> On Friday you choose which one of three support initiatives to fund for Q2. I recommend fixing the three workflows that generate most of our tickets (invoice export, SSO setup and bulk import): two engineers for one quarter, [cost]. It came out ahead when I had the three options judged against four facts about our support load, with one exception that matters.
>
> The exception is the Q2 renewals. Eleven enterprise accounts renew in Q2 and seven of them have open escalations older than 30 days. The fixes were judged to do little for those seven and adding support agents was judged the better answer to them, so funding the fixes leaves the escalations to be handled another way: [plan for the seven escalations].

The whole memo and the notes that came with it are in
[`examples/grid-run/`](examples/grid-run/). A second write-up, from a codebase
grid in a refine loop, is in [`examples/problem-grid/`](examples/problem-grid/).

## On any draft

It asks six questions of the draft, in order:

1. **What is the main point?** It goes in the opening, not the fifth paragraph.
2. **What does the reader need to do?** Decide, do something, weigh in, or
   nothing. "Let me know your thoughts" is not an ask.
3. **What is known, what is the author's read, and what is not known yet?**
   Measured facts first, then the read, marked as a read.
4. **Does each fact earn its place?** Outcomes against plan stay. Activity
   and who-did-what go.
5. **What could go wrong, and what cuts against this?** The biggest risk gets
   a sentence of its own, never a sub-bullet.
6. **Is it as plain and short as the content allows?**

Two rules outrank the questions:

- **Add no facts, and show the holes.** The rewrite goes out under your name,
  so every fact, cause, mitigation and recommendation in it comes from your
  draft. Where the argument needs something only you can supply, the skill
  leaves a short bracketed placeholder, so the question your reader would ask
  first gets answered before you send. It does not raise your confidence level
  either: an "I think" stays an "I think".
- **Restraint.** If the draft already works, it says so and changes little or
  nothing. A three-line message stays three lines.

It replies in the conversation with the rewrite, a short list of what to fix
before sending, and a short list of the sentences that are its synthesis and
not your words. It changes your file only if you tell it to.

A fictional Slack message to a director, before:

> hey Marcus — wanted to get your thoughts on something. so we've been using Airflow for orchestration for about 4 years now and honestly it's been getting pretty painful. the scheduler falls over all the time and everyone on the team hates writing DAGs. I've been looking at Dagster and Prefect and a few of us did a spike on Dagster last sprint and it went really well, the asset-based model is just way better for how we think about data and the local dev experience is night and day. Jen got a pipeline that took her 2 days in Airflow running in like 3 hours. Dagster is clearly the industry direction. the cost for Dagster+ would be around $60k/yr for our usage I think, vs we pay nothing for Airflow now except the infra and the time we spend babysitting it which is a lot. we had 11 scheduler-related pages last quarter. I think we should move. it'd probably take a couple quarters to migrate all ~300 DAGs but we could do it incrementally since they can run side by side. the platform team might have opinions since they run the Airflow infra. anyway let me know what you think, would be good to figure out before Q1 planning locks which I think is Dec 5?

After:

> hey Marcus — I think we should move orchestration from Airflow to Dagster, and I'd like a yes or no from you before Q1 planning locks, which I think is Dec 5.
>
> what we've seen: 11 scheduler-related pages last quarter, and in the Dagster spike a few of us did last sprint, Jen got a pipeline that took her 2 days in Airflow running in like 3 hours (one pipeline, so a small sample). my read from the spike is that the asset-based model is way better for how we think about data and the local dev experience is night and day (I've been looking at Prefect too: [why Dagster over Prefect]).
>
> the cost would be around $60k/yr for Dagster+ at our usage, I think, plus probably a couple quarters to migrate all ~300 DAGs, which we could do incrementally since they can run side by side. against that, Airflow costs us nothing now except the infra and the time we spend babysitting it, which is a lot ([hours a week on Airflow upkeep]). still open: the platform team runs the Airflow infra and might have opinions ([where things stand with platform]).

The recommendation and a yes-or-no ask now open the message. "Falls over all the
time" became the 11 pages. "Clearly the industry direction" and "everyone on the
team hates writing DAGs" are gone, because the draft gave the reader nothing to
check behind them. The brackets mark three things this reader would ask and
only the author can answer.

Plain Claude's version of the same message is in
[`examples/slack-message/plain-claude.md`](examples/slack-message/plain-claude.md).
It is better organized and could be sent as it stands. It also adds a decision
date, a delivery date and a five-item promise the author never made. That is
the trade this skill makes, and [`examples/`](examples/) has all eight runs
from the last two test rounds so you can judge it yourself.

## The CLAUDE.md block

[`claude-md-block.md`](claude-md-block.md) is an optional always-on companion:
seven rules to paste into your `~/.claude/CLAUDE.md` that ask Claude for the
same discipline in what it writes to you. It is written for coding sessions,
so a simple question still gets a line or two. The block has not been tested.

## What has been measured

In round 5, three blind judges compared the skill's output with plain Claude's
on each of seven inputs. All three preferred the skill's on six, and all three
called the seventh a tie. Round 6 re-tested the hand-off from a run on two grid
inputs, after a review changed it, and all three judges preferred the skill's
on both.

| Round | Inputs | Judging | Against Claude with no skill |
|---|---|---|---|
| 1 to 4 | four drafts in rounds 1 to 3, two new ones in round 4 | one judge, not fully blind | won 13 of 14 |
| 5 | six drafts and one grid run | three judges, blind | preferred on 6, tied on 1 |
| 6, the hand-off as it is now | two grid runs, one of them new | three judges, blind | preferred on both |

**Rounds 5 and 6 are the ones to trust, and round 5's margins are small.** Each
comparison went to three separate Claude instances, which read the two responses under
neutral labels, with every file path removed and the order varied. The skill's
version was preferred in 18 of the 21 judgments and plain Claude's in none. The
three ties were the note that needed no changes, which both sides returned
untouched. Fourteen of the 18 margins were "slight". The four "clear" ones were
the grid write-up (all three judges) and the deck outline (one of three).

**Round 6 followed a review of this change.** The review found that a
write-up could take a remedy's zero push for a mark against it, and could
quote reasoning from another run that had the same scores. Both rules were
tightened, and a new input was built to reproduce both failures: a codebase
grid in a refine loop, with a later run for a different operator in its
traces. On it, in two separate runs, the skill's memo contained nothing
from the later run, from a superseded verdict or from the option's earlier
wording. Plain Claude's memo brought in the earlier wording. Five of round 6's six
preferences were "clear". The six questions have not changed since round 5, so
the six drafts were not re-run.

**It wins on fidelity and gives up some form.** Across round 5's 21 judgments the
skill averaged 4.4 out of 5 on fidelity against 3.0 for plain Claude, and 3.7
against 4.1 on fit to the medium. No judge listed more invented or hardened
claims for the skill than for plain Claude: fewer in 16 judgments, the same in
5. Plain Claude's versions were usually tidier and closer to sendable, and they
got there by adding recommendations, dates and commitments the author never
wrote.

**The earlier rounds flatter the skill, and a single judge is noisy.** Rounds
1 to 4 used one judge per comparison. Their prompts asked for a style by a name
that only the skill defined, and plain Claude was sometimes marked down for
having no definition of it. They were also not fully blind: each reply quoted
the path its file was saved to, and the folder name gave the side away. Round 5
fixed all three. A first single-judge pass over round 5, before the paths were
stripped, gave plain Claude two of the seven. The three-judge panel gave it
none.

**The skill has seen most of these drafts before.** It was revised between
rounds against four of the six drafts, so those are not fresh tests. The PR
description and the incident summary are the two it was never revised against,
and it was preferred on both. The grid case was added in round 5 and re-run
once, after a fix to how the skill reads a verdict's reasoning. Expect rougher
results on your own drafts.

**Marking holes in place was learned the hard way.** One earlier version kept
nearly every gap in its notes and out of the message. It lost three of four
comparisons to the version before it, because the message the reader saw still
had the draft's holes.

The judges are models, and eight inputs is a small sample. The inputs are in
[`evals/`](evals/) and every one is fictional. The judges' verdicts are not in
this repository, and of plain Claude's outputs only three are, kept in
`examples/` for comparison.

## Known limits

1. **It is better at openings than at bodies.** It reliably moves the main
   point and the ask to the top. Below that it tends to keep the author's
   paragraphs in their original order and cuts less than it should.
2. **Rewrites come back with blanks.** That is the design, and it means a
   rewrite is usually not sendable until you have filled two or three
   placeholders. If you want something to send in the next minute, this is the
   wrong tool.
3. **A write-up from a run is long.** The two examples run to about 1,000 and
   1,200 words, a few grid terms leak through ("band", confidence levels), and
   the set score gets a paragraph even when the reader is choosing one option.
4. **It never sizes the ask to the evidence for you.** If the draft asks for
   more than its evidence supports, the skill says so in its notes and keeps
   the author's ask in the message.
5. **On a grid of problems it still files the push ranking under what cuts
   against the pick.** It goes on to say why that ranking under-rates a fix,
   but a reader who stops at the opening takes the rank as the main objection.

## Where the six questions come from

They started as a study of one large company's communication culture, drawn
from its public recruiting pages and published interviews with current and
former leaders, and were then combined with general methods from consulting
and intelligence analysis and with published critiques of answer-first
writing. Some rules are correctives to that style, not descriptions of it. It
is an interpretation of public sources and nobody's official standard.

[`references/evidence.md`](references/evidence.md) names every source, with a
label for what kind of source it is, its quote where one could be verified,
and what could not be opened.

## Layout

```
SKILL.md                 the two overriding rules, the six questions,
                         and the hand-off from a run file
references/shapes.md     skeletons by content type
references/evidence.md   sources and their limits
claude-md-block.md       the optional always-on block
evals/                   eight fictional test inputs
examples/                the eight runs from the last two test rounds
```
