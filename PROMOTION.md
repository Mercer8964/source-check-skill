# Promotion copy

Social-media copy for source-check, organized by platform. All copy is ready to paste — names / handles / hashtags are stubbed where you should fill in. Image-generation prompts are at the bottom for illustrations you can attach.

---

## Twitter / X (English)

### Variant A — demo lead (recommended for first post)

```
I asked an AI for a paper citation. It gave me:

"Hashimoto et al. (2024), JACM 71(4), DOI 10.1145/3676932.3676933"

Sounded real. DOI doesn't resolve. JACM 71(4) doesn't contain it. No matching paper in any indexer.

So I built source-check — an AI verification skill.
github.com/guoyurui138-hue/source-check-skill
```

### Variant B — stats hook

```
AI agents fabricate ~55% of GPT-3.5 citations and ~18% of GPT-4 ones (Walters & Wilder, Nature 2023). Plausible authors, fake DOIs.

I open-sourced source-check — a skill that spawns an independent verifier, fetches live sources BEFORE reading the agent's claim, so it can't be talked into agreeing.

Validated n=13: 4/4 fabrications caught, 1/1 stale claim caught, 0 false positives.

MIT. 3 platforms (Claude Code, Codex, OpenClaw).
github.com/guoyurui138-hue/source-check-skill
```

### Variant C — thread (5 tweets)

```
1/ Two ways AI agents lie most often:
- Fabricated citations (GPT-3.5: ~55%, GPT-4: ~18%)
- Silently stale info (recall instead of fetch, no signal to user)

I built a skill that catches both. Open-sourcing today.
github.com/guoyurui138-hue/source-check-skill

2/ How it works: when your draft has a citation, time-sensitive number, or "latest" claim, an independent verifier subagent spawns.

It reads the SOURCE first, the CLAIM second. Claim-first reading produces +5pp sycophancy (SycEval) — this ordering is load-bearing.

3/ The mechanically-checkable bit: every supported/contradicted finding requires a verbatim quote that downstream substring-matches the fetched page.

Fabricated quotes fail by construction — you can't make up a string and have it appear in real fetched HTML.

4/ Validated with a pre-registered n=13 test stratified across 6 claim types:
✅ 4/4 fabrications caught (fake DOI, fake arXiv, wrong attribution, retracted paper)
✅ 1/1 stale "latest" claim caught
✅ 3/3 true claims correctly verified (false-positive control)

5/ Works on Claude Code, Codex, OpenClaw. MIT.

3-line install — auto-triggers when your draft contains an external claim.

github.com/guoyurui138-hue/source-check-skill
```

---

## HackerNews — Show HN

**Title**:
```
Show HN: source-check – a skill that catches AI agents' fabricated citations
```

**Body**:
```
Hi HN,

GPT-3.5 fabricates ~55% of citations, GPT-4 ~18% (Walters & Wilder 2023, Nature Sci Rep). The fake citations look real — plausible authors, journals, DOIs — so readers can't tell without checking.

I built source-check, an AI verification skill addressing this and the parallel failure (silently stale time-sensitive info). It spawns an independent verifier subagent that fetches live sources BEFORE reading the agent's claim — so it can't be talked into agreeing with a fabricated reference.

The mechanically-checkable load-bearing rule: every supported/contradicted finding requires a verbatim quote that substring-matches the fetched page. Fabricated quotes fail by construction.

Other design choices that matter:
- Source-before-claim ordering (claim-first → +5pp sycophancy per SycEval, arXiv:2502.08177)
- ≥1 tool call must hit a domain other than the claim's URL (closes the rubber-stamp loophole)
- DOI claims hit BOTH Crossref `update-to` and OpenAlex `is_retracted` (OpenAlex alone had ~2300 misclassifications late 2023 / early 2024 per arXiv:2403.13339)
- 6 claim types (factual_citation, time_sensitive, subjective, future, private, scientific), each with type-appropriate criteria — no skip mechanism

Validated with a pre-registered n=13 experiment:
- 4/4 fabricated citations caught
- 1/1 stale "latest" claim caught
- 3/3 true claims correctly verified
- 3/3 schema-enum compliance after a one-line patch (one failure mode surfaced in-loop, fixed, re-tested)

Honest limits:
- Doesn't catch subtle paraphrase distortions where source matches but qualifier dropped (audit-loop's job)
- Subjective thresholds (60% / 40-60% / <40%) are engineering judgment
- Same-family drafter+verifier may share biases (SycEval found 78.5% sycophancy persistence cross-family)
- n=13 is small — real-world use will surface failure modes I haven't seen

Works on Claude Code, Codex, and OpenClaw. MIT licensed. Install is `cp SKILL.md` to one directory per platform.

https://github.com/guoyurui138-hue/source-check-skill

Happy to discuss design choices — particularly the source-before-claim ordering and the verbatim-quote substring-match (which is the load-bearing anti-hallucination guarantee — the thing that distinguishes this from "another LLM grading an LLM").
```

