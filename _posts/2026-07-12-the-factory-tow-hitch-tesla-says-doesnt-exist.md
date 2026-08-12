---
title: "The factory tow hitch Tesla says doesn't exist"
date: 2026-07-12 10:00:00 -0500
categories: [garage]
tags: [garage, research]
description: "A three-year-ish quest through Tesla's parts catalog: TW01 harnesses, unicorn cars, a relabeled part after a six-month wait, and the hybrid jumper harness I'm building because the right connector is nearly impossible to order."
---

This one started at a Tesla service center in 2023, when I asked about
adding the factory tow package to my 2021 Model S and was told, in
person, that they had the parts and the procedure but hadn't been given
clearance to install a hitch on the Model S. A year into it, a service
advisor gave me the blunter version, with a straight face: "Model S
doesn't support a tow hitch."

Which is interesting, because the Model S service manual contains the
retrofit instructions. The parts catalog contains the hitch. The SOP16
wiring schematics contain a complete trailer circuit. Europe gets the
hitch as an option. What Tesla means is *"we'd rather not."* And
"we'd rather not" is not a wiring diagram. So began a quest that's
coming up on three years, and since I documented the whole thing across a
long-running forum thread as it happened, this post is the assembled
story. Fair warning: there are a bunch of part numbers ahead. That's the
good part. 🙂

## The archaeology

The Model S left-hand body harness is the nervous system of the car's
whole left side, and it's where the factory tow wiring lives. When it
lives anywhere. Digging through the parts catalog turned up six harness
variants, and three of them carry the designation that matters: **TW01**,
the towing electrical connector in the SOP16 schematics.

| Part | My read |
|---|---|
| 2486394-00-C | HW3 harness, no tow |
| 2486394-01-C | HW3 with TW01, likely the Rev1 "two connector" tow wiring |
| 2486394-71-C | HW3 with TW01, likely the Rev2 "single connector" version |
| 2486394-00-D / -01-D | The HW4 equivalents |
| 3486394-01-B | The Plaid harness, no tie-breaker listed |

Rev1 versus Rev2 matters: Tesla changed the hitch's electrical interface
mid-life. The service documentation spells it out once you know to look:
hitch part numbers **J and above are Rev2; H and below are Rev1.** If you
ever buy one of these off eBay, ignore the model year in the listing and
read the suffix. That one sentence has already saved at least one person
I've corresponded with from buying the wrong revision.

## The part that ships but doesn't exist

So, armed with the numbers, I did the obvious thing: ordered both HW3
TW01 harnesses and let Tesla's parts system tell me the truth.

The **-71-C never shipped.** Months of waiting, then nothing. The
**-01-C did ship**, after a "quick" six months, and when I opened the
box, it was a -00-C wearing a -01-C label. The tow wiring and connectors
simply weren't in it. My best guess: there was a plan to support harness
retrofits, somebody killed it late, and the part numbers survived while
the actual copper didn't. At this point my standing advice is blunt:
**if you want the factory tow harness, you're pulling it off a wreck.**

## Unicorn hunting

Here's where it gets fun. While chasing paperwork, I found evidence that
some cars *left the factory with the tow wiring installed*: an early
Plaid whose owner needed nothing but the Rev1 hitch itself, bolt-on and
program. There appear to be more unicorns out there in North America.

The five-minute check if you own a Palladium S: pop the driver's sill
trim and look in the bottom front corner, next to the left-hand body
controller. If there's a trailer-brake-controller plug and wiring
sitting there unused, congratulations: you're holding a winning lottery
ticket Tesla never told you about!

And the software side confirms the car is willing: I set the trailer
configuration on my own 2021 LR through a Toolbox session. No errors!
The car *knows how* to tow. It's the copper that's missing.

## The build

So: no orderable harness, a proprietary Tesla-specific controller
connector on the S/X side that's VERY hard to order (I never found a
source), and a car that's software-ready.
The engineering answer is a **hybrid jumper harness**, and the unlock
came from the Model Y parts bin: the HW4 Model Y uses the *same hitch
controller* with a different Molex connector at the trunk, served by a
cheap sub-harness (a $50 part) that carries the controller connectors
and the hitch connector. Graft that Y sub-harness onto Model S/X Rev2
controller wiring, follow the pin/terminal/wire-size documentation that
exists on both sides of the graft, and you have a factory-electrics tow
installation Tesla never sold you!

It's slow going (this is the kind of project where a connector spends
weeks on a boat), and it's probably a 1:1 solution rather than
something repeatable. But I think it's fun, and the documentation trail
means the next person starts from mile 20 instead of mile zero.

A bunch of hard-won practicals if you attempt any version of this: the
service center will let you *order* hitch parts but won't *install*
them ("Model S doesn't support a tow hitch," remember), and they won't
put an X-specific part on an S even where the part itself lists Model S
specs. If you go the simpler taillight-signal route instead, run your
connections through the boot at the right-hand lower storage area:
untape the boot from the harness, fish the wires, retape. Cleanest
installation you'll get. And ship hitches with the controller, wiring,
and 7-pin receptacle removed and packed separately. I've had to send
one back because the shipping weight beat the attached parts to death.

## Why bother?

Somewhere in year two, a reasonable person asks: for towing a small
trailer, why not just buy the perfectly good third-party hitch? Yeah,
for most people that's honestly the right answer. What's on my car today is the
factory hitch itself, installed but not yet wired (controller taped off,
waiting on the harness). But "the factory did it this way, therefore it
can be done this way" is a hill I've been climbing since I was flashing
AS-BUILT data into Fords (that's [its own story](/garage/asking-cars-to-do-things-they-werent-ordered-with/)),
and there's a real difference between bolting an accessory onto the car
and putting back what the factory designed in. I want the second one.

The car supports a tow hitch. It says so right there in the manual
nobody at the counter has read.

Thanks for reading!
