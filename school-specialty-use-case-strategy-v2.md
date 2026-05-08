# School Specialty — Use Case Strategy & Engagement Plan

## Context

School Specialty submitted 21 use cases spanning analytics, product matching, bid automation, and service enablement. This document classifies each use case into the right technology platform, identifies what we'll work on together, and provides a clear path forward.

**The core insight:** The initial Employee Agent frustration stemmed from asking an agent to answer analytics questions — that's a platform mismatch, not a product failure. With the expanded use case list, we now have a clear separation between analytics work (Tableau) and action/task-completion work (Agentforce Employee Agent).

**School Specialty context:**
- K-12 education supplies distributor ($1.4B revenue)
- Customers = school district procurement offices
- Busy season: March–August (budget cycles + back-to-school)
- SI Partner: Thunder (implementation partner for agent use cases)

**Current Salesforce stack:**
| Product | Licenses/Tier |
|---|---|
| Service Cloud + Agentforce | AF1 (550) |
| Sales Cloud + Agentforce | AF1 (200) |
| CPQ Plus | 650 |
| Data Cloud | Yes |
| MuleSoft | Titanium |
| Tableau Plus | 525 Viewer, 10 Creator, 12 Explorer |
| Marketing Cloud | Full stack |

---

## The Framework: Where Each Use Case Lives

**Tableau + Tableau Agent** → "What happened? What's the pattern? Where should we look?"
- Aggregation, segmentation, trend analysis, portfolio-level visibility
- Interactive exploration, drill-down, filtering
- Natural language querying against dashboards

**Agentforce Employee Agent** → "What should I do right now? Help me complete this task."
- Single-task with clear completion criteria
- Reasoning over a handful of records to produce an answer or output
- Triggered at a moment (new bid, mid-quote, case creation)
- Generates something: a recommendation, a document, a match, a draft

**Hybrid** → Tableau surfaces the pattern, Employee Agent delivers the action to the right person at the right moment.

---

## Full Use Case Classification

### Tableau + Tableau Agent (Use Cases 1–6, 10)

These are analytics/visualization use cases. The right home is Tableau dashboards with Tableau Agent for natural language querying.

| # | Use Case | Type | Why Tableau | Tableau Agent Example |
|---|---|---|---|---|
| 1 | Quote Volume & Coverage | BI | Spatial/temporal analysis across territories, reps, accounts — classic dashboard | "Show me territories with zero quote activity this month" |
| 2 | Quote-to-Order Conversion | BI | Win rate segmented by multiple dimensions — trend lines, quarter-over-quarter | "What's our win rate by product category in the Midwest vs. last quarter?" |
| 3 | Price Realization & Discounting | BI | Distribution analysis, guardrail visualization, discount vs. outcome correlation | "Show me quotes where discount exceeded 20% and we still lost" |
| 4 | Margin Profile by Quote | BI | Line-level profitability, scatter plots, segment-level margin analysis | "Which won quotes had margin below target last month?" |
| 5 | Product Mix & Demand Shifts | BI | Association analysis, trending demand before orders land, attach gap identification | "What's the attach rate for art supplies on orders over $5K?" |
| 6 | Quote Cycle Time & Responsiveness | BI | Funnel timing, bottleneck identification, SLA tracking | "Where are quotes getting stuck longest this quarter?" |
| 10 | Pipeline Linkage & Attribution Quality | BI | Data quality metrics — hierarchy validation, linkage coverage % | "What % of quotes from April are missing opportunity linkage?" |

**Action required:** Build Tableau dashboards for these 7 use cases. Tableau Agent provides natural language access without requiring users to learn filters. 525 viewers means the entire sales org can access them immediately.

---

### Hybrid: Tableau for Insight + Employee Agent for Action (Use Cases 7, 8, 9, 13, 14)

These have both an analytics layer (portfolio-level patterns) and an action layer (individual rep needs a nudge at a specific moment).

| # | Use Case | Tableau Role | Employee Agent Role |
|---|---|---|---|
| 7 | Competitive Signals in Quotes | Dashboard: competitor frequency, pricing pressure trends, win/loss by competitor | Agent surfaces mid-quote: "This district went to Staples last quarter on price — here's the positioning playbook" |
| 8 | Lost-Quote Reasons & Objection Themes | Dashboard: loss reason distribution, trends by segment/category | Agent post-loss: "This is your 3rd furniture loss on lead time this month — pattern emerging, here's the countermeasure" |
| 9 | Customer/Site Health Leading Indicators | Dashboard: risk scoring, quoting-vs-ordering divergence, reactivation pipeline | Agent proactively: "Lincoln County has quoted 4x in 60 days with 0 orders. Last contact 22 days ago. Recommend outreach." |
| 13 | Bid Intake - RFP Scoring | Dashboard: historical win rates by bid characteristics, effort vs. outcome | Agent on new bid: "Score 72/100. High value, low win probability based on geography + competitor history. Recommend escalation." |
| 14 | Bid Tab - Competitor Analysis | Dashboard: lost bid patterns, competitor pricing trends, award rates | Agent triggers post-loss: request bid tab from customer, extract competitor data, populate fields automatically |

