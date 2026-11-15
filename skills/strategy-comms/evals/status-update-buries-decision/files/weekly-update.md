Subject: Data Platform weekly update - week of Nov 9

Hi Dana,

Hope you had a good weekend. Here's the weekly update for the data platform team.

This week the team continued working on the lakehouse migration. We finished migrating the marketing attribution tables (14 tables) and the finance GL extracts, and Sam and Lucia spent most of the week on the customer 360 pipelines which have turned out to be more complicated than we expected because of the way the legacy jobs handle late-arriving records. We also had two production incidents on the legacy Hadoop cluster on Tuesday and Thursday, both related to NameNode memory pressure, which took about 9 hours of on-call time between them. We closed 23 Jira tickets and opened 17.

On the cost side we got the October cloud bill and compute spend was $412K against a forecast of $380K, mostly because we're running both platforms in parallel during the migration and the dual-run period has been going longer than we planned. I've asked finance for the breakdown by workload.

We onboarded the new analytics engineer, Tomás, who started Monday. He's shadowing Lucia.

On the migration timeline, as I mentioned the customer 360 pipelines are harder than expected. We've migrated 61 of 140 pipelines so far (we were supposed to be at 85 by now according to the plan we shared in September). At the current pace I think we'll probably finish sometime in late February rather than the Jan 15 date. The issue with that is the Hadoop support contract expires Jan 31 and renewing it for a quarter is about $190K according to the quote we got last spring, although that might have changed. One option would be to bring in two contractors from the vendor's professional services team for 8 weeks which they quoted at $160K and they think that would get us back to roughly Jan 31, but I'm not sure whether we have budget for that and I'd need to know pretty soon since they need 3 weeks lead time. Another option would be to descope the 22 low-usage pipelines that nobody has queried in 90 days, which would help some.

We also presented at the architecture review board on Wednesday and got approval for the streaming ingestion design.

Next week we'll continue on the customer 360 work and start on the risk reporting pipelines. Let me know if you have any questions!

Thanks,
Priya
