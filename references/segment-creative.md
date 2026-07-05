# 創意段 (Creative Segment) — Phase 3 / 4 / 5 / 6 完整流程

> 本檔為創意段的完整操作細節。進入創意段（發想、評估、定稿）的第一動作＝Read 本檔。
> 進場前置（由 SKILL.md 路由執行）：專案子資料夾必須已有 `*_策略對齊表*.md`；載入最新版對齊表＋最新 insight-pass＋dialogue-log 後開工，不重跑策略段。
> 評分量表細節（六權重評分細則、HumanKind 十階詳表、Grey Scale、Gap Analysis、防灌分規則、四視角評審）→ `[[scoring-calibration.md]]`（Phase 4 必 Load）。
> 三層批判憲法完整版 → `[[creative-constitution.md]]`（Phase 4 必 Load）。
> 最終輸出模板（One-Pager／Top-3／Campaign Platform／Quick Brief）→ `[[../assets/output-templates.md]]`（Phase 5 必 Load）。
> 硬規則：HumanKind 低於 7 的創意不得作為最終提案輸出。

### Phase 3: IDEATION (idea generation)

Load: `[[references/methods-catalog.md]]` + `[[references/method-selection-matrix.md]]`

For storytelling tasks additionally: `[[references/storytelling-frameworks.md]]`

**Algorithm:**

0. **Prime against the canon.** Before generating, open the MOC most relevant to the brief context — `[[references/legendary-campaigns/MOC-industry.md]]` (industry match), `[[references/legendary-campaigns/MOC-budget.md]]` (budget constraint), or `[[references/legendary-campaigns/MOC-emotion.md]]` (emotional intent). Scan 5-7 canonical cases. Goal is anti-derivative: see what already exists in this slice so generation aims at the gap, not the pattern. Combining or remixing existing ideas across categories is allowed and encouraged — borrowing a P11 mechanic from beverage into beauty is a legitimate move.

1. Using `method-selection-matrix.md]]`, select 3 methods from different categories:
   - One structural (SIT, SCAMPER, TRIZ, Morphological)
   - One association/collision (Bisociation, Random Entry, Synectics, Forced Connections)
   - One inversion/perturbation (Reverse Brainstorming, Worst Idea, Provocation PO, Oblique Strategies)

2. Generate 8-12 ideas, applying each method

3. Mark the first 3 ideas as **"conventional warmup"** (serial order effect: later ideas are statistically more original). Don't delete them, but bias toward ideas 5-12+

4. Each idea is tied to a specific insight/tension from Phase 2

5. Each idea is formulated in one sentence + 2-3 lines of development

6. **Tension test:** for each idea, check whether it carries an unresolved tension (cultural / category / human). If everything resolves cleanly → originality is weak. The best work lives in the unresolved gap. See `[[references/legendary-patterns.md]]`.

---

### Phase 4: EVALUATE + REFINE (recursive cycle)

Load: `[[references/scoring-calibration.md]]` + `[[references/creative-constitution.md]]`

#### PASS 0: Idea Level Check

Before evaluation, verify: does the level of generated ideas match the `idea_type` requirement from Phase 1? Use the full Pollard 7-level taxonomy from `[[references/idea-taxonomy.md]]`:

- `business` / `brand` — must scale for years, must answer "what does the company stand for?"
- `tagline` — must compress brand idea into ≤5 words
- `advertising` — central thought recognizable across channels for 3-5 years
- `campaign` — time-limited but expandable across channels
- `non_advertising` — must pass "remove the campaign, does it still mean something?" test
- `execution` — specific and implementable

