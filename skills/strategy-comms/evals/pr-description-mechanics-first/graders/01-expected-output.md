---
type: llm
focus: trace
weight: 1
---

The result matches this description: The four template headings are kept. The description now leads with the behavior change (exports no longer duplicate rows when the worker retries after a timeout) and why it matters (14 support tickets since September), ahead of the list of mechanics. What was verified (212 unit tests, the new test, the 2M-row local run with three worker kills) is kept apart from what was not: the integration suite has not been run and test_large_export_memory is skipped. Those two gaps, and the deploy risk that in-flight exports restart from zero, are prominent and not buried in prose. "I think this fixes it for good" stays a judgment. Code identifiers and figures (5,000 rows, 41.2s to 42.5s, about 3%) are unchanged, nothing is invented, and the README typo and dependency bump are demoted or cut with a note. 
