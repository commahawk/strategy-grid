# Q2 support funding: recommend fixing the three workflows behind 62% of last quarter's tickets

On Friday you choose which one of three support initiatives to fund for Q2. I recommend fixing the three workflows that generate most of our tickets (invoice export, SSO setup and bulk import): two engineers for one quarter, [cost]. It came out ahead when I had the three options judged against four facts about our support load, with one exception that matters.

The exception is the Q2 renewals. Eleven enterprise accounts renew in Q2 and seven of them have open escalations older than 30 days. The fixes were judged to do little for those seven and adding support agents was judged the better answer to them, so funding the fixes leaves the escalations to be handled another way: [plan for the seven escalations].

## The comparison

The fixes were judged to change two of the four facts below, and each alternative one. The judging is a strategy-grid run (2026-10-02-support-backlog), in which a model scores each option against each fact separately. The facts are the ones I gave it. Every score is the model's judgment, not a measurement.

1. 62% of support tickets last quarter came from three workflows: invoice export, SSO setup and bulk import.
2. Eleven enterprise accounts renew in Q2, and seven of them have open escalations older than 30 days. (The one marked pivotal in the run.)
3. Fewer than 1 in 10 customers who filed a ticket opened a help article in the session before filing.
4. Ticket volume rises about 40% in the six weeks after each quarterly release.

| Option | Facts it changes (of 4) | Overall fit (-1 to +1) | Main mark against it | Cost |
|---|---|---|---|---|
| **Fix the top workflows** (recommended), so that fewer tickets are filed. Two engineers for one quarter on invoice export, SSO setup and bulk import. | 2: ticket concentration (fact 1) and post-release spikes (fact 4) | +0.31 | Barely bears on the escalations in fact 2 (+0.25, against +0.75 for adding agents) | [cost] |
| **Add four support agents**, so that tickets are answered faster. Four hires into the existing support team, trained on enterprise accounts first. | 1: the enterprise escalations (fact 2) | -0.11 | Fixed headcount is a poor fit for the post-release rise in fact 4 (-0.5) | [cost] |
| **In-app help** inside the three workflows, so that customers solve problems without filing a ticket. Inline guidance and error explanations at the points where tickets are filed today. | 1: ticket concentration (fact 1) | -0.23 | Depends on customers reading help at the moment of trouble, which fact 3 cuts against (-0.75) | [cost] |

Overall fit runs from -1 (the facts work against the option) to +1 (they work for it), with each option's worst fact counted more heavily. The grid does not weigh cost, so the cost column sits outside its scores.

## Why the fixes

Both of the grid's measures put the fixes first. I lead with how many facts each option changes, because the four are mostly problems to fix and the fit score can under-rate an option whose job is to fix one. On fit, the fixes are the only option above zero.

The case is strongest on ticket concentration (+1.0, high confidence): the option "targets exactly the three workflows the condition names as the source of 62% of tickets". On the post-release spikes the fixes were judged to help (+0.5, medium confidence) if releases are what break or change these workflows, though the fact as stated does not say the spike comes from these three. The spikes are also where the fixes and adding agents differ most (+0.5 against -0.5).

Read as a set, the three options score 0.53 (band 0.47 to 0.59) against 0.31 for the fixes alone, and the one fact the set answers better than the fixes is the renewals. In-app help adds nothing to that set score (it is the best answer to none of the four facts), which is not by itself a mark against it, because it was judged to change ticket concentration.

## What cuts against it

The renewals are the main point against, because they are the fact marked pivotal and the fixes barely bear on them (+0.25, medium confidence). The grid's reasoning is that the seven escalations "are already open and older than 30 days; fixing workflows this quarter does nothing to close them before the renewals". The most it credits is a little agent time freed late in the quarter. Adding agents was judged the better answer (+0.75, high confidence) because it treats the escalations as a capacity problem and addresses them directly. The grid's own caveat on that score is that new hires take time to become useful on enterprise escalations, "and Q2 may be over before they are".

The recommendation also has no case without the 62% figure, and nor does in-app help, so two of the three options stand or fall with it. The grid's point against is that the figure says where tickets come from, not that the causes can be fixed in a quarter, and "some of that volume may be setup questions no fix removes".

The third fact, on help articles, is left as it is: no option was judged to change it. For the fixes it was judged to have no bearing (0.0, high confidence), which is their lowest score.

## What would change this

On the grid's own account, three checks would move its read: a breakdown by cause of the tickets in the three workflows, whether the seven open escalations sit in those three workflows, and the ticket mix in the six weeks after a release. None of the three is in the run. The second bears directly on the renewals: [where the seven escalations sit].
