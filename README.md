# amazon sde 2 interview questions: what the L5 loop actually asks, round by round, and how to prep the coding rounds in 4–8 weeks

If you're searching this, you're probably 3–6 years into your career, you have a loop scheduled, and you've already noticed that nobody agrees on what's actually asked. The OA was fine. It's the four-to-five onsite rounds that feel like a black box.

Here's the useful thing about Amazon SDE 2 interviews: the shape is unusually predictable. SDE 2 is the L5 level, roughly 3–7 years of experience, sitting above new-grad SDE 1 (L4) and below SDE 3 (L6). At L5 you get something L4 candidates don't: a dedicated system design round. And every single round, coding included, opens with Leadership Principles questions.

So the question bank isn't a mystery. What trips people up is the second half of each question — the follow-up, the trade-off, the "what breaks at 10x." Below is what recent candidates report being asked, and where the actual difficulty lives.

## The loop, as candidates describe it

The pattern across 2026 interview write-ups looks like this:

| Stage | What happens | What it's scored on |
| --- | --- | --- |
| Online Assessment | 2 coding problems + Work Style Simulation. Some reports mention 3 hard DSA problems plus a system design MCQ section | DSA correctness, consistency with Leadership Principles |
| Round 1 | Intro + LP (15–20 min), then 1–2 medium DSA problems | Code, complexity analysis, communication |
| Round 2 | High-Level Design (HLD) | Requirements gathering, architecture, trade-offs, failure handling |
| Round 3 | Low-Level Design (LLD) | Clean OOP, extensibility, design patterns |
| Round 4 | Bar Raiser: LP-heavy plus a technical problem | Depth of stories, problem solving under pressure |

Timelines vary by org, but candidates consistently report the full loop scheduled within two to three weeks of clearing the OA, with a decision in days afterward. The whole thing runs 4–8 weeks end to end.

One thing worth knowing before anything else: the Bar Raiser is not labeled. They come from outside the hiring team and hold veto power over your offer. Other interviewers can love you and one Bar Raiser concern can end it. Treat every round as if it could be that round.

## The coding questions that actually show up

Recent write-ups repeat a small set of patterns. This isn't a leak — it's the reality that Amazon's problem set is built on the same categories year after year, and loops reuse variants rather than brand-new problems.

| Pattern | Reported questions | Why Amazon uses it |
| --- | --- | --- |
| Sweep line / difference arrays | Car Pooling (LeetCode 1094) variant; same problem modified to return max passengers at any point | Tests whether you can move past a working `TreeMap` answer to a better one |
| Monotonic stack | Largest rectangle in histogram; Asteroid Collision; "next greater/smaller" variants | Signals you understand amortized complexity, not just the pattern |
| Graph traversal | Rotten Oranges (multi-source BFS); bipartite graph check with 2-coloring; BFS web crawler | Fundamentals round for almost every SDE 2 loop |
| Shortest path | Dijkstra-based OA problem; grid min-initial-power problem | Weights and priority queues under time pressure |
| Topological sort | Reconstruct an array from subsets (e.g. `[[1,2,3],[1,6],[7,6]]` → `[1,2,3,7,6]`); dependency ordering | Kahn's algorithm and cycle detection |
| DP | Grid power DP; tree DP in the SDE 2 Bar Raiser | Recognising the structure before choosing the transition |
| Prefix sums | A prefix/suffix sum OA problem where the twist is extra logic layered on top | Separates "can compute arrays" from "can solve the actual problem" |
| Sorted matrix / binary search | Find a common element in every sorted row; search in a row- and column-sorted matrix | Search-space reasoning and complexity trade-offs |

Two details matter more than the list itself.

First, the follow-up is where the round gets decided. In one reported loop, a candidate solved a Car Pooling variant with a `TreeMap` and was then pushed to rewrite it with a difference array, then asked how the solution changes if the trip data lives in a distributed database. The first answer got them into the follow-up. The follow-up got them the offer.

Second, Amazon's current OA includes a format change. A February 2026 SDE 2 report describes an OA with a merge-intervals variant, a Dijkstra-based graph problem, and a Work Style Simulation weighted toward Ownership and Bias for Action. Another report from the same period describes an AI repository task: navigate a real codebase with an AI assistant, find a business-logic bug causing test failures, and manually write the fix — the AI can help you explore, but not produce the final patch. If that's in your loop, pattern recognition alone won't carry it.

## System design questions for SDE 2 (HLD)

At L5, the HLD round is where the level gap shows. Amazon's questions tend to be services the company actually builds, and interviewers dig into failure modes rather than accepting a diagram.

