---
title: "Running down the clock on HW3"
date: 2026-08-09 12:00:00 -0500
categories: [garage]
tags: [garage, research]
description: "FSD v14 Lite is cooking HW3 computers, and nobody who has watched the platform is surprised. The three computers in a Tesla, the wattage and packaging math behind the retrofit problem, and why I think Tesla is playing for time."
---

The story making the rounds this month is that FSD v14 Lite is tied to
rising HW3 computer failures. Owners reporting overheat and fault
errors, mostly on hot summer drives. My reaction was not surprise. I
drive a 2021 Model S Long Range with HW3 and paid FSD, so this is not
an academic topic for me, and I've been posting pieces of this analysis
in owner threads for a couple of years now. This post is the assembled
version.

## The three computers

First, some context, because "the computer in a Tesla" is actually
three computers, and people mix them up constantly:

**Infotainment.** Intel or AMD depending on the year. Runs the screens
and the audio. This is the one that reboots when you hold the scroll
wheels.

**Gateway.** A custom low-power ARM processor that is on all the time.
It owns the in-vehicle networks (LIN and CAN) and brokers commands to
the other vehicle control modules. You never think about it, which is
the point.

**The AP computer.** Two custom inferencing processors and all the
cameras. This is what takes over when you turn on cruise or FSD, and
it's the thing everyone means when they say HW2, HW3, HW4, or HW5.

Since the Model 3 launched, all three have been packaged together as a
single compute package sharing an on-package databus. Same idea across
the fleet, VERY different form factors across the generations. That
last part matters a lot in a minute.

## The wattage tells the story

In my day job I size AI infrastructure, so when I look at the AP
computer generations I don't see car parts, I see accelerators:

| Generation | Package power | Inferencing is said to be roughly a... |
|---|---|---|
| HW3 | 200-300W | Nvidia T4 |
| HW4 | 400-450W (and ~75% larger by footprint) | RTX 4070 |
| AI5 / HW5 | projected 600-700W | RTX 5090 / Blackwell class |

Those are datacenter numbers riding around behind your dash! And they
explain a bunch of otherwise-random engineering decisions, like why
Tesla moved the cars from 12v to 16v around the HW4 transition (more
watts through the same gauge wire), and why the Cybertruck went all the
way to 48v. The power delivery, the cooling loop, and the harness are
all sized to the package. Remember that when we get to the retrofit
question.

## Why the failures aren't a mystery

The HW3 board has a bunch of redundancy built into it. Two inferencing
nodes, with the workload designed to run at around 50% capacity so
that either side can take over if the other faults. Full failover, by
design. And even in normal operation, Memory, Storage, and Turbo
processor errors occur routinely on these boards. They're aging, and
they were built with headroom precisely so that aging wouldn't matter.

Then v14 Lite arrived. Even the distilled model needs so much compute
and memory that HW3 runs the package well past the 50% design point.
Maybe not a flat 100%, but judging by the failures, close. The failover
headroom is mostly gone, and the computer is dumping close to double
the planned heat into a thermal loop that is already taxed on a hot
summer day. So you get overheat and fault errors, concentrated in exactly the
conditions owners are reporting.

In datacenter terms: they're running the N+1 cluster at N, at the top
of its thermal envelope, in August. Anyone who has watched a
passively-cooled T4 throttle in a chassis with lazy airflow knows how
this movie ends! (My mining-era bench taught me more about thermal
throttling than any datasheet ever did.) This is also why
unsupervised driving is simply never coming to HW3 (Tesla finally
admitted as much this spring). The redundancy
budget and the compute budget are the same budget, and v14 has already
spent most of it.

## The retrofit math

Tesla has said, in various wordings over the years, that cars with
paid FSD will get an upgrade path. Okay. What would that actually
take? Walk the stack with me:

**Power.** Going from a 200-300W package to a 400W+ or 700W package is
not a board swap. You're into the Power Conversion System and the
vehicle controllers (VCBatt, VCFront), plus jumper and adapter
harnesses to bridge the old connectors to the new package.

**Permutations.** How many versions of HW3 shipped across Model 3, Y,
S, and X over the years? The number is pretty nuts. Every permutation
is its own retrofit engineering problem, with its own kit and service
procedure. On my own car's platform alone there's the 8GB computer,
a rare 16GB revision of the same board, and then the "Gaming" E
through G revisions that widened the eCall connector to sneak Ethernet
pins in. I ordered the update jumper harness and tried it on the older
revision myself. It doesn't work.

**Packaging.** This is the part people hand-wave. On the Model S, both
generations of car computer live in the same place (passenger
footwell, behind the footrest panel), and that's about where the
compatibility ends. Go pull up the service manual: HW3 secures
with two bolts and one nut, HW4 with two bolts and two nuts, on a
different pattern, and the stud that locates an HW3 sits right about
where it would puncture an HW4. The coolant plumbing moved too. HW3's
coldplate ports sit close together near the middle of the unit; HW4
spread them out to the sides, so a swap means re-plumbing the thermal
loop, on a procedure that already requires a full coolant drain (with
a five-hour clock on the drain routine) and a vacuum refill.

