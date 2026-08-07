---
name: what-to-use
description: Route a request to the best assets in the user's own installed stack (skills, subagents, plugins, MCPs, CLIs) and hand back a ready-to-paste prompt that uses them well. Trigger on "what should I use", "what skill for this", "how do I do this with my stack", "what do I have for", "best way to do X", "am I using the right tool", "route this", or when the user is clearly about to do something manually that an installed asset does better. Do NOT use when the request is too vague to route (clarify it first) or when the target is a different AI tool such as Midjourney, ChatGPT, or Cursor.
---

# What To Use

## Objective

Answer "given everything I have installed, what is the actually best way to do this?" with a named recommendation and a prompt the user can paste immediately. They have far more installed than anyone can hold in their head. The value here is recall and composition, not generic advice.

## Narrow to a crew first

Before searching, sort the request into a crew: Research, Content, Design, Motion, Engineering, Business, or System. Deciding the crew first cuts the candidate set from roughly 100 assets to about 15, which makes the rest of this cheap and the answer more confident. If the request spans crews, that is usually a signal to recommend a pipeline rather than a single asset.

If the user keeps a crew map (a hand-maintained index of their stack grouped by crew, with the pipelines that already work for them), read it first and use it as the index. Ask for its path once and remember it. Treat it as an index, not the source of truth: confirm anything you plan to name against the live inventory below, since a hand-written map lags the machine.

## Read the live inventory. Never answer from memory.

The stack changes weekly, so a hardcoded list is wrong within days. Look before recommending:

- Skills: `ls ~/.claude/skills/`, then grep the `description:` line of the candidates
- Subagents: `ls ~/.claude/agents/`, then read their frontmatter
- Plugins: `claude plugin list`
- MCPs: `claude mcp list`
- CLIs: check PATH for whatever the task would need (`gh`, `stripe`, `gcloud`, `ffmpeg`, and any project-specific tooling)

Grepping is cheaper than guessing wrong. Never name an asset without confirming it exists.

## Deliverable

- **Top pick**, named exactly, with one line on why it beats the alternatives
- **The chain**, in order, when more than one asset applies
- **A ready-to-paste prompt** in a code block, written so the assets actually fire
- **What not to use**, when something plausible-looking would be the wrong call
- **How to invoke it**, per the rules below

## Invocation rules (the most common source of confusion)

- **Skills** in `~/.claude/skills/` are slash-invokable by their exact name, for example `/carousel` or `/short-form`. Natural language works too, since descriptions auto-trigger.
- **Plugin skills** carry their plugin prefix where one exists, for example `myplugin:schema-markup`.
- **Subagents** in `~/.claude/agents/` have **no slash form**. Ask for them by name inside a sentence ("have the security reviewer look at this"), or just describe the task and let auto-delegation fire.
- **MCP tools** are never slashed. Describe the action and the tool is selected.

## Chain shapes that tend to work

These are shapes, not asset names. Match each role to something you confirmed in the live inventory, and drop any role the user has nothing for rather than inventing a stand-in.

- **Trend to content**: a trend or listening tool finds the signal, a strategy skill picks the angle, a per-platform content skill produces, an SEO skill hardens anything written.
- **One source to many pieces**: an ingest skill pulls the source in, an atomizer splits it into claims, then per-platform skills finish each cut.
- **Draft to publishable**: a content skill drafts, an AI-tell auditor strips the giveaways, a voice or character pass adds personality, and for long-form a structure critic checks the arc holds.
- **Research that compounds**: search local or indexed notes first, fan out to web research only for the gaps, then write the synthesis back into the notes so the next run starts ahead.
- **Engineering**: scope it, spec it, plan it, implement, review, ship. Most engineering plugin suites expose one asset per stage; name the ones actually installed.
- **Anything big or read-heavy**: decide model and parallelism first. Escalate to a stronger model for judgment, fan out to cheaper workers for volume.

## Done looks like

The user can act without a follow-up question. They know what to type, why that asset won, and what they are deliberately skipping. Recommend at most three assets; a list of ten is the same as no answer.

## Verify before answering

Confirm each recommended asset exists in the live inventory, and that a subagent was not presented as slash-invokable. If nothing installed genuinely fits, say so plainly and suggest the direct approach. Forcing a skill onto a task it does not suit is worse than no recommendation.

## Boundaries

- This skill routes within the user's own stack. It does not write prompts for other AI tools (Midjourney, ChatGPT, Cursor); hand that to a prompt-authoring skill if one is installed.
- It does not clarify vague requests. If the ask is too fuzzy to route, clarify first (or hand off to a prompt-sharpening skill), then route.
- It matches capability, not cost. Model choice and parallelism belong to whatever the user uses for that.
- When two assets tie, prefer the more specific one, and prefer the user's own authored skills over third-party equivalents, since those are tuned to their files, their paths, and their voice.
