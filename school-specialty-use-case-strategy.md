# School Specialty — Use Case Response Strategy

## Context

School Specialty is frustrated with Agentforce Employee Agent — it's not meeting expectations. They sent 10 analytics-heavy use cases around quoting behavior, conversion, pricing, and pipeline health. The correct read: most of these are **Tableau + Tableau Agent + Tableau Next** use cases, not Agentforce Employee Agent use cases. But there's a $2M attrition protection at stake, and we need to:

1. Validate which use cases belong in Tableau (analytics) vs. Agentforce (action)
2. Identify Employee Agent use cases that can genuinely succeed for School Specialty
3. Craft a response that reframes the conversation productively — showing them where AI agents add value vs. where dashboards/analytics add value

**School Specialty context:**
- K-12 education supplies distributor ($1.4B revenue)
- Customers = school district procurement offices
- Own: Service Cloud AF1 (550), Sales Cloud AF1 (200), CPQ Plus (650), Data Cloud, MuleSoft Titanium, Tableau Plus (525 Viewer, 10 Creator, 12 Explorer), full Marketing Cloud stack
- Busy season: March–August (budget cycles + back-to-school)
- Already own Tableau — can leverage it immediately for the analytics use cases

---

## The Framework: Analytics vs. Action

**Tableau answers:** "What happened? What's the pattern? Where should we look?"

**Agentforce answers:** "What should I do right now? Help me take action on what the data is telling me."

They complement each other. The frustration likely stems from trying to force analytics questions into a conversational agent — that's a mismatch.

---

## Use Case Classification

### Primarily Tableau + Tableau Agent (7 of 10)

These are analytics/visualization use cases. The right home is Tableau dashboards with Tableau Agent for natural language Q&A:

| # | Use Case | Why Tableau | Tableau Agent Example |
|---|---|---|---|
| 1 | Quote volume & coverage | Spatial/temporal analysis across territories, reps, accounts, products — classic dashboard | "Show me territories with zero quote activity this month" |
| 2 | Quote-to-order conversion | Win rate segmented by multiple dimensions — pivot tables, trend lines, filters | "What's our win rate by product category in the Midwest this quarter vs. last?" |
| 3 | Price realization & discounting | Distribution analysis, correlation charts, guardrail visualization | "Show me quotes where discount exceeded 20% and we still lost" |
| 4 | Margin profile by quote | Line-level margin calculation, scatter plots, profitability segmentation | "Which won quotes had negative margin last month?" |
| 5 | Product mix & demand shifts | Association analysis, trending, cross-sell gap identification | "What's the attach rate for art supplies on orders over $5K?" |
| 6 | Quote cycle time | Funnel timing, bottleneck identification, SLA tracking | "Where are quotes getting stuck longest this quarter?" |
| 10 | Pipeline linkage & attribution | Data quality metrics, hierarchy validation, coverage % | "What % of quotes from April are missing opportunity linkage?" |

### Hybrid: Tableau for Insight + Agentforce for Action (3 of 10)

These have a clear analytics component AND a clear "what do I do about it" component where an Employee Agent shines:

| # | Use Case | Tableau Role | Employee Agent Role |
|---|---|---|---|
| 7 | Competitive signals | Dashboard: competitor frequency, pricing pressure trends | Agent: "You're quoting ABC District who we lost to Staples last quarter on price — here's what happened and suggested positioning" |
| 8 | Lost-quote reasons & objections | Dashboard: loss reason distribution, trends by segment | Agent: "3 quotes lost on lead time this week in your territory — here's the pattern and suggested countermeasure" |
| 9 | Customer/site health indicators | Dashboard: risk scores, reactivation pipeline | Agent: "District XYZ has quoted 4 times in 60 days with 0 orders — recommend outreach. Here's their history." |

---

## Employee Agent Use Cases (What to Propose)

These are the Agentforce use cases that map to School Specialty's world and could succeed. They draw FROM the data insights above but deliver them as proactive, actionable nudges to reps and managers.

### Use Case A: Quote Intelligence Assistant

**Persona:** Sales rep building/reviewing quotes in CPQ  
**Trigger:** Rep asks the agent or agent proactively surfaces during quote creation  
**What it does:**
- "Accounts like this one (K-8 district, 500 students, Midwest) typically also order [science kits, PE equipment] — consider adding"
- "This account ordered $180K last year but has only been quoted $40K this cycle — potential gap"
- "Your discount on this line exceeds the guardrail for this product category — quotes at this discount level have a 30% lower win rate"
- "Similar quotes that converted had follow-up within 7 days — this quote is at day 12"

**Why it works:** Directly supports use cases 3, 5, 6, and 9 — but as in-context action, not retrospective reporting.

---

### Use Case B: Territory Health Briefing Agent