The connectors moved around too: HW3's Autopilot board has ten
connectors plus a separate gaming-computer connector, HW4's has eight
and no gaming connector at all. Nobody has published measured
dimensions, but
the teardown folks who have had both units on a bench call HW4 a
totally different form factor, thinner overall but with a bigger board
footprint. I've been telling people who ask about computer swaps on
HW3 Palladiums the same thing for over a year now: not going to
happen, the sheetmetal and plumbing isn't compatible. There isn't really a way to
make it fit.

**Cameras.** HW3 cameras have the resolution of a potato next to the
5MP HW4 set, and the newer cameras run different voltage and cabling.
And Tesla now concedes the new computer needs the new cameras (that
admission finally came this April), so the "computer swap" is really a
computer, power, harness, and camera project. On a car that was never
wired for any of it.

**Feature parity.** And there's a newer wrinkle: even inside HW4, the
computer quietly got worse at the end. Every Model S since early 2023
is an HW4 car, but the ones built after roughly June 2025 lost the
discrete AMD GPU on the infotainment side and had storage halved from
256GB to 128GB, which converged the Model S computer on the
Cybertruck's configuration (the truck never had the GPU, and never got
Steam). New S and X deliveries had already lost Steam back in May
2024, via a quiet message to customers waiting on their cars, while
everyone already delivered kept it. So the last and "best" Model S
computers ever built do less than the 2023 units do, and with S and X
production ending this past quarter, nobody is putting it back. My
September 2021 build has the 8GB infotainment computer, so I never had
Steam to lose (Sad Panda), but I followed the 16GB retrofit threads
for a year. And there's precedent for upgrades that subtract: ask the
HW2.5 crowd from the last computer-swap era, whose companion MCU
upgrade cost them their broadcast radio, plus $500 more to get FM and
XM back. AM never returned.

Which brings us to the silicon roadmap, because the retrofit target
keeps moving. AI5 was supposed to be in cars in the second half of
2025 (that was the promise at the June 2024 shareholder meeting).
Instead, the design was declared finished in July 2025, followed by
Saturday design reviews all fall, followed by "almost done" in January
2026, with volume production pushed to mid-2027 and tape-out finally
happening this April. Read between those lines however you like. My
read is the part didn't meet the need and they started over, and given
that a chip takes about two years from design start to volume silicon
(with roughly a year of that after tape-out), the dates line up.

Meanwhile the stopgaps arrived: AI4.5, a three-chip unit that quietly
started shipping in Model Ys in January, and the AI4+ announced in
April with double the memory per chip (the official framing is that
AI5 goes to Optimus and the datacenters first, which is its own tell).
So what does that mean for HW3 cars? A repackage was going to be
required for any retrofit anyway, because nothing newer fits the HW3
envelope. There's a VERY good chance that AI4+ repackaging work is
what eventually goes into HW3 cars. Tesla has even floated dedicated
retrofit micro-factories, because the service centers would choke on
the volume. That's the space I'm watching.

## Running down the clock

Meanwhile, notice what Tesla is actually doing: free FSD transfers to
newer cars, offered again and again. Every owner who transfers retires
a retrofit liability at zero hardware cost. Elon's favorite saying is
that the best part is no part, and the cheapest retrofit is the one
you never have to ship. So my read, and I've been saying this in the
forums for a while, is that they're running down the clock on those of
us holding paid FSD on HW3 cars.

Now think about the conundrum for the folks deepest in this hole. Say
your car started life as HW2.5. You bought FSD. You already went
through one computer swap to get HW3, and if you were on the old
infotainment, the MCU upgrade that swap wanted is the one that took
your radio. Musk has promised the next upgrade free to FSD buyers and
called it "painful and difficult." Painful and difficult toward WHAT,
exactly? Do you get AI4? AI4+? AI5? The most anyone will commit to is
AI4-something plus the cameras, and the answer decides whether the
thing you paid for, twice now in hardware terms, ever actually shows
up.

If you're shopping used right now, the advice writes itself: buy the
newest car you can afford, and if the budget lands you on an HW3 car,
make sure FSD is an included package so you're in line for whatever
hardware update eventually materializes. If it never does, well, you
didn't pay extra for it.

As for my car: the early Palladium Model S is the most upgradable
Tesla ever made, and I've already done a bunch of the incremental
updates the newer cars shipped with (that instinct has
[its own origin story](/garage/asking-cars-to-do-things-they-werent-ordered-with/),
and the [tow hitch quest](/garage/the-factory-tow-hitch-tesla-says-doesnt-exist/)
is ongoing). If an AI4+ retrofit program ever opens, I'll be first in
line, and you can bet I'll document every connector and part number
here! Until then the car stays supervised, and on hot days I know
exactly what that computer is going through. 🙂

Thanks for reading!
