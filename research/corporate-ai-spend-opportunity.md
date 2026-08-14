# What Corporations Will Pay For in AI — and Where the Next Consulting Opportunity Is

**Audience:** Independent / boutique tech-transformation consultants  
**Date:** August 2026  
**Question:** Where is corporate willingness to pay real, and what should a transformation consultant sell next?

---

## Bottom line

Corporations are not short of AI budget. They are short of **P&L results**.

That gap is the opportunity.

- Global AI spend is forecast at **$2.59 trillion in 2026** (+47% YoY). Most of that is hyperscaler infrastructure, not consulting. The enterprise-relevant slice is still enormous: **~$586 billion in AI services** and **~$453 billion in AI software**. ([Gartner, May 2026](https://www.morningstar.com/news/business-wire/20260519405832/gartner-forecasts-worldwide-ai-spending-to-grow-47-in-2026))
- Boards are doubling company-level AI investment from **0.8% to ~1.7% of revenue**. **94%** say they will keep spending even if 2026 does not pay off. The buyer is now the **CEO** (72% say they are the main decision maker). ([BCG AI Radar 2026](https://www.bcg.com/publications/2026/as-ai-investments-surge-ceos-take-the-lead))
- **88%** of organizations use AI somewhere. Only **39%** can point to any EBIT impact. Only **~6%** are high performers (AI contributing 5%+ of EBIT). The strongest predictor is not the model. It is **workflow redesign**. ([McKinsey State of AI, Nov 2025](https://www.mckinsey.com/capabilities/quantumblack/our-insights/the-state-of-ai))
- MIT NANDA’s 2025 field study found **95% of GenAI pilots produced no measurable P&L impact**. External partnerships succeeded at roughly **2x** the rate of internal builds. Back-office work (ops, finance, procurement, legal) often returned more than the sales-and-marketing tools that absorbed most of the budget. ([MIT NANDA, Jul 2025](https://mlq.ai/media/quarterly_decks/v0.1_State_of_AI_in_Business_2025_Report.pdf))

**What to sell next is not another AI strategy deck.** Most large companies already bought that. The next paid work is: take one or two economically critical workflows into production, redesign the operating model around agents, put cost and risk controls on machine work, and prove ROI in a language a CFO will sign.

**How this evolves from here:** see the companion roadmap — [ai-project-evolution-roadmap.md](ai-project-evolution-roadmap.md). Short version: 2026 is the peak of agent hype; 2027 is the trough (chatbot spend peaks, 40%+ of agent projects get canceled); 2028+ demand sits inside core systems and a machine-work operating model. The binding bottleneck shifts from models (solved) to **data/context + workflow**, then **cost and risk**, then **people**. Power binds the physical stack, not your SOW.

---

## 1. Do not confuse the $2.59T headline with client budgets

Three numbers get mixed together. They measure different things.

| Figure | What it actually measures | Source |
| --- | --- | --- |
| **$2.59T** worldwide AI spend in 2026 | Full-stack procurement: chips, servers, cloud, software, services, models, cyber | Gartner, May 2026 |
| **$37B** enterprise generative-AI spend in 2025 | What enterprises paid for models, apps, and related genAI software (not Nvidia, not cloud inference) | [Menlo Ventures, Dec 2025](https://menlovc.com/perspective/2025-the-state-of-generative-ai-in-the-enterprise/) |
| **~1.7% of revenue** | What corporations *plan* to spend on AI in 2026 (tech, data, talent, third parties) | BCG AI Radar 2026 |

Gartner’s 2026 mix (May forecast, $ millions):

| Segment | 2025 | 2026 | 2027 | YoY 2025→2026 |
| --- | --- | --- | --- | --- |
| AI Infrastructure | 975,581 | 1,431,509 | 1,890,310 | +47% |
| AI Services | 436,351 | 585,527 | 759,418 | +34% |
| AI Software | 282,897 | 453,209 | 638,431 | +60% |
| AI Cybersecurity | 25,920 | 51,347 | 85,997 | +98% |
| AI Models | 15,494 | 32,604 | 59,161 | +110% |
| Data science / ML platforms | 21,292 | 29,928 | 42,639 | +41% |
| App-dev platforms | 6,587 | 8,416 | 10,922 | +28% |
| AI Data | 826 | 3,126 | 6,480 | +278% |
| **Total** | **1,764,947** | **2,595,667** | **3,493,358** | **+47%** |

**Implication for a consultant:** more than half of the $2.59T is compute build-out you cannot sell against. Your addressable pool is:

1. **AI services** (~$586B) — implementation, integration, managed services, and advisory.
2. **The services *inside* software and cloud deals** — Gartner’s “indirect” AI services, growing faster than classic consulting hours, and forecast to overtake direct consulting-led work around **2028**.
3. **Change, data, governance, and FinOps** that sit next to those deployments.

Gartner’s John-David Lovelock: *“Up to this point, AI spending has primarily been driven by technology companies and hyperscalers. Enterprises have yet to really flex their spending potential. That is coming and 2026 will be the inflection year.”* He also notes that enterprises still prefer **tactical, incremental** AI over disruptive change, which is why CIOs struggle to prove value. That is exactly the transformation problem a consultant is hired to close.

---

## 2. What corporations are actually buying

Follow the dollars, not the keynotes.

### A. Software seats and “buy, don’t build”

Menlo’s 2025 enterprise survey (~500 US buyers) is the cleanest picture of *paid* genAI:

- GenAI spend: **$1.7B (2023) → $11.5B (2024) → $37B (2025)**
- **$19B** of that went to applications people use
  - Horizontal copilots: **$8.4B** (Copilot-class tools ~$7.2B)
  - Departmental tools: **$7.3B**, of which **coding is $4.0B**
  - Vertical industry apps: **$3.5B** (healthcare the largest)
- **76% of AI use cases are now purchased**, not built in-house (vs. 53% purchased in 2024)
- AI deals convert to production at **47%**, vs. **25%** for traditional SaaS
- **27%** of application spend arrives via product-led growth (employees pull tools in); shadow AI may push that near 40%

Coding is the first “killer” paid use case. Half of developers use AI daily. That market is already captured by Cursor, Copilot, and peers. Do not compete there as a consultant unless you are implementing SDLC operating-model change around those tools.

### B. Agents — the 2026 budget line CEOs have already committed

- CEOs have committed **>30% of 2026 AI investment to agentic AI**. Trailblazer CEOs (15% of the BCG sample) are putting **more than half** of their AI budget into agents. ([BCG](https://www.bcg.com/press/15january2026-as-ai-investments-surge-ceos-take-lead))
- Gartner puts agent software on a steep curve (~$86B in 2025 → ~$206B in 2026 → ~$376B in 2027, per secondary compilations of Gartner tables). Gartner also predicted **>40% of agentic AI projects will be canceled by end-2027** because of cost, unclear value, or weak risk controls, plus widespread “agent washing.” ([Gartner, Jun 2025](https://www.gartner.com/en/newsroom/press-releases/2025-06-25-gartner-predicts-over-40-percent-of-agentic-ai-projects-will-be-canceled-by-end-of-2027))
- McKinsey (Nov 2025): **23%** scaling agents in at least one function; **62%** experimenting.

**Consultant read:** the budget is allocated. The cancellation rate is the pipeline. Anyone who can take an agent from demo to a governed, costed, production workflow is selling into a funded, under-delivered mandate.

### C. Services, integration, and the unsexy majority of a real project

Vendor-side pricing guides (not Gartner, treat as directional) consistently show the same cost structure:

- Tool subscription is often **30–50%** of year-one cost
- Integration, data prep, change, and governance are the rest
- Year-one all-in is often **1.8–2.2×** the “build” quote
- **67%+ of projects overrun** the original budget

McKinsey’s July 2026 agentic-economics paper is more important than any pricing table: **93% of enterprises in a May 2026 FinOps survey had already exceeded their AI budgets.** Token prices fell; bills rose, because agents consume ~1,000× more tokens than chat, ~60% of agent cost is refinement/rework, and the same task can vary **30×** in cost depending on path. AI is heading toward **~25% of IT budgets** over the next several years. ([McKinsey, Jul 2026](https://www.leadersnet.at/resource/download/188592/2026-07-22,is-that-ai-agent-worth-it-agentic-economics-and-the-modern-operating-model,pdf))

### D. Upskilling — where trailblazers actually put the money

BCG’s trailblazer CEOs put **~60% of AI budget into upskilling and retraining**, vs. ~24–27% for pragmatists and followers. That is not L&D theater. It is how they convert seats into workflow change.

### E. Security, data, and compliance

AI cybersecurity nearly doubles in 2026 in Gartner’s table ($26B → $51B). AI data is tiny but the fastest grower. EU AI Act and sector regulators are turning “responsible AI” from a slide into a paid workstream (risk classification, model inventory, human-in-the-loop, audit trails).

---

## 3. Willingness to pay: what they will fund vs. what they will not

### They will pay for

| Offer | Why it clears procurement | Typical 2026 price band (boutique → Big 4) |
| --- | --- | --- |
| **Production use-case with a P&L metric** | CEO/CFO now own AI; “show ROI” is the 2026 brief | $100k–$500k per workflow; $500k–$5M+ multi-workflow programs |
| **Workflow / operating-model redesign around agents** | McKinsey’s strongest correlate with EBIT impact | Strategy-plus-delivery retainers, not a 4-week deck |
| **Data / context layer for agents** (ontology, retrieval, systems of record, telemetry) | McKinsey: as models converge, **context is the moat** | Often larger than the model work; $200k–$1M+ |
| **AI FinOps / cost-per-outcome** | 93% over budget; token price is no longer the unit of management | New category; $50k diagnostic → ongoing ops |
| **Governance, risk, and “stop rules” for agents** | Gartner cancellation causes #2 and #3; regulated industries must have this | $75k–$400k frameworks; then embedded in delivery |
| **Change, adoption, and capability building** | Trailblazers spend 60% here; shadow AI (90% of workers vs 40% official seats) is the real adoption map | Workshops are cheap; embedded enablement is the paid product |
| **Buy-vs-build portfolio and vendor selection** | 76% buy; internal builds fail ~2× as often (MIT) | High-trust advisory, often as Phase 0 of delivery |
| **Back-office automation with contract-level savings** | MIT: $2–10M/year from replacing BPO/document review; $1M risk-check savings; 30% agency-spend cuts | Easier ROI math than “smarter marketing” |
| **Managed AI operations after go-live** | Production, not pilots, is where value (and cost) live | $5k–$25k/month boutique; much higher at SIs |

### They will not (or no longer) pay well for

- **Another AI strategy / roadmap with no implementation.** The first wave is done. Firms that only produce decks are under margin pressure.
- **Chatbot POCs and Copilot rollouts as “transformation.”** Individual productivity is real; it rarely shows up in EBIT. MIT: generic LLMs get used; custom tools stall.
- **Custom foundation models** for most clients. Menlo: enterprises shifted hard to buy. McKinsey: keep internal build for glue layers, differentiated agents, and learning — not for generic intelligence.
- **Infrastructure, GPUs, or competing with hyperscalers.**
- **Hours billed against vague “AI CoE standup”** with no owned P&L metric.
- **Agent washing** — wrapping a chatbot and calling it an agent. Buyers and Gartner are now hostile to this.

### Who signs the check

Sell to the **CEO and the P&L owner**, with the CIO/CFO as the economic operators.

- 72% of CEOs say they are the main AI decision maker (2× last year)
- 50% believe their **job** depends on getting AI right
- 90% believe **agents** will produce measurable returns in 2026
- Confidence falls as you move away from the C-suite (62% of CEOs vs 48% of non-tech execs outside the C-suite). The people doing the work are more skeptical. Price that in: your delivery has to make middle management look good, not just the CEO’s narrative.

Western CEOs (US 52% confident of ROI, UK 44%) are more likely to be spending from **FOMO / investor pressure**. That is a worse buyer than an Indian or Greater China CEO who is confident they will get paid back — unless you attach a hard metric and a stop rule.

---

## 4. Where the next big opportunity is

Ranked for a tech-transformation consultant (not a GPU vendor, not a model lab).

### Opportunity 1 — “Pilot to P&L”: one workflow, redesigned, in production

This is the largest, most durable consulting market of the next 24 months.

**Why now:** 88% adoption, ~6% real impact, 95% of GenAI pilots with no P&L (MIT), 40%+ of agent projects headed for cancellation (Gartner). The constraint is not models. It is integration, learning/memory, workflow fit, and measurement.

**What to sell:** a 90-day (mid-market) to 6-month (enterprise) engagement that:

1. Picks **one economically critical process** (claims, close, KYC, invoice-to-pay, ticket-to-resolution, underwriting, order-to-cash — not “knowledge chatbot”).
2. Redesigns the workflow for human + agent (escalation paths, stop conditions, source-of-truth).
3. Integrates with systems of record.
4. Stands up evaluation, cost-per-outcome, and audit.
5. Transfers ownership. Does not leave a demo.

**Proof this is what high performers do:** McKinsey high performers are ~3× as likely to have **fundamentally redesigned workflows**. Half of them intend to use AI to **transform the business**, not just cut cost. They also put **>20% of digital budgets** into AI.

**Do not lead with “AI strategy.”** Lead with a named process, a baseline metric, and a production date.

### Opportunity 2 — Agentic operating model + AI FinOps (new category)

Almost nobody has this as a productized offer yet. Demand is already visible.

McKinsey’s July 2026 argument: tokens are the bill, not the value. CEOs must **allocate intelligence like capital**, measure **cost per completed business outcome**, set autonomy budgets and stop rules, and treat agent ops as a multi-year discipline (the equivalent of lean, cyber, or cloud FinOps).

Offer components:

- Model routing (frontier only when it changes the outcome; cheap models for the rest)
- Context/cost engineering (long-lived context is the silent budget killer)
- Consumption pooling vs. seat waste
- Vendor/stack economics (same model, different prices)
- CFO pack: forecast, unit economics, make/buy/place decisions

This is how you stay on the account after the first use case. It is also how you talk to CFOs who just learned that 93% of peers blew the AI budget.

### Opportunity 3 — Context and data as a product, not a “data lake reboot”

McKinsey: process advantage is getting harder to defend because agents compress execution. **Proprietary context** (call transcripts, decision history, process telemetry) becomes the differentiator. CIO mandate: connect systems of record, unstructured content, and enterprise semantics so agents can act.

This is classic transformation work (data contracts, MDM-lite, retrieval, permissions, lineage) with a new buyer story: “without this, every agent you buy is a generic intern.”

Sell a **context layer for 2–3 target workflows**, not a 3-year data-platform program.

### Opportunity 4 — Back office, not the board-visible front office

MIT NANDA: ~50–70% of GenAI budget imagination goes to sales and marketing because those metrics are easy to show a board. The documented million-dollar saves were in:

- Replacing outsourced support and document review (**$2–10M/year**)
- Cutting external agency spend (**~30%**)
- Financial risk monitoring (**~$1M/year**)
- BPO and consultant substitution

Rivian’s finance-close agents (Amazon Bedrock / SAP) are a public example of the same pattern: month-end work, purchase-order accruals, cycle-time. That is transformation consulting language, not data-science language.

**Target functions:** finance operations, procurement, legal/contract ops, customer operations, risk/compliance, shared services.

### Opportunity 5 — Mid-market speed vs. enterprise theater

MIT: enterprises lead in **pilot volume** and lag in **scale-up**. Mid-market top performers went **pilot → production in ~90 days**; enterprises took **9+ months**.

If you are not Accenture, this is your structural advantage. Mid-market ($25M–$1B revenue) will pay **$90k–$350k** for a first production system (directional boutique pricing). They will not pay $2M for a 18-month SI program. They *will* pay for someone who ships.

Enterprises still pay $500k–several million — but only if you attach to a named P&L owner and survive procurement. Many already burned a year on CoEs and Copilot.

### Opportunity 6 — Embedded AI inside existing transformation (the 2028 shift)

Gartner’s services mix is moving from **direct, consulting-led AI projects** (slower growth, ~16% CAGR in later vintages) to **indirect services** (AI as a line inside ERP, CX, core-banking, claims, or cloud programs, ~35% CAGR; overtakes direct around 2028).

**Practical meaning:** stop selling “an AI project.” Sell **AI inside the transformation they already budgeted** — SAP/Oracle finance, ServiceNow, Salesforce, core banking, claims, supply chain. That is where the large checks already live.

### Opportunity 7 — Sector focus

Use **intensity of spend + measurability of outcome + regulation**, not hype.

| Sector | Spend signal | Why a consultant gets paid | Watch-out |
| --- | --- | --- | --- |
| **Financial services** | ~**2.0% of revenue** (BCG); highest non-tech intensity | Fraud, KYC/AML, credit ops, claims, servicing — outcomes are priced; regulators force governance | Slow procurement; in-house builds fail more (MIT) |
| **Technology / software** | ~**2.1% of revenue**; coding already bought | Operating model for AI-native SDLC, support, and product agents | They may not need you for tools |
| **Healthcare / life sciences** | High growth; Menlo vertical apps **$1B+** in healthcare | Ambient documentation, prior auth, RCM, clinical admin — clear time and denial metrics | Clinical models are still mostly pilots; stay on admin/ops unless you have domain credentials |
| **Manufacturing / industrials** | Lower % of revenue (**<1–1.3%**) but **fastest adoption growth** in McKinsey | Predictive maintenance, quality vision, planning — hard baselines | Physical/OT integration is the real work |
| **Consumer / retail** | ~**1.6% of revenue** | Service, forecasting, personalization | Easy to get stuck in chatbot land |
| **Professional services** | High adoption, weak EBIT | Document, research, delivery ops | They are also your competitors |
| **Energy, public sector, education** | Low intensity | Selective compliance and admin | Not the 2026 growth engine |

**Best first beachhead for an independent transformation consultant:** financial services operations, healthcare admin/RCM, or mid-market manufacturing/finance ops — anywhere a cycle time or error rate is already on a dashboard.

---

## 5. How to package and price it

A commercial motion that matches 2026 buying behavior:

### Phase 0 — Paid diagnostic (2–4 weeks, $15k–$75k)

Not a free pitch. Deliver:

- Inventory of shadow AI vs. official AI
- 3 candidate workflows with baseline economics
- Cost-to-serve vs. cost-of-agent estimate
- Buy vs. build vs. embed recommendation
- A 90-day production plan with a kill criterion

This filters FOMO buyers from P&L buyers.

### Phase 1 — One production workflow (10–16 weeks, $100k–$400k boutique; more at SI rates)

Fixed scope, outcome metric, weekly cost dashboard. Senior people on the work. No junior slide factory.

### Phase 2 — Operating system (ongoing)

FinOps, evaluation, governance, enablement, next two workflows. Monthly managed service or a 6–12 month transformation retainer.

**Pricing posture that wins in 2026:**

- Fixed-fee Phase 0 (buyers are allergic to open-ended discovery)
- Outcome-tied milestones on Phase 1 (cycle time, cost/ticket, close days, leakages)
- Do not compete with MBB on strategy narrative; compete on **time-to-production** and **cost-per-outcome**
- Staff delivery people who can sit in the process with the ops team. MIT’s “learning gap” is about tools that do not remember or fit the work — your edge is making them fit.

**What a $1B-revenue company might spend if they follow BCG’s 1.7%:** ~$17M all-in on AI (software, infra, people, vendors). A credible boutique can own **$0.5–2M** of that if you are on the critical workflow, not on the CoE periphery.

---

## 6. Competitive landscape — where a boutique still fits

| Player | What they take | Gap you can occupy |
| --- | --- | --- |
| MBB / Big 4 | CEO agenda, multi-year programs, $500k–$5M+ | Slow; strategy-heavy; mid-market overkill |
| Accenture / IBM / Capgemini | Scale implementation, 6–18 months | Expensive; junior leverage; weak on agent economics |
| Hyperscalers | Cloud, models, reference architectures | Will not redesign the client’s operating model |
| AI-native software (Cursor, ServiceNow, Salesforce, vertical SaaS) | The application layer ($19B and growing) | Need someone to integrate, govern, and change the work |
| Internal CoEs | Coordination | MIT: internal builds fail ~2× vs. external partners |

The winning boutique position: **operator who ships agents into real processes, then stays for economics and adoption.** MIT’s implementation advantage (external partners succeed about twice as often as internal builds) is the permission structure for this offer.

---

## 7. A practical 12-month bet for a transformation consultant

If you can only pick one thesis:

> **Help the CEO convert the 2026 agent budget into one or two production workflows with a CFO-grade unit economic, then productize the operating model (context, FinOps, governance, upskilling) that lets the next ten workflows not fail.**

Concrete BD list:

1. **Accounts:** $200M–$5B companies in FS ops, healthcare admin, manufacturing, or multi-entity finance. Avoid “we need an AI strategy” RFPs.
2. **Entry:** paid diagnostic tied to a process the COO/CFO already hates (close, claims, AP, KYC, ticket backlog).
3. **Proof:** 90-day production, not a pilot. Publish the metric.
4. **Expand:** FinOps + context layer + two adjacent processes.
5. **Do not:** build a generic chatbot, a custom LLM, or a 40-use-case roadmap.

The next big opportunity is not “AI.” It is **making AI show up in the P&L before the 2027 cancellation wave takes the budget back.**

---

## Sources (primary first)

1. Gartner, “Forecasts Worldwide AI Spending to Grow 47% in 2026,” 19 May 2026. Table via Business Wire / Morningstar reprint: <https://www.morningstar.com/news/business-wire/20260519405832/gartner-forecasts-worldwide-ai-spending-to-grow-47-in-2026>
2. Gartner, “Predicts Over 40% of Agentic AI Projects Will Be Canceled by End of 2027,” 25 Jun 2025: <https://www.gartner.com/en/newsroom/press-releases/2025-06-25-gartner-predicts-over-40-percent-of-agentic-ai-projects-will-be-canceled-by-end-of-2027>
3. BCG, *AI Radar 2026: As AI Investments Surge, CEOs Take the Lead,* 15 Jan 2026: <https://www.bcg.com/publications/2026/as-ai-investments-surge-ceos-take-the-lead> and [press release](https://www.bcg.com/press/15january2026-as-ai-investments-surge-ceos-take-lead)
4. McKinsey QuantumBlack, *The State of AI: Global Survey 2025,* 5 Nov 2025 (n≈1,993): <https://www.mckinsey.com/capabilities/quantumblack/our-insights/the-state-of-ai>
5. McKinsey QuantumBlack, *Is that AI agent worth it? Agentic economics and the modern operating model,* Jul 2026
6. Menlo Ventures, *2025: The State of Generative AI in the Enterprise,* 9 Dec 2025: <https://menlovc.com/perspective/2025-the-state-of-generative-ai-in-the-enterprise/>
7. MIT NANDA / Challapally et al., *The GenAI Divide: State of AI in Business 2025,* Jul 2025: <https://mlq.ai/media/quarterly_decks/v0.1_State_of_AI_in_Business_2025_Report.pdf>
8. Stanford HAI, *AI Index 2026* (corporate AI investment / inference-cost deflation cited in McKinsey)
9. Secondary compilations used only to cross-check Gartner segment tables: [Axis Intelligence](https://axis-intelligence.com/ai-spending-statistics/) (Jul 2026). Industry *absolute* spend figures on aggregator blogs ($186B vs $407B vs Gartner’s $2.59T) **conflict and should not be used** — stick to BCG % of revenue and Gartner segments.
10. Roadmap (project funnel, 2027 trough, bottleneck stack): [ai-project-evolution-roadmap.md](ai-project-evolution-roadmap.md)

### Caveats

- BCG 1.7% is **planned** spend, not audited actuals.
- McKinsey EBIT impact is **self-reported**.
- MIT’s “95%” is a field study (52 org interviews, 153 leaders, 300 public cases), not a census. It is directionally consistent with McKinsey’s 6% high-performer share, not identical.
- Boutique pricing bands come from practitioner guides, not Gartner. Use them as commercial ranges, not forecasts.
- Gartner revised AI services **down** in later 2026 vintages while raising software — buyers are routing more transformation money through software and cloud, less through classic billable hours. Adjust the offer accordingly: deliver inside the software the client already bought.
