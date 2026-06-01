# PM Coaching Agent — Build Handoff & Research Dossier

> A personal AI coach that watches how Tyler actually practices product
> management — across his Obsidian vaults (KraneBrain = work, BrittBrain =
> personal), Linear, Slack, and his Claude/Codex history — and gives honest,
> evidence-grounded feedback: better discovery questions, better sequencing,
> decision tracking, working on the right things, improving over time.
>
> Working name: **Athena** *(placeholder — collides with AWS Athena; pick a
> distinct name).* Sibling to the recovery-reviews research engine (same
> ingest → knowledge base → synthesize loop).
>
> **This is a handoff doc.** §A below is the kickoff for a local agent with full
> machine access; §0–§10 are the completed research it should use as context.

---

## §A. ▶ START HERE — Kickoff for the local agent

**You are picking up a project to build a personal PM coaching tool for Tyler.**
The research below (§0–§10) is done. Your job: get oriented against his *actual*
environment, answer the open questions, and start building the simplest useful
version. Read this section, then run the Orientation Checklist before writing code.

### The mission (one sentence)
Build Tyler a personal coach grounded in his real work behavior that helps him ask
better questions, sequence better, track decisions, and work on the right things —
and that **challenges him rather than flatters him**. (Why this is a real gap, and
the PM rubric it scores against: §0–§2.)

### Decisions already made — don't relitigate
- **Personal-first. Build for Tyler, keep it as simple as possible.** Open-source/
  productizing is explicitly **deferred** (§8 is "someday, not now"). We can rebuild
  it cleanly for OSS later; right now it just has to help *him*.
- **Claude Code is the primary runtime AND interface.** Tyler barely uses his Hermes
  work-agent; he works through **Claude Code + Codex**, which already write to the
  vaults and already have **Slack + Linear connectors**. Lean on that instead of
  building ingestion. (OpenClaw/BrittBot = personal harness; secondary consumer.)
- **Files-as-memory is fine for v1.** The vaults are markdown-on-disk already; the
  coach's state (decision log, weekly reviews, scorecards) can live as markdown in
  KraneBrain. **No database required to start.**
- **Tokens/API credits are not a constraint** (Claude Max + ChatGPT Pro). Optimize
  for impact and learning, not cost.
- **The coach must push back.** Sycophancy is the #1 risk (§4) — design v1 to
  challenge, and add the flip-rate probe (§4) as soon as there's something to test.

### Simplest build path — smallest first, in order
- **v0 — "The Friday Review" (a Claude Code skill, ~one sitting).** Pull last week's
  Linear activity (existing connector) + Tyler's stated priorities/roadmap (from
  KraneBrain) → one-page review: *did my week match my priorities, and what decision
  is rotting?* Write it into KraneBrain. **No new infra. Proves value immediately.**
- **v1 — Decision log + 2–3 rubric dimensions.** Add a markdown decision log in the
  vault; start scoring a few §2 dimensions from meeting transcripts — recommend
  **Inquiry Quality** (questions vs assertions) and **Decision Velocity/reversibility**
  first. Accumulate weekly; let the picture build over time.
- **v2 — the real engine (only if v0/v1 earn it).** Standalone MCP server with temporal
  memory (Zep/Graphiti or Mem0) + a light People/Decisions/Initiatives graph + a
  **separate evidence-grounded critic** + an eval harness with sycophancy probes,
  grounded against the Lenny corpus as the "what good looks like" layer (§3, §7, §9).
  This is the learning-heavy phase (knowledge graphs, evals, MCP).

### Orientation checklist — DO THIS FIRST (you have local access the research didn't)
1. **Inventory the vaults.** Locate KraneBrain & BrittBrain on disk; map folder
   structure, frontmatter/tag conventions, daily-note format, link taxonomy.
2. **Inspect meeting transcripts in KraneBrain** — naming, summary vs raw transcript,
   whether action items/decisions are captured in any structured way.
3. **Hunt for an existing decision log.** Does Tyler record decisions + rationale +
   confidence anywhere today? If not, that absence is the single highest-ROI fix
   (unlocks rubric dims 6 & 7 — see §2 note).
