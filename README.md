# KERF Agent Skill

[KERF](https://kerf.au) is an online waterjet cutting service in Melbourne, Australia: upload a file, see the price in seconds, pay online, and the flat parts are cut in Dandenong South, then shipped Australia-wide or collected for free.

This repository holds the KERF MCP configuration and the KERF Agent Skill, `skills/kerf`, for Claude Code, Codex, Cursor and any other agent that supports remote MCP servers and skills. It covers file preparation, materials and thicknesses, what moves the price, ordering, pickup and shipping, and the meaning of every check the quote workspace runs. It is written from the same guides a person reads at kerf.au, and it works with no connection to KERF at all.

## Install

```bash
npx plugins add kerf-au/kerf-plugin
```

or the skill alone:

```bash
npx skills add kerf-au/kerf-plugin --skill kerf
```

For the MCP tools in Claude Code:

```bash
claude mcp add --transport http kerf https://kerf.au/mcp --header "Authorization: Bearer $KERF_API_KEY"
```

## What the skill can and cannot do

- It can tell you whether KERF can cut a part, which stocked materials and thicknesses suit it, how to prepare the file so it prices first time, what a warning or refusal on a part card means, and how ordering, pickup and delivery work. It then sends you to https://kerf.au/quote, where you upload, confirm the measured size, choose the spec and pay.
- With the MCP server connected (`.mcp.json` here points at `https://kerf.au/mcp`; set `KERF_API_KEY` to a key from your kerf.au account, see https://kerf.au/guides/developers) it can also upload a file, read the price, build a quote, return the checkout link and check an order. It never pays: checkout returns a link and the person pays.
- It never states a price, a tolerance or a stocked material that the site does not. If a fact is not documented, it says so and points to hello@kerf.au.

## Layout

- `skills/kerf/SKILL.md`: the entry point and the rules.
- `skills/kerf/references/`: services and limits, file preparation, materials and thicknesses, ordering and policies, and the workspace's warnings and refusals with their documented fixes.
- `skills/kerf/references/tool-results.md`: what each MCP tool returns.
- `.mcp.json`: the MCP server connection.
- `.plugin/plugin.json`: the plugin manifest.

More at https://kerf.au/guides/developers. Questions to hello@kerf.au with "Developers" in the subject.

MIT licence. KERF CUTTING, ABN 94 497 289 449.
