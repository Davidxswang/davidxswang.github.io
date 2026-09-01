---
layout: post
title: "The Bottleneck Is Not Coding"
date: 2026-09-01
excerpt: Coding agents can accelerate explicit work, but people and organizations remain responsible for making the work explicit.
tags: [what]
---

Imagine giving a coding agent a one-sentence task: add a new way to use a model in the product.

The agent reads the repository, writes clean code, adds tests, and opens a polished pull request. The tests pass. The diff looks reasonable. Someone reviews it quickly and merges it. Only later does the team discover that the feature uses the model incorrectly, solves the wrong user problem, and depends on an assumption that nobody actually agreed with.

It is easy to call this an agent failure. But the agent may have done an excellent job with the task it received. The real failure happened before any code was written. Nobody established what the model was good at, who the feature was for, what success meant, or how the result should be tested. The agent filled in the missing pieces with plausible assumptions, and the reviewer mistook technical competence for correctness.

**A coding agent can produce a technically excellent implementation of an organizational misunderstanding.**

This is why I do not think the main question for an AI-native organization is how many coding agents it can run. More implementation capacity is useful, but implementation is only one part of delivering the right product. Agents can accelerate explicit work. People and organizations remain responsible for making the work explicit.

## Two Systems

There are really two systems involved in software development.

**The first is the organizational system.** It turns a need into product intent and product design. Someone has to decide whether something should be built and why, which concepts users need to understand, what journey and experience the product should create, how it should behave, which priorities and tradeoffs matter, and how success will be demonstrated.

**The second is the coding system.** It turns that product direction into a working implementation. Technical design belongs here: database design, backend and machine-learning architecture, frontend code structure, module boundaries, data flow, and the interactions among components. The work also includes writing code, running tests, debugging failures, and preparing the result for review.

Coding agents can make the second system much faster. They can build proofs of concept, change large numbers of files, write production code, and run validation loops with substantial independence. I wrote about the local conditions that help them do this in [How to Help a Coding Agent](/2026/03/07/how-to-help-coding-agent.html): context, tools, and direct feedback.

But making the coding system faster does not automatically improve the organizational system. A team can generate more pull requests without becoming better at deciding which changes should exist. It can reduce the time from specification to code while leaving the time from confusion to specification untouched.

As implementation becomes cheaper, that untouched part becomes a larger share of the bottleneck.

## Before There Is a Product Specification

A common mistake is to assume the human already understands the problem and only needs to describe it to the agent. In a real product, the human may also be missing important context. The relevant behavior may be spread across code, documents, old decisions, production traces, and the knowledge of several people. Different parts of the organization may have different ideas about what the product is supposed to do.

This is work that agents can help with. An agent can trace a workflow through the repository, find the code that enforces a policy, compare documentation with current behavior, inspect logs, collect examples, and show where two sources disagree. It can turn a vague concern into a set of concrete questions without requiring a person to search every source by hand.

But gathering information is not the same as choosing the goal. An agent cannot legitimately decide which customer problem matters most, how conflicting stakeholder goals should be resolved, or which business tradeoff the organization is willing to accept. It cannot recover knowledge that was never written down or supplied by anyone. And when the evidence supports several reasonable directions, it should not silently convert one of them into organizational intent.

**Agents can organize, challenge, and operationalize human intent. They cannot be the original source of intent for an organization that has not formed one.**

That makes discovery before a product specification a joint process. Humans bring purpose, domain knowledge, and responsibility for the decision. Agents help collect the material facts, test claims against the system, and expose what is still unknown.

## Evidence, Target, and Proof

Once discovery has surfaced the material facts and uncertainties, the result should be more than a long prompt. I think a useful product specification has three parts.

**Evidence of the current problem.** What is happening, why does it matter, and what directly supports that account? The evidence might be code, a reproducible failure, focused logs or traces, redacted request-response examples, or a rendered product artifact. One example can prove that a failure is possible. It cannot establish how common the failure is. Claims about a distribution need representative evidence.

**Properties of the desired state.** What product behavior and user experience should change? What boundaries and invariants must remain intact? Which product choices have been agreed on, and what is explicitly out of scope?

**Executable proof of the transition.** How will we demonstrate that the problem was solved and required behavior was preserved? This may include tests, evals, a reproduction script, inspection of a rendered page, or another observable result. The proof should correspond to the actual goal, not merely to what is convenient to measure.

The compact version is: **evidence of the problem, properties of the desired state, and executable proof of the transition.**

The product specification defines what should change and why. It does not need to prescribe how the software will be structured. That belongs in a technical design or implementation plan within the coding system.

This is the management work behind a good task. A capable manager gives a team the purpose, relevant context, expected outcome, hard constraints, decision boundaries, and standard of success. The manager may suggest a path without prescribing every step. The same principle applies to agents. Vague delegation is not autonomy. It is the transfer of ambiguity from the person who owns the outcome to the worker who has the least context.

