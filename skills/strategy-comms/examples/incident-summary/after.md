Subject: Tuesday payments incident: need your OK to tell the top 20 merchants (root cause not yet known)

Hi Renee,

For about 44 minutes on Tuesday, roughly 18% of payment attempts failed, and we don't know the root cause yet. I think we should probably tell the top 20 merchants proactively; comms would need to go through your team, so can you OK that by [decision needed by]?

Two merchants (Northwind and Castell) emailed their account managers about the incident. In all, 3,140 payments failed out of roughly 17,400 attempts. Customers saw a generic error and could retry. We don't know yet how many retried successfully vs gave up; finance is pulling that and said they'd have it Thursday. Based on average order value it could be as much as $212K in delayed or lost orders, but that's a ceiling, since a lot of people probably retried.

What we know so far: Devon (on-call) was paged around 2:10pm for elevated error rates on the payments API. He first thought it was related to the 1:45pm deploy and rolled that back at 2:25, but errors kept going. Aisha then joined and noticed the database connection count was maxed out. They restarted the connection pooler at 2:52pm and errors dropped back to normal within a couple of minutes.

What we don't know is why the connections maxed out. The leading theory is a slow query introduced in last week's reporting release that holds connections open; Aisha found some evidence of that in the slow query log, but it's not conclusive. Another possibility is the pooler config, which hasn't been touched since we doubled traffic in the spring. We're going to do a deeper investigation and should have a root cause by early next week.

Until we have one, this could happen again. As a stopgap we raised the pool limit from 200 to 300 and added an alert at 80% connection usage so we'd catch it earlier, but we haven't done anything about the reporting query yet.

Thanks,
Jordan
