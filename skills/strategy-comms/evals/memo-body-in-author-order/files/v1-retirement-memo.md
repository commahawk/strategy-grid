# Reporting API v1: where things stand

Kofi,

Some background first. We built the v1 reporting API in 2019 and it served us well for a long time. When we launched v2 in 2023 the plan was always to move everyone across, but v1 kept running alongside it, at roughly $11K a month in infrastructure. A year ago v1 still served 31% of reporting requests. In January we started the migration in earnest. At that point 212 customers were still calling v1.

Since then we've done a lot of outreach. We sent the first notice in July, ran a webinar in August that 41 people attended, held office hours through September, added an in-app banner in October and sent personal emails to every remaining account in November. Tomasz has handled most of the customer questions and has been great at it, on top of keeping v1 running, which also takes about half of Ana's time.

The result is that 23 customers are still on v1 today, and it now serves about 4% of reporting requests.

Two of the 23 are enterprise accounts, Halvorsen and Brightwater, worth $1.9M in ARR between them. Halvorsen renews in March. Their integration lead told us they need until May to finish, I think because of their own release freeze. Brightwater has not replied to anything since September.

There is also a security angle. v1 depends on an auth library whose vendor support ends on April 30. After that we would have to patch it ourselves. Ana estimates about three weeks of work to take that on, though she hasn't looked at it closely.

We also need to check the contracts. Some enterprise agreements require 90 days' notice before we withdraw an API, and legal hasn't confirmed yet whether that applies to these two. If it does, notice for a March 31 date would have to go out by December 31.

One more thing on reliability while I think of it: 9 of the 14 reporting incidents in the last two quarters were in v1.

So the options as I see them are: retire v1 for everyone on March 31; retire it on March 31 but keep it running until June 30 for Halvorsen and Brightwater only, which means we take on the auth library patching; or push the whole retirement to June 30.

I think the second one is probably right. It gets almost everyone off before the library goes out of support and doesn't put the Halvorsen renewal at risk. Would be good to settle this at Tuesday's staff meeting.

Thanks,
Lena
