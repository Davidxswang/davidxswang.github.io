---
layout: post
title: "How to DO Evals"
date: 2026-03-05
tags: [how]
---

When people talk about evals for AI products, they often jump straight to metrics. I think that misses the point. The first job of an eval is not to produce a number. The first job is to help a developer answer a practical question: does this system actually work for the use case I am trying to serve?

That question matters because manual testing is deceptive. If you build an agentic workflow, try one prompt, then another, then a third, it is very easy to convince yourself that the product works. Usually you are also tuning prompts, tweaking tools, and fixing obvious bugs at the same time, so every example starts to look better. But that still does not tell you whether the system works in scale. It only tells you that you can make a few examples look good.

## Why Evals Exist

I think evals serve **two purposes**.

**First, they help developers test in scale during development.** Once the core loop of the product is stable enough, you should be able to run ten, twenty, or fifty examples against the current code and see what breaks. That is much more useful than manually poking at one or two cases. It helps you catch regressions, find prompt bugs, find tooling failures, and see whether a new design actually improved the product.

**Second, evals help product design.** A good end-to-end eval does not just test a model response in isolation. It simulates the experience a user actually has: the frontend behavior, the backend logic, the state transitions, and the agentic core loop that ties everything together. That makes eval a way to test whether the product is usable, not just whether the model said something plausible.

## The Formula

The cleanest way I know to frame this is:

$$
Q = \mathbb{E}_{x \sim P_{\mathrm{real}}}[\mathrm{score}(f_{\mathrm{prod}}(x))]
$$

This is the true product quality. It is the expected score of your production system over real user inputs.

What eval gives you is only an estimate:

$$
\hat{Q}_n = \frac{1}{n} \sum_i \mathrm{score}(f_{\mathrm{eval}}(x_i))
$$

If you want that estimate to mean anything, **three conditions** have to hold.

**The first condition** is that `n` must be large enough. Testing on one or two examples might just be luck. As you increase the number of cases, the average becomes more trustworthy. In practice this is why a small eval set is already better than intuition alone. Even twenty examples can tell you more than two hand-picked demos.

**The second condition** is that `x` must follow something close to `P_real`. Your test cases need to look like real user behavior. If your eval set comes only from your imagination, then even a perfectly consistent eval can converge to the wrong answer. You are measuring the wrong distribution with high confidence.

**The third condition** is that `f_eval` should be close to `f_prod`. If the environment under evaluation is meaningfully different from production, then you are not measuring the real product. You are measuring a substitute. Sometimes that difference is obvious. Sometimes it is subtle: a missing backend feature, a stale config, a different rendering path, or a tool that behaves slightly differently outside prod. Those gaps can make an eval look precise while actually being misleading.

Most bad evals fail because of one of those three problems: not enough samples, the wrong input distribution, or the wrong environment.

## The Four Pieces

So how should you build an eval system? I think there are **four key pieces**.

**The first piece is a user agent.** In an agentic product, the workflow may be fixed, but the input changes every time. That means you need a way to simulate user behavior. The important rule is negative, not positive: what a real user cannot see, the user agent should not see either. If a real user only interacts through a UI, the user agent should use that same interface. Do not leak hidden state, backend logs, or internal prompts into the user simulation.

**The second piece is the simulation core loop.** This is the engine that runs the interaction between the user agent and the system under test. It should faithfully simulate the product, including frontend state, backend behavior, file uploads, tool results, and whatever other state transitions the real product depends on. If this loop is not faithful, your eval can look sophisticated while still testing the wrong thing.

**The third piece is the evaluator.** After the conversation or workflow finishes, you still need to decide whether the result was good enough. Sometimes that can be a fixed rule. Sometimes it can be a rubric scored by a model. But before asking whether the product passed, you should first ask whether the eval itself is well defined. A bad metric can make a correct product look wrong, or a broken product look acceptable. I do not think every eval needs exact string matching. In many cases, it is better to define the goal clearly and let an evaluator judge whether the session actually achieved it.

**The fourth piece is visualization.** This is the part teams often skip, and then regret later. Developers need to be able to inspect the full eval setup, the simulated conversation, the tool calls, the final verdict, and the internal trace of what happened. If the only thing your eval produces is a red or green cell in a table, then debugging will be slow and painful. A good eval system should help you understand failures, not just count them.

## What Not to Automate Too Early

One more rule matters here: **do manual eval before automated eval**. You cannot automate judgment you do not yet understand. If you have never looked at bad outputs yourself, you will not know how to write a useful rubric. If you have never walked through the real user experience, you will not know what the important failure modes are. Manual review is how you build taste. Automation should encode that taste, not replace it.

Not every evaluation problem should use an LLM either. If a failure can be detected deterministically, use the deterministic check. If something can be measured with a simple rule and near-perfect accuracy, that is almost always better than asking a model to judge it. LLMs are most useful when the problem is genuinely ambiguous and requires reasoning over context.

## The Real Goal

**The practical goal of an eval is not elegance.** The goal is to help the team iterate faster with more confidence. If the system helps developers test in scale, simulate real use, inspect failures, and make better decisions, then it is doing its job.