**Why both:** Managers need the dashboard for portfolio visibility. Individual reps need the agent to surface insights at the moment they matter — during quote creation, after a loss, on a stale account.

---

### Employee Agent — Best Fit Use Cases (Use Cases 11, 12, 15, 16, 17, 18)

**These are where Agentforce genuinely excels and Tableau has no role.** They require reasoning, retrieval, document comprehension, and generation — not aggregation or trend analysis.

| # | Use Case | What the Agent Does | Why Agent, Not Dashboard |
|---|---|---|---|
| **11** | Product Match - Furniture (LE) | Match requested items against multiple catalogs using attributes (size, color, finish, config, discontinued status). Return ranked alternatives with price/margin/lead time. | Reasoning task across complex attributes. No visualization needed — needs an answer. |
| **12** | Product Match - Supplies (EE) | Match requested supply items to catalog products and alternative brands. Simpler than furniture — many have direct equivalents. | Direct matching with substitution logic. Agent returns ranked results. |
| **15** | Bid Document Completion | Populate customer-provided bid forms (PDF, Excel) from Salesforce CPQ data. Identify missing fields. Write back. | Task automation + generation. Pure action, no analytics component. |
| **16** | Bid Summarization | Parse 5–10 bid documents/attachments. Extract scope, due dates, requirements, risks, exceptions, action items into a single summary. | Document comprehension. Classic LLM task — no dashboard can do this. |
| **17** | Product Spec Sheet | Populate spec sheet templates from Product2, supplier catalogs, and manufacturer data. Cite sources. | Retrieval + template population across multiple data sources. |
| **18** | Guided Service Response | Surface the right template, Knowledge article, and suggested response language based on case context. Agent assists, human decides. | In-flow task assistance. Low risk, high value, agent stays in advisory role. |

---

### Placeholder Use Cases (Pending Detail)

| # | Use Case | Status |
|---|---|---|
| 19 | Case Management IDP | No detail provided — awaiting requirements |
| 20 | Service Assistant | No detail provided — awaiting requirements |
| 21 | Maps Routing / Post-Task | No detail provided — awaiting requirements |

---

## What We're Working On

### Phase 1: Rebuild Trust (Immediate)

**Use Case 18 — Guided Service Response**

| | |
|---|---|
| **Why first** | Lowest risk. Agent-assist only (human stays in control). Doesn't require complex data integration — uses Knowledge + case data already in Salesforce. 550 Service Cloud users = massive reach. |
| **Measurable outcomes** | Average handle time reduction, template accuracy, new agent ramp time |
| **Effort** | Low — leverages OOTB Agentforce for Service capabilities |
| **Dependencies** | Knowledge articles mapped to case types, approved email/service templates configured |

---

### Phase 2: Sales Quick Win (Month 1)

**Stale Quote Nudge Agent**

| | |
|---|---|
| **Persona** | Sales rep |
| **Trigger** | Proactive — fires when quotes exceed expected cycle time for their segment |
| **What it does** | "Quote #4521 for Lincoln County Schools has been pending 18 days — avg for this segment is 9 days. Suggested action: call [contact]. Similar quotes that converted had phone follow-up within 48 hours of this stage." |
| **Why now** | Narrowest scope, clearest trigger, easiest to measure (did stale quotes convert faster?). Directly supports Use Case 6 but as a push notification, not a report. |
| **Dependencies** | Quote status timestamps, average cycle time by segment (can derive from historical data) |

---

### Phase 3: High Strategic Value (Month 2–3)

**Use Cases 15 & 16 — Bid Summarization + Document Completion**

| | |
|---|---|
| **Why these** | Attacks the largest time-consumer for the bid team. Immediate, measurable hours saved per bid. |
| **Leverages** | MuleSoft IDP (already owned via Titanium tier) for document parsing and write-back |
| **Use Case 16 flow** | Bid package uploaded → Agent parses all attachments → returns single summary with scope, deadlines, requirements, risks, action items |
| **Use Case 15 flow** | Quote finalized in CPQ → Agent maps CPQ fields to customer form fields → populates and returns completed document with gaps flagged |
| **Dependencies** | MuleSoft IDP configured for bid document formats, field mapping between CPQ and common customer forms |

---

### Phase 4: Highest Impact (Quarter 2)

**Use Cases 11 & 12 — Product Matching**

