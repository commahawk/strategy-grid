Not ready to post yet: the opening needs you to say whether merge waits on the integration run, since that is the first thing she will ask.

**Revised** (for a staff eng who skims and already knows the export worker, as a PR description in your What/Why/Testing/Notes template)
pr-description-revised.md

**What changed:** headings and their order are untouched. What now opens with the outcome and the two open testing items, with the change list below. Notes leads with rollback and deploy.

**Before sending**
1. Fill `[whether merge waits on the integration run]` in the opening. It decides what her approval means.
2. Fill `[in this PR or a follow-up]` for `test_large_export_memory`. She may also ask whether the flakiness is the test or a real memory change, which the draft doesn't say.
3. Fill `[reason for the bump]` on `tenacity`. A dependency bump inside a bug fix gets asked about.
4. Likely question: with a checkpoint every 5,000 rows, what happens to rows written after the last one when the worker dies? Your kill test shows no duplicates but not why.
5. Likely question: were the 14 tickets traced to timeout retries? "Fixes it for good" rests on that.

**Your call**
1. The first sentence under What is my synthesis of your Why and change list. Check it describes the PR.
2. "so they can still duplicate rows that once" (Notes, Deploy) is my inference from "restart from zero one last time". Cut it if that restart doesn't duplicate.
3. Cut: "a bunch of complaints" (the 14 tickets carry it) and "adds a little overhead" (the 3% figure carries it).
