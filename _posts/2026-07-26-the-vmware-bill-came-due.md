---
title: "The VMware bill came due. Now what?"
date: 2026-07-26 10:00:00 -0500
categories: [migration]
tags: [work]
description: "The honest decision tree I walk enterprises through when the virtualization renewal lands and the number has changed."
---

If you run enterprise infrastructure, you've either had this meeting or
you're scheduled for it: the virtualization renewal comes in, the number
has grown a multiple, the licensing model has changed shape, and somebody
upstairs wants a plan on their desk by Friday.

I've spent the last several years of my career in exactly these meetings
(across Retail, Manufacturing, Healthcare, and Financial Services), and the
first thing I tell every team is the same: you have more options than you
think, but you don't have forever to pick one. The second thing I tell them
is that panic-migrating everything is how you turn a licensing problem into
an outage problem.

So, what do you actually tell the person who wants the plan by Friday?
It's a fair question.

## The actual decision tree

Every estate is different, but the branches are not. There are four, and
you'll probably use more than one of them.

**Option 1: Pay, and buy time deliberately.**
Yeah, sometimes the right answer for *this renewal* is to pay. But pay
for a shorter term than procurement wants, and spend that term executing
one of the other options. The mistake isn't renewing; it's renewing for
three years and then doing nothing for two and a half of them. If you
take this branch, the renewal IS the deadline for the rest of the plan.

**Option 2: Swap the hypervisor, keep the datacenter.**
Hyper-V, Proxmox, Nutanix, OpenShift Virtualization: the field is real,
and I've seen each of them hold production weight! The honest trade-off:
you're re-training your ops team and re-plumbing your backup, DR, and
monitoring stack no matter which one you pick. The replacement hypervisor
is rarely the expensive part of this whole "swap"; the retooling around it
is where the money actually goes. Budget for the ecosystem.

**Option 3: Lift the estate to cloud, mostly as-is.**
Every major cloud will happily run your VMs, and for workloads with a
limited remaining lifespan this is often the right call: you're buying
yourself a way out of the datacenter! Be clear-eyed about what it is,
though. Your cost model shifts from a big renewal every few years to a
bill every month, and if you treat cloud like a rented datacenter
*permanently*, you'll pay rented-datacenter prices permanently. This
branch works if you actually keep moving once you land.

**Option 4: Re-platform what actually deserves it.**
Some fraction of your estate (usually smaller than the modernization
deck claims) genuinely benefits from moving to managed services,
containers, or PaaS. The way to find that fraction is not a vendor's
assessment tool defaulting to "yes." It's asking, per application: who
maintains this, what breaks if we touch it, and what do we get for the
effort? "It's technically possible" doesn't answer any of those questions.

## What actually decides bake-offs

Having sat on the vendor side of a bunch of these evaluations, let me tell
you what separates the teams that land well from the teams that don't. It
isn't the platform choice. It's three unglamorous things:

1. **A real inventory.** And I don't mean the CMDB. I mean the ACTUAL
   estate, including the forgotten VMs that turn out to run the badge
   readers. Every difficult migration I've ever seen got difficult in the
   discovery gap.
2. **A named owner per workload.** Migrations stall when no one in
   particular owns an app; everyone's job ends up at the bottom of
   everyone's list.
3. **A definition of done that includes operations.** "It boots in the new
   place" is not done. Backup, DR, monitoring, patching, and the on-call
   runbook are done.

So, there's no vendor-neutral answer to "where should it all go," and
anyone who gives you one before seeing your estate is selling something.
But there's always a *sequenced* answer: this workload now, that one at
renewal, those three never. The order you move things in matters more
than the destination you picked.

I'll go deeper on each branch in future posts, including the hybrid
patterns for the workloads that genuinely can't leave the building. Data
residency, latency-pinned manufacturing floors, and regulators all exist,
and a plan that ignores them only works on the slide.

Thanks for reading!
