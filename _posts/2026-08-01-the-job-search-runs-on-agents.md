---
title: "The job search runs on agents so I don't have to"
date: 2026-08-01 10:00:00 -0500
categories: [career]
tags: [work, research]
description: "How I built an agentic harness around my job search (the resume pipeline, the fact-checked record, the voice profile) so the human hours go to networking, interview prep, and figuring out what comes next."
---

Microsoft cut our entire Infrastructure GBB organization in early July.
Within a week, every
well-meaning person in my life had told me the
same thing: "job searching is a full-time job." Yeah, they're right, but
nobody ever asks WHICH parts of that job actually deserve full-time
attention. Most of a modern job search is mechanical: tailoring documents,
formatting, tracking, drafting yet another polite follow-up. Mechanical
work is what I have agents for. So the first thing I built after the
layoff wasn't a spreadsheet of openings. It was a harness.

Same deal as [the home lab](/workbench/agentic-management-of-a-personal-home-lab/):
this post is the setup (the harness, the agents, the process, and what
it's produced) and then the actual point, which is what all of it buys.

## The harness

The search lives in a project directory that's structured like an
engineering repo, because it is one: resumes, cover letters,
correspondence, imported conversations, and a build pipeline. The agent
works inside it with full context; nothing lives in my head or a random
Downloads folder.

**Resume-as-code.** There's one archetype resume (the maintained,
canonical account of the career) and it never ships. What ships is a
build: a Node pipeline using the `docx` library that produces a tailored
Word document and PDF per application. Formatting bugs got fixed once, in
code! When a posting demands a different emphasis, the delta is a
tailoring pass, not an hour of fighting Word's list indentation at
midnight.

**A fact base with usage rules.** Underneath everything sits a performance
record (every deal, metric, and win from the career, compiled once and
kept internal) with hard rules the agents must draft against: numbers
come only from the record, opportunities are never written as bookings,
losses are stated as pursuits, customer names stay generalized unless
there's a release. In other words: the resume can't inflate, because the
data layer refuses. I'd love to claim this was a character decision, but
honestly it's just good engineering (the same reason you validate at the
schema and not in the UI).

**A voice profile.** The strangest and most valuable piece. The agents
draft correspondence in MY voice, trained not on vibes but on my actual
sent mail, and refined the way you'd tune any model: by diffing what the
agent drafted against what I actually sent, and folding every correction
back into the profile. It has learned things about my writing I hadn't
noticed myself: that I thank people for specific things rather than in
general, that I'd rather hand someone a menu of options than an
open-ended ask. The drafts land close enough now that my edit pass is
minutes!

## The process

The loop per application is boring, which (like the lab) is the point:

1. **Intake:** posting comes in; an agent maps its language against the
   record. Where there's a truthful match, the resume mirrors the
   posting's exact terms. Where there isn't, it doesn't (see rules above).
2. **Build:** tailored resume and cover letter compile out of the
   pipeline. I review a diff, not a document.
3. **Correspond:** replies, scheduling notes, thank-yous, warm-network
   messages get drafted in-voice with the full thread as context. **I
   send everything myself.** The agent has no send button, and it never
   will. The harness stops at the edge of every relationship.
4. **Track:** state lives in the repo. "Where are we with X?" is a
   question the directory can answer.

## The Self-JD: scoring the search in both directions

The piece of the harness I'd recommend even to someone who automates
nothing else: before I applied to a single posting, I wrote **the job
description for the role I would post for myself.** Scope, altitude,
domain, the customer-facing/engineering mix, location and travel shape,
and the markers of the places I've done my best work. It forces the
"what do I actually want" conversation out of your head and into a
document that can be argued with.

Then it becomes a scoring system, and the weighting is where most people
(me included, first pass) get it wrong. The temptation is to weight the
shiny factors (brand, title, the comp headline). The fix is evidence:
go back through your own history, role by role, and ask what actually
correlated with thriving versus what correlated with leaving. Those
factors get the heavy weights. My own record says autonomy and problem
quality predicted every good year I've had, and title predicted nothing;
the weights now reflect that, whatever the shiny part of my brain thinks
on a given Tuesday.

Every posting that comes in gets scored two directions:

- **The role against my Self-JD:** weighted fit, does this deserve
  pipeline space at all. A posting that can't clear the bar doesn't get
  a tailored resume, however good the note that came with it felt to read.
- **Me against the role:** requirement by requirement. And here's the
  rule that makes it worth doing: **every claim needs a receipt from the
  record.** "I can do this" doesn't score; "I did this, here, with this
  outcome" scores. The same discipline as everything else in the
  harness, and it cuts both ways. It catches the
  impostor reflex that undersells real evidence, and it catches the
  stretch fantasy where I talk myself into a fit that's really three
  gaps in a trench coat. What's left is an honest gap list, which is
  exactly the interview-prep syllabus and the cover letter's talking
  points.

And it's a living system: **refinement and rescoring.** After every
conversation and every loop, what I learned goes back in: a weight was
wrong, a requirement on paper turned out not to matter in the room, a
"dream fit" revealed a travel load the posting never mentioned. The
agents rerun the scoring across the whole pipeline, and the ranking
reshuffles without sentiment. Sunk cost doesn't get a vote; the
month-old conversation gets rescored like it walked in today.

## The outcomes

So, the honest scorecard, a month in: the mechanical layer of an application
(the part that used to eat an evening per posting) now takes minutes of my
attention. The documents going out are fact-checked to a standard no tired
human at 11 PM holds himself to: nothing ships that the record can't back.
And the pipeline is doing what a pipeline should: a bunch of active
conversations running, interview loops in motion. No outcomes to announce
yet; when there's one, this thread will hear about it.

## The actual point

Here's the thing the harness is really for, and it isn't efficiency for
its own sake. Everything above exists to protect the hours that matter,
because the parts of a job search that produce a job were never the
documents:

**Networking.** Every conversation that has moved my search forward has
come through an actual person rather than a portal. The harness means
that when someone offers me twenty minutes, my prep is done and my
follow-up is thoughtful and same-day; the machine handles the ceremony
so I can show up prepared and actually present.

**Interview prep.** Loops at the principal level are won in the room.
The reclaimed evenings go to the actual work: systems thinking, stories
with numbers I can defend, whiteboards.

**Self-reflection.** The least automatable task on the list: figuring out
what the next decade should actually be, rather than sprinting into the
first thing that looks like the last thing. The Self-JD is where that
thinking gets written down, but the thinking itself happens on long
walks, away from any terminal. A layoff hands you a rare, unwelcome "gift"
(a forced pause with real stakes), and it would be a waste to spend it
fighting Word.

So that's the design: the agents handle the search's mechanics, and
the human parts stay human. If you're in the same boat (and this
industry has put a lot of good people in this boat lately), build the
harness once, and then go spend yourself where it counts.

Thanks for reading!
