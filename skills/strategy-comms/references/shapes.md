# Shapes by content type

Fuller skeletons for the shapes summarized in SKILL.md, some with a short example.

Three things to keep in mind while using them:

- **The part names are not headings to copy.** They are here so you can find your way around this file. Use headings in the output only when the piece is long enough that the reader will jump between sections, and never in a short message.
- **Drop any part the draft has nothing for.** The exception is a hole the reader would hit first, which gets a placeholder. Never add a part because the skeleton lists it.
- **None of these is any company's documented format.** `evidence.md` says where each one comes from. Do not present them to the user as how a particular company does it. The examples in this file are invented.

Contents: [Chat message](#chat-message) · [Email](#email) · [Recurring status update](#recurring-status-update) · [Decision memo or proposal](#decision-memo-or-proposal) · [Deck outline](#deck-outline) · [PR description or technical write-up](#pr-description-or-technical-write-up) · [Announcing a decision](#announcing-a-decision) · [Bad news or an adverse number](#bad-news-or-an-adverse-number)

## Chat message

1. The ask or the answer, with enough context that the reader can reply without asking a question back.
2. The one to three facts behind it.
3. The deadline and what drives it.

No headers, no bold labels. Keep the author's register. Detail that does not fit is cut and listed in your notes.

> Before: "hey, quick question about the vendor thing when you have a sec. the Fivetran contract is up friday and the replacement connector isn't done, 9 of 12 sources pass testing and I think the last three need about four more weeks. so we'd want to extend a quarter, which is $18K. can you approve? would need to know by thursday"
>
> After: "can you approve extending the Fivetran contract one quarter ($18K)? the replacement connector passes testing on 9 of 12 sources, and I think the last three need about four more weeks. the contract is up friday, so I need a yes or no by thursday."

Every fact in the After is in the Before, the "I think" survives, and the register is unchanged.

If the draft is only an opener ("hey, quick question about the vendor thing"), there is nothing to rework. Say that it will cost the reader a round trip, and ask the author what they need, from whom and by when.

## Email

- **Subject line:** the ask or the takeaway, not the topic.
- **First two sentences:** the main point, and what is needed from the reader by when.
- **Then:** only what the reader needs in order to act, in plain paragraphs.

A short email has no headings. If nothing is needed, say "no action needed" in the opening so the reader can stop there.

## Recurring status update

- **Subject line:** the takeaway. "Billing cutover two weeks behind; need a go/no-go on the vendor by Friday" beats "Weekly update".
- **Opening:** three sentences at most. Status against plan, what changed since the last update, and what is needed from the reader by when.
- **Decisions needed:** each with the options in comparable terms, and the author's pick if they have one.
- **Risks:** each with what is at stake, what is being done, and what is needed, as far as the draft says.
- **Progress:** outcomes against plan, with numbers. Two or three lines.
- **Next:** what will be true by the next update.

Routine activity (tickets closed, meetings attended, onboarding) is cut or gets one "Also" line. If the reader will see it on a phone, the opening has to work on its own and the rest stays as short plain paragraphs.

A four-box variant (what is going well, what is not, what worries the author, what decisions are needed now) is for a draft already in that form or an author who asks for it by name. Keep the takeaway and the ask in a line above the boxes.

## Decision memo or proposal

1. **Issue, and why now.** What needs deciding and what makes it timely. One or two sentences.
2. **Recommendation.** The decision requested, who decides, and by when.
3. **Options.** Every option the draft contains, including doing nothing when the draft covers it, compared in the same terms (cost, time, risk) so they can be read against each other. A small table works well.
4. **Why this one.** The argument, with measured facts first and the author's read of them after.
5. **Assumptions.** The ones the recommendation depends on, and what would change the answer.
6. **Risks.** Each with its mitigation, and the strongest point against the recommendation.
7. **What this sets in motion.** Cost, people, follow-on decisions, anything that must be renegotiated.
8. **Who was consulted,** and where they disagree.

Parts 4 to 6 are where reasoning lives, so write them in sentences. A list of fragments cannot carry "because". When the draft is silent on parts 5 to 8, leave them out of the memo and raise them in your notes.

## Deck outline

Work out the storyline before touching slides.

- **Main message:** one sentence the audience should leave with, including what is being asked of them.
- **Executive summary:** a few sentences that make the whole argument and end with the ask. If the meeting were cut to two minutes, this slide would be enough.
- **Body:** one message per slide. The title is a full sentence stating the takeaway, short enough to fit on two lines. The slide body is the evidence that proves it, not the title said again.
- **The ask:** its own slide, with the options side by side (what each costs and what each does), the author's recommendation, and the date the decision is needed. An outline is a working document, so mark each of these the draft lacks with a placeholder.
- **Appendix:** reference tables, methodology, dashboards, anything that answers a likely question but does not carry the argument.

Test: read only the titles, in order. They should make the case on their own.

> Before: "Customer Support Metrics"
>
> After: "First-response time doubled to nine hours after the March volume spike, and has not recovered"
>
> (The figures, the March spike and the lack of recovery are all on the slide's own chart.)

Let the number of supporting points follow the content. Three is common and is not a rule.

## PR description or technical write-up

1. **What changes and why,** stated as the outcome for users or the system, not the mechanics. "Search results no longer go stale when the indexer falls behind" before "adds a cache-invalidation hook". Add why now only when timing is the point (an incident, a deadline, a dependency).
2. **What was done.** Brief. The diff shows the rest.
3. **Evidence it works.** Tests run and their results, measurements before and after, screenshots where they help.
4. **Risks and rollback.** What could break, how you would know, how to back it out.
5. **Where reviewers should look,** and what is deliberately left out.

If the repository has a PR template, keep its sections and apply this order inside them. For readers outside the team, expand acronyms and say what the system does in a clause.

## Announcing a decision

1. The decision in one sentence, with its date and what the reader needs to do.
2. Why, including the considerations on the other side when the call was close.
3. What would change it.
4. What it means for the reader.

Context ahead of the decision follows the general rule: at most two sentences, and only when the reader may not agree on the question or is likely to disagree.

If this is a first version that will be revisited, say so and say when.

## Bad news or an adverse number

1. The number or the event, plainly, in the opening, with what is needed from the reader, or that nothing is.
2. What produced it, at the confidence the author actually has.
3. What it means, and what it does not mean, as far as the draft says.
4. What is being done, by whom, by when.

> "Monthly churn rose from 3.2% to 4.1% in Q3. Nothing is needed from you yet. I think most of the rise is timing: the July price change moved annual renewals earlier, so cancellations we would have seen in Q4 landed in Q3. I have not confirmed that. The breakdown by renewal cohort will settle it, and I will send it by Friday. If the rise shows up across all cohorts, the price change is costing us more customers than we planned for."

The author's explanation is marked as a read, the check that would confirm it is named, and the reassuring reading is not stated as fact. Do not soften the first line and do not explain before stating. If the cause is not yet known, say that, and give the next check and its date if the draft has them.
