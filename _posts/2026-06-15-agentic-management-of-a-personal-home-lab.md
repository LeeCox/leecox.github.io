---
title: "Agentic management of a personal home lab running Windows Server, Azure Linux, and Azure Local"
date: 2026-06-15 14:00:00 -0500
categories: [workbench]
tags: [lab]
description: "The full setup guide: an isolated lab, VS Code Remote Tunnels, GitHub Copilot in agent mode, and the skill-building that turns an AI into a competent junior admin for your rack."
---

Every home lab has the same dirty secret: the gap between what's racked and
what's *managed*. The gear accumulates faster than the discipline. Firmware
drifts, the documentation lives in your head, and the network diagram
exists mostly as a feeling. I've run labs for a couple of decades and made
peace with the truth that a lab is a second job that pays in knowledge and
charges in weekends.

This year I changed the deal: I gave an AI agent a seat in the lab as
operations staff. Not a chatbot to ask questions: GitHub Copilot in agent
mode, with a terminal inside the lab, a documentation repo, and a steadily
growing bunch of responsibilities. It works well enough that this post is a
**how-to**. Stream of consciousness ahead (fair warning). Read the
whole thing before you touch anything.

***General Disclaimer***: **You're on your own.** You are about to give a
language model a shell inside your network. Done the way I describe, the
blast radius is contained and the lab stays isolated from the internet. Done
lazily (tunnel on your domain controller, auto-approve everything), you've
built a very enthusiastic intruder. Don't go complaining to me, GitHub, or
Microsoft when it deletes a VM you loved.

## The lab (so you can calibrate)

Mine is seven physical nodes, 768 GB RAM total, in (and around) a 22U
rack: a two-node
**Azure Local 2603** cluster on Lenovo SE350 edge servers, a standalone
**Windows Server 2025** Hyper-V box on an EPYC 4464P build (with a pair of
HPE DL360 Gen9s staged behind it, *cough*, old habits...), a Lenovo MX1021
queued up for its slot, and an SE350 carrying an **NVIDIA A2**, currently
being stood up as the edge-inference node (that build, and the Kubernetes
flavor it lands on, gets its own post). Arc-enabled everywhere, because half the point is
living with the same management plane I'd recommend to a customer. None of
this is required. The pattern works on two NUCs. What matters is the
topology, so let's start there.

## Step 1: Topology (isolation is the whole foundation)

The security model of everything below rests on one fact: **the lab is its
own island.** Mine runs on its own internet connection and its own network,
physically separate from the network my family and my work machines live
on. Yours doesn't need a second ISP: a dedicated VLAN with deny-by-default
rules to your home network accomplishes the same thing. The requirements:

1. **No inbound ports from the internet. None.** Everything below works on
   outbound connections only.
2. **A management VM inside the lab.** This is the agent's body: a plain
   Windows Server VM on the Hyper-V host (Linux works fine too). Install
   the tools an admin would have: RSAT, the PowerShell modules for your
   stack, kubectl, git. This box has line-of-sight to the nodes.
3. **Out-of-band management stays out of reach.** BMCs (iLO, XCC, iDRAC)
   live on a management network the agent's VM **cannot route to**. The
   agent never needs to reflash firmware, and neither does an intruder.
   Same for your identity infrastructure and firewall administration.
   Humans only. This is the line I do not move.

## Step 2: VS Code Remote Tunnels, the drawbridge

So, you need to reach that management VM from wherever you are, without opening
a single port. That's exactly what **VS Code Remote Tunnels** do: the VM
makes an *outbound* connection to the tunnel service, authenticated through
your GitHub account, and your VS Code client (or vscode.dev in a browser)
connects through it.

On the management VM (pick your own tunnel name; I'll use `lab-mgmt` in
the examples):

```
winget install Microsoft.VisualStudioCode.CLI
code tunnel user login --provider github
code tunnel service install --accept-server-license-terms --name lab-mgmt
```

The login command hands you a device code to sign in with, and the last
command registers the tunnel as a service so it survives reboots. From
anywhere in the world, VS Code → Remote Explorer → `lab-mgmt`, and you
have a terminal INSIDE the lab! Protect the GitHub account like the crown jewels it now is:
hardware-key 2FA, because whoever holds that account holds your drawbridge.

Two gotchas that cost me a "bit" of time so they don't cost you any: run the service
as an account with the admin rights you actually want the agent to have
(service install defaults are conservative), and if the tunnel name doesn't
appear, check that the service actually started (Windows will happily
install it stopped).

## Step 3: GitHub Copilot, hiring the staff

Open the tunnel session, sign in to **GitHub Copilot**, and switch to
**agent mode**, the mode where it can run terminal commands, read files,
and iterate, not just complete lines. Now the important part, because the
default settings are wrong for this job:

