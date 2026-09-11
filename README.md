# KERF Agent Skill

[KERF](https://kerf.au) is an online waterjet cutting service in Melbourne, Australia: upload a file, see the price in seconds, pay online, and the flat parts are cut in Melbourne and shipped Australia-wide.

This repository holds the KERF MCP configuration and the KERF Agent Skill, `skills/kerf`, for Claude Code, Codex, Cursor and any other agent that supports remote MCP servers and skills. It covers file preparation, materials and thicknesses, what moves the price, ordering and shipping, and the meaning of every check the quote workspace runs. It is written from the same guides a person reads at kerf.au, and it works with no connection to KERF at all.

## Install

```bash
npx plugins add kerf-au/kerf-plugin
```

or the skill alone:

```bash
npx skills add kerf-au/kerf-plugin --skill kerf
```

For the MCP tools in Claude Code, then `/mcp` and Authenticate (you sign in on kerf.au):

```bash
claude mcp add --transport http kerf https://kerf.au/mcp
```

Claude Code may ask before the `checkout` tool runs, since that is the one call that creates something outside the conversation (an unpaid draft in the shop, which expires on its own). Approve it when asked, or add `mcp__kerf__checkout` to the allow list in your Claude Code permissions. Every other tool declares itself read-only or non-destructive and runs without a prompt.

In Codex:

```bash
codex mcp add kerf --url https://kerf.au/mcp
codex mcp login kerf
```

Claude's and ChatGPT's custom connectors sign in the same way: add `https://kerf.au/mcp` as a custom connector and approve it on kerf.au. For a machine with no browser, create an API key in your kerf.au account card and send it as `Authorization: Bearer` (Claude Code `--header`, Codex `--bearer-token-env-var KERF_API_KEY`).

## What the skill can and cannot do

- It can tell you whether KERF can cut a part, which stocked materials and thicknesses suit it, how to prepare the file so it prices first time, what a warning or refusal on a part card means, and how ordering and delivery work. It then sends you to https://kerf.au/quote, where you upload, confirm the measured size, choose the spec and pay.
- With the MCP server connected (`.mcp.json` here points at `https://kerf.au/mcp`; sign in when your agent asks) it can also upload a file, read the price, build a quote, return the checkout link and check an order. It never pays: checkout returns a link and the person pays.
- It never states a price, a tolerance or a stocked material that the site does not. If a fact is not documented, it says so and points to hello@kerf.au.

## Layout

- `skills/kerf/SKILL.md`: the entry point and the rules.
- `skills/kerf/references/`: services and limits, file preparation, materials and thicknesses, ordering and policies, and the workspace's warnings and refusals with their documented fixes.
- `skills/kerf/references/tool-results.md`: what each MCP tool returns.
- `.mcp.json`: the MCP server connection.
- `.plugin/plugin.json`: the plugin manifest.

More at https://kerf.au/guides/developers. Questions to hello@kerf.au with "Developers" in the subject.

MIT licence. KERF CUTTING, ABN 94 497 289 449.
