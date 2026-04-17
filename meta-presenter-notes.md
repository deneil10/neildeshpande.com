# Meta — Repair Console
## Presenter Notes

These notes are meant to support a verbal walkthrough of the case study — either in a portfolio presentation or a live interview. They contain the full context, reasoning, and story behind each section, including detail that doesn't appear in the online case study itself.

---

## Opening / Hero

**What to say:**

Start by grounding them in the scale of the problem. Meta runs one of the largest computing infrastructures on the planet — Facebook, Instagram, and WhatsApp combined serve billions of people daily. That experience is entirely dependent on hundreds of thousands of servers running without interruption across their global data centers.

When a server goes down, a repair technician needs to diagnose it, figure out the fix, and get it back online. My team was brought in to make that process dramatically more efficient.

**Important context to have ready:**

When I joined, this project was already a year in. Discovery had been done by another designer (Kaitlin). There was a general direction. But the design hadn't shipped, and there was real skepticism from leadership about whether the team was solving the right problems. One stakeholder said directly: *"Convince me this project is worth investing in — we've already been working on this for a year."*

That question reframed how I approached my role. Rather than just inheriting a brief and designing screens, I spent the first few weeks getting deeply into the problem space myself — re-interviewing users, auditing the existing tools, and pressure-testing the assumptions that had been established before I arrived.

**My specific role:**
- I was the design lead for the Repair Authoring workstream (Action Plans + Issue view)
- I supported the lead researcher with additional user interviews and on-site observations
- I collaborated with a partner designer (Kaitlin) who owned an adjacent part of the platform
- Team: 1 PM, 1 lead researcher, 11 engineers across Dublin/Berlin/Bay Area, 3 designers total

---

## The Problem

**What to say:**

The core issue was fragmentation. Meta had grown incredibly fast — and so had its internal tooling, but not in a coordinated way. Different teams across different regions had each built their own repair systems to meet their immediate needs. By the time we started this project, there were **7 separate repair ticketing systems** across the company, maintained by 7 separate engineering teams.

Each system was optimized for the team that built it. None of them talked to each other. Repair knowledge stayed locked inside individual teams. When a server failed in Dublin the same way it had failed in Singapore two weeks earlier, the Dublin technician had no way to know that. They'd start from scratch.

