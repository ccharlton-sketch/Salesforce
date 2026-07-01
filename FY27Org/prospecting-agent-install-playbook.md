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
| Data Cloud Provisioning | Ensure $0 Foundations SKU is in place (quoting requirement) | AE |
| ZoomInfo status | Determine: existing contract (OAuth in) or net-new (2,000 free credits) | SE + Customer |

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
| Check org version | Confirm they're on a release that supports Prospecting (260+) |
| Review known bugs | Non-admin user access bug (W-22312870) — confirm fix is deployed on their pod |
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

1. **Salesforce Go** → Setup → Search "Agentforce for Sales" → Toggle ON "Prospecting"

2. **Permission Sets** (Setup → Permission Sets):
   - Assign `Sales Agentic Prospecting` → all pilot users + managers
   - Assign `Sales Agentic Prospecting Manager` → admin(s) who configure
   - Assign `ZoomInfo Prospecting Access` → user who will OAuth into ZoomInfo

3. **Add Prospecting Tab**:
   - Setup → App Manager → Sales app → Edit → Navigation Items → Add "Prospecting" → Save
   - Verify: Navigate to the Sales app — "Prospects" tab should appear
   - If not visible: append `/lightning/page/prospecting` to the org URL

4. **Verify Access**:
   - Log in as a non-admin pilot rep — confirm they can see the Prospecting tab
   - If blocked: check the known permission bug, may need Manager perm set as workaround

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

**Only if customer uses Gong for call recording:**

1. **Install/Update Gong Managed Package**:
   - [AppExchange listing](https://appexchange.salesforce.com/appxListingDetail?listingId=a0N3A00000FeGgcUAF)
   - Gong Admin → CRM Settings → "Gong for Salesforce" tab → Enable "Include Transcript"

2. **Enable Einstein Conversation Insights (ECI)**:
   - Setup → Salesforce Go → Search "Einstein Conversation Insights" → Get Started → Turn On
   - Assign ECI licenses to pilot users (start small, expand after validation)

3. **Backfill Historical Transcripts**:
   - Submit request to Gong support (contact.gong.io/hc/en-us/requests/new)
   - Recommend backfilling 1–2 years of transcripts
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

| Problem | Fix |
|---------|-----|
| Reps can't see Prospecting tab | Permission set not assigned, or tab hidden in app |
| Non-admin users get "no access" error | Known bug (W-22312870) — may need Manager perm set as interim fix |
| No prospects generated | Report source empty, ICP too restrictive, or agent not activated |
| Low-quality prospects | ICP too broad, needs more specific qualifiers or disqualifiers |
| ZoomInfo errors | OAuth expired, or credit allotment exhausted |

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

## Appendix B: Key Limitations to Set Expectations

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

## Appendix C: Quick Reference Links

| Resource | Link |
|----------|------|
| Prospecting Agent Hub (internal) | https://salesforce.enterprise.slack.com/docs/T01G0063H29/F0ASQ3Z4V8C |
| Setup Guide Canvas | https://salesforce.enterprise.slack.com/docs/T01G0063H29/F0AUTU6PHAA |
| Help & Training (customer-facing) | https://help.salesforce.com/s/articleView?id=sales.sales_agent_prospecting_parent.htm&language=en_US&type=5 |
| Agentforce for Sales FAQ | https://docs.google.com/document/d/1_M1fuUKAPuoJSWLzFoWTAcw9ehlBCzv3/edit#heading=h.qgw34l3bjjvk |
| Flex Credit Calculator | https://docs.google.com/spreadsheets/d/1LbqsW_LsgfuVLQUnxLG_ew42eDe06SLrA4LNA_ImkIw/edit?gid=1921851083#gid=1921851083 |
| SDO Demo Setup | https://salesforce.enterprise.slack.com/docs/T01G0063H29/F0APLRWRCL8 |
| FDE Technical Deep Dive Recording | https://drive.google.com/file/d/1de_ewRbuaNthxLKtPNenn2ZOfBGB1VJ2/view?usp=sharing |
| CBS SME Request | Workflow shortcut in #cbs-solutions |
| Questions channel | #help-sell-ai |
