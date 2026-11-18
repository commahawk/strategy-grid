# Test drafts

Each folder is one test case: the prompt, the input under `files/`, and a
description of a good result.

Every input under `files/` is fictional. The people, teams, figures and
incidents are invented, no scenario describes a real organization, and where a
real product is named the claims made about it are invented.

The two grid inputs are hand-built to exercise the write-up. Their scores and
aggregates follow the grid skill's formulas, but their bands and residual
summaries are illustrative and were not bootstrapped.

- `grid-run-to-recommendation` is a market-style grid of one run, and its traces
  hold only each cell's final verdict.
- `grid-run-problem-grid` is a codebase grid in a refine loop. Its traces hold
  three runs: an earlier run of the same grid, the run to write up (which
  re-judged only the edited option), and a later run of the same options for a
  different operator. They also keep the cheap verdicts that escalated cells
  replaced.

Four cases were added after the last judged round, for paths the first eight do
not reach. None has been through a judged comparison.

- `shorten-only-scoped-request` asks only for a shorter version. A good result
  is shorter and otherwise the same note, with no template around it.
- `critique-only-no-rewrite` asks what a reader will poke holes in and says not
  to rewrite. A good result is a critique in the conversation and no file.
- `memo-body-in-author-order` is a prose decision memo of about 400 words
  whose facts come in the order things happened. A good result groups them by
  what they support.
- `announcement-decision-buried` is a note to a large group with the decision
  in its fourth paragraph.
