# Examples

Each folder is one run from the last test round (the record is in
[the skill's README](../README.md#what-has-been-measured)), unedited apart
from one thing: in `notes.md` a local file path is shortened to the file name.

- `before.md` is the draft.
- `after.md` is what the skill saved.
- `notes.md` is the reply that came with it: what to fill in before sending,
  and which sentences are the skill's own for the author to confirm.
- `plain-claude.md`, in two folders, is what Claude saved for the same request
  with no skill.

| Folder | The input | Three blind judges |
|---|---|---|
| `grid-run/` | A strategy-grid run to write up for a COO. The run file is in [`../evals/grid-run-to-recommendation/files/`](../evals/grid-run-to-recommendation/files/) | all preferred the skill, clearly |
| `deck-outline/` | A steering-committee outline with topic titles and no stated message | all preferred the skill, one clearly |
| `weekly-status/` | A schedule slip and a decision buried in the fifth paragraph | all preferred the skill, slightly |
| `pr-description/` | Mechanics first, with the untested parts buried in prose | all preferred the skill, slightly |
| `slack-message/` | An ask that never gets made, with opinions stated as facts | all preferred the skill, slightly |
| `incident-summary/` | A minute-by-minute account whose cause is not yet known | all preferred the skill, slightly |
| `already-tight/` | A note that already worked. It came back untouched | all called it a tie |

Bracketed text in an `after.md`, such as `[your pick]`, is a placeholder. It
marks something the reader would ask first and only the author can supply.

The two `plain-claude.md` files show the trade. In both, plain Claude
reorganized the whole message and produced something closer to sendable, and
it did so by adding dates, commitments or firmer wording the author never
wrote. The skill kept to the draft and left blanks.

Every input here is fictional. The products named are real, and the claims
made about them are invented.