4. **Verify Claude Code's connectors** — confirm Linear + Slack actually work from
   here; enumerate available fields (Linear: cycles/estimates/projects? Slack: which
   channels/DMs?).
5. **Find his stated priorities/roadmap/OKRs** (KraneBrain? Linear? a doc?) — required
   to score effort-vs-priority alignment (rubric dim 8).
6. **Mine existing repos for reusable patterns:** `brittbrain`, `brittbrain-architecture`
   (his memory architecture), the OpenClaw/BrittBot setup, KraneBot if it exists.
7. **Check Claude/Codex history export** — availability + format (richest source of his
   actual *thinking*, not just outputs).
8. **Report findings against the Open Questions (§10) before coding.**

### Then propose → confirm → build
After orientation, give Tyler: (a) what you found, (b) updated answers to §10,
(c) a concrete v0 spec for the Friday Review, then get his go-ahead and build v0.
Ship something he can feel this week. Keep it simple.

---

## 0. The one-sentence wedge

**No shipped product combines real cross-source behavioral data + craft-specific
PM judgment + longitudinal critique.** Tools today are either content-blind
breadth (Viva), single-channel depth (Read.ai counts your filler words),
on-demand document review (ChatPRD), or chat-based self-report coaches wearing
"behavioral" marketing (BetterUp, Valence). The gap: a coach that watches *how
you actually practice product management over months* and pushes back.

---

## 1. Competitive landscape (what exists, and where it stops)

Three non-overlapping camps, none of which closes the loop:

| Camp | Examples | What it does | Where it stops |
|---|---|---|---|
| **Note-takers** | Granola, Otter, Fireflies (as notetaker), Supernormal | Transcribe, summarize, extract action items | No behavioral feedback; "AI coaching insights" is marketing |
| **Single-channel behavior coaches** | Read.ai Speaker Coach, Wonderway (sales calls), Quantified (practice video) | *Real* longitudinal coaching on one signal (your meeting speech / calls) | One source only; communication style, not PM craft |
| **Time/metadata aggregators** | Viva Insights, Reclaim.ai | Cross-source, private, personal nudges (focus time, meeting load) | **Metadata only — deliberately content-blind for privacy**; never judges effectiveness |
| **Chat/self-report coaches** | BetterUp Grow, Valence (Nadia), CoachHub (AIMY), Rocky.ai | Conversational coaching grounded in chat + org frameworks | Integrations are *delivery channels & HR triggers*, not data ingestion |
| **PM-specific** | ChatPRD, Reforge/Lenny skills | ChatPRD: CPO-style review of a doc you submit. Lenny skills: framework injection | Reactive, in-session, single artifact — not longitudinal behavioral critique |

**The one real comp:** **Ariso** (JupiterOne founder, surfaced Jan 2026) — auto-tracks
every email/meeting/Slack convo, weekly reflections, "performance review from real
data over months." This *validates the thesis* but is **brand-new, generic, and
manager/team-framed — not PM-craft-specialized.** That specialization is your moat.

Key sources: read.ai/coaching · learn.microsoft.com/viva/insights (MyA FAQ —
"only analyzes metadata… not content") · betterup.com/products/betterup-ai-coaching ·
valence.co/trust-and-security · chatprd.ai/product/features/get-product-coaching ·
ariso.ai + grepbeat.com/2026/01/20 (Ariso coverage).

**Hype to discount:** Limitless "boost your communication skills," Supernormal
"AI coaching insights," BetterUp's "behavioral signal detection" (= platform
engagement analytics, not work-artifact analysis). All unverified marketing.

---

## 2. The PM excellence rubric (the heart — what the coach scores against)

The coach is only as good as its theory of "good PM." Eight measurable dimensions,
each tied to a citable authority and a signal extractable from your work data.