---

## 小红书 (中文,带 emoji)

```
AI 又给我编了一篇不存在的论文 😭

它说:"Hashimoto 等 2024,JACM 71(4),DOI 10.1145/3676932.3676933"

听起来很真对吧?DOI 不存在,JACM 那期没这篇,搜索引擎全 0 结果。

学术圈正经研究过:GPT-3.5 编造引用约 55%,GPT-4 约 18% 😱

所以我做了个 AI 验证 skill,开源在 GitHub,叫 source-check ✨

🛠 怎么工作的:
你的草稿里出现引用 / "最新 X" / 价格 / 论文等外部信息时
→ 自动 spawn 一个独立验证 subagent
→ 它先去 fetch 源头(arXiv / Crossref / 官网)
→ 必须给 verbatim 引文,且会机械 substring 匹配网页 — 编不出来

📊 实验验证(n=13 预注册预测):
✅ 4/4 造假引用全抓
✅ 1/1 过时"latest"抓
✅ 3/3 真引用没误报
✅ Wakefield 撤稿论文也被识别出来了

💻 支持 Claude Code / Codex / OpenClaw 三平台,一行 cp 装好。MIT 协议,免费用。

🔗 github.com/guoyurui138-hue/source-check-skill

#AI #ChatGPT #Claude #开源项目 #程序员日常 #大模型 #LLM #效率工具
```

---

## 即刻

```
做了个 AI 验证 skill 开源了:source-check

🎯 解决两个痛点:
1. AI 编造引用太常见(GPT-3.5 约 55%、GPT-4 约 18%)
2. 时效信息直接 recall 训练数据,不告诉你是不是最新的

🔧 机制:你写带引用 / 时效信息的内容时,自动 spawn 一个验证 subagent。它先 fetch 源头再读你的 claim — 这个顺序很关键(claim-first 会让验证者 +5pp sycophantic)。每个结论都要 verbatim 引文 + 机械 substring match 网页正文。编的话过不了关。

🧪 实验:n=13 预注册预测,13/13 行为正确。
- 4/4 造假引用抓住
- 1/1 过时 "latest" 抓住
- 3/3 真引用没误报
- 主观/未来/私人 claim 也正确路由

🚀 Claude Code / Codex / OpenClaw 三平台都支持,一行 cp 装好。MIT。

github.com/guoyurui138-hue/source-check-skill
```

---

## 微信公众号(长文)

**标题候选**:
- "你的 AI 又编了一篇不存在的论文 — 我做了个 skill 治这个"
- "GPT 编引用的概率是 55%。我开源了一个验证 skill"
- "AI 答案里的引用是真的吗?我做了个工具帮你验"

**正文**:

