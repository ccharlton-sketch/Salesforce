# Prospecting Agent — Customer Install Project Plan

**Created:** 2026-07-01  
**Updated:** 2026-07-01  
**Source:** Internal Slack research (#help-sell-ai, FDE recordings, Prospecting Agent Hub canvas)  
**Author:** Christian Charlton  

---

## Executive Summary

This playbook covers a structured engagement to install and activate Prospecting Agent in a customer's production org. The goal is to leave the customer with a working agent generating prioritized prospect lists within the first week post-activation.

**Product Context:**
- Prospecting Agent (codename "Hunter") GA'd March 30, 2026 as part of the Agentforce for Sales add-on SKU
- Built on NGA/AgentScript architecture (originated from Bluebirds acquisition)
- Replaces Prospecting Center, which hit End-of-Sale on April 20, 2026
- Uses a custom object: `Prospecting Agent Recommended Target` (not the standard Prospect object)
- Batch processing cadence (currently weekly; real-time signal-based triggers on roadmap)

---

## Phase 1: Pre-Onsite (1–2 Weeks Before)

### 1.1 Confirm Licensing & Entitlements

| Checkpoint | Action | Owner |
|-----------|--------|-------|
| Agentforce for Sales add-on (Sales EE/UE) OR Agentforce 1 Edition | Verify with AE that SKU is provisioned | AE |
| Flex Credits allocated | Confirm credit pool exists and quantity is sufficient (~1,000 credits/account assuming 3 prospects + ~20 credits per action) | AE/RevOps |
| `SalesAgenticProspectingAddOn` PSL visible | Check Setup → Permission Set Licenses — if missing, the Prospecting toggle won't appear in Salesforce Go | SE + Admin |
| Flex Credit Provisioning SKU (not legacy) | Must be `Flex Credit Provisioning - A1E` (200004881) + `Data 360 Provisioning - A1E` (200004889) — legacy `Data Services Provisioning` SKUs will block permission sets from appearing | AE |
| Data Cloud Provisioning | Ensure $0 Foundations SKU is in place (quoting requirement — agent doesn't query DC directly) | AE |
| ZoomInfo status | Determine: existing contract (OAuth in) or net-new (2,000 free credits) | SE + Customer |

**Critical provisioning note:** Prospecting Agent ONLY works off Flex Credits. If the customer has legacy Einstein Requests or old `Data Services Provisioning - A1E` SKU, they must swap to Flex Credits before permission sets appear. The swap requires:
- `Data Services Provisioning - A1E` → `Flex Credit Provisioning - A1E` (200004881)
- `Data Cloud Provisioning - A1E` → `Data 360 Provisioning - A1E` (200004889)
- Keep `Flex - Entitlements - A1E` (not a legacy SKU)

Einstein Requests and Flex Credits **cannot co-exist** in the same org.

**Per-user vs. per-org licensing:** Any user deploying Prospecting Agent needs an Agentforce for Sales license (add-on or A1E). One user CAN configure it on behalf of the org, but commercially the intent is per-user licensing for anyone receiving prospects. The Agentforce for Sales add-on is $125 PUPM and includes: Prompt Builder, SDR & Sales Coach, Einstein Activity Capture, Sales Engagement, 200K Einstein Requests (for Prompt Builder), and Standard/Custom Actions.

**Foundations provides free starter credits:** The `Salesforce Foundations - Entitlements - Flex Credits` SKU (200004650) delivers 200,000 Flex Credits as a freemium seed. Customers can start here and buy usage packs when they scale.

### 1.2 Discovery & Scoping Call (30–45 min)

Run discovery to shape the agent configuration BEFORE arriving onsite. You cannot configure on the fly without this.

**Questions to answer:**

1. **Who is the outbound team?**
   - How many SDRs/BDRs will use this?
   - Who manages them? (they get Manager perm set)
   - Is there a RevOps/SalesOps admin who will own ongoing config?

2. **What does their ICP look like?**
   - Which accounts should the agent research? (Top accounts? Specific segments? Industries?)
   - What makes a good prospect? (Title, seniority, department, signals?)
   - Are there binary qualifiers vs. nice-to-haves?
   - Any disqualifiers? (competitors, existing customers, specific geographies to exclude)

3. **What data sources do they have?**
   - ZoomInfo (existing contract? which edition?)
   - Gong (call recordings — how far back?)
   - EAC/Einstein Activity Capture (email sync enabled?)
   - Intent data vendors (Demandbase, 6Sense, G2, Bombora)?
   - LinkedIn Sales Navigator? (note: no automated integration possible)
   - Any proprietary/niche data? (may need custom Agentforce Action)

4. **What does their current process look like?**
   - How do reps build lists today? (manual research, Clay, spreadsheets?)
   - Do they use Sales Engagement tools? (Outreach, Salesloft, Gong Engage?)
   - How are leads routed? (territory-based, round-robin, named accounts?)

5. **What does success look like?**
   - Meetings booked per rep per week?
   - Pipeline generated?
   - Time saved on research?
   - Define 30/60/90 day metrics

### 1.3 Pre-Work (Customer Homework)

Send the customer a prep checklist 1 week before onsite:

```
CUSTOMER PRE-WORK CHECKLIST
============================

□ 1. Identify 1-2 admin users who will configure the agent
     (They need "Sales Agentic Prospecting Manager" perm set)

□ 2. Build an Account Report in Salesforce
     - Must include an Account field
     - Examples: Top 200 Target Accounts, Closed/Lost Last 12mo,
       Accounts by Industry/Segment
     - This is the agent's "source" — it researches these accounts

□ 3. Prepare ZoomInfo credentials
     - If existing customer: have OAuth login ready
     - If new: we'll create account onsite (2,000 free credits)

□ 4. If using Gong: Confirm admin access to Gong
     - We'll need to install/update the Gong managed package
     - We'll enable "Include Transcript" in Gong settings
     - Decide how far back to backfill transcripts (recommend 1-2 years)

□ 5. Define your ICP in writing (even rough bullet points)
     - What makes a good account to pursue?
     - What titles/roles are you targeting?
     - What signals indicate buying intent?

□ 6. Identify 3-5 reps who will be pilot users
     - They need Salesforce licenses (Sales EE/UE)
     - They'll be the first to receive agent-generated prospects

□ 7. Confirm Salesforce Foundations setup is complete
     - Setup → Salesforce Foundations page (if not already done)
```

### 1.4 SE Pre-Work (Your Homework)

| Task | Details |
|------|---------|
| Prepare ICP draft | Based on discovery, draft natural language ICP criteria to paste into Agent Builder |
| Review account report | Ask customer to share screenshot — confirm it has Account field |
| Check org version | Confirm they're on a release that supports Prospecting (v260+). Non-admin bug fix in v262.5 |
| Verify PSL provisioning | Ask admin to check Setup → Permission Set Licenses for `SalesAgenticProspectingAddOn`. If missing, engage AE to fix before onsite |
| Review known bugs | Non-admin user access bug (W-22312870) — fixed in v262.5. Also: W-22498128 (agent dropdown empty for non-admins), targeted fix v262.11 |
| Confirm no Einstein Requests | If org has legacy Einstein Requests, Flex Credits cannot co-exist — must swap SKUs before onsite |
| Prepare demo story | Have a "before/after" narrative: what reps do today vs. what changes |
| Book follow-up | Schedule a 30-min check-in for 1 week post-onsite before you leave planning |

---

## Phase 2: Onsite (Day-Of)

### Recommended Agenda (Half-Day: 3–4 Hours)

```
TIME        ACTIVITY                                      WHO IN ROOM
─────────────────────────────────────────────────────────────────────
0:00-0:15   Kickoff & Recap Goals                        SE + Admin + Sales Leader
0:15-0:45   Live Setup: Permissions & Navigation         SE + Admin
0:45-1:15   Configure Agent: ICP & Report Source         SE + Admin + Sales Leader
1:15-1:30   ── BREAK ──
1:30-2:00   ZoomInfo Integration                         SE + Admin
2:00-2:30   Gong/ECI Setup (if applicable)               SE + Admin
2:30-3:00   Activate Agent & First Run                   SE + Admin
3:00-3:30   Rep Walkthrough & Adoption Coaching          SE + Admin + Pilot Reps
3:30-3:45   Next Steps & Success Metrics                 All
```

### 2.1 Live Setup: Permissions & Navigation (30 min)

**Step-by-step with the admin:**

1. **Salesforce Go** → Setup → Search "Agentforce for Sales" → Turn on Agentforce → Toggle ON "Prospecting"
   - If the Prospecting toggle doesn't appear: `SalesAgenticProspectingAddOn` PSL is missing from the org
   - If toggle shows "Purchase Required": wrong SKU or legacy provisioning — STOP and engage AE

2. **Permission Sets** (Setup → Permission Sets):
   - Assign `Sales Agentic Prospecting` → all pilot users + managers
   - Assign `Sales Agentic Prospecting Manager` → admin(s) who configure
   - Assign `ZoomInfo Prospecting Access` → user who will OAuth into ZoomInfo
   - **Note:** Permission sets must be assigned via Setup → Permission Sets, NOT via Setup → Lead Nurturing → Prospecting → Manage Users (different assignment paths can cause issues)

3. **Add Prospecting Tab**:
   - Setup → App Manager → Sales app → Edit → Navigation Items → Add "Prospecting" → Save
   - Verify: Navigate to the Sales app — "Prospects" tab should appear
   - If not visible: append `/lightning/page/prospecting` to the org URL

4. **Verify Access**:
   - Log in as a non-admin pilot rep — confirm they can see the Prospecting tab
   - If blocked with "You don't have access to this record": known bug W-22312870 (fixed v262.5)
   - **Workaround:** Assign `Sales Agentic Prospecting Manager` perm set to affected users
   - If agent dropdown is empty for standard users: bug W-22498128 (targeted fix v262.11) — may also need `Manage Bots` system permission via Profile

### 2.2 Configure Agent: ICP & Report Source (30 min)

**This is the most critical step. Get the sales leader in the room.**

1. **Navigate to Agentforce Builder** (App Launcher → "Agentforce Builder") OR use the Prospecting tab → New Agent

2. **Select Prospecting Agent template** → Name it (e.g., "Outbound Prospecting Agent - Enterprise")

3. **Add Report Source(s)**:
   - Search for the Account report the customer prepared
   - Must contain an Account field
   - Can add multiple sources (e.g., Top Accounts + Closed/Lost)

4. **Define ICP in natural language**:
   - This is where you paste the criteria developed in discovery
   - Use clear, binary qualifiers + nice-to-haves
   - Example:

   ```
   QUALIFY accounts that meet ALL of these criteria:
   - Company has 500+ employees
   - Industry is Technology, Financial Services, or Healthcare
   - Headquartered in United States or Canada
   - Has not been a customer in the last 6 months

   PRIORITIZE accounts that also show these signals:
   - Recent leadership change (new CRO, VP Sales, or CMO)
   - Active job postings for sales or marketing roles
   - Company mentioned in recent news for growth or funding
   - Previous closed/lost opportunity in the last 18 months

   TARGET these personas:
   - VP Sales, VP Marketing, VP Revenue Operations
   - Director of Sales Development, Director of Demand Gen
   - CRO, CMO
   ```

5. **Configure prospect distribution**:
   - Assign reps who will receive prospects
   - Each rep needs the `Sales Agentic Prospecting` perm set
   - Optimal: 50–150 prospects per assigned owner

> **CRITICAL WARNING:** Once you define ICP in Agent Builder, you CANNOT revert to the front-end guided UI. Confirm with the customer they're comfortable with this before proceeding. Start with the guided flow if unsure.

### 2.3 ZoomInfo Integration (15–30 min)

1. In the Prospecting Agent Builder → **Tools** page → Click **Setup**
2. Enter ZoomInfo credentials (OAuth for existing customers, or create new account)
3. Confirm connection is successful
4. Discuss credit consumption: ~1 credit per contact lookup

**If customer has no ZoomInfo:**
- Sign up for free account → 2,000 free credits included with every Agentforce for Sales license
- Only ONE manager-level login needed (not per-rep) — the agent uses this single credential
- Credits are consumed per contact lookup (~1 credit per contact enrichment)
- After free credits exhaust, customer needs a commercial ZoomInfo relationship
- ZoomInfo is the ONLY supported enrichment provider at GA (no Clearbit, Apollo, etc.)

### 2.4 Gong/ECI Setup (if applicable) (30 min)

**Only if customer uses Gong for call recording. ECI is a HARD dependency — Prospecting Agent cannot use Gong transcripts without it.**

**How it works technically:**
- A nightly cron job queries `Gong__Gong_Call__c` custom objects (populated by Gong managed package)
- Batches of up to 200 calls are processed via an MQ handler
- ECI's VideoCall Creation API creates VideoCall records with metadata, participants, and related records
- ECI's Transcript Upload API ingests the transcript content
- Vendor type stored as "Gong" in VideoCall object with external IDs prefixed with `gong:` for deduplication
- Data Cloud is NOT required for Gong ingestion — transcripts are ingested natively into Salesforce core

**Step-by-step:**

1. **Install/Update Gong Managed Package**:
   - [AppExchange listing](https://appexchange.salesforce.com/appxListingDetail?listingId=a0N3A00000FeGgcUAF)
   - Gong Admin → CRM Settings → "Gong for Salesforce" tab → Enable "Include Transcript"

2. **Enable Einstein Conversation Insights (ECI)**:
   - Setup → Salesforce Go → Search "Einstein Conversation Insights" → Get Started → Turn On
   - Assign ECI licenses to pilot users (start small, expand after validation)
   - **Warning:** If Gong/ECI setup is not completed, the Prospecting setup page may show an infinite loading state

3. **Backfill Historical Transcripts**:
   - Submit request **directly to Gong** (not Salesforce) — contact.gong.io/hc/en-us/requests/new
   - Up to 2 years of historical transcripts can be imported (GA as of April 2026)
   - This gives the agent historical context for Closed/Lost resurrection plays
   - **Note: This takes days to process — won't be ready same-day**

### 2.5 Activate Agent & First Run (30 min)

1. Review all configuration one final time with the admin
2. **Activate the agent**
3. Monitor the first batch run — confirm it's processing accounts from the report
4. Review initial prospect recommendations as they appear
5. Show admin where to see agent activity and status

**Set expectations:**
- First meaningful results may take 24–48 hours (batch processing)
- Quality improves over time as reps provide accept/reject feedback
- Gong transcript backfill will enrich results when it completes (days later)

### 2.6 Rep Walkthrough & Adoption Coaching (30 min)

**Get pilot reps in the room for this section.**

Walk them through:

1. **Where to find prospects**: Prospects tab in the Sales app
2. **What they'll see**: Prioritized list with "Why This Account" and "Why This Prospect" context
3. **How to act**: Review → Accept or Reject → Engage (draft outreach or add to cadence)
4. **The feedback loop**: Accept/Reject teaches the agent — the more they do, the better it gets
5. **What NOT to expect**: This isn't a magic button. It eliminates research time, not selling effort.

**Coaching talking points for the sales leader:**
- Set a daily habit: "Check your prospect list first thing every morning"
- Goal: Work through 5–10 prospects per day (not 50 at once)
- Provide feedback (accept/reject) on EVERY recommendation — this trains the model
- Combine with their existing engagement tools (Outreach, Salesloft, etc.)

### 2.7 Close: Next Steps & Success Metrics (15 min)

Align on:

| Metric | Target | Timeframe |
|--------|--------|-----------|
| Agent active and generating prospects | Yes/No | Day 1 |
| Reps reviewing prospect lists daily | 80%+ of pilot | Week 1 |
| Accept/Reject feedback provided | 50+ per rep | Week 2 |
| First meeting booked from agent-sourced prospect | At least 1 | Week 2-3 |
| Pipeline influenced by Prospecting Agent | Trackable | Month 1 |
| Flex Credit consumption on track | Within budget | Month 1 |

**Schedule the follow-up:** Book a 30-min virtual check-in for 1 week post-onsite.

---

## Phase 3: Post-Onsite (Weeks 1–4)

### Week 1: Validate & Troubleshoot

| Action | Details |
|--------|---------|
| Check-in call (30 min) | Are reps seeing prospects? Any access issues? Quality feedback? |
| Review agent output | Look at first batch of recommendations — are they aligned with ICP? |
| Adjust ICP if needed | Common: criteria too narrow (no results) or too broad (low quality) |
| Confirm Gong backfill | If applicable — has the transcript backfill completed? |
| Monitor Flex Credits | Check consumption rate vs. budget |

**Common Week 1 Issues:**

| Problem | Root Cause | Fix |
|---------|-----------|-----|
| Reps can't see Prospecting tab | Permission set not assigned, or tab hidden in app | Assign `Sales Agentic Prospecting` PS + add tab to Sales app navigation |
| "You don't have access to this record" | Bug W-22312870 — `dbValueRequired="true"` on FK fields triggers aggressive access checks. Fixed v262.5 | Assign `Sales Agentic Prospecting Manager` PS as workaround |
| Agent dropdown empty for reps | Bug W-22498128 — access check on `getProspectingAgents()` too restrictive. Targeted v262.11 | Grant `Manage Bots` system permission via Profile |
| Standard users can't create agents | Bug W-23003467 — permission gap for agent creation | Assign Manager perm set |
| No prospects generated | Report source empty, ICP too restrictive, or agent not activated | Verify report has records, loosen ICP qualifiers, confirm agent is Active |
| Low-quality prospects | ICP too broad, no disqualifiers defined | Add specific disqualifiers; tighten persona targeting |
| ZoomInfo errors | OAuth expired, or credit allotment exhausted | Re-authenticate; check credit balance in ZoomInfo admin |
| Prospecting setup page loading forever | Gong/ECI setup incomplete (optional features causing UI hang) | Complete or skip Gong/ECI setup in Salesforce Go |
| Permission sets not appearing in org | Legacy SKU (`Data Services Provisioning - A1E`) blocking Flex Credit provisioning | Swap to `Flex Credit Provisioning - A1E` (200004881) — requires AE/Deal Desk |
| Deleting an agent deletes all prospects | Known data-loss bug — cascade delete on agent removes all corresponding prospects | Do NOT delete agents in production; deactivate instead |

### Week 2: Optimize & Expand

- Review accept/reject patterns — are reps finding value?
- Refine ICP based on what's being accepted vs. rejected
- Consider adding a second report source (e.g., Closed/Lost accounts)
- If Gong backfill is complete: validate that Closed/Lost resurrection insights are appearing
- Discuss: should we add more reps to the pilot?

### Week 3–4: Scale & Measure

- Expand to full SDR/BDR team if pilot is successful
- Set up reporting: track meetings booked from agent-sourced prospects
- Discuss multi-agent strategy if needed (different segments/territories)
- Discuss add-to-cadence workflows (Outreach, Salesloft, Gong Engage integration)
- Review Flex Credit consumption — is pacing aligned with budget?

### Success Handoff

At 30 days, deliver a brief summary to the sales leader and AE:

```
PROSPECTING AGENT — 30-DAY SUMMARY
====================================
• Agent Status: Active / Generating [X] prospects per week
• Reps Engaged: [X] of [Y] using daily
• Prospects Accepted: [X] (acceptance rate: [X]%)
• Meetings Booked from Agent Prospects: [X]
• Pipeline Influenced: $[X]
• Flex Credits Consumed: [X] of [Y] allocated
• Recommendations: [expand team / refine ICP / add data source / etc.]
```

---

## Appendix A: Multiple Agents Strategy

If the customer has multiple segments, territories, or ICPs — recommend multiple agents:

| Scenario | Agent Config |
|----------|-------------|
| Enterprise vs. Mid-Market | Separate agents with different ICP criteria and report sources |
| By territory (AMER vs. EMEA) | Separate agents with geography-specific targeting |
| By vertical (Finance vs. Healthcare) | Separate agents with industry-specific signals |
| By use case (New business vs. Win-back) | Agent A: Top accounts report / Agent B: Closed/Lost report |

**Best practice:** Start with ONE agent for the highest-value use case. Prove it. Then expand.

---

## Appendix B: Flex Credit Consumption Model

Understanding how Prospecting Agent consumes Flex Credits is critical for budgeting and setting customer expectations.

### Pricing Fundamentals

- 1 Action = 20 Flex Credits = ~$0.10
- $500 per 100,000 Flex Credits (list price)
- Sandbox: 1 Action = 16 Flex Credits = ~$0.08
- Foundations SKU includes 100K–200K free Flex Credits to start

### Agent Processing Pipeline (per account)

| Stage | What Happens | Credit Consumption |
|-------|-------------|-------------------|
| **Pre-filtering** (Q0) | Salesforce Reports, Salesforce filters, ZoomInfo filters narrow account pool | **0 credits** — pure configuration gate |
| **Account Research** (Q1) | Agent checks all ICP instructions and signals for the account | **1 action (20 credits)** per account — regardless of how many instructions configured |
| **Find Prospects** (Q2) | Agent identifies contacts matching ICP persona (titles, seniority) | **1 action per contact found** — contact-level research |
| **ZoomInfo Match** (Q3) | Net-new contacts not in Salesforce get enriched via ZoomInfo | Handoff step — ZoomInfo credits consumed here |
| **Prospect Assignment** | Prospects assigned to reps based on ownership/round-robin | **0 credits** — configuration-driven |

### Estimation Formula

```
Per account: ~16 actions × 20 credits = ~960 credits (round to 1,000)
Breakdown:
  - Research the account (web/data lookups)
  - Identify ~3 prospects
  - Enrich each prospect (ZoomInfo lookup per contact)
  - Draft personalized context per prospect

Total = Accounts/week × 1,000 credits × 52 weeks
```

### Real-World Examples

| Scenario | Math | Annual Credits |
|----------|------|---------------|
| 2 reps, 10 accounts/week each | 20 accts × 1K × 52 | ~1M |
| 10 reps, 10 accounts/week each | 100 accts × 1K × 52 | ~5.2M |
| 17 reps, 10 accounts/week each | 170 accts × 1K × 52 | ~8.8M (~$44K/yr at list) |

### Customer Reference

Batteries Plus ran 10 agents at scale → burned ~2M credits/week. They saw good results but this is the high end.

---

## Appendix C: Known Bugs & Version Tracking

| Bug ID | Description | Fixed In | Workaround |
|--------|-------------|----------|------------|
| W-22312870 | Non-admin users blocked from Prospecting tab ("no access" error). Root cause: `dbValueRequired="true"` on FK fields | v262.5 | Assign Manager perm set |
| W-22290120 | Prospecting page invisible for non-manager users (viewer page renders manager-only elements) | Related to above | Same as above |
| W-22498128 | Agent dropdown empty in "Generate Prospects from Account" for standard users | v262.11 (targeted) | Grant `Manage Bots` system permission |
| W-23003467 | Standard users unable to create agents despite correct permission sets | TBD | Assign Manager perm set |
| Data loss bug | Deleting an agent cascade-deletes all corresponding prospects | TBD | NEVER delete agents — deactivate instead |

**Version check:** Ask admin to go to Setup → "Release Updates" or check the org's release number at the bottom of any Setup page.

---

## Appendix D: Key Limitations to Set Expectations

Share these transparently with the customer:

- **English only** (US locale) — localization targeted FY27 Q2 (no confirmed date)
- **No LinkedIn automation** — LinkedIn policy prevents programmatic access; reps must manually cross-reference
- **ICP is one-way** — once defined in Agent Builder, CANNOT revert to the guided front-end UI
- **Batch processing only** — results aren't real-time (weekly cadence currently; real-time signal-based triggers on roadmap)
- **ZoomInfo is the only enrichment provider** — no Clearbit, Apollo, or other alternatives at GA
- **ZoomInfo credits finite** — 2,000 free is a starter allotment; scale requires commercial relationship
- **Prospecting Center deprecated** — End-of-Sale April 20, 2026; customers on it should migrate to Prospecting Agent
- **Data Cloud not used** — despite the $0 Foundations SKU quoting requirement, agent doesn't query DC directly
- **Custom object** — prospects stored in `Prospecting Agent Recommended Target`, not standard Lead/Contact
- **No EAC aliasing** — Einstein Activity Capture email aliasing fix scheduled October 2026; until then, email matching may miss alias variants
- **Non-admin user access bug** (W-22312870) — may still exist on some pods; workaround is Manager perm set

---

## Appendix E: Quick Reference Links

| Resource | Link |
|----------|------|
| Prospecting Agent Hub (internal) | https://salesforce.enterprise.slack.com/docs/T01G0063H29/F0ASQ3Z4V8C |
| Setup Guide Canvas | https://salesforce.enterprise.slack.com/docs/T01G0063H29/F0AUTU6PHAA |
| Implementation Guide (slides) | https://docs.google.com/presentation/d/1QYrK0D7E_TPzYEdjoMoQWQFVLWGaKRaMpW4YaXqYWYg/ |
| First Call Deck: Agentforce Sales | https://docs.google.com/presentation/d/1tpoCfMJJ1zaPSBNHpnm21GxRPVuXlrUHQwYwa5GLF9E/ |
| Agentforce for Sales FAQ (internal) | https://docs.google.com/document/d/1_M1fuUKAPuoJSWLzFoWTAcw9ehlBCzv3/edit#heading=h.qgw34l3bjjvk |
| Internal FAQ for A4X SKU | https://docs.google.com/document/d/1Huk_Ue2dfVqJcvRU_BVF0H5BLFa54XvnHb8Au_AtkhQ/ |
| Flex Credit & BVS Calculator | https://docs.google.com/spreadsheets/d/1C9HqWlY0xeMwSEu-FiDo1XeTCYIKmZ1XzP3LGegQ4IU/edit?gid=1921851083#gid=1921851083 |
| FY27 Agentforce SKU Quoting Guide | https://salesforce.enterprise.slack.com/docs/E7T5PNK3P/F09818V42UE |
| Help & Training (customer-facing) | https://help.salesforce.com/s/articleView?id=sales.sales_agent_prospecting_parent.htm&language=en_US&type=5 |
| Einstein Generative AI Setup (prerequisite) | https://help.salesforce.com/s/articleView?id=ai.generative_ai_enable.htm&type=5 |
| ECI Gong Transcription Providers | https://help.salesforce.com/s/articleView?id=sales.ci_setup_transcription_providers.htm |
| SDO Demo Setup | https://salesforce.enterprise.slack.com/docs/T01G0063H29/F0APLRWRCL8 |
| FDE Technical Deep Dive Recording | https://drive.google.com/file/d/1de_ewRbuaNthxLKtPNenn2ZOfBGB1VJ2/view?usp=sharing |
| CBS SME Request | Workflow shortcut in #cbs-solutions |
| License extension requests | #q-branch-xdo-license-extension-requests |
| Questions channel | #help-sell-ai |
