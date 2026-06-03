---
name: script-five-dimensional-analysis
description: Use this skill when the user asks for short-drama script five-dimensional analysis, episode-by-episode drama breakdown, director-oriented script analysis, AI video generation risk review, Markdown analysis reports, or any Chinese prompt/workflow involving "五维分析", "剧集拆解", "逐集分析", "剧本分析报告", "导演视听拆解", "台词口型", or "AI生成风控".
---

# Script Five-Dimensional Analysis

## Purpose

Use this skill to turn a complete short-drama script into a professional Markdown report for directors, storyboard artists, AI image/video prompt writers, performers, and dubbing/lip-sync teams.

The report must analyze the whole script, not just the first episode. It should combine five dimensions for every episode:

- Narrative rhythm
- Camera and audiovisual design
- Character performance
- Dialogue, dubbing, and lip-sync
- Body action and AI generation risk control

## Quick Workflow

1. Confirm the script source and episode boundaries.
   - Prefer explicit episode headings such as `第一集`, `第 1 集`, or imported episode records.
   - If a script uses scene headings like `1-1`, group them under the closest episode heading.
   - If the source is long, process in batches, but the final report must cover every episode.

2. Read the whole script before analyzing.
   - Identify title, genre, target audience, main conflict, core characters, repeated locations, and high-risk scenes.
   - Do not judge the full drama from only the opening.

3. Use the full prompt template in `references/analysis_prompt.md` when composing the model request.
   - Load it when generating or revising a five-dimensional report.
   - Keep the 12 detailed questions as internal analysis angles, not as a mechanical Q&A list.

4. Produce Markdown.
   - Include an overall judgment section.
   - Include a complete per-episode section.
   - Include an execution summary.
   - If the UI cannot show all text, display a preview but save/download the complete report.

## Output Standard

The report should be execution-oriented, specific, and usable by production teams. Avoid generic summaries.

For every episode, include:

- This episode's function in the whole drama
- Narrative rhythm analysis
- Camera/audiovisual execution suggestions
- Character performance notes by important role
- Dialogue/dubbing/lip-sync guidance
- Body action and AI generation risk controls

For sensitive or risky material, do not expand explicit details. Reframe it as:

- Shootable expression
- Lens avoidance
- Editing implication
- AI generation safety/risk constraints

## Quality Checks

Before finishing, verify:

- Every imported episode has a corresponding analysis section.
- The report has not stopped at episode 1.
- The report is not just plot retelling.
- The five dimensions are integrated with episode-specific characters, scenes, conflicts, and actions.
- The final Markdown headings are easy to parse and split per episode.

## Useful Trigger Phrases

User requests that should trigger this skill include:

- "给这个剧本做五维分析"
- "逐集拆解这个短剧"
- "生成导演组能用的剧本分析报告"
- "分析每集的叙事、镜头、表演、台词、肢体动作"
- "做 AI 视频生成前的剧本风控分析"
- "把五维分析提示词用到这个项目/agent"
