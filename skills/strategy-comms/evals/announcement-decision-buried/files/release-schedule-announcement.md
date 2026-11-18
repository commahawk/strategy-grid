Subject: Some thoughts on our release process

Hi all,

As many of you know, we've been looking hard at how we release software over the last few months. It started with the retro after the September outage, where several of you pointed out that changes were sitting for days before going out, and it has involved a lot of conversations since. I want to thank the release engineering team, and Priyanka in particular, for running the pilot and pulling the numbers together.

A few things stood out from that work. Last quarter 38% of our hotfixes were for changes that had waited more than four days between merge and release. When changes wait, they pile up, and big releases are harder to debug than small ones. In November the Search and Billing teams piloted releasing twice a week, and their hotfixes went from 11 the month before to 4. That's two teams and one month, so I don't want to oversell it, but it matches what we expected.

We did look at going straight to daily releases. QA doesn't have the capacity for that right now, so we've parked it and will look again in Q2.

So, starting January 12, we're moving from one release a week to two. Releases will go out on Tuesdays and Thursdays. The code freeze moves from Thursday at 5pm to Monday and Wednesday at 3pm. Each team needs to update its release checklist for the new freeze times by January 8. The Payments team will stay on the weekly schedule until February because of the processor certification.

We'll watch the change failure rate closely. If it goes above 15% for two weeks in a row, we'll go back to weekly releases while we work out why.

Let me know if you have any questions or concerns.

Thanks,
Olu
