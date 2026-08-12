---
layout: default
title: About
permalink: /about/
---

<section class="hero">
  <h1>About</h1>
</section>

<div class="post-content" markdown="1">

I'm **Lee Cox**, a principal enterprise architect out of the Nashville
area. Most recently I was at Microsoft, in the Global
Black Belt organization advising large enterprises on cloud migration,
hybrid infrastructure, and AI platforms. Before that: HPE, Ericsson, and
HP, going all the way back to pulling cable in healthcare IT. So I've
been the guy in the briefing room, the guy in the datacenter, and
(occasionally) the guy who broke the thing in the datacenter.

The honest bio: I'm a Techie, a '90s Nerd, a Car Guy, a Father, and a
Husband, and every one of those outranks any job title. The Techie and
Car Guy parts generate the blog posts: advising enterprises on where
their workloads should live, running an Adaptive Cloud home lab (Azure
Local, Windows Server, and AKS on a rack of edge hardware, increasingly
managed by AI agents), and asking cars to do things they weren't
ordered with. The '90s Nerd part is the site you're looking at. The
Father and Husband parts mostly stay off the blog, which is how they'd
prefer it.

**What this site is:** field notes. Migration strategy I'd give a
customer, engineering logs from the workbench, honest takes on AI
infrastructure, car projects up on jack stands, and the occasional story
from a career spent in enterprise tech.

**What this site isn't:** the opinion of any employer, past or future.
All views are mine. Customer stories are anonymized or composite. No
generative disclaimers needed on the opinions: those are handcrafted,
small-batch, and occasionally wrong!

## The paperwork {#resume}

<span id="deck"></span>
Looking for the professional documents? Both are viewable right in the
browser (contact details redacted; that's what the form below is for!):

- **[The résumé](/resume/)**: the Principal Solution Engineer cut.
- **[The walking deck](/deck/)**: nine slides on who I am, what I've
  shipped, and what I'm looking for.

## Contact {#contact}

This form composes an email in your own mail app; nothing is stored on
this site, because this site is a pile of HTML and vibes. You can also
reach me on [LinkedIn](https://www.linkedin.com/in/leecoxtn) or
[GitHub](https://github.com/LeeCox). I read everything!

<form class="mailform" id="mailform">
  <label for="mf-name">Your name:</label>
  <input type="text" id="mf-name" name="name" size="40">

  <label for="mf-subject">Subject:</label>
  <input type="text" id="mf-subject" name="subject" size="40" value="Hello from leecox.pro">

  <label for="mf-body">Message:</label>
  <textarea id="mf-body" name="body" rows="8" cols="60"></textarea>

  <div class="mailform-actions">
    <button type="submit" class="btn95">📨 Send E-Mail</button>
    <button type="reset" class="btn95">Clear</button>
  </div>
</form>

<script>
document.getElementById('mailform').addEventListener('submit', function (e) {
  e.preventDefault();
  var addr = ['lee', 'cox'].join('') + String.fromCharCode(64) + ['gmail', 'com'].join('.');
  var name = document.getElementById('mf-name').value.trim();
  var subject = document.getElementById('mf-subject').value.trim() || 'Hello from leecox.pro';
  var body = document.getElementById('mf-body').value;
  if (name) body += '\n\n— ' + name;
  location.href = 'mailto:' + addr +
    '?subject=' + encodeURIComponent(subject) +
    '&body=' + encodeURIComponent(body);
});
</script>

Thanks for reading!

</div>
