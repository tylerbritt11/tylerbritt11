# PM Coaching Agent — Concept-Refinement Research Brief

> A personal AI system that ingests a product manager's own *work exhaust*
> (meeting transcripts in Obsidian/KraneBrain, Linear issues/cycles, Slack
> messages, Claude/LLM chat history) and produces meta-reviews + coaching:
> better discovery questions, better sequencing, decision tracking, working
> on the right things, improving over time.
>
> Working name: **Athena** (a PM-craft coach, sibling to the recovery-reviews
> research engine — same ingest → knowledge base → synthesize loop).
>
> Status: concept refinement, **not** an implementation spec. Carry this into
> the full-access session on the computer (where Krane + private repos are visible).

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

## 7. Open questions for the full-access session

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

*Research method: 5 parallel web-search agents (work-exhaust tools · AI coaching
landscape · PM excellence frameworks · RAG/KG architectures · subjective-output
evals), sources fetched and claims confidence-rated. Several primary domains
(arxiv, svpg, producttalk, amazon, intercom) 403'd direct fetch; their figures
come from search-result extracts of the correct URLs — flagged inline where the
exact number is study/vendor-specific rather than universal.*