- **Real-time error log monitoring.** Reported requirements: 1M+ writes per second, sub-second alerting. A candidate used Kafka for ingestion, Flink for windowed aggregation, Elasticsearch for querying — and then spent 15 minutes on backpressure, i.e. what happens when consumers can't keep up with a log spike.
- **Notification fan-out at 50M users.** The trap is deduplication: idempotency keys, TTL choices (one candidate settled on ~7 days for most notification types and ~90 days for payment confirmations), and the difference between at-least-once delivery and exactly-once processing. "Can you guarantee exactly-once delivery?" has one honest answer: delivery no, processing yes, and here's how.
- **Attendance management for hourly employees.** Deliberately under-specified. The interviewer expects you to extract requirements, state assumptions, then design APIs, data model, event flow, consistency and observability.
- **An auction platform.** Database schema, scaling, low-latency APIs, near-real-time leaderboards (Redis sorted sets came up), and not treating the cache as the source of truth for winning bids.
- **Amazon-flavored classics.** Rate limiters (token bucket with Redis Lua for atomicity, in one Bar Raiser round), URL shorteners, product search with inverted indexes and ranking, Dynamo-style key-value stores, order management with exactly-once processing, and a Vending Machine Leasing Company — which showed up in multiple independent reports.

What distinguishes a passing SDE 2 answer: you name what you gave up. Strong candidates say which shard key prevents hot partitions, what the cache eviction policy is and why, and what happens if a region goes down during Prime Day. Mentioning Redis isn't the answer; explaining cache stampede handling is.

## Low-level design: the rounds people under-prepare

LLD at Amazon is not UML diagramming. Recent examples include:

- **Vending machine leasing** — classes for `VendingMachine`, `LeaseAgreement`, `PaymentStrategy`, with a State pattern for idle/out-of-stock/maintenance conditions.
- **A terminal-based file search utility** supporting filters by name, size, extension, combined with logical AND/OR — an extensibility problem that maps neatly to Specification/Composite patterns.
- **A bank application** with functional and non-functional requirements, where concurrency handling in the core classes drew specific positive feedback.
- Commonly cited classics: parking lots with multi-level pricing, loggers with multiple sinks and async drain, Splitwise, and lottery systems where prize probability scales with ticket price.

The scored signal is whether your abstraction survives a future change. If adding an `ElectricVehicle` subclass forces you to rewrite the parking lot logic, you've missed the point. Over-engineering is the other failure mode — one candidate reportedly got dinged for introducing Kafka and microservices into a class-design question.

## Leadership Principles: the half of the interview that fails strong engineers

Every round has LP questions. Amazon publishes 16 Leadership Principles, and the Bar Raiser will go several follow-ups deep on whichever ones they pick. The STAR framework is a scaffold, not a rubric — scoring happens in the Action and Result.

Recent reports include prompts like:

- "Tell me about a time you compromised quality to deliver quickly."
- "Tell me about a time you took an unpopular stance and your team disagreed."
- "Tell me about a time you made a decision with incomplete information."
- "Tell me about a time you had to influence a decision without direct authority."

The difference between a strong answer and a weak one is usually specificity. "We cut testing to hit the date" fails. "I deprioritised two edge cases based on user data showing under 0.5% impact, then set up alerts to detect regressions on those paths post-launch" passes, because it names the trade-off and the safety net.

> If you take one prep instruction from all of this: build six to eight real engineering stories, tag each to two to four LPs, and make sure every story contains a metric, a trade-off you accepted, a mistake, a risk, and the mechanism you added afterward so it wouldn't repeat.

Amazon rarely accepts a story without numbers, and Bar Raisers are trained to spot rehearsed ones.

## Where structured pattern prep fits — and where it doesn't

Most SDE 2 candidates don't fail the coding round because they can't write the solution. They fail it because they burned fifteen of forty-five minutes trying approaches that didn't fit. That's a pattern-selection problem, and it's the specific gap AlgoMonster is built around.

AlgoMonster is a text-and-diagram interview prep platform put together by ex-Google engineers and competitive programmers. Instead of a 3,000-problem bank, it organises material around recurring patterns with decision flowcharts for picking the right one, plus a Speedrun mode for timed review and an AI assistant that debugs submitted code and tracks the patterns you keep getting wrong. Its subscription page currently advertises 500+ lessons across six courses with 1,000+ illustrations and 3,000+ editorials, and it includes company-specific ladders — Amazon among them. That matters here, because the Amazon loop rewards exactly the thing the platform drills: recognise the category in two to three minutes, then implement.