## Diligence Without Ceremony

This does not mean every task needs a long design document. Diligence is a standard for what we know and how we know it, not a document-length requirement.

A small, reproducible bug may need only a failing test, an identified cause, and a clear expected result. An ambiguous product feature or a change to a foundational abstraction may require much deeper discovery. The effort should scale with uncertainty, impact, and the cost of being wrong.

I would call a task ready when its owner can explain what is happening, show the important evidence, identify the cause or clearly label the remaining hypotheses, describe what must change, and state the material uncertainties. The person does not need to be fully aware of every fact in a large system. They need to be sufficiently aware of the facts that could change the decision and explicit about what they still do not know.

A useful handoff test is simple: can another responsible person understand both the current problem and the intended destination without reconstructing the reasoning from scratch?

Expertise matters here, but not because experts should always trust their first instinct. Expertise tells us which explanations are implausible, which risks deserve attention, and which evidence to collect. Verified evidence can still overturn memory or intuition. **Expertise should determine which questions we ask, not predetermine which answers we accept.**

## Autonomy After Alignment

Once the product direction is aligned, the agent should have substantial autonomy within the coding system. It should be able to propose technical designs, choose local abstractions, edit the necessary files, run the verification loop, and correct ordinary failures without asking for approval at every step.

Technical design is legitimately part of implementation, and the agent may see constraints or alternatives that the human missed. But consequential technical choices still need to be surfaced, reviewed, aligned, and recorded before they are embedded deeply in code and become expensive to reverse.

**Agent autonomy should begin after product alignment, not replace product alignment.** The rule is not that agents make no technical decisions. It is that no consequential design happens by accident.

The product specification is authoritative about the intended outcome, but it is not infallible. Technical design and implementation test that direction against reality. The work should stop and reopen the relevant product or technical decision when new evidence invalidates the original problem, when real constraints make the desired behavior infeasible, or when the actual blast radius is materially larger than expected. The same may be necessary when the proposed verification cannot demonstrate success.

This is not drift. It is the feedback loop working correctly. The difference is that the agent makes the contradiction visible instead of silently choosing a new direction.

## Review the Outcome

Better agents also change what human review should emphasize. If the product specification is concrete and the verification is trustworthy, the reviewer does not need to reproduce every mechanical action or read every generated line with equal attention.

The review can begin at the level of intent:

1. Does the result match the agreed behavior, boundaries, and non-goals?
2. Were the required tests and checks actually run, and were their results observed?
3. Does the material evidence show that the product works, not merely that a command exited successfully?
4. Does a focused sample of the diff reveal architectural drift, hidden assumptions, or an important anti-pattern?

The depth of evidence should scale with consequence, complexity, and observability. A local refactor and a change to a payment path should not require the same review process. But in both cases, human accountability remains at the outcome level.

Verification can be delegated to tests, evaluators, and other agents. Responsibility for defining and accepting success remains with the people who own the outcome.

## The Incentive to Produce

There is an organizational reason this problem is easy to miss. When agents make code and pull requests cheaper to produce, the new output is immediately visible. Rework, architectural damage, and product misalignment often appear later.

That creates a predictable incentive to reward visible activity as evidence of AI fluency. Pull-request count, feature count, token use, and code volume all look measurable. But each is causally ambiguous. More pull requests may represent faster delivery, or they may represent more fragmented work and more rework. More generated code may create customer value, or it may increase the amount of code the organization must understand and maintain.

**When implementation becomes cheap, organizations may reward its volume precisely when they should become more selective about what gets implemented.**

More agent activity scales the consequences of the organizational input, whether clear or confused. A grounded goal, specification, and validation loop can turn parallel agents into real leverage. An ambiguous goal can produce several polished versions of the wrong thing at the same time.

## Quality Is the Consequence

The point of this diligence is not process. It is the quality of the product.

Metrics and automated checks are evidence about quality, not quality itself. People still need to use the actual product, inspect what the user experiences, and decide whether the result feels correct and useful. A dashboard cannot rescue a product that is visibly broken or unpleasant to use.

Karri Saarinen makes a related argument in [Why is quality so rare?](https://linear.app/now/why-is-quality-so-rare): faster creation can separate judgment and care from the act of making, while immediate metrics can displace attention to the thing being built. I think coding agents make that risk more important, not less. When creation is fast, judgment becomes a larger part of the work that remains.

## The Practical Point

Coding agents should reduce the amount of human attention spent on mechanical implementation. That attention should move upstream into discovery and design, and downstream into judgment and acceptance. Human diligence becomes both the bottleneck and the source of leverage.

The human job is not to perform every step or to micromanage the agent. It is to make the goal real enough to act on: understand the problem, expose the relevant context, resolve the important choices, define the proof, and remain accountable for the result.

**The organizations that benefit most from coding agents will not simply be the ones with the most implementation capacity. They will be the ones that become exceptionally good at deciding, explaining, and verifying what should be built.**
