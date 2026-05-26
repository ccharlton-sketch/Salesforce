# DSR Handoff — Christian Charlton OOO 5/30 – 6/13

## Coverage Window
- **Out:** Friday 5/30
- **Back:** Monday 6/16
- **Escalation contact:** [TBD]

## Quick Reference

| DSR# | Account | Covering | Complexity | Use Case Focus | Key Date |
|------|---------|----------|-----------|----------------|----------|
| DS-1706318 | School Specialty | Reilly [confirm] | HIGH-TOUCH | Employee Agent + Service Agent | |
| DS-1684618 | Morningstar | Andrew | HIGH-TOUCH | Employee + Service + Engagement | DC migration |
| DS-1668949 | Smarsh | Craig | HIGH-TOUCH | Service Agent + Employee Agent | 6/9 MCP Client |
| DS-1684616 | Getty Images | Don/Reilly [confirm] | LOW-TOUCH | Service Agent - Service Assistant | Handoff call Thu |
| DS-1678704 | Internova | TBD (Samir leading) | LOW-TOUCH | Service Agent - Case Classification | Weekly calls |
| DS-1672810 | Fusion Academy | TBD | LOW-TOUCH | Service Agent - Recommendations | Call Thu |
| DS-1619827 | TriNet | TBD | LOW-TOUCH | Employee + Service + Engagement | AF Coworker demo |
| DS-1669809 | Huron Consulting | TBD | ADVISORY | Employee Agent - Knowledge | Demo 5/27, then wait |

---

## School Specialty
**DSR:** DS-1706318 | **Complexity:** HIGH-TOUCH | **Covering:** Reilly [TBD confirm]

### Background
Started as an Employee Agent use case but customer didn't know what they wanted — they expected "ChatGPT for Salesforce" and weren't getting it. Onsite visit turned things around when we showcased AF Coworker. COO did a 180 and they're now experimenting and going live with Coworker across Sales (day-to-day productivity). We've also identified strong Customer Service use cases.

