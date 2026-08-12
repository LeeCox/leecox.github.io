---
title: "I audited my backlog with 13 AI agents (and deleted a quarter of it)"
date: 2026-08-05 10:00:00 -0500
categories: [ai-engineering]
tags: [research]
description: "What happened when I pointed a small raid group of AI agents at 200+ backlog items and told them to verify every claim against the actual code."
---

Every project of any age carries a backlog that is part roadmap, part
archaeology. Items written by past-you, for reasons past-you understood,
in a codebase that has since moved on. You KNOW some of it is stale.
You also know that verifying a couple hundred items by hand is a full week
of unglamorous work, which is why nobody ever does it.

This is a story about doing it anyway. It took an afternoon and a raid
group of 13 AI agents.

## The setup

My current OS side project had accumulated just over 200 backlog items
across a bunch of documents. Some
dated back to the first week of design. The codebase had been through a
compositor rewrite and a licensing "cleanup" since. I no longer trusted the
list, and an untrusted backlog is worse than no backlog, because you stop
reading it.

So, I gave an agentic coding tool a job description instead of a task list:
take every item, go read the actual repository, and classify it. Is this
**done** (the code exists and works)? **Stale** (the premise no longer
exists)? **Still real** (verified gap)? Each agent took a slice, and each
had to cite the files it checked. No citations, no verdict.

## The part that matters: adversarial verification

Here's the thing I'd tell any engineering leader looking at agentic
workflows: **the first pass is not the product.** The first pass is a
hypothesis. Language models behave like enthusiastic interns. They work
fast and they never get tired, but every so often one of them is
confidently wrong about something important.

So, the second pass was a different set of agents whose only job was to
*attack* the first set's conclusions. Take a "this is done" verdict and try
to prove it wrong. Take a "stale" classification and check whether the
premise actually still exists under a renamed identifier. The adversarial
pass caught real misses! Items marked done where the code
existed but the *verification* didn't, which in my shop means it isn't
done.

## The results

- **53 items were stale-done** (marked done or claimed complete, but no
  longer matching reality). A quarter of the backlog was noise!
- The survivors got reorganized into a short **Now** queue (25 items),
  a Deferred pile, and (my favorite artifact) a `not-doing.md` file that
  records what I've *decided not to build* and why. Decisions rot slower
  when you write down the reasoning.
- Total wall-clock time: an afternoon! Most of it was me reviewing
  verdicts rather than producing them.

## What I actually learned

Did the agents replace my judgment? Well, no. They changed
where I spend it. I went from *producing* claims ("is this done?") to
*reviewing* them, and reviewing is where a couple decades of scar tissue
actually pays off.

**One rule** sits underneath all of this, and the rule is the whole post:
*never accept an agent's claim that isn't verifiable against the tree.*
Citations or it didn't happen. The same discipline I'd apply to a vendor's
migration assessment applies to an AI's backlog audit: an assessment earns
my trust when I can check it against the tree myself, no matter how
confident it sounds.

More in this series soon: the CI gates that keep AI-written code honest,
and what "proven fail-then-pass" means when the intern types faster than
you can read.

Thanks for reading!