Mismatch = flag and adjust. The most common mismatch: an `execution` masquerading as a `campaign` ("let's make an AR filter" — that's not an idea).

#### PASS 1: Three-axis evaluation

**Axis 1: Brief Compliance (pass/fail)**

8 questions. If even one fails, the idea doesn't pass:

1. Is there an idea? (can be formulated in one sentence)
2. Does it convey the intended message?
3. Does it respond to the insight?
4. Does it suit the target audience?
5. Are mandatory elements included?
6. Does it comply with legislation/ethics?
7. Is the brand voice preserved?
8. Is it supported by product attributes?

**Axis 2: Idea Strength (6 weighted criteria)**

| Criterion | Weight | What is evaluated |
|-----------|--------|-------------------|
| Originality | 0.25 | Unexpected? Have you seen this before? Would 9/10 teams do this? **Empirical check:** open `[[references/legendary-campaigns/MOC-pattern.md]]` for the idea's pattern. If 3+ canonical cases show the same mechanic → cap originality at 7. Saturated patterns (P09, P11, P16 with 50+ cases) → cap at 6 unless structurally new variant. This is empirical saturation, not subjective novelty. |
| Strategic fit | 0.20 | Solves the brief's objective? Hits the TA? |
| Emotional response | 0.20 | Provokes a reaction? Which specific emotion? Use Tier 1/2/3 from `[[references/emotion-hierarchy.md]]`. **Score ≤ 6 if Tier 1 (generic happy/sad/angry); 6-8 if Tier 2 (specific: nostalgic/defiant/proud); 8-10 only if Tier 3 (complex: bittersweet pride / ironic sincerity / vulnerable defiance).** Score 9+ requires Tier 3. |
| Feasibility | 0.15 | Implementable within budget/timeline/constraints? |
| Scalability | 0.10 | Series? Other media? Other markets? |
| Simplicity | 0.10 | Explainable in 10 seconds? One sentence? |

Weighted sum (1-10) = Score.

In parallel: **HumanKind Score** (1-10). Holistic assessment: "acts, not ads."

**Gap Analysis:**
- Score 8+ and HumanKind < 7 = "clever but doesn't matter" → strengthen human impact
- Score < 7 and HumanKind 8+ = "matters but boring" → strengthen craft and originality

**Axis 3: Scalability (4 questions)**

1. How long-lasting is it?
2. Can you move up/down levels of abstraction?
3. Can it be deployed across different channels?
4. Do the executions form a unified system?

**Multi-perspective panel:**
Evaluate from four roles:
- **CD**: craft, originality, simplicity
- **Strategist**: brief fit, insight, TA
- **Consumer**: "is this interesting to me? would I show a friend?"
- **Cannes jury**: award-worthy? cultural impact?

Select **top 3**.

Diagnostics: for each of the top 3, answer "why isn't this a 9?"

#### PASS 2: Targeted improvement (if top < 9.0)

For each of the top 3:
1. Identify weak criteria (below 8)
2. Apply specific improvements to weak areas
3. Use a DIFFERENT method from `[[references/methods-catalog.md]]` (rotation is mandatory)
4. Recalculate Score and HumanKind
5. If delta < 0.3 per pass, the idea has plateaued

#### PASS 3-5: Deep improvement or restart

- Score >= 9.0 AND HumanKind >= 7 → run **Pre-Mortem** (`[[references/legendary-patterns.md#pre-mortem]]`) on the top idea, then EXIT → Phase 5
- Score 7.0-8.9 and improving → continue with a new method
- Score < 7.0 OR plateau → **RESTART with case-soaking.** Don't just rotate methods on the same insight — the insight itself may be weak. Open 3 different MOCs (`MOC-pattern.md` + `MOC-emotion.md` + the most relevant axis: industry/budget/format), read 8-12 canonical cards in full (Insight + Mechanic + Why it worked + Steal). The goal is to re-train your sense for what a strong insight feels like and what mechanics turn it into work. Then return to Phase 2 with new HMWs and Phase 3 with new methods. **Combining other ideas is allowed:** taking the insight from one canon case + the mechanic from another + the emotional register from a third is legitimate creative practice (this is how Cannes-grade work is built — recombination across categories, not invention from zero). Cite the cards you remixed so the lineage is clear.
- Each pass: a different Oblique Strategy as a thinking perturbation

#### Pattern Calibration (before exit)

For the top candidate, run pattern calibration against the case library:

1. Identify the closest pattern (P01-P18) from `[[references/legendary-patterns.md]]`
2. Open `[[references/legendary-campaigns/MOC-pattern.md]]` and scan 3-5 canonical cases under that pattern
3. Articulate: how is your idea different from the canon? What unexpected angle does it bring?
4. **Saturation rule:** if the pattern has 50+ cases (P09, P11, P16) → originality cap = 7. To exceed, the idea must add a structurally new variant, not just a topical refresh.
5. If you cannot articulate a meaningful difference → the idea is sub-canon. Discard or radically reframe.

#### Stopping Criteria

**(a)** Top idea >= 9.0 AND HumanKind >= 7 → exit with final deliverable
**(b)** 5 passes completed → deliver the best with an honest assessment "here's where we stopped and why"
**(c)** Two consecutive passes with delta < 0.2 → convergence, deliver with a note "plateau reached"

---

### Phase 5: ARTICULATE (final output)

Load: `[[assets/output-templates.md]]`

Final deliverable using the template from `[[assets/output-templates.md]]`. Format depends on the request:
- Full cycle → **Top-3 Presentation Format**
- One idea in detail → **Creative Concept One-Pager**
- Strategic platform → **Campaign Platform**
- Quick response → **Quick Brief Response**

---

### Phase 6: DEBRIEF (mandatory, runs after Phase 5)

**Purpose:** Catch brief drift, false insights, and structural gaps AFTER seeing the full output. The most common failure mode is solving the wrong problem well.

Run these 5 questions every time:

**1. Brief audit**
Was the brief asking the right question? Does the delivered idea solve what actually matters to the TA, or what the brief *said* matters?
- Flag if insight in Phase 2 was later contradicted by ideation findings
- Flag if the top idea addresses a symptom, not the root cause

**2. Insight stress-test**
Would the TA hear the insight and say "yes, exactly, but I've never said it that way"? Or would they say "that's not quite it"?
- If uncertain: the insight was probably a hypothesis, not a finding

**3. Blind spot inventory**
What dimensions were NOT discussed that should have been?
- Technical constraints, cultural context, competitive moves, channel realities
- Ask: "What would make this idea fail in the real world that we haven't addressed?"

**4. Sycophancy check**
Did the evaluation get too comfortable with its own ideas? Run the **Kill Your Darlings** test on the top idea: argue AGAINST it for 3 sentences. If the counter-argument is stronger than the defense, the idea is weaker than scored.

**5. Next-iteration note**
One sentence: what would Phase 1 look like if we started over with what we know now?

**DEBRIEF output format:**
```
## DEBRIEF

Brief was: [correct / drifted — how]
Insight held up: [yes / partially / no — why]
Missed dimensions: [list]
Top idea stress-test: [held / cracked — where]
If restarting: [one sentence on what Phase 1 would ask differently]
```

---

