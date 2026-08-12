---
title: "The six months my lab spent in a bottling plant"
date: 2026-07-22 10:00:00 -0500
categories: [ai-infra]
tags: [work, lab]
description: "Why I drove my own servers to a customer site, ran Azure Local and AKS on hardware that was never on the validated list, and left them there for half a year."
---

There is a moment in a lot of enterprise infrastructure projects where
everybody in the room agrees the idea is good, and then nothing happens
for nine months. The reason is almost never technical. It's hardware. Somebody has to
pick a validated SKU, get a quote, wait out an OEM lead time, and then
walk a capital request through a committee that meets once a month and
has opinions about depreciation schedules. That's 4-6 months of
calendar in the best case, and it starts AFTER everyone agrees. In the
meantime the champion who fought for the project gets reassigned, the
plant manager's priorities shift, and the whole thing quietly dies of
old age.

I've watched that happen more times than I'd like. So a couple of years
ago, on a project at a bottling plant not far from where I live, I did
something about it: I pulled two **HPE DL360 Gen9** servers out of
[my own lab](/workbench/agentic-management-of-a-personal-home-lab/),
put them in my car, drove them to the facility, and racked them myself.
They stayed there for about six months.

Those are my servers, not Microsoft's, not the customer's, not a
loaner off somebody's demo pool. They were my personal property, out
of my own rack, sitting in a production food and beverage plant.

## Why my hardware and not theirs

I know what you're thinking, because I thought it too the first time
somebody suggested it to me years ago: this is a terrible idea, and
somewhere there is a procurement person having a bad feeling and not
knowing why.

Here's the argument for it anyway. What the customer needed to decide wasn't "does Azure Local work." Of
course it works. What they needed to decide was whether the *pattern*
worked in THEIR plant: with their line data, their OT network, their
historians, their people, and their tolerance for a screen that goes
dark during a shift. That is not a question you can answer in a slide,
and it is definitely not a question worth spending six months of lead
time to ask. It's a question you answer by putting the thing in the
building.

So the deal I proposed was simple. I'd bring the compute, at no cost
and no PO, and stand up the platform in the plant. If the pattern
proved out, they'd buy validated, supported hardware and we'd
migrate onto it. If it didn't, I'd take my servers home and nobody
would have spent a dollar of capital finding out. The only thing at
risk was my time and my gear.

That reframes the entire conversation. "Should we spend $200K to find
out?" is a hard question that goes to a committee. "Do you mind if Lee
puts two servers in the corner?" is a much easier one, and it goes to
whoever has the keys.

## What actually ran

The stack was deliberately boring, because boring was the point:

**Azure Local**, running **AKS on Azure Local** on top of it. Two
nodes. Arc-attached, managed the same way the eventual production
cluster would be managed, so that nothing anyone learned in those six
months would have to be unlearned later.

And no accelerators. Not one. That matters because of where the story
goes at the end: those Gen9s had CPUs, RAM, disks, and NICs, and that
is the complete list. Everything running on that
cluster was a bunch of ordinary containerized workloads: the edge tier of a
manufacturing data platform, pulling from the line and pushing to the
cloud. The interesting math was happening elsewhere.

## The part where I contradict myself

When I write up the A2-at-the-edge build (it's coming; the sermon is
already drafted), you'll see me get fairly religious about validated
configurations. The position: the support matrix is the boss, when a
vendor hands you a table of validated combinations you read it and you
obey it, and I told customers this weekly.

Then here I am telling you I ran Azure Local on DL360 Gen9s!

Gen9 is not on the Azure Local validated hardware list. It was old
before this project started. If you'd opened a support case for that
cluster, the conversation would have been short (and you'd have
deserved it!). So which is it? Both, and the difference matters more
than either rule on its own.

The support matrix **is the boss** when you are building the thing the
customer is going to run. Production, or anything that will become
production by accident, gets validated hardware, a supported
configuration, and a path to a real support case at 2 AM. No
exceptions, no cleverness, no "it'll probably be fine."

The support matrix is **NOT the boss** when you are building the thing
that proves the customer should buy the thing they're going to run.
Different job entirely. That box exists to answer a question and then
go away. It needs to be honest about the platform's behavior and it
needs to not lie to anyone about what it is. Past that, if it's
sitting in a corner running a proof, who cares whether the SKU is on a
list.

The failure mode isn't picking one or the other. It's forgetting which
one you're doing. I have seen a "temporary" POC cluster still carrying
production load three years later, on hardware nobody will support,
because it worked so well that everybody forgot it was supposed to
leave. That is the actual risk, and the way you manage it is to say
out loud, on day one, in writing, that this hardware has an expiration
date and here is what replaces it.

Mine left on schedule. That's the whole trick. (You wrote the
expiration date down, right?)

## What six months bought

More than a POC usually does, because it wasn't a POC in the way that
word usually gets used. There was no test plan with a success criteria
column that somebody would grade at the end. It just ran, in the
plant, next to the line, while people used it.

You learn a bunch of things that way. You learn what the OT network
actually does at shift change versus what the network diagram says it
does. You learn which data the operators trust and which numbers they
quietly ignore because they've been wrong before. You learn that the
screen placement matters more than the dashboard, and that a
sub-second refresh nobody asked for is worth less than a number
somebody believes. None of that shows up in a bake-off.

And critically, by the time the capital conversation happened, it
wasn't a capital conversation about a hypothesis. It was a capital
conversation about something already running that people had gotten
used to, where the only question left was what hardware it should live
on permanently. That is a MUCH easier meeting!

## Where it went, and where my part stops

The site went on to become a reference deployment. At a large industry
conference the following spring, the manufacturing data platform
running at that plant was showcased with an integration into NVIDIA's
**Omniverse**: physically accurate 3D digital twins of the production
line, rendered through Omniverse Kit App Streaming, running on NVIDIA
GPUs in Azure, feeding a browser-based view the operators use on the
floor. It's a genuinely impressive piece of work (and it's public, so
it's easy enough to find).

I want to be careful here, because this is exactly the kind of story
that gets stretched in the retelling. I didn't build that part. The
Omniverse layer came later, it came from the platform vendor, and the GPUs it runs on are in Azure, not in
that plant and not in my servers. My line stops at the on-prem
platform tier: the two nodes, Azure Local, AKS, the edge workload, and
six months of it staying up.

What I'd argue is that the on-prem tier is what made the rest possible
on that timeline. The reason there was a story to showcase in the
spring is that somebody put hardware in the building in the fall
instead of waiting for a PO. The fancy part gets the keynote. The
boring part is why the fancy part had a plant to run in.

## So what's the takeaway

"Forward deployed" gets used a lot right now, mostly to describe a
seat where you sit closer to the customer than a normal architect
does. Fine. But in my experience it usually cashes out
as something less glamorous and more literal: at some point, if the
project is actually going to happen, somebody has to physically show
up with the thing.

I've spent a lot of my career in the briefing room, and I like the
briefing room. But the projects I'm proudest of all have a moment in
them where the slides ran out and the work turned into a drive, a
loading dock, and a rack.

If you're sitting on a project that everyone agrees is a good idea and
that hasn't moved in two quarters, it's worth asking what would happen
if the hardware problem simply went away for six months. Sometimes the
answer is that you'd find out you were wrong, cheaply and early, which
is also a win. And sometimes the answer is that the thing you've been
trying to get funded turns out to be the thing somebody puts on stage.

One of these days I'll write up what the migration off my Gen9s and
onto the validated gear looked like, because that cutover had its own
lessons.

Thanks for reading!
