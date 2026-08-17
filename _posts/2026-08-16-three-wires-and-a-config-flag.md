---
title: "Three wires and a config flag"
date: 2026-08-16 10:00:00 -0500
categories: [garage]
tags: [garage, research]
description: "Matrix headlights on a Palladium Model S or X: what the light actually needs from the car, why the connector changed, why the Model 3 gets this without an adapter, and what you give up."
---

The Palladium refresh Model S and X launched in the middle of the Covid
supply chain mess, and Tesla "simplified" a bunch of things to keep the
line moving. The Global headlight, the ambient lighting, the taillight
design, and the motorized tilt screen all got cut or changed. Later
build years put most of it back. So there are a lot of 2021 and early
2022 cars driving around missing features their own platform already
supports.

The Matrix headlight is the one people ask me about most, and it's the
one I'd do first. The light output isn't close.

*(Disclaimer: I make the aftermarket adapters for this, so read
accordingly. The engineering is the same whether you buy mine or build
your own.)*

## What changed in Feb 22

Matrix lights came to the Model S in the 2022.5 refresh and the Model X
in the 2023.5. Both got a different front harness. The connector went
to 3 pin where the earlier cars have a 14 or 6 pin, and in most cases
that's the only change in the entire harness.

Three pins is all a Matrix light uses. Pin 1 Power, pin 2 LIN, pin 3
Ground. No high beam, no low beam, no turn signal, no DRL. Tesla pushed
the blinker across the LIN bus when they made the switch, so the analog
blinker circuit goes away on these cars. Everything the light does now
arrives as a command over LIN.

![A matched pair of Matrix headlight adapters, three-pin end and car-side end](/assets/img/matrix-adapters/adapters.jpg)
*Small end to the light, big end to the car.*

The lights are also physically different. The "Premium LED" units have
a vertical mounting hole in the inner corner. The Matrix lights have a
horizontal mount point in the same spot, and the plastic inner front
frame on the refresh cars takes both on the same bolt. Matrix housings
are a little smaller and carry a bumper retention clip on the bottom.
On the old lights that's a separate bolt-on piece you pull off with the
light.

## Why the 3 and Y don't need any of this

Matrix conversions on the Model 3 and Y are plug and play with a config
change, and Tesla supports it officially there because the old
headlights aren't made anymore. No adapter, no harness work.

Tesla's stated position on the S and X is that the retrofit isn't
possible, and the reason they give is the connector difference. That's
the whole gap. Same bus, same three circuits, different plug.

You can tap the factory harness instead of using an adapter. I don't
recommend it on a car under warranty.

## Voltage, and the flag that sets it

The "Premium" lights run 12v. The Matrix lights run 16v, stamped right
on the housing.

![Rear of a Model S Matrix headlight housing showing the LED driver module](/assets/img/matrix-adapters/matrix-housing-model-s.webp)
*Model S unit from the back. The finned silver block is the LED driver.*

![Rear of a Model X Matrix headlight housing with the 16V rating on the label](/assets/img/matrix-adapters/matrix-housing-model-x.webp)
*Model X housing. SAE HL 22, LED, 16V.*

Nothing in the adapter does that conversion. The controller voltage is
set by the MCU software. Change the config to Global lights and the car
raises the voltage on that circuit and changes the LIN command version
at the same time. Teslas run on feature flags for the hardware
installed, and this is one of them.

The Palladium cars can do it because of the same parts mess that
started this: they were designed to run both. The capability shipped
with the car and nobody ticked the box.

Gen1 Model S and X can't, and it's two problems, not one. They're 12v
cars, and the Gen1 software branch has no Global light option in it at
all. There's no flag to flip because the code to drive these lights was
never written into that branch. If you have a 2016 car, that's the
answer, and no adapter changes it.

## The process

1. Shut the car down.
2. Pull the plastic cladding and the frunk tub.
3. Unplug the old headlight connectors and plug the new lights in
   loose, sitting in the frunk.
4. Power the car back up.
5. Connect the diagnostic tool and launch Toolbox.
6. Infotainment Dashboard, set Headlight Config to **Global**.
7. Reinstall the software in service mode, or Canbus Redeploy from
   Toolbox.
8. Confirm the config took and the lights actually function. Then shut
   the car down again.
9. Swap the physical lights.
10. Calibrate with the Global procedure in the service manual.

![The finished adapter installed in the car, plugged in behind the fascia](/assets/img/matrix-adapters/adapter-installed.jpg)
*In the car. Nothing cut, nothing spliced, and it comes back out.*

Steps 3 through 8 are the part people skip. You're proving the config
and the lights while everything is still loose in the frunk. Do it the
other way around and you're pulling the fascia off again to figure out
whether it was the config or a connector that wasn't seated. Ask me how
I know. 🙂

The diagnostic access and the media converter side of this is a whole
subject on its own, and it's getting its own post.

## What you give up

Adaptive is not an always-on feature and a bunch of people think theirs
is broken. It needs Auto high beams on and the Adaptive box checked in
the lights menu, and it only engages on very dark roadways. On a lit
street you'll never see it do anything.

The cornering lights are gone. Those are in the silver top section of
the "Premium" headlight and that section isn't functional on a Matrix
light. An update was supposed to reproduce the behavior with the Matrix
segments. It's incredibly subtle. The best I can describe it is the hot
spot of the beam shifting when you turn, and it's nothing like the old
spotter lights coming on. Plan on losing them.

2025 and newer cars don't need any of this, they come with the lights.
They also end the broader recontenting story. Those 2021-2024
controllers carry leftover wiring for front radar, USS, the premium
headlights and more, and that's not an exhaustive list. If even one of
those wires got reused for another function the controllers aren't
compatible. Lots of unknowns still in there.

## After

Done right, there's nothing to see. No warning lights, no service
message, better headlights than the car was sold with. That's held for
nearly everyone who's done it, though I won't claim it's been
universal. When it does go sideways it's almost always the config not
taking or a connector not fully seated, which is why step 8 is in the
list.

The [factory tow hitch](/garage/the-factory-tow-hitch-tesla-says-doesnt-exist/)
post is the long version of a parts hunt, three years chasing a tow
package Tesla says doesn't exist. How I ended up making adapters at the
kitchen table is
[over here](/garage/the-accidental-parts-supplier/).

Next up from the garage: the programming side, and the tools that make
it possible.

Thanks for reading!