| # | Dimension | Authority | Measurable signal from your data |
|---|---|---|---|
| 1 | **Discovery rigor / customer contact** | Torres (≥1 interview/wk); Cagan (~3 hrs/wk, "expert on the customer") | Interviews per week; weeks-with-zero-contact; calendar hrs in direct customer contact; % opportunities traceable to a logged interview |
| 2 | **Inquiry quality (questions vs assertions)** | Torres (story-based, non-leading, participant talks most) | Ratio of open/discovery questions to solution statements in transcripts; % "tell me about a time…" vs hypothetical/leading; your talk-time share (lower = better) |
| 3 | **Outcomes over output** | Cagan ("teams measured by outcomes not output") | % OKRs framed as outcomes vs feature counts; % roadmap items as "problem to solve" vs "feature to ship"; North Star + lead/lag/guardrail metrics present |
| 4 | **Assumption testing before commit** | Torres (assumption tests); Cagan (4 product risks: value/usability/feasibility/viability) | # riskiest-assumption tests per shipped feature; % builds preceded by an experiment; coverage of all 4 risk types |
| 5 | **Solution diversity (anti–first-idea)** | Torres (compare ~3 solutions in parallel) | Avg # solutions evaluated per opportunity (~3); % decisions with ≥2 documented alternatives; "first-idea-shipped" rate |
| 6 | **Decision quality & process (anti-resulting)** | Duke (resulting, calibration, premortem) | % significant decisions with written rationale + confidence estimate at decision time; calibration score (predicted vs realized); premortems present |
| 7 | **Decision velocity matched to reversibility** | Bezos (one-/two-way doors, 70% rule, disagree-and-commit); Duke (happiness test) | Time-to-decision split by Type 1 vs Type 2 (Type 2 should be fast); reversal rate (low for one-way, some fine for two-way; *near-zero overall = over-deliberation*); decisions stalled awaiting consensus |
| 8 | **Leverage / effort-to-priority alignment** | Grove (leverage = impact/time); RICE; Cagan ("only the PM can do") | % effort mapped to top-RICE/top-OKR vs low-leverage IC/admin; correlation of effort allocation to RICE scores; % work that's "only-the-PM-can-do" vs delegable |

**Safe-to-cite quantitative anchors:** Torres weekly-interview rule; RICE =
(Reach × Impact × Confidence) ÷ Effort; Bezos 70%-information + Type 1/2 doors;
Grove leverage = impact ÷ time. **Verify against the books:** Cagan's "3 hrs/wk"
and Torres's "~3 solutions" reached the researcher via secondary summaries (primary
pages 403'd the fetcher) — real and widely attributed, but confirm in *Inspired*
(2nd ed.) and *Continuous Discovery Habits* before publishing.

Sources: producttalk.org/opportunity-solution-trees + /2024/04/story-based-customer-interviews ·
svpg.com/the-product-manager-contribution + /empowered-product-teams + /product-fail ·
aboutamazon.com/news/company-news/2016-letter-to-shareholders ·
intercom.com/blog/rice-simple-prioritization-for-product-managers ·
grahammann.net notes on *Thinking in Bets* and *How to Decide*.

> **Meta-point:** dimensions 6 & 7 require a **decision log** to measure. If
> KraneBrain has no decision log, *that absence is itself a measurable signal of
> decision-process maturity* — and the highest-ROI behavior change the coach can
> drive (start logging decisions with a confidence estimate). Good first nudge.

---

## 3. Technical architecture (how to build it well)

**Verdict: do NOT build GraphRAG from scratch for a small personal corpus.**

- **RAG vs GraphRAG is not a clean win for either** (Han et al., arXiv:2502.11371,
  controlled study): RAG wins single-hop fact lookup ("what did I decide about X"),
  GraphRAG wins multi-hop sensemaking ("why does this keep happening"). GraphRAG
  also costs more (construction LLM calls, latency, storage) — hard to justify for
  a small corpus.
- **Start with vector RAG + a managed temporal-memory layer.** The single most
  valuable coaching signal — *when* a decision was made and how behavior changed
  over time — is exactly what plain RAG loses and what bi-temporal memory captures.
  - **Zep / Graphiti** — temporal knowledge graph; bi-temporal (when a fact was
    *true* vs *recorded*) + edge invalidation. Best when time matters (it does here).
    (arXiv:2501.13956 — vendor-authored, treat exact % as optimistic.)
  - **Mem0** — fact-extraction memory, largest community, best general default.
    (arXiv:2504.19413.) Optional graph variant.
  - **Letta/MemGPT** — max control/self-hosted, steeper ramp; has **sleep-time
    agents** (offline reflection that consolidates history into stable memory) —
    *directly* relevant to a coach that periodically re-reviews your record.