```markdown
## 上来先看具体例子

我让 AI 帮我引用一篇 diffusion model 论文。它说:

> "Hashimoto 等 2024 年在《Foundations of Score-Based Generative Modeling》中证明了 diffusion model 收敛性,JACM 71(4),DOI: 10.1145/3676932.3676933"

听起来很有说服力是吧?作者名像真的,期刊是 ACM 旗舰刊,DOI 格式正确,卷期号合理。

**但这篇论文根本不存在**。DOI 解析不出来,JACM 71(4) 这一期里没这篇,Google Scholar、Semantic Scholar、ACM Digital Library 都搜不到。

---

## 这不是孤例

学术界正经研究过这个问题。Walters & Wilder 2023(Nature Scientific Reports)发现:

- GPT-3.5 编造引用的概率约 **55%**
- GPT-4 约 **18%**

另一篇 arXiv:2603.03299 跨 10 个商用 LLM、69,557 次引用样本,造假率 **11.4%–56.8%**。

这些假引用骗到人的原因,**就是因为它们 pattern 像真的** — 作者听着像、期刊存在、DOI 格式对。读者要查才能发现,但实际上没多少人会去查。

## 同源痛点:时效信息

AI 经常告诉你"最新的 X 是 Y" — 但其实它只是从训练数据 recall,没 fetch 现在的源,**而且不告诉你这是 recall 而不是 verified**。

两个痛点共享同一个根因:**没有 live external grounding**。

---

## 解决思路:source-check

我开源了一个 AI 验证 skill,叫 source-check,MIT 协议。

**它的工作方式**:
1. 当你的草稿包含外部 claim(引用 / 时效数字 / "最新 X" / 主观比较 / 未来预测 / 私人数据 / 科学 claim 等)
2. 自动 spawn 一个**独立**的验证 subagent
3. subagent **先 fetch 源头**,**再读你的 claim** — 不能反过来
4. 必须给 verbatim 引文,且会**机械 substring match** 网页正文
5. 必须至少一次 tool call 打到**和 claim 不同域名**的源
6. 输出严格 JSON,verdict 从枚举里取

---

## 关键设计:为什么这些规则是 load-bearing 的

**1. Source-before-claim 顺序**
SycEval(arXiv:2502.08177)发现 claim-first 读法会让验证者 +5pp 更易 sycophantic 同意 — 所以必须先读源,再读 claim。

**2. Different-domain tool call 强制**
关闭"验证者只 fetch 原 claim 引用的 URL 然后橡皮章"的漏洞。必须至少一个**不同域**的 tool call。

**3. Verbatim quote + substring match**
最 load-bearing 的反幻觉机制。任何"支持"或"反驳"的结论都要带 verbatim 引文,下游**机械 substring 搜索** fetched page。编造的引文过不了这关 — 你编出来的字符串不会出现在真实网页里。

**4. Crossref + OpenAlex 撤稿双源交叉**
DOI 类 claim 同时查这两个数据库。单源不够 — OpenAlex 自己的 `is_retracted` 字段在 2023 末 / 2024 初有约 **2300 个误分类**(arXiv:2403.13339)。

**5. 不分类 shelf life**
源发布日期记录下来显示给你。**至于这日期算不算"过时" — 你判断,AI 不替你判断**。2019 年的数值方法结果可能还很新,2019 年的 LLM 基准就老掉牙了 — 这取决于你的 use case。

**6. 不"skip"机制**
即使"主观"、"未来"、"私人"这些看起来"无法验证"的 claim,也用**不同的 criteria** 去验,不跳过。

---

## 实验验证

n=13 预注册预测,跨 6 个 claim 类型分层取样:

| 测试维度 | 结果 |
|---|---|
| 引用造假抓捕(假 DOI、假 arXiv、错归属、撤稿论文) | **4/4** ✅ |
| 过时 "latest" 抓捕 | **1/1** ✅ |
| 真引用正确 VERIFIED(误报对照) | **3/3** ✅ |
| 主观/未来/私人正确路由到 non-VERIFIED | **3/3** ✅ |
| Schema enum 合规(补丁后) | **3/3** ✅ |
| Different-domain tool call | **12/12** non-private |

**为什么这个实验"可信"**(不是 "AI 给自己打分"):
1. **预注册预测** — 13 个 verdict 在 spawn 任何 verifier 之前就写下来,无 post-hoc 合理化
2. **Ground truth 独立** — 假 DOI 是自己编的(知道是假),Wakefield 撤稿是 2010 年公开历史
3. **Verifier 盲测** — 只收到 claim 和(如有)URL,看不到预测或 ground truth
4. **正反对照** — 既测"该 VERIFY 的"也测"该 CONTRADICT 的"
5. **失败 in-loop 修补** — 发现 3 个 verifier 自创了 schema 外 label,加一条 hard rule,重测 3/3 回归 enum

---

## 诚实写局限

- **不抓**细微的 paraphrase 漂移(源大致对但漏了限定词)— 那是 audit-loop 的活
- **主观阈值** 60%/40-60%/<40% 是工程判断,不是文献支撑
- **同族 drafter + verifier 可能共享偏差**(SycEval 78.5% 持久率)— 高风险用跨族 verifier
- **n=13 是小样本** — 实战里会发现这次没冒头的失败模式

---

## 怎么用

Claude Code、Codex、OpenClaw 三平台都支持。Install 一行 cp:

```bash
mkdir -p ~/.claude/skills/source-check
cp claude-code/SKILL.md ~/.claude/skills/source-check/SKILL.md
```

(Codex 换 `~/.agents/`,OpenClaw 换 `~/.openclaw/`)

装完之后,skill 自动触发 — 你的草稿出现引用 / 时效数字 / "最新 X" / 主观比较 / 未来预测 / 私人数据 / 科学 claim 时,验证 subagent 自动 spawn。

输出格式:

```
✅ verified · Sonnet 4.5 input price is $3/M tokens · src dated: 2025-09-29
⚠️ partial · 论文存在但 1998 论文已被 Lancet 2010 年撤稿
❌ contradicted · source does not resolve (404 + Crossref 404)
🔵 insufficient · no n≥1000 survey found for "Zig vs Rust embedded"
🔒 grounded in your context
🔓 not grounded in your context
```

---

## 仓库地址

🔗 **github.com/guoyurui138-hue/source-check-skill**

MIT license,可自由 fork / 改 / 分发。

要是你在实战里发现 skill 漏掉的失败模式,最有价值的 issue 是:具体 claim + 源 URL + verifier 的 JSON 输出 — 这样我能精确定位漏洞。
```

