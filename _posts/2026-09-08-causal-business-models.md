---
layout: post
title: "Causal business models, or what to build once the whole business is in one data lake"
date: 2026-09-08 17:47:13 -0500
series: field-notes
period: "2025–2026"
period_sort: "2025-01-07"
employer_label: "a 3,500-person global manufacturer"
tags: [causal-business-models, metric-tree, causal-inference, fp-and-a, planning, natural-experiments, data-products, measurement]
description: "A metric tree that breaks the FP&A plan down into the drivers teams can actually move, kept as a living dataset rather than a dashboard and refreshed from the data lake often enough to compare what projects promised against what the business actually did - and how the natural experiments hiding in the data made the estimates better."
---

Imagine you are handed the annual plan. It has a revenue number in it, a margin number, an operating expense number - the figures that FP&A and the leadership team have agreed the company will deliver. Now imagine you are a supply chain manager, or the sales leader for one region, or the person who runs the loyalty program. Which of those numbers is yours? None of them, exactly. You can't move revenue. You can move the things that move revenue, if somebody would tell you which things those are and by how much.

That gap - between the numbers a company plans in and the levers people actually hold - is the problem I've spent a good share of the last two years on at a 3,500-person global manufacturer. The tool we built for it is something I've taken to calling a causal business model. These are my notes on what it is, why I push back when someone calls it a dashboard, and where the estimates come from.

<!--more-->

## Why a data lake makes this possible at all

Historically, the honest answer to "what moves revenue?" was a blend of experience and a spreadsheet somebody built two reorganizations ago. That was never a failure of judgment. The evidence simply lived in a dozen systems that did not share keys: orders in one place, program participation in another, pricing in a third, operating spend in a fourth, and each of them with its own idea of who a customer was.

The enterprise data lake changed the ground under that problem. Once orders, customers, programs, pricing, inventory and fulfillment, marketing activity, and operating expense sit in one place, with resolved identities and a shared timeline, the relationships between them stop being a matter of opinion. You can actually look. What the lake gave us, more than storage, was the joins - and a causal business model is what you build once you have them.

## The form: a metric tree with two kinds of edges

The model is a tree. At the root sits the number the plan is written in - revenue, say, or gross margin. Each node breaks down into the nodes beneath it, and the breakdown comes in two flavors that I keep very deliberately separate.

The first flavor is arithmetic. Revenue is orders times average order value. Orders are ordering accounts times orders per account. Ordering accounts are the ones you kept plus the ones you won. These are the business equations, and they are true by construction; there is nothing to estimate and nothing to argue about. Most driver trees I've seen stop here, and a tree that stops here is a perfectly nice way of explaining what already happened.

The second flavor is causal, and it starts where the arithmetic runs out. What moves orders per account? Product availability, perhaps. Service levels. Price position against the alternatives. How much of the loyalty program's benefit an account has unlocked. Sales coverage. Training. None of these relate to orders by an equation; they relate by an effect, and an effect has to be estimated. So in the causal layer every edge carries three things: an estimated effect size, an interval around it, and a note on where the estimate came from. An edge with no estimate is allowed to exist. It is a research question the tree makes visible, which is far more useful than a number someone made up to fill the box.

The reason to be strict about the two flavors is that they answer different questions. The arithmetic tells you what happened. The causal layer is the only part of the tree that can tell you what would happen if you did something. Blur the two and you get a dashboard that looks like a model.

## Every node has an owner who can move it

The alignment problem from the opening mostly resolves itself once the tree exists, because every node in the causal layer belongs to a team that can actually push on it. Nobody owns revenue. Somebody does own availability, and somebody owns the loyalty program's tiers, and somebody owns coverage in a region. The tree shows each of them the arithmetic path from their node up to the plan, which turns "hit the number" into "move your driver by this much, and here is how it rolls up." Teams rally around a driver in a way they never rally around a top line, because a driver is something they can do something about - with a project, with a training program, with a technology deployment, with a policy change.

## Not "just dashboards": three tables and a rollup

Here is where I'd push back on the word dashboard, because the artifact is a dataset and the dashboards are windows onto it. Underneath, the model is a handful of governed tables in the lake:

