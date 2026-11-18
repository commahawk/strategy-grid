Subject: Search ranking test results

Hi Ines,

Quick update on the search ranking test we talked about a few weeks ago. As you know the team has been working on the new ranking model since August and it has been a long road, with a lot of back and forth with the data science group about how to measure it, so it's good to finally have something to show.

We ran the A/B test for two weeks, Oct 5 to Oct 19, on 10% of web traffic. The headline is that click-through on the first three results went from 31.4% to 34.9%, which Wei's analysis says is statistically significant. Searches that ended with no click at all went from 22% to 19.5%. We did not see any movement in add-to-cart rate, which stayed at about 6.1% in both groups, so I think it's too early to say this makes us money, although my guess is it will once the long-tail queries improve.

One caveat is that the test ran only on web, and behaviour in the mobile app could be different because of the smaller screen. Also p95 latency went up by about 40ms, which infra says is within budget but worth watching. Big thanks to Wei for turning the analysis around in three days.

I'd like to roll out to 50% of web traffic next and start an app test in parallel. I'd need two weeks of Dana's time from the app team for that, so let me know if that works.

Thanks,
Marta
