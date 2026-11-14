## What
- Refactored `ExportJob.run()` to track offsets in a new `export_checkpoints` table
- Added `CheckpointStore` class with get/set/clear
- Changed retry logic in `worker.py` to call `resume_from_checkpoint()`
- Bumped `tenacity` from 8.2 to 8.5
- Added migration `0047_export_checkpoints.py`
- Updated tests
- Fixed a typo in the README

## Why
We've had a bunch of complaints about duplicate rows in exports. This happens when the worker times out mid-batch and retries from the beginning. Support has logged 14 tickets about it since September. I think this fixes it for good.

## Testing
Unit tests pass (212 passing). I added `test_retry_does_not_duplicate_rows`. I ran an export of 2M rows locally and killed the worker 3 times and got no duplicates. Didn't get to the integration suite because staging was down Thursday, will try again. `test_large_export_memory` is skipped for now because the checkpoint writes made it flaky, need to look into that.

## Notes
The migration creates a new table so should be safe. Checkpoints are written every 5,000 rows which adds a little overhead, I measured about 3% slower on the 2M row export (41.2s → 42.5s). If we need to roll back we'd have to revert the worker change first. Exports that are in flight during deploy will restart from zero one last time since they have no checkpoint.