| | |
|---|---|
| **Why last** | Highest value but requires deepest data work — supplier catalogs, Epicor configurator integration, product attribute normalization |
| **Use Case 11 (Furniture)** | Most complex matching workload. AI weighs trade-offs across size, color, finish, fabric, grade, configuration, dimensions, brand, discontinued status, lead time. Returns ranked matches with price/margin/availability. |
| **Use Case 12 (Supplies)** | Simpler — many items have direct equivalents. Agent matches by SKU, brand, pack size, UOM with approved substitutes. |
| **Dependencies** | Salesforce Product2 enriched with attributes, supplier catalog integration, Epicor configurator connection via MuleSoft, substitution rules defined |

---

### Parallel Track: Tableau Dashboards (Use Cases 1–6, 10)

These run in parallel with the Employee Agent work. No dependency between the two tracks.

| Priority | Use Case | Rationale |
|---|---|---|
| 1 | Quote Volume & Coverage | Establishes baseline — easiest starting point |
| 2 | Pipeline Linkage & Attribution | Determines whether the rest of the insights can be trusted |
| 3 | Quote-to-Order Conversion | High executive value, uses data from priorities 1 & 2 |
| 4 | Margin Profile by Quote | Critical profitability visibility |
| 5 | Price Realization & Discounting | Requires pricing detail and guardrail definitions |
| 6 | Customer/Site Health (hybrid) | Mixes data from SF + Data 360 |
| 7 | Product Mix & Demand Shifts | Requires product hierarchy normalization |
| 8 | Quote Cycle Time | Requires reliable status timestamps |

---

## Implementation Ownership

| Track | Owner | Use Cases |
|---|---|---|
| Tableau dashboards + Tableau Agent | School Specialty internal / Tableau team | 1–6, 10 (analytics layer) |
| Employee Agent — Service | Thunder + Salesforce | 18 (Guided Service) |
| Employee Agent — Sales nudges | Thunder + Salesforce | Stale Quote Nudge, Territory Briefing |
| Employee Agent — Bid automation | Thunder | 15, 16, 17 (leveraging MuleSoft IDP) |
| Employee Agent — Product matching | Thunder | 11, 12 (requires catalog data work) |
| Hybrid (Tableau insight → Agent action) | Joint | 7, 8, 9, 13, 14 (Phase 3+) |

---

## Why the Original Employee Agent Wasn't Working

The first 10 use cases were all analytics questions:
- "Where is quoting activity happening?"
- "Which quotes become orders and why?"
- "Are discounts improving wins or eroding value?"

These require aggregating thousands of records, segmenting across multiple dimensions, and presenting interactive visualizations. An Employee Agent trying to answer these will either:
1. Return a text summary that's too shallow to act on
2. Try to produce tables that are worse than a dashboard
3. Hallucinate specifics when the question requires precise aggregation

**The fix isn't a better agent — it's the right tool for the right question.**

When the agent is asked action questions instead — "match this product," "summarize this bid," "what should I do about this stale quote" — it excels because those require reasoning over a small set of records to produce a specific output.

---

## Key Talking Points

- "You don't have an Agentforce problem — you have a use case alignment problem. The analytics questions you listed are excellent, and they belong in Tableau. Agentforce's job is to take those insights and push them to reps at the right moment."

- "You already own Tableau Plus with 525 viewers. Your entire sales org can access dashboards today. Tableau Agent lets them query in natural language without learning filters."

- "The Employee Agent becomes 10x more valuable when it's focused on task completion: match this product, summarize this bid, alert me when this quote is dying. Those are things no dashboard can do."

- "Let's start with one agent use case that's impossible to do in a dashboard — guided service response or stale quote alerts. Once you see value there, we layer on bid automation and product matching."

- "Tableau tells your managers 'we lost 12 bids to Staples this quarter on price.' The Employee Agent tells your rep mid-quote 'this district went to Staples last time — here's the winning playbook.' Same data, different moment, different value."

---

## Success Criteria

| Phase | Use Case | Metric | Target |
|---|---|---|---|
| 1 | Guided Service Response | Avg handle time | 15% reduction in first 60 days |
| 1 | Guided Service Response | New agent ramp time | 25% faster to full productivity |
| 2 | Stale Quote Nudge | Stale quote conversion rate | 10% improvement vs. control group |
| 3 | Bid Summarization | Hours per bid package review | 50% reduction (from ~4 hrs to ~2 hrs) |
| 3 | Bid Document Completion | Manual rekeying errors | 80% reduction |
| 4 | Product Match (Furniture) | Time to find substitute | 70% reduction (from 30 min to <10 min) |
| 4 | Product Match (Supplies) | Match accuracy | 90%+ first-suggestion accuracy |
