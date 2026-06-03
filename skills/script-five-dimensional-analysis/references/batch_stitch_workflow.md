# Multi-Batch Stitching Workflow

Use this template when a short-drama script is too long to analyze in a single turn. The goal is to produce one coherent Markdown report without missing or duplicate sections.

## Overview

```
Script (e.g., 100 episodes)
    │
    ├── Batch 1 (eps 1–15)  →  per-episode analysis fragments
    ├── Batch 2 (eps 16–30) →  per-episode analysis fragments
    ├── Batch 3 (eps 31–50) →  per-episode analysis fragments
    ...
    │
    └── Final stitching pass
         ├── Read all fragments
         ├── Generate overall judgment (once)
         ├── Generate execution summary (once)
         └── Write complete_report.md
```

## Step-by-Step

### 1. Plan the batches

Decide the batch size based on the model's effective context window. For a typical analysis prompt (prompt template ≈ 2K tokens + per-episode output ≈ 0.5–1K tokens), a safe batch size is:

| Model context | Batch size guideline |
|---|---|
| 32K tokens | ~10–15 episodes per batch |
| 64K tokens | ~20–30 episodes per batch |
| 128K+ tokens | ~40–50 episodes per batch, or entire script at once |

Rule of thumb: leave at least 10K tokens free for the prompt template + input script portion.

### 2. Distribute analysis prompts

For each batch, construct the analysis request following this pattern:

```
[analysis_prompt.md template content]

Focus on episodes {BATCH_RANGE} below:

[script content for episodes in this batch only]

Do not write overall judgment or execution summary sections — only produce the "Per-Episode Five-Dimensional Analysis" sections. Use exactly the same heading level as the template (## episode headings).
```

Save each batch's output to a temporary file, e.g.:

- `_batch_eps1_15.md`
- `_batch_eps16_30.md`
- `_batch_eps31_50.md`

### 3. Build the per-episode table of contents

Before stitching, confirm all batch ranges are contiguous and no episode is missing or duplicated. A simple check:

```
Batch 1: eps 1–15  →  episodes 1..15  (15 episodes)
Batch 2: eps 16–30 →  episodes 16..30 (15 episodes)
Total: 30 episodes ✓
```

### 4. Stitch per-episode sections

Concatenate all batch output files in order, stripping any accidental duplicates of:

- Repeated overall judgment can be header text that some models may re-introduce (there should be a single `## 一、全剧整体判断` at top)
- Repeated execution summary (single `## 三、全剧执行总结` at bottom)

The stitched file at this stage should contain **only** the per-episode sections, not yet the meta-sections.

### 5. Final pass: generate meta-sections

Send the full stitched per-episode content to the model with this request:

```
Based on the complete per-episode analysis below, generate:

1. **Overall Judgment** — genre, target audience, core conflict, character relationships, visual tone, AI risk overview (use the template structure from analysis_prompt.md, section "一、全剧整体判断").

2. **Execution Summary** — emotional curve, key explosive episodes, high-risk scenes, storyboard priority, dialogue/lip-sync optimization, character consistency guide (use the template structure from analysis_prompt.md, section "三、全剧执行总结").

Do NOT add or repeat any per-episode analysis sections.
```

### 6. Assemble final report

Combine into one Markdown file:

```
# 《剧名》全集五维深度分析报告

## 一、全剧整体判断        ← from step 5
## 二、逐集五维深度分析    ← from step 4 (all batches concatenated)
## 三、全剧执行总结        ← from step 5
```

### 7. Final validation

Run through the quality checklist in SKILL.md, plus these batch-specific items:

- [ ] Every episode heading present and in correct order
- [ ] No duplicate overall-judgment sections
- [ ] No duplicate execution-summary sections
- [ ] Heading levels consistent (e.g., all use `## 第 X 集` not mixed with `### 第 X 集`)
- [ ] No content gaps at batch boundaries (e.g., episode 15's ending and episode 16's opening feel continuous)
- [ ] At least one read-through to catch style drift between batches

## Common Issues & Mitigations

| Issue | Mitigation |
|---|---|
| Style drift between batches (different tone, detail level) | In final pass, ask model to "lightly normalize tone across all episodes" — but do not let it rewrite away episode-specific content. |
| Batch boundary discontinuity (episode 15 ends abruptly, 16 starts without context) | When prompting each batch, add: "Start your analysis assuming previous episodes have already been covered; do not reintroduce the plot from scratch." |
| Repeated meta-section headers in per-episode batch output | Instruct each batch prompt: "Do NOT write '全剧整体判断' or '全剧执行总结' sections." Strip any that appear during stitch. |
| Model runs out of tokens during final pass | Keep final pass prompt concise; if the stitched per-episode content is still too long, ask the model to extract only structural patterns (character arcs, shot style patterns, risk clusters) rather than rereading every line. |