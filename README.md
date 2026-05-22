# source-check

**A verification skill that catches the two most common ways AI agents lie: fabricated citations and silently stale information.**

When an AI agent gives you an answer with citations, dates, prices, or "latest" claims, this skill spawns an independent verifier subagent that fetches live sources and checks every claim against fetched text using multi-criteria scientific verification. The verifier never sees the agent's draft until *after* it has read the source — so it can't be talked into agreeing with a fabricated reference.

For Claude Code, Codex, and OpenClaw.

---

## The two problems this solves

**Problem 1 — Citation fabrication.** Empirically severe:
- GPT-3.5 fabricates ~55% of citations; GPT-4 ~18% ([Walters & Wilder 2023](https://www.nature.com/articles/s41598-023-41032-5))
- 11.4–56.8% fabrication rate across 10 commercially deployed LLMs over 69,557 citation instances ([arXiv:2603.03299](https://arxiv.org/abs/2603.03299))
- The fake citations are rhetorically effective — they pattern like real ones, with plausible authors, journals, DOIs.

**Problem 2 — Silently stale information.** Agent answers time-sensitive questions ("latest model", "current price", "today's rate") from training memory instead of fetching, with no signal that the answer is recall-based.

Both share the same root: no live external grounding. This skill enforces grounding with type-appropriate criteria — refusing to skip claims as "unverifiable" (which would be a known pseudoscience pathology, [Hansson SEP 2025](https://plato.stanford.edu/entries/pseudo-science/) §4 "unwillingness to test").

---

## Install

Pick your platform. The skill auto-triggers when your draft contains an external claim — no command to remember.

### Claude Code

```bash
mkdir -p ~/.claude/skills/source-check
cp claude-code/SKILL.md ~/.claude/skills/source-check/SKILL.md
```

The skill will auto-trigger on citations, time-sensitive numbers, "latest/current/recent" language, subjective comparisons, future predictions, private data references, and scientific claims.

### Codex (codex CLI)

```bash
mkdir -p ~/.agents/skills/source-check
cp codex/SKILL.md ~/.agents/skills/source-check/SKILL.md
```

Codex spawns subagents on explicit request — when the skill triggers, it will announce *"Spawning source-check verifiers for N claims"* and you'll see them run.

For best results, define a custom `source_verifier` agent in `~/.codex/agents/` pinned to a different model family than your drafter (e.g., drafter on Claude, verifier on GPT-5.5). Cross-family reduces preference leakage from 28–37% to ±1.5% ([Preference Leakage, ICLR 2026](https://arxiv.org/abs/2502.01534)).

### OpenClaw

```bash
mkdir -p ~/.openclaw/skills/source-check
cp openclaw/SKILL.md ~/.openclaw/skills/source-check/SKILL.md
```

Uses `sessions_spawn` with `context: "isolated"` for true verifier independence. OpenClaw's `agents.list[].subagents.model` makes cross-family verifier setup the easiest of the three platforms — strongly recommended.

---

## How it works

```
Your draft contains a claim
         │
         ▼
┌─────────────────────────────────┐
│ Step 0: classify the claim type │
│ (no skip mechanism — every      │
│ external claim gets verified)   │
└─────────────────────────────────┘
         │
         ├─ factual_citation ──► Verifier (a): fetch URL, retraction check, verbatim quote substring-match
         ├─ time_sensitive   ──► Verifier (b): fetch authoritative + cross-corroboration (≥2 independent domains)
         ├─ subjective       ──► Verifier (c): measured community signal (n≥1000 survey or peer-reviewed benchmark)
         ├─ future           ──► Verifier (d): verify predictor-attribution (not predicted-future-state)
         ├─ private          ──► Verifier (e): faithfulness to user-supplied context (GROUNDED / UNGROUNDED)
         └─ scientific       ──► Verifier (f): Crossref + OpenAlex retraction cross-check, Hansson pathology checks
         │
         ▼
┌─────────────────────────────────┐
│ Verifier returns strict JSON:   │
│ - verdict (from enum)           │
│ - verbatim evidence quote       │
│ - ≥200 chars surrounding context│
│ - tool calls (≥1 different      │
│   domain than the cited URL)    │
└─────────────────────────────────┘
         │
         ▼
You see a headline emoji per claim:
✅ verified · ⚠️ partial · ❌ contradicted · 🔵 insufficient · 🔒 grounded · 🔓 ungrounded
+ source date so you judge staleness yourself
```

**Key design choices** (rationale links to the literature in the skill file):
- **Source-before-claim ordering** — verifier reads the source *before* the agent's claim, to prevent claim-first sycophancy ([SycEval, arXiv:2502.08177](https://arxiv.org/abs/2502.08177) — claim-first ordering = +5pp sycophancy).
- **Different-domain tool call required** — verifier must fetch at least one source from a domain not in the candidate claim's URLs, closing the "fetch only the cited URL and rubber-stamp it" loophole.
- **Verbatim quote with substring-match** — every supported/contradicted finding requires a quote that downstream substring-matches the fetched page. Non-matching = automatic FAIL. This is the **load-bearing anti-hallucination check** (you can't fabricate a quote that passes mechanical substring matching).
- **Retraction cross-check** — DOI claims hit both Crossref `update-to` and OpenAlex `is_retracted`; OpenAlex alone had ~2300 misclassifications late 2023 / early 2024 ([arXiv:2403.13339](https://arxiv.org/abs/2403.13339)).
- **Source date shown, staleness not classified** — verifier records when the source was published; whether that's stale is your judgment, not the AI's. A 2019 numerical-methods result is fresh; a 2019 LLM benchmark is ancient — only you know your use case.
- **No skip mechanism** — claims that *appear* unverifiable (subjective, future, private) get *different criteria*, not skip. Skipping would be pseudoscience pathology #4 (unwillingness to test) dressed as honesty.

---

## Evidence it works

Validated with a pre-registered n=13 experiment ([full details + raw verifier outputs](experiments/v8-validation.md)). Headline results:

| Category | Result |
|---|---|
| **Citation fabrication caught** (fully-fake DOI, fake arXiv ID, wrong author attribution, retracted paper) | **4/4** ✅ |
| **Stale info caught** (model claims as "latest" when it isn't) | **1/1** ✅ |
| **False positive rate on true claims** (correctly verifying real citations) | **3/3** ✅ |
| **Subjective/future/private correctly routed** to non-VERIFIED labels | **3/3** ✅ |
| **Schema compliance** after enum patch | **3/3** ✅ |
| **Different-domain tool call** (anti-rubber-stamp) | **12/12** non-private cases |

**Why the experiment is credible** (not just "the AI passed its own tests"):
1. **Pre-registered predictions** — all 13 verdicts were written down *before* spawning any verifier, so no post-hoc rationalization.
2. **Ground truth independent of the skill** — fabricated DOIs were fabricated by us (so we knew they were fake); the retracted paper (Wakefield 1998 Lancet) has been a public retraction since 2010; the Vaswani 2017 Transformer paper is a verifiable real reference.
3. **Verifiers blind to predictions and ground truth** — each verifier only received the claim and (if applicable) the cited URL.
4. **Positive AND negative controls** — we tested both "should be VERIFIED" cases (to catch false positives) and "should be CONTRADICTED" cases (to catch false negatives). 
5. **Failure surfaced and patched in-loop** — 3 verifiers self-invented verdict labels off-schema; we added one hard rule pinning verdict to the enum; re-ran the 3 cases and all came back enum-compliant.

---

## Limitations (honest)

- **Does NOT catch subtle paraphrase distortions** where the source mostly matches but a qualifier is dropped. That's `audit-loop`'s job (orthogonal skill — can fire alongside source-check).
- **Subjective consensus thresholds (60% / 40–60% / <40%)** are engineering judgment, not literature-derived. Calibrate against your own post-deployment data.
- **Same-family drafter + verifier may share biases.** SycEval found 78.5% sycophancy persistence across model families ([arXiv:2502.08177](https://arxiv.org/abs/2502.08177)). For high-stakes claims, configure cross-family verifiers — easiest on OpenClaw.
- **Niche subjective claims** with no n≥1000 survey available → honest verdict is INSUFFICIENT_EVIDENCE, not "consensus per HN sentiment". The skill refuses to retreat to forum signals.
- **Future predictions are never VERIFIED as "Y will happen"** — only as "X predicted Y on date Z". Future-truth isn't verifiable; predictor-attribution is.
- **Hansson #3 (handpicked examples) and #7 (abandoned explanations)** are NOT mechanizable at minutes scale. Verifier output explicitly lists them in `out_of_scope_pathologies` so silence isn't mistaken for clearance.
- **PubPeer has no public keyless JSON API; Cochrane is auth-gated.** Both partially limit replication-tagging signals for scientific claims.
- **Sample size n=13 in the validation experiment** is small. Real-world use across many drafts will surface failure modes that didn't appear here.

---

## For high-stakes citations: `source-check-max`

If you're working in legal / medical / financial / regulatory domains where one fabricated citation can cause material harm, there's a separate `source-check-max` skill (not in this repo) that adds a second verifier doing URL-rediscovery from author+title+year (instead of fetching the cited URL). Dual-verifier costs ~2× but distinguishes real-but-obscure from fabricated. Validated separately (n=13 paired test, 13/13 predictions matched).

For everyday work, the single-verifier source-check (this skill) is sufficient.

---

## License

MIT. Use, modify, and redistribute freely.

If you find a failure mode in the wild, open an issue with the specific claim + source URL + verifier JSON output — that's the highest-value bug report.
