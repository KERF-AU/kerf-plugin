---
name: kerf
description: Prepare and check flat parts for KERF, the online waterjet and laser cutting service in Melbourne, Australia (kerf.au). Use when the user asks whether KERF can cut a part; how to prepare a DXF, DWG, SVG, AI, PDF, EPS, STEP or IGES file for cutting; which materials and thicknesses KERF stocks (aluminium, stainless, mild steel, brass, copper); what moves the price; how ordering, Australia-wide shipping, lead time, reorders or tax invoices work; why a file was refused or warned about; or how to design a part for waterjet or laser cutting. Uses the KERF MCP tools when connected (upload, price, quote, checkout link, order status) and works without them; prices come only from a quote.
---

# KERF

KERF cuts flat parts from your file on a fibre laser or a waterjet in Melbourne and ships Australia-wide. Upload at https://kerf.au/quote, the price appears in seconds, pay online, parts are cut within 15 business days of payment.

Use this skill as the single entry point for KERF. Use the MCP tools (server `https://kerf.au/mcp`, API key from the person's kerf.au account) when the task uploads a file, prices parts, builds a quote, returns a checkout link or checks an order. Use the references for fabrication facts. Without a connected MCP, everything that needs a price goes through the person opening https://kerf.au/quote in a browser.

Do not invent tolerances, kerf widths, minimum feature sizes, lead times, prices, discounts or stocked materials. If a fact is not in these references, say it is not documented and point the person to hello@kerf.au. Never state a price or a price range; the live quote is the only price.

## What KERF does and does not do

- Cuts flat parts, one outline per file, from the stocked metals and thicknesses in [materials.md](references/materials.md). Some of that stock is currently unavailable and cannot be ordered for now. The table marks it.
- Takes eight file formats up to 25 MB each. See [file-preparation.md](references/file-preparation.md).
- Does not bend, fold, tap, weld, machine, engrave, deburr, coat or paint. Every part is supplied as cut. There is no finish to choose.
- Does not price bent, folded or machined 3D parts automatically. A bent part needs its flat pattern exported, or an email to hello@kerf.au.
- Material that is not in the table (thicker plate, other metals, non-metals) is by request to hello@kerf.au, not through the instant quote.

## Currently unavailable stock

Some stocked material is listed but cannot be ordered for now. The site and the tools call it "currently unavailable". The table in [materials.md](references/materials.md) marks it, and `find_materials` gives the live answer.

- Say it in those words. Show the person what is available now in that material and ask which to use.
- If the whole material is unavailable, say so and ask which other material to use.
- Never substitute a thickness or a material yourself.
- Do not give a reason and do not say when it will be available. Neither is documented.
- A saved quote keeps a line like this. That line cannot be ordered until the person changes its material or thickness, or removes it.

## Quote with the MCP tools

1. Confirm the files, quantities, material and thickness. Ask only for what affects the price. Do not ask about a finish: there is none to choose, and every part is supplied as cut.
2. Call `find_materials` with the material and thickness. A `no_exact_match` (not stocked in that thickness) or a `currently_unavailable` (stocked, but it cannot be ordered for now) lists the thicknesses available now: show them and ask. Never substitute. An empty list means the whole material is currently unavailable: say so and ask which material to use.
3. Call `create_upload` once per file and PUT the file bytes to `upload_url` from a shell. Do not read the file into the conversation or send base64 through a tool. The PUT response is the measured part; if `cuttable` is false, explain `errors` in the workspace's words (dfm-review.md) and stop there for that part.
4. Call `create_quote` once with the part ids and exact material, thickness and quantity, every one named (the server refuses a line that leaves material or thickness to it). Leave `finish` out: it defaults to `as_cut`, and `deburred` is refused. A thickness that is currently unavailable is refused with `thickness_unavailable` and the list available now: show it and ask. Use one `idempotency_key` per quote so a retry cannot create a duplicate; `checkout` gets a key of its own, never the quote's.
5. Report the quote id, each line's unit price and quantity, the total, `holdUntil`, and `url`. Say the total is for the parts only and delivery is added at checkout. Quote the numbers exactly. If the quote has `problems`, give each one in its own words and ask what to change: a quote with a problem cannot be checked out.
6. If the person wants to order, call `checkout` and hand them `checkoutUrl` with the total. You never pay and never say an order was placed until `get_order` shows it paid.

Read [tool-results.md](references/tool-results.md) before handling `no_exact_match`, `currently_unavailable`, `thickness_not_stocked`, `thickness_unavailable`, `finish_invalid`, `cuttable: false`, `problems`, `quote_locked`, idempotency or any field that is not clear.

## Help someone get a quote without the MCP

1. Classify the part: flat, constant thickness, one closed outer outline with closed holes. If it has bends, a varying thickness or a 3D form, say so and offer the flat-pattern route.
2. Check the file against [file-preparation.md](references/file-preparation.md): format, units, closed contours, text converted to outlines, nothing but cut geometry in the file, one part per file.
3. Check the spec against [materials.md](references/materials.md): the material is stocked in the thickness asked for, and the table does not mark that thickness currently unavailable. Never substitute a nearby grade or thickness silently; show the options available now and ask.
4. Check the limits in [services.md](references/services.md): 25 MB per file, 4,000 mm in any dimension, 100 lines per quote, 100 per line (larger runs by email), parcel limits for delivery.
5. Tell the person exactly what to do at https://kerf.au/quote: upload, confirm the measured size on the part card, pick material, thickness and quantity, read the price, Checkout. There is no finish to pick. A menu option marked "(currently unavailable)" cannot be picked. No account is needed. Quantity is set on the quote, not by repeating the outline in the file.
6. If the workspace flagged the file, read [dfm-review.md](references/dfm-review.md) and explain the flag in the site's own words. Do not guess at fixes the references do not give.

## Answer ordering questions

Use [ordering.md](references/ordering.md) for prices and GST, the 14-day price hold, the 7-day life of an unpaid checkout, the review before cutting, refunds, lead time, shipping, oversize orders, reorders, tax invoices and file ownership. Use [services.md](references/services.md) for contact details and hours.

Do not say an order has been placed, paid or scheduled unless the person tells you so. Do not promise a delivery date; state the production window and that delivery time is on top.

## Handle uncertainty safely

- Units are the most common mistake. DXF and DWG carry units; SVG, AI, PDF and EPS are measured at their printed size. Always tell the person to check the measured width and height on the part card before ordering.
- A file the checker refuses is not a dead end: the "Get help fixing it" link on the part sends the file to KERF, and hello@kerf.au handles anything the instant quote cannot.
- If a person asks about a material, thickness or process not in the references, say KERF does not list it and suggest the material-request route: hello@kerf.au with the part's size, material and thickness.
- If a person asks for a finish, deburring included, say KERF does not offer one: every part is supplied as cut.
- If a person asks why stock is currently unavailable, or when it will be available, say that is not documented and point them to hello@kerf.au. Do not guess.
- Never ask for, store or repeat payment details, sign-in links or account tokens.

## Load the needed reference

| Need | Read |
| --- | --- |
| Formats, units, contours, layers, text, nesting, STEP and IGES, rejection reasons | [file-preparation.md](references/file-preparation.md) |
| Materials, thicknesses, what is currently unavailable, finish, weight, what is by request | [materials.md](references/materials.md) |
| What KERF cuts, boundaries, size and quantity limits, lead time, shipping, contact | [services.md](references/services.md) |
| Pricing factors, price hold, checkout, payment, review, refunds, reorders, invoices, IP | [ordering.md](references/ordering.md) |
| Warnings and refusals the workspace shows, and the documented fixes | [dfm-review.md](references/dfm-review.md) |
| MCP tool fields, statuses, suggestions, refusals, idempotency, limits | [tool-results.md](references/tool-results.md) |

Treat the live quote at kerf.au/quote as authoritative for price, measured size and whether a file can be cut. Treat these references as authoritative for everything else, and as the limit of what you may claim.
