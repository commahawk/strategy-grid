Subject: Incident summary - Tuesday

Hi Renee,

Wanted to give you a rundown of what happened Tuesday. Around 2:10pm the on-call (Devon) got paged for elevated error rates on the payments API. He started digging into it and at first thought it was related to the deploy that went out at 1:45pm so he rolled that back at 2:25 but errors kept going. Then Aisha joined and noticed the database connection count was maxed out. They restarted the connection pooler at 2:52pm and errors dropped back to normal within a couple minutes. So total impact was about 44 minutes (2:10 to 2:54).

During that window about 18% of payment attempts failed, which was 3,140 failed payments out of roughly 17,400 attempts. Customers saw a generic error and could retry. We don't know yet how many successfully retried vs gave up, finance is pulling that and said they'd have it Thursday. Based on average order value it could be as much as $212K in delayed or lost orders but that's a ceiling since a lot of people probably retried.

We still don't know why the connections maxed out. The leading theory is a slow query introduced in last week's reporting release that holds connections open, Aisha found some evidence of that in the slow query log but it's not conclusive. Another possibility is the pooler config which hasn't been touched since we doubled traffic in the spring. We're going to do a deeper investigation and should have a root cause by early next week.

In the meantime we raised the pool limit from 200 to 300 as a stopgap and added an alert at 80% connection usage so we'd catch it earlier. We haven't done anything about the reporting query yet. Two merchants (Northwind and Castell) emailed their account managers about it. I think we should probably tell the top 20 merchants proactively but wanted to check with you first since comms would need to go through your team.

Thanks,
Jordan
