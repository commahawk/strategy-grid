# Test drafts

Each folder is one test case: the prompt, the input under `files/`, and a
description of a good result.

Every input under `files/` is fictional. The people, teams, figures and
incidents are invented, no scenario describes a real organization, and the
products named are real while the claims made about them are invented.

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