- **Do NOT blanket-auto-approve terminal commands.** Copilot will ask
  before running commands; keep it that way for anything mutating. I
  allowlist a small set of obviously read-only patterns (`Get-*`,
  `kubectl get`, `git status`) and approve everything else by hand.
- Approvals are per-workspace. Set them in the *lab workspace*, not user
  settings, so the posture travels with the repo.

CAN you auto-approve everything and let it rip? Sure. You can also hand a
new hire domain admin on day one. The lab might even survive it. "Might"
is doing a lot of work in that sentence.

## Step 4: The docs repo, ground truth the agent reads and writes

Create a **private** repo (mine has a suitably boring name) and open it
as the workspace in every tunnel session. Structure it the
way you'd brief a new admin. Mine:

```
00-lab-overview.md        # what this place is, quick stats, diagram
01-hardware-inventory.md  # every node: model, CPU, RAM, storage, NICs, role
02-network-topology.md    # subnets, VLANs, switch ports
03-clusters-workloads.md  # what runs where and why
04-identity-security.md   # accounts, the rules, what's off-limits
05-management-access.md   # how humans (and the agent) get in
06-ip-reference.md        # assignments and reservations
```

**Keep credentials out of it.** Names and addresses of things, yes. Secrets,
never. The agent doesn't handle credentials at all in this design; it runs
as the identity you gave the tunnel service, and that identity has exactly
the rights you chose. The repo stays private either way: your rack layout
is nobody's business (yes, I just told you mine, but the difference is
that I picked what to share).

Every agent task starts from these docs instead of rediscovering the
network, and (this is the part I didn't see coming) **the agent
maintains them!** Repatch a switch port, retire a VM, and the doc updates
in the same session, same commit. Twenty years of "I'll document it later"
ended the month the documentation became something my staff needed to do
their job.

## Step 5: Skill development (the part everyone skips)

Out of the box, the agent is a talented generalist who knows nothing about
your lab and has no manners. Both problems are fixed the same way:
**instruction files**, and treating them like a skill you're developing,
not a config you set once.

Start with `.github/copilot-instructions.md` in the docs repo. Mine boils
down to house rules:

- Read the relevant doc before touching anything; cite what you checked.
- Read-only is free. **Propose mutations before running them**: exact
  commands, expected effect, rollback.
- Never touch BMCs, identity, or firewall config. Don't ask.
- Verification before "done": show the hotfix list, the validation output,
  the pod states. A summary is not evidence.
- Update the docs in the same session as the change.

Then grow a library of **prompt files**, reusable skills for routines:
`patch-scan.prompt.md` (inventory update status across the Azure Local
cluster and the Hyper-V host, report only), `health-check.prompt.md`,
`doc-sync.prompt.md` (walk the inventory, verify it against reality, flag
drift). The development loop is simple: **every time the agent does
something wrong or dumb, the correction goes into the instructions.** It
misread which node was the cluster witness once; the docs got a clarifying
line and it never happened again! That's the skill (yours and its)
compounding in the repo, in version control, reviewable like everything
else.

## Step 6: Execution (what a working day looks like)

So, with the pieces in place, the rhythm is almost boring, which is the point:

- **Questions are free.** "Which nodes are behind on updates?" "What's
  eating the storage pool?" A sentence goes in and a cited answer comes
  out, and nothing needed approval because nothing mutated.
- **Changes are proposals.** "Stage the June updates on the Azure Local
  cluster" produces a plan (order, commands, expected reboots) and then
  waits. I approve, it executes, and it shows receipts before the word
  "done" appears.
- **The long tail is where it shines.** Arc extension quirks, AKS node
  image updates, the research-then-do work I used to burn a ton of evenings on.
  The kind of thing the posts I wrote for
  [our team blog](/topics/work/) were made of, now executed by staff
  while I review.
- **First of everything gets watched.** The first time a new task type
  shows up, I babysit it. The second time I spot-check. By about the
  fifth time, it has become a prompt file.

## The honest close

Is this more setup than pointing a chatbot at your lab? Yeah, by a lot.
But it's also the difference between a party trick and an operating model. The
pattern is isolated network, outbound-only access, docs as ground truth,
skills in version control, humans holding approvals and identity, and
verification as a gate. I think that's the shape enterprise operations is
drifting toward. The lab is where I get to find the sharp edges first, on
hardware where the worst case is my weekend instead of someone's business.

Next up in this thread: the SE350 + NVIDIA A2 edge inference build, from
BIOS to first token, and what a 16 GB GPU can honestly do at the edge.

Thanks for reading!
