---
title: "Build 2026 post-mortem: fog, first tokens, and a badge that says EXPERT"
date: 2026-06-07 15:00:00 -0500
categories: [career]
tags: [work]
description: "A week at Microsoft Build on the San Francisco waterfront: proctoring Lab 510, booth duty for Azure Linux, Satya and Jensen sharing a stage, and my first rides in cars with nobody in them."
---

I'm writing this from the couch, properly wrecked, with a conference
badge on the counter that says **EXPERT** in a reassuring shade of blue.
(The badge printer doesn't know about impostor syndrome. The badge
printer simply believes.) Build 2026 is in the books, and since the
whole week was equal parts teaching, booth duty, and staring at the
Golden Gate Bridge between sessions, it deserves a proper post-mortem.

<img src="/assets/img/build2026/badge-and-bottle.jpg" alt="Microsoft Build EXPERT badge for Lee Cox, Azure GBB, next to a black water bottle with the Golden Gate Bridge rendered in ASCII art">
<p class="small">The badge believes in you. The bottle is elite swag: that's the Golden Gate in ASCII.</p>

## The venue: Fort Mason, which cheats

This year's Build ran on the San Francisco waterfront, in the old pier
buildings at Fort Mason. Is it fair to put a tech conference somewhere
this pretty? No. It is not. Walk out of a session and the Golden
Gate is RIGHT THERE, doing its postcard routine over the water.
Alcatraz sat moodily in the fog behind the stacks of road cases like it
had been art-directed. At one point a flotilla of kayakers paddled into
the lagoon between the piers, flags up, just to watch the circus. Ten
years of convention centers did not prepare me for a venue with
seagulls and a view.

## Lab 510: running a model ≠ production readiness

So, my main job for the week: proctoring **Lab 510: taking LLMs from
prototype to production on AKS**. The session's opening slide is the
whole thesis, and I'd staple it to a bunch of conference-room whiteboards
if facilities would let me: *running a model is not production
readiness.* The lab
walks through the five things that close that gap: repeatability,
routing, observability, scaling, and a consumer-friendly endpoint.
Then it makes attendees earn each one on a live cluster.

Proctoring is its own discipline. You're not presenting; you spend the
session walking rows of monitors, watching a room full of people hit a
room full of *different* walls, and the walls are the curriculum.
Watching where people actually
get stuck in a hands-on AI lab is the best field research there is:
almost nobody struggles with the model. They struggle with everything
AROUND the model: the plumbing, the endpoints, the "why can't the
cluster see my deployment" moments. Which is to say: the production
part. The slide is right, and the room proved it all session, every
session.

## Booth duty: explaining Azure Linux until my voice gave out

Between lab sessions I worked the showroom floor at the **Azure Linux**
booth with my teammate Carlos, who is both excellent company and the
kind of colleague who can hold three technical conversations while
pointing a fourth person toward coffee. Booth duty at Build is speed
chess: forty-five seconds to figure out if the person in front of you
wants the elevator answer ("it's Microsoft's own Linux distribution.
It runs under more of Azure than you'd guess") or the real
conversation, which at this show was AKS node pools, edge deployments,
and where Azure Linux actually sits underneath the services people
already use.

<img src="/assets/img/build2026/booth-with-carlos.jpg" alt="Lee and Carlos smiling at the Azure Linux booth on the Build showroom floor">
<p class="small">Booth crew at the Azure Linux station. Note the Tux pin on the lanyard; the penguin worked the booth too!</p>

## The Satya-and-Jensen show

Keynote highlight, no contest: **Satya Nadella in person**, with Jensen
Huang beaming in ten feet tall on the side screens, for the
announcement of the **RTX Spark line**. I've watched a decade of these
keynotes from home; being in the room when the NVIDIA and
Microsoft logos came up side by side hits different!

<img src="/assets/img/build2026/satya-nvidia-stage.jpg" alt="Satya Nadella on stage in front of NVIDIA and Microsoft logos, with Jensen Huang on the side screens">
<p class="small">Satya on stage, Jensen ten feet tall on the side screens, and two logos that spend a lot of time together in my rack.</p>

Yeah, whatever else you want to say about this industry's current
moment, the two companies whose stacks I spend my days (and, who am I
kidding, my nights) wiring together were on one stage pointing the same
direction.
I'll have more to say about the small-end-of-the-GPU-market implications
once I've digested the announcements properly (the edge-inference
corner of my brain was taking notes the whole time).

## The city, all of it

So, two more San Francisco notes, because a post-mortem should be honest
about the whole week and not just the badge-scanned parts.

**The protesters were part of the week too.** On the hill above the
venue, demonstrators hung banners about AI datacenters and the
company's business, and you could see them from the piers all week.
I'm not going to
pretend they weren't there, and I'm not going to pretend a conference
about deploying AI at scale has nothing to do with the questions being
raised. You can believe in the work and still believe the hard
questions deserve to ride along. They walked with me to the lab more
than once.

**And the Waymos.** First ride of my life early in the week; by Friday
I'd stopped counting. I'm the guy who [reprograms his own
cars](/topics/garage/), so understand the gravity of this
sentence: I got into a car with no driver, no steering input from me,
and no gateway config I was allowed to touch, and within four minutes
it was the most normal thing in the world. The lane discipline is
genuinely better than half of Nashville. I have a hundred car-guy
questions about the sensor stack, but honestly I spent most of the
rides just looking out the window at the hills. I want to go back!

## The take-homes

A water bottle with the Golden Gate rendered in ASCII (elite swag,
whoever approved that gets a raise!), a voice that needed two days of
rest, a phone full of pier photos, and the same conviction I brought
home from the lab room: the models are the easy part now. The
production engineering (the repeatability, the routing, the
observability, all the "boring" plumbing) is where the actual work
lives, at every scale from my SE350 to the clusters those keynote
slides were built on.

It was a great week with a great team in a frankly ridiculous venue,
and I'll have more soon from the rack.

Thanks for reading!
