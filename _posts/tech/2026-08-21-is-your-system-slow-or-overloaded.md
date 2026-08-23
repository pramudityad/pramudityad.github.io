---
title: "Is Your System Slow, or Overloaded? The First Question Before Scaling"
tags: Tech
date: 2026-08-21 12:58:50 +0700
---

"Production is slow." That sentence has started every bad incident postmortem I've read. It also starts a lot of pointless ones, because "slow" is hiding two very different problems, and the fix for one is useless — sometimes harmful — for the other.

I keep coming back to a distinction from my system-design notes, and it has saved me more time than any monitoring dashboard:

- A **performance problem** means the system is slow even for a *single* user. One request, no load, still 3 seconds. The bottleneck is inside the work itself: a missing index, an N+1 query, a synchronous call that should be async, garbage collection pauses.
- A **scalability problem** means the system is fast for one user but collapses under many. Ten people in, everyone is still happy. A thousand, and latency climbs until timeouts start firing. The bottleneck is in how the work is shared, not the work itself.

The symptoms overlap — users complain about latency either way — but the playbook is different.

## Why the distinction matters before you touch anything

If it's a performance problem and you respond by scaling out, you are paying for more machines to run the same slow code. I've seen exactly this: new pods, same P95, the N+1 query faithfully reproduced across every instance. Horizontal scaling multiplies a performance problem; it does not fix it.

If it's a scalability problem and you respond by profiling the code, you will find nothing. A single request looks fine in isolation because it *is* fine in isolation. The fix lives elsewhere: caching, sharding, queueing, removing a shared lock, making the database the only thing that touches persistent state. Classic patterns from the distributed-systems literature — Twitter running Redis at 105 TB of RAM across thousands of instances, or teams converging on caches precisely because the database can't be the bottleneck.

The cheap diagnostic: load the system with one user and time a request, then with many and watch the P95. If the lonely request is already slow, fix performance first — scaling comes later. If the lonely request is quick but P95 degrades linearly with load, you have a scaling problem and micro-optimizing the code is a distraction.

## The two playbooks, briefly

Performance work means going down the stack. Brendan Gregg's [Linux Performance Analysis in 60 Seconds](https://medium.com/netflix-techblog/linux-performance-analysis-in-60-000-milliseconds-accc10403c55) is still the best starter ritual I know; [latency numbers every programmer should know](http://norvig.com/21-days.html#answers) recalibrate what "slow" actually means. Profile first — intuition about where the time goes is wrong more often than right.

Scalability work means redesigning how load is distributed. Cache the reads. Move writes onto a queue so peaks don't hit the database synchronously. Shard when one node can't hold the data or the traffic. Remove shared state wherever possible, because shared state is where contention lives. Eric Brewer's [Lessons from Giant-Scale Services](https://people.eecs.berkeley.edu/~brewer/papers/GiantScale-IEEE.pdf) covers this mindset well; Jeff Dean's [LADIS keynote](https://www.cs.cornell.edu/projects/ladis2009/talks/dean-keynote-ladis2009.pdf) is the short version of what it looks like in practice at Google scale.

One trap worth naming: premature scaling. Most systems that "need to scale" don't — they have a performance problem wearing a scalability costume, and the answer is an index or a join rewrite, not Kafka.

## What I take from this

The value of the distinction isn't academic. It decides the first question you ask when production is slow: *is one request slow, or are many requests slow?* Answer that before touching capacity, before adding infra, before anyone says the word "microservices." Everything else follows from there.