**Persona:** Sales manager or rep starting their day/week  
**Trigger:** Rep asks "brief me on my territory" or scheduled daily digest  
**What it does:**
- "You have 3 accounts that quoted repeatedly but haven't ordered — here they are with last contact dates"
- "Quote volume in your NE territory is down 20% vs. same period last year — 4 accounts haven't been quoted yet"
- "2 quotes are pending >14 days with no customer response — suggest follow-up"
- "You won 5/7 quotes last week — the 2 losses were both on price for furniture. Competitor XYZ was referenced."

**Why it works:** Synthesizes use cases 1, 2, 7, 8, 9 into a daily actionable briefing rather than requiring the rep to go read a dashboard.

---

### Use Case C: Win/Loss Debrief Agent

**Persona:** Sales rep or manager after a quote outcome is recorded  
**Trigger:** Quote status changes to Won or Lost  
**What it does:**
- Summarizes the deal context (products, discount level, cycle time, customer segment)
- For losses: pulls the structured loss reason, finds similar recent losses, suggests pattern ("This is your 3rd loss to incumbent this month in K-12 districts >1000 students")
- For wins: highlights what worked ("This quote converted 2 days after follow-up, discount was below guardrail, attached 3 product categories")
- Suggests next action: "Consider adjusting approach for large districts — competitive positioning doc attached"

**Why it works:** Makes use cases 7 and 8 actionable at the moment of truth rather than in a monthly review.

---

### Use Case D: Stale Quote Nudge Agent (Recommended Starting Point)

**Persona:** Sales rep  
**Trigger:** Proactive — fires when quotes exceed expected cycle time  
**What it does:**
- "Quote #4521 for Lincoln County Schools has been pending 18 days — avg cycle time for this segment is 9 days"
- "Suggested action: call [contact name]. Last interaction was [date]."
- "Similar stale quotes that eventually converted had a phone follow-up within 48 hours of this stage"

**Why it works:** Directly supports use case 6 but as a push notification, not a report.

**Why start here:** Narrowest scope, clearest trigger, easiest to measure success (did stale quotes convert faster after nudges started?). Rebuilds trust in Agentforce with a quick, tangible win.

---

## Response Structure

### 1. Acknowledge and Validate
"These are excellent use cases. You clearly understand the analytics you need to run your commercial org. Let's sort them into the right platform so each one gets built correctly."

### 2. Reframe the Two Layers
- **Layer 1 — Analytics (Tableau):** "What's happening in my business?" → Dashboards, Tableau Agent for NL queries, Tableau Next for predictive
- **Layer 2 — Action (Agentforce):** "What should I do about it right now?" → Employee Agent delivering contextual nudges, briefings, and recommendations to reps in their flow of work

### 3. Show the Map
Present the classification table — 7 pure Tableau, 3 hybrid.

### 4. Propose the Employee Agent Use Cases
Present Use Cases A-D. Emphasize:
- These CONSUME the same data the Tableau dashboards produce
- They're complementary, not competitive
- An Employee Agent that says "your quote is stale, here's why and what to do" is more valuable than a dashboard the rep has to remember to check

### 5. Recommended Starting Point
Given the frustration, recommend **one** high-confidence, narrow use case to rebuild trust:
- **Use Case D (Stale Quote Nudge)** is the easiest win
- Then layer on Use Case A (Quote Intelligence) once they see value

### 6. Address the Frustration Directly
"The frustration you're experiencing is likely because the current Employee Agent was asked to answer analytics questions that require cross-referencing large datasets — that's what Tableau is for. When we redirect the agent toward contextual, action-oriented prompts that fire at the right moment, you'll see dramatically better results."

---

## Onsite Agenda (May 13-15)

**Day 1 (May 13):**
- Understand current Employee Agent implementation — what's broken, what were they expecting
- Review their data model (CPQ quotes, orders, the fields that feed these use cases)
- Align on the Tableau vs. Agentforce framework

**Day 2 (May 14):**
- Design session: pick 1-2 Employee Agent use cases from above
- Map the data requirements (what fields, what objects, what prompts)
- Build a working prototype of Use Case D (Stale Quote Nudge) — it's narrow enough to demo in a day

**Day 3 (May 15):**
- Demo the prototype to CIO/CFO
- Lay out the roadmap: Tableau for analytics layer, Employee Agent for action layer
- Get alignment on next steps / success criteria

---

## Key Talking Points

- "You don't have an Agentforce problem — you have a use case alignment problem. The analytics questions you listed are brilliant, and they belong in Tableau. Agentforce's job is to take those insights and push them to reps at the right moment."
- "You already own Tableau Plus with 525 viewers — your entire sales org can access these dashboards today. Tableau Agent lets them query in natural language without learning filters."
- "The Employee Agent becomes 10x more valuable when it's sitting on top of solid analytics. Let's build the foundation (Tableau dashboards) and the action layer (Agent) in parallel."
- "Let's start with one agent use case that's impossible to do in a dashboard — proactive stale quote alerts. If a rep has to go look at a dashboard, they won't. If the agent tells them 'your quote is dying, here's what to do,' that's a different value prop."