**The DIMM story (use this — it's powerful):**

One of the most striking examples came from an interview with a repair triage engineer. A manufacturing defect had caused nearly 11,000 bad DIMMs to be deployed across Meta's global server fleet. But because no team had visibility into the whole fleet, nobody noticed. Individual teams saw their own tickets, their own servers — and assumed their failures were isolated incidents. It took **a full year to identify the trend**, another year to create an action plan, and then another significant effort to actually replace the affected hardware. With a unified platform and better trend visibility, that kind of systemic issue could surface in weeks, not years.

**Why this matters for the design:**

This isn't just an inefficiency story — it's a fleet reliability story. Meta was planning to significantly expand its T16/T20 server count (their most complex and AI-critical machines). Scaling up hardware without fixing the repair process would have made the problem dramatically worse. The business mandate was clear: reduce unplanned downtime, reduce triage time, enable non-experts to handle more repairs.

---

## Research

**What to say:**

We interviewed over 40 people across Dublin, Berlin, Bay Area, Singapore, and Austin. I personally ran about 15 of those interviews and did on-site observation sessions at two data centers. We also reviewed a sample of historical tickets to understand patterns in how failures were actually being diagnosed and resolved.

**The most important finding:**

Despite wildly different contexts — different regions, different server types, different team structures — everyone we interviewed was struggling with the same core problems. The fragmentation wasn't just a tooling issue; it was a cultural and organizational one that had baked itself into how people worked.

**The three archetypes:**

We identified three distinct triaging behaviors, which became the foundation for how we designed the platform:

1. **Pattern Finder (Diagnostic Tech)** — Wants to understand how their current ticket relates to a broader trend. Frustrated by working on individual tickets in isolation. Would rather bulk-diagnose 12 servers with the same failure than handle them one by one.

2. **Monitor (Team Lead)** — Needs visibility across their team's work queue. Wants to know: what's in progress, what's breaching SLA, where are the blockers. Currently has to dig into individual tickets to get any of this.

3. **Actioner (Repair Tech)** — On-site, hands-on. Needs clear, prioritized, step-by-step instructions. Doesn't want ambiguity. Currently spends most of their time in triage — figuring out what to do — rather than actually doing the repair.

**Key pain points (with detail for discussion):**

- **Duplicate effort:** A fix figured out in Dublin might be solved from scratch in Singapore the following week. No mechanism to share repair knowledge across teams.
- **Triage time:** For complex T16/T20 servers, only a handful of SMEs could confidently diagnose failures. Everyone else spent hours reading generic runbooks, checking Slack, and making educated guesses. This was the biggest time sink we identified.
- **No knowledge persistence:** Repair history lived across tickets, wikis, Slack threads, and people's memories. When someone left the team, their expertise left with them.
- **Runbooks were ignored:** They went out of date faster than they could be updated. One user told us: *"We've changed our process 10 times since you last interviewed us."*

**Constraints (good to mention if asked):**

- Headcount was frozen — couldn't design for more people, had to make the existing team more efficient
- Couldn't disrupt live operations — had to roll out incrementally alongside existing tools
- Distributed team — design decisions needed to work across time zones and async-first workflows

---

## Design Principles

**What to say:**

Before moving into ideation, I worked with the team to define three principles that would act as decision filters throughout the project. These weren't aspirational — we used them to evaluate every concept and every tradeoff.

1. **Visibility:** Break down siloed work. Give every team member visibility into the repair ecosystem beyond their own queue — so they can detect trends, learn from each other, and understand the downstream impact of their decisions.

2. **Codify knowledge:** Externalize expertise. Turn the knowledge that lived in individual engineers' heads into structured, versioned, reusable processes that the whole system could learn from — and that would survive team turnover.

3. **Self-service:** Let experts evolve the system. Every time a team needed dev support to update a workflow or add a repair type, it created friction and delay. We wanted repair SMEs to have that power themselves.

**Why these three:**

They map directly to the three biggest failure modes we found in research: lack of visibility (teams working in silos), knowledge loss (no documentation that stayed current), and process rigidity (engineers dependent on dev cycles to change anything).

---

## Ideation

**What to say:**

With research in hand, I facilitated a series of cross-functional workshops — bringing together engineers, repair technicians, PMs, and subject matter experts. We used the team's different domains of expertise deliberately: engineers flagged what was technically feasible, repair techs told us what would actually help them on the floor, and SMEs helped us understand the depth of knowledge we'd need to capture.

We generated dozens of concepts. Two rose to the top:

**Idea 1 — Issues (grouped repairs):**
Instead of technicians working ticket-by-ticket, the system would automatically cluster failures sharing the same root cause into an "Issue" — letting teams diagnose and act on a group of failures at once. This directly attacked the duplicate effort problem.

**Idea 2 — Action Plans:**
Replace scattered runbooks with structured, expert-authored repair plans — attached to specific failure types, versioned, approved, and tracked for effectiveness. This attacked triage time and knowledge loss.

**Why both, not just one:**
They solve different parts of the problem and they're complementary. Issues give you the right scope (all servers with this failure type). Action Plans give you the right process (exactly what to do about it). Together they create a repair workflow that's both faster and more consistent.

---

## Testing & Iterating

**What to say:**

We built lo-fi prototypes quickly and took them on-site. This was critical — data center repair is a highly contextual activity. You can't fully understand it from a conference room.

**The big pivot — on-site testing:**

Our first design for the monitoring view was a broad, global dashboard showing failures across the entire fleet — aggregated metrics, issue counts, the works. We thought it was compelling. We took it on-site and tested it with technicians.

They were overwhelmed. The feedback was clear: *"I don't need to see what's happening everywhere. I need to know what I'm working on today."*

That was a pivotal moment. We had designed for the vision — the ideal state we wanted to get to — rather than where users actually were. A technician at a data center isn't thinking about global fleet health. They have a queue. They need to get through it. They want to know which servers are failing, why, and what to do.

We pivoted to a **hierarchical view**: global → regional → site/campus. Each level gives the right user the right scope:
- Leadership/program managers use the global view for trend-spotting and resource allocation
- Regional leads use the region view to track their geography
- Technicians use the site view — zoomed into their campus, their queue

**Action Plans iteration (4 rounds):**

1. **Free-form text editor** — Fast to build, but zero consistency across plans. Not machine-readable. Experts wrote walls of text. Technicians couldn't navigate them.
2. **Structured template** — Better. Required fields for failure type, steps, expected outcome. But experts hated the rigidity — real repairs branch based on what you find. A single path couldn't capture that.
3. **Tree-based action builder** — Each node is an action; branches represent different outcomes. Flexible, trackable, automatable. This was the breakthrough.
4. **Tree + reusable action library** — Added a library of pre-built action components (e.g., "Check logs on host," "AC Cycle chassis") that experts could drag into plans. Captured consistency without forcing them to start from scratch each time.

**Usability test findings (detail for discussion):**

- **"Entities Awaiting" CTA:** Most users completely missed the prompt to add servers into a plan. It was visually buried. We added a prominent badge with count + color accent. Click-through improved significantly.
- **Search criteria ("+" button):** Users building plan filters expected a "+" button directly in the search bar. Our design put it elsewhere. Task success rate for this flow was 40%. We moved the button. It jumped to 85%.
- **Plan navigation — depth problem:** T16/T20 power failure plans could be 6 levels deep. Users constantly lost their place. We explored a breadcrumb + mini-map approach (too cognitively heavy), then a simplified action history list view (clear winner in testing). This is what shipped.
- **Right panel behavior:** Early prototypes always defaulted to "Entity Info" on the right. Once execution began, users stopped looking at entity details and desperately needed the action history to stay oriented. We added a tab switch and set the default to History the moment execution started.
- **Approval flow:** Technicians were uncomfortable executing plans that hadn't been validated. The solution: a lightweight SME review flow. Approved plans had a distinct visual badge. Unreviewed drafts were visually de-emphasized.

---

## Final Design

**What to say:**

Repair Console shipped as three connected surfaces — all drawing from the same underlying data for the first time.

**Surface 1 — Global monitoring (3 levels):**

[Show Global_Profile screenshot]
The global view shows all of Meta's data center regions on a world map, color-coded by unavailability percentage. Fleet health bars break down server status by type (T1, T16, T20). The right panel shows the highest-priority issues across the fleet in real time.

[Show Region_Profile screenshot]
Drilling in shows a regional map — in this case US South — with individual data center locations plotted and color-coded.

[Show Geo_Profile screenshot]
The deepest zoom is the campus/site view — rendering the actual building footprint of a data center campus, with each building color-coded by its current unavailability rate. For the first time, a team lead could see at a glance which specific building was driving the most failures in their region.

**Surface 2 — Issue view:**

[Show Issue.png screenshot]
This is the core workflow change. Four tickets with the same failure type (Zion T16/T20 Power Failures) are automatically grouped into a single Issue. The technician sees all affected servers, their priorities and statuses, how long each has been waiting — in one table. One "Start Repair" button launches the Action Plan for the entire group.

The Conversations panel (bottom right) deserves mention — it came directly from research. Teams were coordinating via Slack threads that disappeared. We needed Repair Console to be the record of communication, not just of actions.

**Surface 3 — Action Plans:**

[Show Repair_Plan.png screenshot]
Early in this plan: "What was the error in the log?" — the technician picks from pre-defined outcomes, and the plan branches. The right panel defaults to Entity Info here — server type, rack, location, data center — all the context they need without leaving the flow.

[Show Action_Plan_Executing.png screenshot]
Further into the same plan: the technician is executing "Shuffle | GPU" — physically unplugging GPUs one at a time. The right panel has switched to History, showing the full path taken: Started Execution → Check logs → AC Cycle → Shuffle GPU (current step). This right-panel behavior was a direct result of the usability testing findings I mentioned.

---

## Results

**What to say:**

The engineering team built an MVP of Repair Console that was used in a controlled comparison against the existing repair process over a 60-day period across two data center regions.

- **22% reduction in average repair time** — measured from initial triage to server back online
- **31% increase in repair accuracy** — measured by reduction in tickets reopened for the same failure
- **7 → 1 ticketing systems** consolidated into a single unified platform

Beyond the numbers: teams moved from a reactive posture to a more proactive one. With trend data visible across the fleet for the first time, the operations team could now identify systemic issues (like a DIMM manufacturing defect) in weeks rather than the years it had previously taken.

---

## Reflection

**What to say:**

A few things I'd do differently:

**Test earlier in context.** The on-site pivot was the right call, but it cost us several weeks. We should have done contextual observation earlier — before committing to the global view direction. The lesson: don't prototype in isolation when the work environment is as specific as a data center floor.

**Designing for a distributed team is its own design problem.** Getting alignment across Dublin, Berlin, and San Francisco required more rigor than I anticipated. I started running short weekly design syncs specifically with the Dublin team — 30 minutes, just to maintain shared context. It became one of the most valuable rituals of the project.

**Engineers were my best design partners.** Involving engineering early — in ideation, not just handoff — changed the quality of what we shipped. When engineers understand why a decision was made, they advocate for it when tradeoffs arise. And they often surface implementation ideas that make the design better, not just cheaper to build.

---

## Likely Interview Questions & Suggested Answers

**"What was the hardest design challenge?"**
Action Plans. The core tension was: structured enough to be machine-readable and trackable, flexible enough to handle the genuine complexity of real repairs. The breakthrough was the branching tree model — but getting there required 4 iteration cycles and multiple rounds of usability testing.

**"How did you handle the pivot after on-site testing?"**
It was a lesson in humility. We'd designed for an idealized user — someone who wanted the full global picture. The real user wanted clarity about their own queue. Once we had that feedback, the pivot was straightforward: the three-level hierarchy (global/regional/site) gave every user type the right scope without losing the trend data leadership needed.

**"How did you collaborate with other designers?"**
Kaitlin (the other designer) owned an adjacent workstream — she came to the project before me and had done most of the early discovery. I made sure to absorb her research before running my own interviews, to avoid re-covering ground. We had a weekly design review together and used the three design principles as a shared language for evaluating each other's work.

**"How did you navigate working with such a large, distributed engineering team?"**
Short, regular syncs were critical. I also made it a priority to include engineers in ideation — not just share specs. That had two benefits: they understood the why, and they often pushed back in ways that made the design stronger.

**"What would you do differently?"**
I'd push for on-site testing much earlier. The global view direction cost us time we didn't have. Contextual research — being in the environment where the work actually happens — would have surfaced that mismatch faster than any prototype session in an office.
