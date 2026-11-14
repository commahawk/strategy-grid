Subject: Decision needed: merchant comms on Tuesday's payments outage

Hi Renee,

I'd like your go-ahead to proactively tell our top 20 merchants about Tuesday's 44-minute payments outage, in which 3,140 payments failed (about 18% of attempts).

Payments are back to normal, but we haven't found the root cause yet; we expect to have it by early next week.

The details are below, most important first.

Merchant comms: Two merchants, Northwind and Castell, have already emailed their account managers about the outage. I think we should tell the top 20 proactively. The comms would need to go through your team, so I'm checking with you before anything goes out.

Customer impact: Between about 2:10 and 2:54pm, 3,140 of roughly 17,400 payment attempts failed. Customers saw a generic error and could retry. At average order value, that is up to $212K in delayed or lost orders. That figure is a ceiling, since many customers probably retried. We don't know yet how many did; finance is pulling the numbers and expects to have them Thursday.

Cause: Database connections hit their limit, and restarting the connection pooler cleared it. We don't know yet why they maxed out. The leading theory is a slow query from last week's reporting release that holds connections open. Aisha found evidence for it in the slow query log, but it isn't conclusive. The other possibility is the pooler configuration, which hasn't been touched since traffic doubled in the spring.

What we've changed: We raised the connection pool limit from 200 to 300 as a stopgap and added an alert at 80% connection usage so we'd catch a repeat earlier. We haven't done anything about the reporting query yet, so until the cause is confirmed we can't rule out a repeat.

How it unfolded: Devon, on call, was paged at about 2:10pm for elevated error rates on the payments API. He suspected the 1:45pm deploy and rolled it back at 2:25, but the errors continued. Aisha joined and noticed the database connection count was maxed out. They restarted the connection pooler at 2:52pm, and errors were back to normal within a couple of minutes.

Thanks,
Jordan
