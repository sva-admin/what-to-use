<p align="center"><a href="https://loop.sv-academy.org"><img src="https://raw.githubusercontent.com/sva-admin/claude-skills/main/assets/sv-academy-512.png" width="88" alt="Silicon Valley Academy"/></a></p>

# what-to-use

Routes a request to the best assets in your own installed stack (skills, subagents, plugins, MCPs, CLIs) and hands back a ready-to-paste prompt that actually makes them fire.

> **Where this came from**
> An original skill by Silicon Valley Academy.

## The problem it solves

Once you have fifty or a hundred skills, plugins, and MCPs installed, you stop remembering what you own. So you do things by hand that something installed already does better. This skill reads your live inventory and answers "given everything I have, what is the actually best way to do this?"

## Install

Paste this into Claude Code:

```
Install the what-to-use skill from github.com/sva-admin/what-to-use
```

## Use it

```
/what-to-use I need to turn this customer interview into a week of posts
```

You get back:

- **The top pick**, named exactly, with one line on why it beats the alternatives
- **The chain**, in order, when more than one asset applies
- **A ready-to-paste prompt** written so the assets actually trigger
- **What not to use**, when something plausible-looking is the wrong call
- **How to invoke it**, since skills, plugin skills, subagents, and MCP tools each work differently

## What it is opinionated about

- It never answers from memory. It runs `ls ~/.claude/skills/`, `claude plugin list`, `claude mcp list` and reads what is actually there, because a hardcoded list goes stale within days.
- It never names an asset it has not confirmed exists.
- Three recommendations maximum. A list of ten is the same as no answer.
- If nothing installed genuinely fits, it says so and suggests the direct approach. Forcing a skill onto the wrong task is worse than no recommendation.

## Optional: a crew map

If you keep a hand-written index of your stack grouped by crew (Research, Content, Design, Motion, Engineering, Business, System), point the skill at it and routing gets faster and more confident. It is optional. The skill works without one, and it always verifies the map against the live inventory anyway.

## Learn it free

Full walkthrough, including how to build the crew map: [Find the right tool](https://loop.sv-academy.org/articles/find-the-right-tool)

## License

MIT. See [LICENSE](LICENSE).

---

Part of the [SV Academy Claude skills collection](https://github.com/sva-admin/claude-skills) · [sv-academy.org](https://sv-academy.org)
