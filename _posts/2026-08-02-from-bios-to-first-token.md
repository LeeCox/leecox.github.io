---
title: "From BIOS to first token: standing up an NVIDIA A2 at the edge"
date: 2026-08-02 10:00:00 -0500
categories: [ai-infra]
tags: [lab, research]
description: "Bringing up a 16 GB NVIDIA A2 in a Lenovo SE350 edge server: GPU Operator, KAITO, Dynamo with a vLLM worker, and every gotcha between power-on and the first generated token."
---

Everyone's AI infrastructure story starts in a datacenter with a rack of
DGXs and a power bill that needs its own approval chain. Mine starts in a
22U rack next to the water heater, with a Lenovo SE350 (a ruggedized edge
server about the size of a large hardcover book) and a single **NVIDIA
A2**: 16 GB of GDDR6, low-profile, sipping around 40-60 watts. The
Warthog of GPUs. Nobody writes keynotes about it, but it goes everywhere,
survives everything, and it's the vehicle you actually drive through the
whole campaign.

I've spent years advising enterprises on GPU platform strategy from the
architect's seat (factory-floor vision systems, store-edge inference at
national scale), and the honest truth is that if you count the sites
instead of the FLOPs, far more enterprise AI *deployments* run on cards
like the A2 than on anything that gets a launch event. So when it came
time to build the edge-inference tier of
[my home lab](/workbench/agentic-management-of-a-personal-home-lab/), I
wanted the real experience: bare metal to generated token, every step
scripted, every gotcha documented. This post is that story.

## The stack, and why each layer won

**Ubuntu 24.04 LTS.** Yeah, the decade-ish-at-Microsoft guy put Ubuntu on
it. (This box spent its previous tour running AKS with Azure Linux, as
described in the lab post; that build got wiped for this one, because I
wanted the vendor's recommended path from bare metal.) Why? Because
**the support matrix is the boss.** NVIDIA
publishes exactly which OS/kernel combinations are validated for the A2
and which rows of the GPU Operator's driver-container matrix get
*precompiled* images: 24.04 on the 6.8 kernel is the row you want, 22.04 on
the same kernel falls back to a slower DKMS build, and the shiny new
26.04 LTS isn't in the matrix yet at all. When the vendor hands you a
validated-configurations table, you read it and you obey it. I told
customers this weekly for years; it would be poor form to ignore my own
advice in my own rack.

**k3s.** Single-node edge Kubernetes with the operational weight of a
sandwich. The enterprise version of this conversation involves fleet
management and a fleet-scale distro, but the Kubernetes API is the
Kubernetes API, which is the entire point of the exercise.

**NVIDIA GPU Operator.** If you haven't watched it work: the Operator can
own the entire GPU enablement stack, driver included: hand it a bare
node and it will do the rest. In my build the driver lands on the host
first (script 01 of my pipeline, old habits from years of hand-built
nodes), so the Operator runs with `driver.enabled=false` and manages
everything above it: container toolkit, device plugin, DCGM monitoring,
all as Kubernetes-native objects it keeps healthy. (If you let the
Operator own the driver too, the install-order gospel below relaxes.
That's half of why the Operator exists.) Either way, the first time
`nvidia-smi` runs *in a throwaway CUDA pod you didn't hand-configure*,
you get the same small thrill as the first ping across a network you
built! Validation is scripted: if the pod can't see the silicon, the
pipeline stops, loudly.

**KAITO, in bring-your-own-GPU mode.** Microsoft's Kubernetes AI
Toolchain Operator, pointed at my own node instead of provisioning cloud
GPUs. That exercises its workspace CRD against hardware I already own,
which is the part enterprises with existing GPU fleets actually care
about. And the Microsoft-operator-managing-NVIDIA-silicon crossover
episode felt like required viewing given my résumé.

**NVIDIA Dynamo, fronting a vLLM worker.** The serving layer: Dynamo's
frontend with a single vLLM worker (the `vllm-runtime:1.3.0` image from
NGC, serving a small Qwen3 model to prove the plumbing). Is a
datacenter-scale serving framework overkill for one 60-watt-ish card?
Completely. That's the point: the *shape* of the deployment is the
shape enterprises run at scale, so the frontend, the worker lifecycle,
and the deployment topology all transfer when one worker becomes forty.
(With one worker, the fancy disaggregated prefill/decode split stays
strictly theoretical: I know exactly which Dynamo features need a
bigger fleet than mine.) Plus a Hugging Face model served through TGI on
the same box. Not at the same moment, mind you: the device plugin hands
out the card whole, and vLLM books most of the 16 GB the second it wakes
up, so the serving stacks are roommates who take turns. Kicking the
tires on two of them on identical hardware is exactly the kind of thing
this lab exists for!

## The gotchas (collect them all)

A bunch of things between power-on and first token that the quickstarts
don't mention:

1. **Secure Boot will eat your driver install** if you let it. The prep
   script checks for it FIRST, because discovering it *after* the
   driver "installed successfully" is a rite of passage I only needed
   once.
2. **Order is everything (in a host-managed-driver build).** Driver
   readiness before Kubernetes, Kubernetes before Helm, GPU Operator
   before any workload dares mention a GPU. My scripts are numbered `00`
   through `09` because the dependencies are genuinely that linear, and
   each one checks current state before acting, so re-running anything
   is safe. Idempotence is the difference between automation and a very
   fast way to break things twice.
3. **16 GB is a budget, not a suggestion.** The A2 will happily serve a
   small quantized model, vision workloads, or embeddings all day on a
   thermal envelope that doesn't require explaining anything to the
   power company. Ask it for more and it will politely decline. Edge
   inference is mostly an exercise in living inside the VRAM you
   brought.
4. **Remote management is step nine, not step zero-point-nine.** Once the
   GPU tier worked, the box got the full "treatment" (SSH hardening,
   Tailscale, Cockpit, k9s) and joined the
   [agent-managed lab](/workbench/agentic-management-of-a-personal-home-lab/)
   like everything else in the rack.

## Why this matters beyond my water heater

So, what does a hobby box next to a water heater have to do with
enterprise AI strategy? It's a fair question. In the enterprise
engagements I've worked (automotive plants, national retail footprints),
the edge AI conversation is NEVER about the biggest GPU. It's about
exactly what this build practices: modest silicon in hostile places,
driver lifecycle you don't hand-touch, Kubernetes as the control plane,
and a serving layer whose shape scales from one worker to hundreds
without changing the architecture. The SE350 + A2 combo is my one-node
rehearsal of a pattern I've watched deploy at real fleet scale.

The useful lesson for the platform-strategy crowd: the stack above mixes
NVIDIA's operator and serving layers, a Microsoft AI operator, an
open-source inference engine, and a community Linux. And the *support
matrix*, not the logo, decided every layer. That's the actual state of
enterprise AI infrastructure in 2026, and the architects who accept it
early save their customers a ton of money (and a lot of awkward vendor
meetings).

Next up in this series: what a 16 GB card can honestly do (model sizes,
quantization trade-offs, and where the A2 taps out versus its bigger
siblings). The token counter is running!

Thanks for reading!
