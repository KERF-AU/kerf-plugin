# KERF Agent Skill

[KERF](https://kerf.au) is an online waterjet cutting service in Melbourne, Australia: upload a file, see the price in seconds, pay online, and the flat parts are cut in Dandenong South, then shipped Australia-wide or collected for free.

This repository holds the KERF Agent Skill, `skills/kerf`, for Claude Code, Codex, Cursor and any other agent that supports skills. It covers file preparation, materials and thicknesses, what moves the price, ordering, pickup and shipping, and the meaning of every check the quote workspace runs. It is written from the same guides a person reads at kerf.au, and it works with no connection to KERF at all.

## Install

```bash
npx skills add kerf-au/kerf-plugin --skill kerf
```

## What the skill can and cannot do

- It can tell you whether KERF can cut a part, which stocked materials and thicknesses suit it, how to prepare the file so it prices first time, what a warning or refusal on a part card means, and how ordering, pickup and delivery work. It then sends you to https://kerf.au/quote, where you upload, confirm the measured size, choose the spec and pay.
- It cannot upload a file, read a price or place an order. There is no live KERF API or MCP server yet; when there is, this repository gains an `.mcp.json` and the skill gains the tools. Until then, anything an agent claims to be doing "through the KERF API" is not coming from us.
- It never states a price, a tolerance or a stocked material that the site does not. If a fact is not documented, it says so and points to hello@kerf.au.

## Layout

- `skills/kerf/SKILL.md`: the entry point and the rules.
- `skills/kerf/references/`: services and limits, file preparation, materials and thicknesses, ordering and policies, and the workspace's warnings and refusals with their documented fixes.
- `.plugin/plugin.json`: the plugin manifest.

More at https://kerf.au/guides/developers. Questions to hello@kerf.au with "Developers" in the subject.

MIT licence. KERF CUTTING, ABN 94 497 289 449.