### Current State
- **AF Coworker:** Rolling out to Sales team, actively experimenting
- **Service side:** Showed them Case Summarization and Close Summarization onsite — it worked, they need to continue building it out
- **Orville:** Their internal customer service agent that surfaces knowledge to reps — want to expand on it
- **Future:** Email Agent (they're primarily email-to-case)
- **Tableau:** Separate workstream pushing Tableau Agents and Pulse — customer has a plan in place for that

### Key Contacts
- Customer: **Neal Gelderman** — SF Developer/Architect, strong in Salesforce, critical to getting product live
- Internal: [AE TBD], Tableau team has their own workstream

### Next Steps (during coverage window)
- [ ] Be a point of contact if there's frustration on AF Coworker rollout — field questions, make sure it goes smoothly
- [ ] Continue pushing the Customer Service front: Case Summarization + Close Summarization (they know how to build it from onsite demo, keep momentum up)
- [ ] Tableau team continues independently pushing Tableau Agents/Pulse
- [ ] Keep overall momentum — don't let it stall

### What to Avoid / Watch Out For
- Do NOT get into discussions about improving/customizing AF Coworker — it is OOTB and cannot be fully adjusted. If they ask, set expectations.
- Customer previously lost faith in AF — the relationship is recovered but fragile. Keep pushing forward, don't dwell on past frustrations.
- Neal is the key person — keep him engaged and unblocked.

---

## Morningstar
**DSR:** DS-1684618 | **Complexity:** HIGH-TOUCH | **Covering:** Andrew

### Background
Multi-agent engagement — customer has Web Agent, Voice Agent, SDR Agent, and Employee Agent all live. However, everything is currently halted due to migration from Data Cloud One to standard Data Cloud. The DC migration means their AF Data Libraries and retrievers need to be rebuilt/reconnected.

### Current State
- **DC Migration:** Moving from DC1 → standard Data Cloud. This is the blocker for all other work.
- **Agents (halted):** Web, Voice, SDR, Employee all previously live but paused until DC migration completes
- **Employee Agent frustration:** Customer doesn't find it truly agentic and questions its usefulness
- **AF Coworker:** Planning to showcase to the team as a better path for the employee use case

### Key Contacts
- Customer: **Sabrina, Jennifer, Charles** (admin team)
- Internal: **Lauren Murphy** (Signature Success) — work through her for support coordination

### Next Steps (during coverage window)
- [ ] Support the DC1 → standard Data Cloud migration — help get AF Data Libraries and retrievers back up and running
- [ ] Showcase AF Coworker to the team and get that up and running
- [ ] Work through Lauren Murphy (Signature Success) for coordination

### What to Avoid / Watch Out For
- Nothing major — they are a nice team to work with
- Be aware of employee agent frustration (customer sees it as not truly agentic) — AF Coworker demo should help address this

---

## Smarsh
**DSR:** DS-1668949 | **Complexity:** HIGH-TOUCH | **Covering:** Craig

### Background
Two agents in play: **Emmy** (internal/employee agent) and **Archy** (customer-facing service agent). Archy is being moved to New Agent Script to take advantage of AF MCP Client (releasing June 9th). Archy will get an additional use case: creating cases + pinging Document360 via MCP once AF MCP Client is live.

### Current State
- **Archy:** Migrating from Legacy Builder → New Agent Script. New use cases queued (case creation, Document360 MCP integration)
- **Emmy:** Internal agent, currently working but AF Coworker may change how this is done
- **AF Coworker:** Being shown this week to see if it fits for their Sales team
- **Observability:** Ongoing discussions about improving observability — needs to be kicked off to the observability team

### Key Contacts
- Customer: **Matt** (SF Developer), **Janine**, **Amy**
- Internal: **Tina Feintuck** (Core SE team) — work through her to see where they need assistance

### Next Steps (during coverage window)
- [ ] Assist in migrating Archy from Legacy Builder to New Agent Script (could hand back to Christian when he returns if not complete)
- [ ] AF Coworker demo this week — gauge Sales team interest
- [ ] **June 9th:** Work with their team on AF MCP Client release for Document360 use case
- [ ] Kick off observability improvement conversation to the observability team
- [ ] Coordinate with Tina Feintuck on where Core SE team needs support

### What to Avoid / Watch Out For
- Avoid deep "Emmy" conversations — AF Coworker could change how Emmy is done entirely, and that discussion is better had when Christian is back
- The New Agent Script migration is the priority path for Archy — don't get sidetracked on Legacy Builder improvements

---

## Getty Images
**DSR:** DS-1684616 | **Complexity:** LOW-TOUCH | **Covering:** Don/Reilly [TBD confirm]

### Background
Lost the FIN.ai compete. Pivoted to internal customer support assistance with Agentforce for Service. Full use case of what was sold is not yet fully known — we're assisting in a low-touch motion (once-a-week call) to go over how they're working and what they've discovered.

### Current State
- **Service Assistant** is the driver — POC beginning 5/28
- Service Assistant is NOT currently on New Agent Script, which will be a problem — approach the situation best we can
- Low-touch cadence: weekly call to check progress and answer questions

### Key Contacts
- Customer: **Shawn Dowd** — all in a shared Slack channel
- Internal: **Don, Reilly** (handoff call Thursday this week)

### Next Steps (during coverage window)
- [ ] Thursday this week: handoff call with Don/Reilly to go over the motion
- [ ] Continue weekly calls with customer — check how they're progressing, field questions
- [ ] Navigate Service Assistant not being on Script — help them work within current constraints

### What to Avoid / Watch Out For
- Service Assistant is not on New Agent Script — be upfront about limitations but don't block progress
- Don't dwell on the FIN.ai loss — focus forward on the Service Assistant path
- Scope is low-touch: once-a-week support, not a heavy build engagement

---

## Internova
**DSR:** DS-1678704 | **Complexity:** LOW-TOUCH | **Covering:** TBD

### Background
Working through Case Classification. Originally built a generative model but it was only getting it right ~60% of the time — pivoted to a predictive model approach (Einstein Case Classification). Samir (Core SE) is leading this workstream. Weekly call cadence with customer's SF Developers and architects.

### Current State
- **Predictive Case Classification** in progress — Samir leading
- Generative model was attempted first but accuracy was insufficient (60%)
- Weekly calls ongoing with customer technical team

### Key Contacts
- Customer: SF Developers and architects (on the weekly call)
- Internal: **Samir** (Core SE) — he is leading this workstream

### Next Steps (during coverage window)
- [ ] Continue weekly calls — let Samir lead on Einstein Case Classification
- [ ] If predictive model hits a wall and they need to pivot back to generative case classification, support that pivot
- [ ] Be available as backup — Samir is driving

### What to Avoid / Watch Out For
- Let Samir lead — don't step on the Case Classification work unless asked
- Don't push generative approach unless predictive model clearly isn't working — the pivot was intentional

---

## Fusion Academy
**DSR:** DS-1672810 | **Complexity:** LOW-TOUCH | **Covering:** TBD

### Background
Multi-agent engagement. The Service Agent for Web Lead Gen is close to live but has been stalled (handled by Coastal Cloud, ran into blocks). Current focus is on the Teacher/Employee side: Resource Recommendations and Course Recommendation. Customer provided 4 large PDF documents to use for recommending resources and courses to teachers based on student information in Salesforce.

### Current State
- **Course Recommendation:** Working through this in their org — call Thursday this week
- **Resource Recommendation:** Christian trialing in his sandbox. PDFs ingested into both a Data Library and a Salesforce object. Querying student info against it to provide suggestions.
- **Service Agent (Web Lead Gen):** Close to live but stalled — Coastal Cloud handling, many blockers
- **AF Coworker:** Planning to show

### Key Contacts
- Customer: **Maia** (main POC)
- Internal: **Trisha Ponce** (Core SE) — work through her for the connection

### Next Steps (during coverage window)
- [ ] Thursday call this week: work through Course Recommendation track in their org
- [ ] Continue validating whether Resource Recommendation is actually beneficial to users — work with Maia and team
- [ ] Christian has a sandbox prototype (PDFs in Data Library + SF object, querying student info) — covering person can reference but may hand back to Christian when he returns
- [ ] Show AF Coworker when appropriate

### What to Avoid / Watch Out For
- **Avoid the Service Agent / Web Lead Gen** — that's Coastal Cloud's lane and has hit many blocks. Don't get pulled into it.
- The sandbox prototype work (Resource Recommendation) may be best to pause until Christian returns if the covering person isn't familiar with the setup

---

## TriNet
**DSR:** DS-1619827 | **Complexity:** LOW-TOUCH | **Covering:** TBD

### Background
Customer went live with SDR Agent and Employee Agent but not getting the usage they expected. Consumption has significantly dropped. This is now a re-engagement play — showing them AF Coworker because that's what their users actually want. Long-term goal is to drive customer service use cases but TBD if possible.

### Current State
- **SDR Agent + Employee Agent:** Live but underutilized — consumption dropped
- **Reverse demo:** Scheduled to have customer show us their current agents
- **AF Coworker:** Planning to use the reverse demo as a chance to showcase Coworker
- **Future:** Want to drive customer service use cases eventually

### Key Contacts
- Customer: **Mathangi, Rishabh** (SF Developers)
- Internal: **Trisha Ponce** (Core SE) — work through her

### Next Steps (during coverage window)
- [ ] Showcase AF Coworker — help them see the value in Agentforce
- [ ] Reverse demo on their current agents — understand what they built and where usage fell off
- [ ] If AF Coworker resonates, help them get started
- [ ] Customer service use cases are a future goal — don't push yet, see if it's possible

### What to Avoid / Watch Out For
- Don't push too hard on the consumption drop — focus on showing them something better (AF Coworker) rather than diagnosing why the old agents aren't being used
- Customer service use cases are aspirational — don't commit to scope on that during coverage

---

## Huron Consulting
**DSR:** DS-1669809 | **Complexity:** ADVISORY | **Covering:** TBD

### Background
Engaging on the Employee Agent side. Customer wants a ChatGPT-like experience in their Microsoft Teams instance for Salesforce data. AF Coworker is the answer — it fits exactly what they're looking for.

### Current State
- **AF Coworker demo:** Showcasing to their developers tomorrow (5/27)
- **Goal:** Get customer to have AF Coworker live in their org to actively test out
- May have more updates after the demo but next steps depend on customer testing it

### Key Contacts
- Customer: Developers (on the demo call)
- Internal: **Daniel Kopp** (Core AE) — work through him to schedule calls

### Next Steps (during coverage window)
- [ ] After the 5/27 demo: let the customer get AF Coworker live in their org and test
- [ ] Be available if they have questions during testing — but don't over-engage
- [ ] Work through Daniel Kopp if calls need to be scheduled

### What to Avoid / Watch Out For
- Don't rush next steps — customer needs to have it live and actively test before we plan further work
- This is ADVISORY level — light touch, be available but don't over-drive