- **The tree itself.** A node table (definition, unit, owner) and an edge table (parent, child, which flavor it is, and for causal edges the effect estimate, its interval, the design that produced it, and an as-of date).
- **Actuals.** A time series per node, computed from the same governed gold products that feed every other report in the company, so the model and the monthly close cannot disagree about what an order is.
- **Plan.** The FP&A plan decomposed onto the tree. The plan says revenue; the tree asks which drivers are assumed to move, and by how much, to get there.
- **Expectations.** One record per project: which node it will move, by how much, over what window, at what confidence, at what cost, owned by whom. A pricing change, a training program, a systems deployment, a new loyalty tier - each lands on a driver with a claim attached.

The rollup is where executives get something a dashboard cannot give them. Sum the expectations on each node, carry them up through the business equations to the root, and you can see - before the year starts - whether the portfolio of projects everyone has committed to actually closes the gap between the plan and the run-rate. Usually it does not, on the first pass. That is a much better conversation to have in January than in October.

And because all of it is built on the lake, it refreshes on the lake's schedule rather than the planning calendar's. Actuals update as the data lands, which means each project's expectation can be checked against what the business actually did - week by week, if the driver moves that fast - instead of waiting for a quarterly review to discover that a promised lift never showed up. The gap between promised and observed becomes a number on the tree, with an owner, while there is still time to do something about it. The tables are versioned with time travel, so "what did we believe in February?" is a query rather than an argument.

## Where the effect sizes come from

The causal edges need estimates, and there are really only three ways to get one. You can ask an expert, which is fast and, in my experience, optimistic. You can run an experiment, which is the gold standard and is almost never available at the scale of a pricing policy or a company-wide program. Or you can go looking for the experiments the business has already run without meaning to.

That third option is the one the data lake unlocks, and it is the part of this work I find most satisfying. Businesses are full of natural experiments: a program with benefit thresholds, a change rolled out to one region before another, a policy that applied to some accounts and not others for reasons that had nothing to do with those accounts.

Take the loyalty program. Its benefits unlock at spend thresholds. An account that just barely crossed a threshold and one that just barely missed it look, on every attribute we can observe, almost identical - which side of the line they landed on is close to random at the margin. So compare what those two groups did next. The jump in order frequency right at the threshold is the causal effect of the benefit on orders, with an interval around it, estimated from the business's own behavior without anyone running an experiment. (Economists call this a regression discontinuity. I call it reading the data the business already paid for.) That effect becomes the estimate on the edge from "program benefit" to "orders per account," and it becomes the prior for any project that proposes lowering a threshold or adding a tier.

The same logic applies to staggered rollouts (compare the region that got the change to the one that had not yet, before and after), to interruptions (a supply disruption is an unplanned experiment in availability), and to dose-response across accounts that received more or less of something. Each estimate is recorded on its edge with the design that produced it, so anyone reading the tree knows how much weight it can bear.

What this did to estimation was change the shape of the argument. A project that claims it will lift orders by some amount now gets compared against the last time that driver moved by a similar amount, and what orders did then. Sometimes the claim survives that comparison. Often it gets smaller, and a smaller number the company believes is worth a great deal more than a larger one it does not.

## What it changed

Planning conversations moved from arguing about the top line to arguing about drivers and evidence, which is a conversation people can actually win. Project intake changed: a proposal names a node, a delta, a window, and a confidence, and a proposal that cannot name a node gets asked what it is for. Post-mortems got a ledger, because expected and observed both live on the tree; you can see, by owner, whose estimates tend to be calibrated and whose tend to be hopeful, and that turns out to be a kinder and more useful thing to know than it sounds. The operating-expense side runs on the same tree - the spend application I've written about elsewhere indexes each cost center's dollars to the outcome nodes those dollars are supposed to move.

## Honest limits

I want to be careful here, because "causal" is a strong word and observational data is a shaky foundation for it. Thresholds can be gamed by accounts that know where they are. Rollouts are rarely random. Effects decay, and they interact with each other in ways a tree does not capture. Our answer to all of that is labeling rather than pretending: every causal edge carries its design and its interval, arithmetic is never confused with an estimate, and an edge we cannot estimate stays empty until we can. I think of the tree as the company's current best map of what causes what - a humbler thing than the truth, kept in a place where it can be argued with and corrected as the data comes in.

## What I'd carry into the next role

I think the main takeaway for me is that a data lake earns its keep through the joins, and a causal business model is what you do with them. At any company with a plan and a lake I would build the tree in the first two quarters: the business equations in the first week, owners on the nodes in the first month, and causal edges as the natural experiments turn up. It changes what a data team is for. Instead of reporting on the business, you are maintaining the company's working theory of itself, and refreshing it often enough that the theory and the business never drift very far apart.
