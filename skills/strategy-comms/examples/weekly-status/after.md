Subject: Decision needed this week: lakehouse migration on pace for late Feb, not Jan 15 (Data Platform weekly, Nov 9)

Hi Dana,

The lakehouse migration is behind plan: 61 of 140 pipelines are migrated against the 85 our September plan called for by now, and at the current pace I think we'll probably finish in late February, not Jan 15. That is after the Hadoop support contract expires on Jan 31. I need a decision from you this week on how we cover the gap, and my recommendation is [your pick].

The options:

1. Change nothing. We probably finish in late February and renew Hadoop support for a quarter, about $190K on last spring's quote, which might have changed.

2. Bring in two contractors from the vendor's professional services team for 8 weeks, quoted at $160K. The vendor thinks that gets us back to roughly Jan 31, the day support ends, so there is no slack. I'm not sure whether we have budget for this.

3. Descope the 22 low-usage pipelines nobody has queried in 90 days. That would help some, by [time it saves].

I need the answer this week because the contractors need 3 weeks' lead time, and 3 weeks plus their 8 weeks of work is the 11 weeks left before Jan 31.

The legacy Hadoop cluster, the one whose support is expiring, had two production incidents this week (Tuesday and Thursday), both related to NameNode memory pressure. They took about 9 hours of on-call time.

October compute was $412K against a forecast of $380K, so $32K over. My read is that's mostly from running both platforms in parallel for longer than we planned, which lasts until the migration finishes. I've asked finance for the breakdown by workload.

Progress: the marketing attribution tables (14) and the finance GL extracts are migrated. The customer 360 pipelines are harder than we expected because of the way the legacy jobs handle late-arriving records. Next week we continue on those and start the risk reporting pipelines.

Also: the architecture review board approved the streaming ingestion design on Wednesday, and Tomás, the new analytics engineer, started Monday.

Thanks,
Priya
