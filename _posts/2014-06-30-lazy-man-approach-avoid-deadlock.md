---
layout: post
title:  A lazy way to avoid deadlock in database
date:   2014-06-30 12:08:26
categories: "develop"
disqus: true
keywords:
- Database
- Deadlock
- Random
- Probability
excerpt: This article review on an real world experience on
  how to use randomness and probability to avoid deadlock in database.
tags:
- development
---

------
## Problem background

In our project, we have millions (literally) tasks need to be
processed and persisted in the database. Each task is isolated in most cases
but in some rare situation they are related. The related tasks, in
an ideal world, shouldn't really be  processed in parallel, cause we know
that they might cause database contention or even deadlock in some cases.

Having said that, we did not really fancy the idea of identifying the related
tasks, because the number of total task is simply too big and it will incur
huge computational cost to group them. And in relativity, the related tasks
are just a very small fraction of all the tasks been processed.

So in the past we had ignored the potential deadlock and
if it is indeed happen, after processing all the tasks, we will process the
failing ones in a single thread. Job done.

However, in recent days we have seen more deadlocks than we would like to
ignore. So we have decided to look into ways to eliminate the deadlock.

## Possible ways

 One thing for sure, we don't want big changes in the code (nobody want
that, right?). The solution has to be as simple as possible.

 One nature way of thinking is to find a task's related tasks on the flu
when processing it, and then processing them in the same thread. This
require some change to our task scheduling, and also kind of tricky to avoid
a situation that some related tasks might be submitted at the same time,
then we have another sort of deadlock! So we quickly dismissed this
approach.

 Another way is to persist a flag of those related tasks, without
identifying them explicitly how they are related, then we could process all
these kinds of tasks in a single thread. Again this requires change to our
task scheduling code. But in the end of the day, this is a viable and
relatively easy to implement approach.

We could have stopped here, but fortunately one come up with another idea.

## The lazy man's approach

We used about 20 threads in the application. Task specification is retrieved
from a database table without a particular order, then submitted to
a thread pool for processing. So the ultimate goal here
is to avoid two related tasks been processed at the same time in different
threads.

With further investigation, we found that related tasks tend to be
persisted together in the task table. Even though no **order by** clause is
specified when retrieving them, in which case the tasks should be retrieved
**randomly**, in relativity, they might well not. It depends on how database
write and read the data block (we use Oracle). _We suspect that because the
related tasks were written closely to the database, they will be retrieved
closely when using a simple **SELECT**_.

Based on this reasoning, we added a **order by RANDOM** in the SELECT statement
when retrieving tasks. When the number of task is big, the probability of
any two related tasks to be processed at same time is very close to zero.

Well, we didn't really eliminate the deadlock in this way. But if it only
happens once a hundred years, we can certainly live with that :) Best part of
it, we don't really need to change anything!

## Lesson learnt
- Lazy man's approach is to not be lazy in understanding the
problem. Always analysis the deep reason why deadlock happens and then think
how best to avoid it.
- If you cannot (or not willing to ) eliminate fully the deadlocks, probability might be
 your best friend.
