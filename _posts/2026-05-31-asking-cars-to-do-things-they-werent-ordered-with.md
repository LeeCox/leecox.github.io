---
title: "Sixteen years of asking cars to do things they weren't ordered with"
date: 2026-05-31 10:00:00 -0500
categories: [garage]
tags: [garage]
description: "From an F250 nav unit in a 2010 Escape, to a factory tow package on a Lincoln MKT, to Tesla gateway configs: one instinct, three generations of vehicle architecture."
---

Every car I've owned has eventually faced the same question: *what else
were you built to do?* Not modified in the fast-and-loud sense (I've
never cut a spring or glued on a wing). I mean the quieter obsession:
the factory designed this platform to support features my particular
car didn't ship with, the wiring or the software or the mounting points
are often still there, and somewhere in a service document is the proof.
The build sheet records a purchase decision somebody made at a
dealership years ago. It doesn't have to be the last word on what the
car can do.

This is the origin-story post for the Garage section, because the
current project (a factory tow hitch Tesla insists doesn't exist, but
that story is coming in its own post)
makes more sense once you know it's the third generation of the same
instinct.

## 2010: the F250 navigation unit

It started with a 2010 Ford Escape XLT and a navigation head unit pulled
from a 2009 F250. Same corporate parts bin, VERY different
window stickers. The physical part of the swap only takes a Saturday.
The electrical part was the real project, because the truck radio and
the little SUV had never been introduced to each other.

The key was Ford's **AS-BUILT data** (the per-vehicle configuration
record that tells every module what the car is and what it's allowed to
do). Load the AS-BUILT values from a donor vehicle whose build DID
include the feature, and the electronics stop arguing. I fed my Escape
a section of a 2010 Escape Limited's configuration, and the nav unit
woke up believing it had always lived there! Drop it in reverse and the
camera screen came up, ready and waiting... for a camera I hadn't
actually wired in yet. 🙂 The pigtails and the custom wiring came later,
but as far as the modules were concerned, that camera had been ticked
at the factory.

That swap taught me the foundational lesson of all of this: **modern
vehicle features are parts + wiring + configuration**, and the
configuration is just data. Data can be read, understood, and (done
carefully, with the service documentation open) rewritten.

## 2013: the tow package that Lincoln forgot

Next platform, same move, more copper. The 2010 Lincoln MKT EcoBoost
(my beloved black-on-black sleeper) shares its bones with the Ford Flex,
and the platform supported a factory tow package mine didn't ship with.
So it gained one! The real thing, factory parts, factory routing,
documented on the owner forums as I went. Along the way that car turned
into a rolling seminar on the rest of the discipline: talking a bunch
of other owners through retraining the collision-warning sensor after bodywork,
EcoBoost charge-air plumbing, and the eternal truth that the parts
catalog knows more than the sales brochure ever admitted.

The MKT era added the second lesson: **the factory's own engineering is
usually the best aftermarket kit you can buy.** It fits, it lasts, and
the documentation (diagrams, torque specs, connector part numbers)
already exists. You just have to be willing to read like it's your job.

## 2022: feature flags on wheels

Then came a 2021 Model S. Does a Tesla even have an AS-BUILT record?
No. Not in the Ford sense, anyway. What it has is a
**gateway config**: a file of feature flags living in the gateway
controller inside the MCU, telling the car's software what hardware it
believes it has. Change the config (through Tesla's own Toolbox, these
days with a paid day pass), redeploy, and capabilities appear exactly
the way my Escape's camera screen did sixteen years ago!

The architecture is genuinely elegant (zonal controllers, smart
devices on LIN and CAN buses, one config to rule them), and it's made
possible a bunch of things the Ford era couldn't dream of: matrix headlights on a
car sold with the lesser lights, a passenger-lumbar retrofit done over
a weekend on my wife's Model Y, tail lamps from a later revision, the
tow-hitch project. The *instinct* is unchanged. The difference is the
posture of the manufacturer: Ford published AS-BUILT structure openly
enough that a guy with a laptop could learn it in 2010. Tesla treats
the equivalent layer as theirs, on a car I own (an approach I once
described on a forum as very Apple, and I didn't mean it entirely as a
compliment). The capability being *in there*, behind a flag I'm not
supposed to flip, changes the project: the engineering is still the
engineering, but now there's a "negotiation" with the manufacturer
sitting on top of it.

## What sixteen years of this actually teaches

**Read the service documentation first, opinions second.** Every one of
these projects began in a wiring diagram or a parts catalog, not a
YouTube thumbnail. The documents don't care what the counter says is
possible.

**The gap between trims is where the fun lives.** Manufacturers build
one platform and sell it as five cars. The wiring for the car you
didn't buy is frequently sleeping in the car you did.

**Configuration is the modern crowbar.** In 2010 it was AS-BUILT hex
values. In 2026 it's gateway feature flags. In both cases, the
difference between "your car can't do that" and a working feature was
never metal. It was a few bytes of permission.

**And know when you're done.** Well, not every gap is worth closing; some
retrofits turn out to be a "bit" bigger than they look on paper, and the
fog-light adapter that once cost me a tow truck (a story for another
post) taught me the price of forgetting that. The discipline isn't
doing everything; it's knowing what the platform supports, and
choosing.

So, the Garage section will mostly be these stories: current Tesla
projects, the occasional MKT flashback, and the tooling that makes it
all go. If your car's build sheet has ever felt like a suggestion,
you're among friends here.

Thanks for reading!