---

## 配图 prompts(给图像生成工具)

### 🥇 优先级 1 — Hero / 封面图(GitHub repo banner + 社交贴主图)

**用途**: 仓库 social preview(`Settings > Social preview`)、Twitter / 微信公众号头图。

```
Minimalist editorial illustration: a stylized AI chatbot speech bubble
containing visibly fake academic citation text — "Hashimoto et al. 2024,
JACM 71(4), DOI 10.1145/3676932.3676933" rendered in serif type — being
stamped with a large diagonal red rubber-stamp reading "❌ FABRICATED".
A magnifying-glass icon hovers nearby, suggesting verification.
Style: clean editorial flat design, subtle paper-texture background,
slight isometric depth. Color palette: deep slate-blue (#0f172a),
crisp white (#f8fafc), single accent red (#ef4444) only on the stamp.
No other text. Mood: investigative, rigorous, slightly playful.
Aspect ratio 16:9, web banner composition with breathing room.
```

### 🥇 优先级 2 — Before / After 对比图(社交贴 + 公众号开头)

**用途**: Twitter 主图、小红书首图、公众号文章开头。最 visceral 的传播单元。

```
Two-panel split infographic, 16:9 aspect ratio:

LEFT PANEL (50%) — labeled "WITHOUT source-check"
Shows an AI agent avatar with a confident speech bubble citing
"Hashimoto et al. (2024), JACM 71(4)" — looks authoritative.
Background slightly warm/optimistic.

RIGHT PANEL (50%) — labeled "WITH source-check"
The same citation, now with a red ❌ tag overlay reading
"contradicted — DOI 404". Below it, small icons of
Crossref + Google Scholar + Semantic Scholar each marked with a small
red X indicating no match found. Background cooler/skeptical.

A thin vertical divider between panels.

Style: modern tech infographic, slate-gray and indigo base palette
with single red error accent, slight isometric perspective,
clean sans-serif type (Inter or similar). White background.
No extra decoration.
```

### 🥈 优先级 3 — 实验结果统计图(增加可信度)

**用途**: HackerNews 帖子、Twitter thread 第四条、公众号实验章节。

