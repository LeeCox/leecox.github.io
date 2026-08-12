---
title: "Carlinkit T2C - Review"
date: 2022-12-14 00:50:58 -0600
categories: [garage]
tags: [garage]
description: "My review of the Carlinkit T2C CarPlay adapter for Tesla: what works, what doesn't, and the data-privacy wrinkle nobody talks about."
---

I've been interested in getting CarPlay to work in my Tesla since I got
it. I started with Tesla-Android project and when Carlinkit came out with
their adapter I pre-ordered it from their store on AliExpress. Here are
some of my thoughts:

**The Good:**

- It works now (in the US on T-Mobile)
- It's compact
- Low Heat
- Decent web interface

**The Bad:**

- It's slow / resolution isn't the best / lite artifacts
- Your Tesla vehicle Data passes through the device
- Build quality of the device could be better
- No 5G Signal (increasingly a problem)
- Always on issues

The initial experience with the device was very underwhelming. I got it
in, and the box was non-functional, wouldn't connect and display CarPlay.
I reached out to the Mfg on Twitter and they asked me a bunch of
questions about my config. Promised to get it working or give me my money
back. I received around half a dozen firmware updates until last nights
which got the unit working. I went out for a short drive, and it behaved
as expected.

On the way back home, I looked in the web interface to see the signal
strength reported in the center console. It wasn't great, and
disconnected a few times in my area, but I think that has to do with
T-Mobile's 4G/5G transition in my area. 5G used to suck, but now it's
fine and 4G sucks. It's a miss that the device doesn't support a 5G
capable modem.

I got back home and went in the house which is where the weirdness
starts. Tesla's wake themselves up with the phone key and mine is very
aggressive with this. Over the next few hours, the car/device connected
to bluetooth on my phone and started playing music. Most notably while I
was putting my daughter to bed where it shares a wall with the garage.
The iPhone kept flashing "Driving" mode on my phone but wasn't quite
connected. I turned off Bluetooth to get it to stop.

Then overnight the car woke up a bunch and sent data out over the device,
or it sent data while awake. This to me is the most troubling part and
where the Mfg needs to add some metrics. It should keep a log of all the
data it receives from the car and what it passes on to Tesla servers or
other connections.

![Seems like my car wants to talk to Tesla at :39 very few hours](/assets/img/carlinkit-t2c/data-traffic.png)
*Seems like my car wants to talk to Tesla at :39 very few hours*

As to the usefulness of the product. I'm on the fence and I understand
why Tesla says that AA and CarPlay aren't needed in their cars. It's just
not all that useful now. Especially, if the rumor about Apple Music
coming in a future update is true. That and Apple Maps/Waze are the
primary reasons I use CarPlay and I can live without the Maps.

If I had it to do over again, I probably wouldn't buy this or even build
the Tesla Android solution. They are fun to tinker with, but the
practicality of them is being reduced regularly with Tesla updates. I've
included pictures of both device setups and the T2C interface. The
Tesla Android solution is superior in most ways but is also a bit
unwieldy.

![My Tesla Android Setup - This works great BTW](/assets/img/carlinkit-t2c/tesla-android-setup.jpg)
*My Tesla Android Setup - This works great BTW*

![CarLinkit T2C - Very Simple](/assets/img/carlinkit-t2c/t2c-device.jpg)
*CarLinkit T2C - Very Simple*

![CarPlay interface - Notice the resolution slight artifacting](/assets/img/carlinkit-t2c/carplay-interface.jpg)
*CarPlay interface - Notice the resolution slight artifacting*

![Device and Version info](/assets/img/carlinkit-t2c/device-version-info.jpg)
*Device and Version info*
