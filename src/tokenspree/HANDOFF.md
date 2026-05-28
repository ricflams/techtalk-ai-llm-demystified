# Gone in 60 Seconds — Claude Code Handoff

## What this is
An interactive token-spending game for a tech talk ("AI Demystified / Tech Talk #182").
Audience clicks spending sprees to see how Pro session quota drains across 40 turns.

## File
`gone-in-60-seconds.html` — fully self-contained, one CDN dependency (Chart.js 4.4.1).
Open directly in a browser. No build step.

## How to run
```bash
open gone-in-60-seconds.html          # macOS
# or serve locally for font loading:
npx serve . && open http://localhost:3000
```

## Architecture
Single HTML file. Three logical sections:

**1. PARAMS object** — ten editable parameters, each `{ label, value }`.
All simulation functions read from PARAMS at call time, so changing a value
and calling `redraw()` is sufficient.

**2. Simulation functions**
- `simulate(p)` — 40-turn turn-by-turn cost model, returns array of `{t, cumCost, cacheBusted}`
- `computeQuotaHits(p)` — multi-session model: first turn of each resumed session always cold cache,
  then bust-cache fires every `PARAMS.bustEvery.value` turns within session.
  Returns array of absolute turn numbers where quota is hit, stopping when next hit > TURNS.
- `buildCurves()` — assembles band chain. Expensive model (s2) always stacked last
  so its band = 2× everything beneath it.

**3. Chart rendering**
- Chart.js `type:'linear'` x-axis, labels 1..40
- `bandPlugin.beforeDatasetsDraw` draws grey baseline fill + coloured spree bands directly on canvas
- `bandPlugin.afterDraw` draws red quota-hit ticks below x-axis
- Only two Chart.js datasets: quota line + total line (drawn on top of plugin fills)
- Tooltip: fixed callout box at top of chart with SVG dashed line to cursor

## Cost model
```
histCost = cached ? hist * 0.1 : hist        // 10% if cached, 100% if not
inC = histCost + docTok + mcpTok + systemTok + inputTok
outC = (outputTok + thinkingTok) * 5         // output weighted 5x
turnCost = (inC + outC) * modelMult
hist += inputTok + outputTok                 // grows each turn
```

Bust-cache spree: history billed at 100% on turns where `t % bustEvery === 0`.
Multi-session: turn 1 of each resumed session always cold (5hr break = expired cache).

## Known issues / TODO
- [ ] Fonts: load via `<link>` tag in `<head>` (already done), but if Syne looks wrong
      try `npx serve` instead of `file://` — Google Fonts needs HTTP
- [ ] Settings panel: currently below chart; could move to a collapsible sidebar section
- [ ] Spree effect lines don't update when params change mid-session (only on next reveal)
      — fix: call `updateCard` for all revealed cards after param change
- [ ] The chart height sizing (`sizeChart`) uses sidebar height minus bottom row height;
      may need tuning if settings panel grows
- [ ] No animation on spree activation — could add a smooth curve transition

## Pending features discussed but not built
- Export/screenshot button
- "All sprees" shortcut button
- Annotation showing "you are here" in a real session context
- Compaction toggle (removed: compaction threshold ~160K raw history = never fires in 40 turns)
- 200K vs 1M context toggle (removed: same reason — threshold never reached in 40 turns)

## Talk context
Presentation: "Tech Talk #182: AI Demystified" at Siteimprove.
Slide arc: tokenization → embeddings → transformer mechanics → context/API → tool calling
→ security → RAG → agents. This widget is the "cost of context" segment.
The game runs as a live demo — presenter clicks sprees in order while audience guesses
what each one does before the reveal.

Spree reveal order for the talk:
1. Expensive model (3×) — "just change the model dropdown"
2. 5 MCP services — "you loaded your tools"
3. Long outputs — "you asked for a plan"
4. Think harder — "extended thinking: you're paying for invisible tokens"
5. Bust the cache — "you walked away for coffee"
6. Big doc up front — "the PDF you uploaded on turn 1 is still here"

---

## Marp + GitHub deployment

See `repo-structure.md` for full setup. Summary:

**Deploy target**: GitHub Pages via `deploy.yml` workflow.
Game lives at `https://USERNAME.github.io/REPO/game/`, slides at root.

**In the Marp slide** (`game-slide-template.md`):
- `<iframe>` loads the game during presentation (requires `--html` flag)
- Markdown screenshot + link below is the GitHub.com fallback
- Replace `YOUR_GITHUB_USERNAME` and `YOUR_REPO_NAME` in the template

**iframe sizing fix needed** (add to Claude Code session):
> "Make the game iframe-friendly for Marp slides (1280×720 viewport):
>  remove body padding, ensure layout fills 100vh with no overflow,
>  chart height calculation should account for being inside an iframe"

**Local presentation command**:
```bash
marp --html slides.md --preview
```