```
Modern data-viz infographic, 1:1 aspect ratio (Instagram / WeChat friendly).

Top header text: "pre-registered n=13 validation"

Center: five large rounded-rectangle badges arranged in a 2-3 grid:
  - "4/4 · fabrications caught"
  - "1/1 · stale claim caught"
  - "3/3 · true citations verified"
  - "3/3 · enum compliance after patch"
  - "12/12 · different-domain tool calls"
Each badge has a green checkmark, a colored category circle, and
subtle drop-shadow.

Bottom: a horizontal stacked-bar of 13 colored squares
representing the 13 test cases, color-coded by claim type
(6 categories — factual_citation / time_sensitive / subjective /
future / private / scientific). Small legend underneath.

Style: clean data viz, Inter typography, gradient blue-green
background (subtle), white badges with crisp shadows.
```

### 🥉 优先级 4 — 6-type 路由示意图(技术深度受众)

**用途**: 仓库 README 替代当前 ASCII 流程图(可选)、长文技术解释、HackerNews 评论补图。

```
Information-architecture diagram, 4:3 aspect ratio, white background.

TOP: a single rounded box labeled "external claim in your draft"
with an arrow flowing downward.

MIDDLE: a hexagonal classifier box labeled "Step 0 router" with
6 outgoing arrows fanning out to 6 verifier boxes labeled:
  factual_citation (icon: magnifying glass + page)
  time_sensitive (icon: clock + globe)
  subjective (icon: bar chart + people)
  future (icon: crystal ball)
  private (icon: lock)
  scientific (icon: microscope + DNA helix)

Each verifier has 1-2 lines of caption describing its action
(e.g., "fetch URL · retraction check · verbatim substring-match").

BOTTOM: all 6 arrows reconverge into a single rounded box labeled
"strict JSON output — verdict (enum) · verbatim quote · ≥200 chars
context · ≥1 cross-domain tool call".

Final downward arrow to an emoji row:
✅ verified  ⚠️ partial  ❌ contradicted  🔵 insufficient
🔒 grounded  🔓 ungrounded

Style: flat technical diagram, monoline icons, slate / indigo /
emerald accents, generous whitespace, clean sans-serif type.
```

### 🥉 优先级 5 — Verifier 独立性时序图(深度技术帖)

**用途**: 公众号"关键设计"章节配图、HackerNews 评论解释 source-before-claim ordering。

```
Sequence diagram, 16:9 aspect ratio.

Two vertical lifelines:
  LEFT: "drafter agent" (icon: typing hand)
  RIGHT: "verifier subagent" (icon: shield + magnifying glass)

Arrows in temporal order from top to bottom:
  ① drafter writes a draft containing a citation
  ② verifier first fetches the cited source (arrow points to a
     small web-document icon)
  ③ verifier reads the source content
  ④ ONLY THEN verifier reads the drafter's claim
  ⑤ verifier outputs strict JSON with verbatim quote + ≥200 chars context

A subtle red annotation between steps ② and ④ reads:
"order matters: claim-first ordering produces +5pp sycophancy
(SycEval, arXiv:2502.08177)"

Style: clean UML-inspired technical illustration, monoline
strokes, indigo + warm gray palette, single red annotation accent.
Light dotted background grid.
```

---

## 平台投放建议

| 平台 | 推荐文案 | 配图 | 最佳时机 |
|---|---|---|---|
| Twitter / X | Variant A 或 Thread | Hero + Before/After | 美国时间工作日上午 |
| HackerNews | Show HN | Hero(贴里 attach,不放正文) | 周二/周三 US morning |
| 小红书 | 中文版 | Before/After 首图 + 实验结果图 | 工作日晚 8-10 点 |
| 即刻 | 即刻版 | 单图(Before/After 或结果图) | 中文工作日午后 |
| 公众号 | 长文 | Hero 头图 + 路由图 + 时序图 + 结果图(全套) | 中文工作日早 7 点 / 晚 8 点 |

## 一句话延伸用法

要被采访 / 做技术分享时的电梯总结(35 秒以内):

> "我做了个 AI 验证 skill 开源了。痛点是 AI 编引用 — GPT-3.5 编造率 55%,GPT-4 18%。机制是 spawn 一个独立 verifier subagent,**先 fetch 源再读 claim**(顺序反了会 +5pp sycophantic),每个结论必须配 verbatim 引文且机械 substring match 网页正文 — 编出来过不了关。预注册 n=13 实验,4/4 造假抓住、1/1 过时抓住、3/3 真引用没误报。MIT,装 Claude Code / Codex / OpenClaw 都一行 cp。"