- **Add a light schema-guided knowledge graph only for the pattern queries.**
  LlamaIndex `SchemaLLMPathExtractor` over an ontology of **People / Decisions /
  Initiatives** (or Graphiti's built-in graph). Microsoft GraphRAG's extract→
  triples→Leiden-communities recipe is the canonical pattern if you go deeper.
  Expect **entity-resolution work** — personal data spells projects 3 ways,
  Slack nicknames ≠ real names.
- **Critique-agent pattern (not generation):** "generate → critique → refine"
  (Reflection). **Keep the critic a separate role from any generator** — single-agent
  self-critique has confirmation bias (Reflexion; Multi-Agent Reflexion arXiv:2512.20845).
  **Ground every criticism in retrieved evidence** (Self-RAG, arXiv:2310.11511) so
  the coach cites *what you actually did*, never hallucinated behavior.

Sources: arxiv 2502.11371 · 2501.13956 · 2504.19413 · 2310.11511 · 2512.20845 ·
docs.letta.com · llamaindex.ai property-graph-index · microsoft.github.io/graphrag.

---

## 4. The biggest risk: sycophancy (this can kill the whole project)

A coach built on a stock RLHF model **will flatter you instead of challenging you.**
This is the central technical risk and it's well-documented:

- Sycophancy is a general RLHF behavior: models adapt answers to match the user's
  stated view, even at the cost of accuracy (Sharma et al., arXiv:2310.13548).
- The sharp finding: **humans *and* preference models prefer convincingly-written
  sycophantic answers over correct ones** a non-trivial fraction of the time — so
  the training objective itself pushes toward flattery.
- **Detection (do this):** a probe set that presents each coaching scenario twice —
  once neutral, once with your strongly-stated opinion baked in — and measures the
  **verdict-flip rate**. High flip rate = sycophantic. Reward the coach for
  respectfully pushing back when evidence warrants.
- **Reduction:** lightweight fine-tune on synthetic opinion-robustness data
  (arXiv:2308.03958); separate-critic architecture (§3); a constitution with an
  explicit anti-sycophancy principle.

### How to evaluate subjective coaching output
- **LLM-as-judge works** (~80%+ human agreement, Zheng et al. arXiv:2306.05685) but
  has **position bias** (favors first answer — mitigate by judging both orders,
  declare winner only if it wins both), **verbosity bias** (over-rewards length),
  and **self-preference** (use a *different model family* as judge).
- **Pairwise > absolute** for subjective qualities like tone/critique usefulness.
- **Golden set:** ~30–50 hand-labeled real coaching scenarios to start; validate the
  judge against *your* labels before trusting it; G-Eval (CoT + criterion-separated
  rubric) for the judge.
- **Tooling:** DeepEval or promptfoo for CI-style custom-rubric evals; LangSmith or
  Braintrust for human-annotation + pairwise. Ragas is RAG-only — skip unless
  retrieval-grounded.

Sources: arxiv 2310.13548 · 2306.05685 · 2308.03958 · deepeval.com · evidentlyai.com/llm-guide.

---

## 5. Privacy (non-negotiable — this is sensitive work data)

- **Default to local-first** (Ollama) for the sensitive ingestion/analysis path —
  data stays on device, cleanest posture for work data. Aligns with your
  existing local-first instinct (folio, breathwork).
- **Tradeoff:** local open models are weaker judges → lean harder on human-reviewed
  golden sets and pairwise comparison than on a local judge's absolute scores.
- Data minimization regardless: redact identifiers, send only needed fields. A
  hybrid (local for raw work data, frontier model for harder synthesis on redacted
  inputs) is reasonable.

---

## 6. Concrete recommendations

1. **Build the shared engine once, point it at two domains.** Athena (work/PM coach)
   and the recovery-reviews research agent are the *same* ingest → knowledge base →
   synthesize loop. The reusable framework is the real learning artifact.
2. **Specialize hard on PM craft** — that's the wedge vs. Ariso/ChatPRD. The §2
   rubric *is* the differentiator. Don't build a generic coach.
3. **MVP = "The Friday Review."** One source (Linear), one question: *did my week's
   actual work match my stated priorities, and what decision is rotting?* Pull last
   week's issue activity + roadmap → one-page Friday digest. Shippable in a sitting;
   the data/agent scaffolding extends to everything else.
4. **Architecture v1:** vector RAG + Zep/Graphiti temporal memory; separate critic
   agent; evidence-grounded critiques; local-first ingestion.
5. **Bake in the anti-sycophancy eval from day one** (§4 probe set) — it's the
   difference between a real coach and a flattering toy.
6. **Lowest-effort highest-ROI nudge to ship first:** if there's no decision log in
   Krane, the coach's first job is getting you to log decisions with a confidence
   estimate (unlocks rubric dims 6 & 7).

### Skills you'll learn (maps to your goals)
API integration (Linear GraphQL, Slack) · knowledge graphs + entity resolution ·
temporal agent memory · critique-agent design · **evals on subjective output**
(the frontier AI-PM skill — practiced on data where you're ground truth).

---

## 7. The Lenny ecosystem — the external-standard layer (and a warm-up build)

Lenny Rachitsky (the biggest PM newsletter/podcast) **open-sourced all ~320
podcast transcripts (Jan 2026)** and shipped an **official data pack + MCP server**
(lennysdata.com), triggering a wave of community builds. Critically, **all of them
are content-retrieval over a fixed public corpus** — "what does Lenny/his guests say
about topic X" — with **no memory of *your* work and no longitudinal model of *you*.**
This *confirms Athena's wedge by contrast*: the behavior-critiquing personal coach is
a different product category. But the ecosystem hands you two gifts.

### What exists
| Tool | Type | Notes |
|---|---|---|
| **LennyBot** (lennybot.com) | Official Q&A | GPT-4 over newsletter + all episodes, source links, voice clone you can call. Built by a third party, Lenny-branded. |
| **Ask Lenny** | 3rd-party RAG | Retrieval-first; answers *only* from real transcript quotes with episode citations; **refuses when transcripts don't cover it**. Cleanest grounded-Q&A example. |
| **Lenny's Knowledge Graph** | 3rd-party graph | 330+ episodes / 7,600+ entities; answers with **episode + timestamp + YouTube deep-links**. |
| **Lenny's Friends (Notion)** | Persona | Guests as "mentor" personas you ask in your workspace — the "ask what X would do" play. |
| **`RefoundAI/lenny-skills`** | Skill pack | **86 PM skills** (frameworks + diagnostic questions) extracted from episodes, installable as Claude/Agent Skills (`npx skills add RefoundAI/lenny-skills`). Likely the repo recalled. |
| MCP servers | Catalog access | `akshayvkt/lenny-mcp`, Wire's hosted MCP — chat the catalog from Claude Code/Cursor. |

Sources: x.com/lennysan/status/2011243567340298651 (transcript release) ·
lennysdata.com + github.com/LennysNewsletter/lennys-newsletterpodcastdata ·
lennybot.com · ask-lenny.vercel.app + iamprayerson.com (build writeup) ·
lennysgraph.vercel.app · github.com/RefoundAI/lenny-skills + refoundai.com/lenny-skills ·
github.com/akshayvkt/lenny-mcp. (Front-ends 403'd automated fetch → live status
*medium* confidence; GitHub repos + Lenny's announcement tweets *high* confidence.)

### Gift 1 — a free, pre-built "what good looks like" knowledge layer
This solves the §2 rubric-grounding problem. **Athena's critic agent grounds feedback
in two sources: (a) your actual behavior from your work exhaust, and (b) the external
standard from the Lenny corpus / `lenny-skills`.** Example output:
> "You shipped without testing the riskiest assumption — here's what Torres says
> about that, cited to episode X."

That turns a vague LLM critique into an **evidence-grounded, citable** one — exactly
the anti-hallucination (Self-RAG) pattern from §3. The §2 rubric stays the scoring
backbone; the Lenny layer supplies the *retrievable supporting evidence and language*.

### Gift 2 — a zero-privacy-risk warm-up build
Want to learn the RAG / MCP / eval stack before pointing it at sensitive work data?
Build your own **"chat Lenny's podcast with citations"** over the open transcripts:
same engine, public data, no privacy stakes. Then swap the corpus for KraneBrain.
A clean on-ramp to the real thing.

**Caveat:** `lenny-skills` is third-party extraction of copyrighted content — fine
for personal use; flag it if Athena ever becomes more than personal.

---

## 8. If this becomes an open-source product

> **DEFERRED — read as "someday, not now."** Decision is personal-first (§A). Kept
> here because building v2 *decoupled* (connectors, rubric-as-config) makes a future
> OSS rebuild cheap — but do **not** let this complexity leak into v0/v1.

Going OSS (ship a framework; users connect their own data sources) is mostly
*decoupling* — and it's good hygiene you'd want anyway. Build it decoupled from
day one and "personal tool" and "OSS framework" are the same codebase at different
maturity. What changes:

1. **The connector becomes the core abstraction.** Don't hardcode KraneBrain/Linear/
   Slack — define a connector interface and ship connectors as plugins. Steal the
   pattern from **Onyx (formerly Danswer)** / **Airbyte**: each connector declares a
   JSON-Schema config + credential loader, implements a full `load` + an incremental
   `poll` with a framework-persisted cursor, and normalizes every source to one
   Document/event schema. (Onyx: MIT core, 40+ work-tool connectors, capability
   mixins `Load`/`Poll`/`Checkpointed`.)
2. **Rubric & ontology become swappable declarative files, not code.** Your PM rubric
   is opinionated; others want eng-manager / founder / designer rubrics. Make them
   YAML/markdown resolved through a pluggable interface (Cognee's `BaseOntologyResolver`
   + OWL is the model). Bonus: same mechanism lets the engine serve recovery-reviews
   with a different "rubric."
3. **Local-first + BYO-model/keys is the whole pitch, not a feature.** Nobody pipes
   their Slack into your server. Full-local (Ollama) or user's own keys; the universal
   seam is an **OpenAI-compatible base-URL** so local and hosted are interchangeable.
   This is the differentiator vs. Ariso/BetterUp (closed cloud you must trust).
4. **Engine ⟂ interface.** Engine (ingest → memory → critic → eval) as a library/
   service; interfaces (MCP, CLI, web digest) as thin layers.
5. **Can't bundle a copyrighted "standard" corpus.** Ship the framework; let users
   point at their own corpus or the official Lenny data pack. The rubric (synthesis of
   public frameworks) is fine; someone else's extracted transcripts/skills are not.
6. **Ship the eval harness + sycophancy probes** so others can trust their deployment.
7. **Telemetry off by default** (Khoj ships it on and users hate it — easy trust win).
8. **Licensing:** MIT-core + proprietary `ee/` (Onyx/Morphik/Cognee open-core model)
   to maximize adoption, *or* pure AGPL to block SaaS rehosting. Pick based on whether
   you value adoption or rehosting protection.

Sources: github.com/onyx-dot-app/onyx (connectors README, LICENSE, MCP docs) ·
Airbyte/Singer protocol specs · docs.cognee.ai (ontology, MCP) · github.com/khoj-ai/khoj
(telemetry) · opencoreventures.com (AGPL vs MIT).

---

## 9. Technical architecture & where it lives

> **v1 simplification (current direction):** Tyler works through **Claude Code**,
> which already has the Slack/Linear connectors and reads the vaults — so **v0/v1
> need no separate server**: a Claude Code skill + files-as-memory in KraneBrain *is*
> the architecture. The standalone-MCP-engine below is the **v2 graduation target**,
> worth building once value is proven (and because it's the part where you learn
> knowledge graphs / temporal memory / evals). Claude Code is the primary MCP client;
> BrittBot/OpenClaw is a secondary consumer.

**v2 decision: build a standalone, independently-versioned engine exposed over MCP —
not a sub-agent buried inside Hermes or OpenClaw.** Claude Code (primary) and
BrittBot/OpenClaw (secondary) then consume it.

*Context (verified via GitHub API):* "Hermes" and "OpenClaw" are large public
frameworks you run instances on, not private naming:
- **OpenClaw** (`openclaw/openclaw`, P. Steinberger, ~376k★) — local-first personal-
  assistant harness; markdown-on-disk memory, skills as markdown+YAML, **native MCP**.
  **BrittBot is your OpenClaw instance** (personal).
- **Hermes Agent** (`NousResearch/hermes-agent`, ~176k★) — self-improving harness with
  persistent memory, agent-created skills, **isolated sub-agents** (`delegate_task`)
  and **MCP toolsets** (`inherit_mcp_toolsets`). Your work agent. (Distinct from the
  "Hermes 4" open-weight *model* family, also Nous.)

Both natively support **MCP servers + sub-agents**, so the "tools vs agents" consensus
applies directly:
- Reusable capability multiple agents want → **standalone MCP engine** (single source
  of truth, independently versioned, callable by work Hermes *and* personal BrittBot,
  and open-sourceable).
- **Sub-agent only for context isolation** → recommended **hybrid**: MCP engine, each
  harness optionally wraps it in a *thin sub-agent* so heavy ingest/reasoning doesn't
  pollute the main thread.
- Avoid **"MCP context overload"** (documented failure mode): expose a *tight* tool
  surface — `get_weekly_review`, `coach_on(topic)`, `query_decision_history` — not 40
  tools.

**The engine, layered:**
1. **Connectors** — KraneBrain (Obsidian md), Linear (GraphQL), Slack, Claude history;
   Onyx/Airbyte pattern (config + creds + full `load` + incremental `poll` + cursor) →
   one normalized Document/event schema.
2. **Storage** — vector index + temporal memory (Zep/Graphiti or Mem0) + light
   People/Decisions/Initiatives knowledge graph (swappable ontology).
3. **Analysis** — rubric scorer (rubric-as-config) + **separate critic agent** grounded
   in retrieved evidence: *your behavior* + the *external standard* (Lenny corpus).
4. **Eval** — golden set + sycophancy probes (§4).
5. **Scheduler** — daily scan / weekly Friday Review.
6. **Interface** — MCP server (tight surface) + CLI + web digest.

**Build vs reuse:** Onyx is the substrate to study/fork (MIT, 40+ connectors, ships an
MCP server) — add temporal memory + rubric + critic + eval on top instead of rebuilding
ingestion. Leaner alt (more learning, less weight): borrow Onyx's *connector interface*
and build a slim custom engine. **Fastest bootstrap:** if Hermes/KraneBot already
ingests work data, read **KraneBrain** (the vault already holds transcripts) for v1,
then add first-class connectors for the portable/OSS version.

**Naming flag:** "Athena" collides with **AWS Athena** — pick a distinct name before
it's public.

Sources: GitHub API (openclaw/openclaw, NousResearch/hermes-agent) · openclaw.ai docs ·
hermes-agent.nousresearch.com/docs · eclipsesource.com (MCP context overload) ·
cra.mr "MCP, Skills, and Agents" · cdata.com (single-agent+MCP vs multi-agent).

---

## 10. Open questions for the full-access session

- What's actually in KraneBrain? (transcript format, frontmatter, is there any
  decision log today?) — determines how much of dims 6/7 are measurable now.
- Linear: do you use cycles/projects/estimates consistently enough to measure
  effort-to-priority alignment (dim 8)?
- How much Claude chat history is exportable, and in what format? (richest source
  of *thinking*, not just outputs.)
- Local model horsepower available vs. willingness to send redacted data to a
  frontier model.
- Build it standalone, or fold into the BrittBrain memory layer you already have?

---

*Research method: 8 parallel web-search agents (work-exhaust tools · AI coaching
landscape · PM excellence frameworks · RAG/KG architectures · subjective-output
evals · Lenny ecosystem · OSS BYO-data frameworks · OpenClaw/Hermes + agent
architecture), sources fetched and claims confidence-rated. Several primary domains
(arxiv, svpg, producttalk, amazon, intercom) 403'd direct fetch; their figures
come from search-result extracts of the correct URLs — flagged inline where the
exact number is study/vendor-specific rather than universal.*