It also shipped a free AI-coding-interview hub with a per-company guide covering Amazon specifically, plus a published scoring rubric — roughly 35–40% problem solving and algorithm selection, 25–30% code understanding and quality, 15–20% verification and debugging, 15–20% communication. Useful context if your loop now includes an AI-assisted round.

What AlgoMonster will not do for you: simulate the pressure of someone senior interrupting your design mid-answer. There's no video content, and the system design material is fundamentals-level — not enough on its own for an SDE 2 HLD round where backpressure and deduplication TTLs are the actual questions. Candidates targeting L5 generally pair it with dedicated system design material and mock interviews with working engineers. The pattern curriculum is designed for a 4–8 week window, so it lines up with a loop you've already scheduled.

👉 [See the current patterns, Speedrun mode and Amazon-specific ladders](https://algo.monster?fpr=li69)

## Plans and pricing

AlgoMonster has no monthly plan. The subscribe page currently shows a free tier and two paid options, with promotional discounts already applied:

| Plan | What you get | Price | Billing | Get it |
| --- | --- | --- | --- | --- |
| Free | Limited access to a subset of patterns and problems — enough to judge the teaching style | $0 | — | [Start on the free tier](https://algo.monster?fpr=li69) |
| Pro — Yearly | Full pattern library, lessons and problems, unlimited AI assistant and Speedrun, company-specific questions updated monthly, bonus Foundation and Build-an-LLM courses | $99/year ($8.25/mo, from $198) | Billed annually, cancel or upgrade anytime | [Check the Pro Yearly price](https://algo.monster?fpr=li69) |
| Pro — Lifetime | Everything in Yearly, plus lifetime content updates | $189 one-time (from $378) | Billed once | [See the Pro Lifetime deal](https://algo.monster?fpr=li69) |

Prices shift with active promotions — one AlgoMonster affiliate landing page currently displays Pro-Yearly at $69.99/year and Pro-Lifetime at $89, and third-party reviews have quoted everything from $99 to $459 over the past year. The number at checkout is the only one that counts, so check it before you decide.

Which one makes sense depends on your timeline. If your loop is six weeks out, the yearly plan covers everything you need, and there's no reason to pay for permanence. If you're planning to interview again in two or three years — and most engineers do — lifetime costs roughly the price of two annual renewals.

## A realistic six-week prep plan for the L5 loop

1. **Weeks 1–2: patterns first.** Work through the highest-frequency categories — two pointers, sliding window, prefix sums, binary search, BFS/DFS, graphs, heap. One or two problems a day, with the flowcharts. You're training recognition, not volume.
2. **Weeks 2–4: add the Amazon-specific set.** Sweep line, monotonic stack, topological sort, and DP on trees and grids. These are the variants that keep appearing in SDE 2 loops, and they're where separate topic practice falls apart.
3. **Weeks 3–5: system design, in parallel.** Two or three sessions a week. Cover rate limiter, URL shortener, notification fan-out, key-value store, and one warehouse/logistics-style problem. Force yourself to state the trade-off out loud every single time.
4. **Weeks 4–6: mocks and LP drilling.** Two mock interviews a week with someone who will interrupt you, and a written story bank of six to eight stories with metrics attached. Rehearse the tagging, not the script.
5. **Final week: Speedrun and review.** Timed re-solves of the patterns you flagged as weak, plus one polished walkthrough of your flagship project — the "what did you give up and how did you measure it" version.

## FAQ

**Do SDE 2 candidates get an online assessment?** Yes. Reports describe two coding problems plus a Work Style Simulation, and more recently an AI-assisted repository debugging task. Some candidates report a system design MCQ section as well.

**Is system design required for SDE 2?** Yes. L5 is the level where a dedicated HLD round enters the loop; L4 new-grad loops typically skip it.

**How many rounds is the SDE 2 loop?** Usually the OA plus four onsite rounds: one or two coding, one HLD, one LLD, and a Bar Raiser. Bar Raiser rounds often combine LP grilling with a technical problem.

**How long should I prepare?** Reports of successful candidates cluster around four to eight weeks, with one to two hours a day. Rushing the pattern work in two weeks usually means you recognise the pattern but can't defend it under follow-ups.

**Can I pass without a paid prep platform?** Plenty of candidates do — free sheets, GitHub repos, and partner mock interviews show up in successful write-ups. What you can't skip is the repetition and the mock pressure. If you're going the free route, budget more weeks.

The honest summary: the questions are knowable, the loop structure is stable, and the hardest part isn't the algorithm — it's the second question. Build pattern speed, then build the habit of explaining what your solution costs and what it gives up. That combination is what L5 is being hired for.

👉 [Start prepping the pattern library and Amazon-specific ladders here](https://algo.monster?fpr=li69)
