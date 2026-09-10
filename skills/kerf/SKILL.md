---
name: kerf
description: Prepare and check flat parts for KERF, the online waterjet cutting service in Melbourne, Australia (kerf.au). Use when the user asks whether KERF can cut a part; how to prepare a DXF, DWG, SVG, AI, PDF, EPS, STEP or IGES file for cutting; which materials and thicknesses KERF stocks (aluminium, stainless, mild steel, corten, brass, copper); what moves the price; how ordering, pickup in Dandenong South, Australia-wide shipping, lead time, reorders or tax invoices work; why a file was refused or warned about; or how to design a part for waterjet cutting. Uses the KERF MCP tools when connected (upload, price, quote, checkout link, order status) and works without them; prices come only from a quote.
---

# KERF

KERF cuts flat parts from your file on a waterjet in Dandenong South, Melbourne, and ships Australia-wide or hands them over for free pickup. Upload at https://kerf.au/quote, the price appears in seconds, pay online, parts are cut within 9 business days of payment.

Use this skill as the single entry point for KERF. Use the MCP tools (server `https://kerf.au/mcp`, API key from the person's kerf.au account) when the task uploads a file, prices parts, builds a quote, returns a checkout link or checks an order. Use the references for fabrication facts. Without a connected MCP, everything that needs a price goes through the person opening https://kerf.au/quote in a browser.

Do not invent tolerances, kerf widths, minimum feature sizes, lead times, prices, discounts or stocked materials. If a fact is not in these references, say it is not documented and point the person to hello@kerf.au. Never state a price or a price range; the live quote is the only price.

## What KERF does and does not do

- Cuts flat parts, one outline per file, from nine stocked metals in thicknesses from 1.6 mm to 12 mm. See [materials.md](references/materials.md).
- Takes eight file formats up to 25 MB each. See [file-preparation.md](references/file-preparation.md).
- Does not bend, fold, tap, weld, machine, engrave, coat or paint. Edges ship as cut, or deburred on parts up to 200 mm in any dimension.
- Does not price bent, folded or machined 3D parts automatically. A bent part needs its flat pattern exported, or an email to hello@kerf.au.
- Thicker material, other metals and non-metals are by request to hello@kerf.au, not through the instant quote.

## Quote with the MCP tools

1. Confirm the files, quantities, material, thickness and finish. Ask only for what affects the price. A missing finish means as cut.
2. Call `find_materials` with the material and thickness. A `no_exact_match` lists the stocked thicknesses: show them and ask. Never substitute.
3. Call `create_upload` once per file and PUT the file bytes to `upload_url` from a shell. Do not read the file into the conversation or send base64 through a tool. The PUT response is the measured part; if `cuttable` is false, explain `errors` in the workspace's words (dfm-review.md) and stop there for that part.
4. Call `create_quote` once with the part ids and exact material, thickness and quantity. Use one `idempotency_key` per quote so a retry cannot create a duplicate.
5. Report the quote id, each line's unit price and quantity, the total, `holdUntil`, and `url`. Say the total is for the parts only and delivery is added at checkout. Quote the numbers exactly.
6. If the person wants to order, call `checkout` and hand them `checkoutUrl` with the total. You never pay and never say an order was placed until `get_order` shows it paid.

Read [tool-results.md](references/tool-results.md) before handling `no_exact_match`, `thickness_not_stocked`, `cuttable: false`, `problems`, `quote_locked`, idempotency or any field that is not clear.

## Help someone get a quote without the MCP

1. Classify the part: flat, constant thickness, one closed outer outline with closed holes. If it has bends, a varying thickness or a 3D form, say so and offer the flat-pattern route.
2. Check the file against [file-preparation.md](references/file-preparation.md): format, units, closed contours, text converted to outlines, nothing but cut geometry in the file, one part per file.
3. Check the spec against [materials.md](references/materials.md): the material is stocked in the thickness asked for. Never substitute a nearby grade or thickness silently; show the stocked options and ask.
4. Check the limits in [services.md](references/services.md): 25 MB per file, 4,000 mm in any dimension, 100 lines per quote, 10,000 per line, parcel limits for delivery.
5. Tell the person exactly what to do at https://kerf.au/quote: upload, confirm the measured size on the part card, pick material, thickness, finish and quantity, read the price, Checkout. No account is needed. Quantity is set on the quote, not by repeating the outline in the file.
6. If the workspace flagged the file, read [dfm-review.md](references/dfm-review.md) and explain the flag in the site's own words. Do not guess at fixes the references do not give.

## Answer ordering questions

Use [ordering.md](references/ordering.md) for prices and GST, the 14-day price hold, the 7-day life of an unpaid checkout, the review before cutting, refunds, lead time, pickup, shipping, oversize orders, reorders, tax invoices and file ownership. Use [services.md](references/services.md) for contact details and hours.

Do not say an order has been placed, paid or scheduled unless the person tells you so. Do not promise a delivery date; state the production window and that delivery time is on top.

## Handle uncertainty safely

- Units are the most common mistake. DXF and DWG carry units; SVG, AI, PDF and EPS are measured at their printed size. Always tell the person to check the measured width and height on the part card before ordering.
- A file the checker refuses is not a dead end: the "Get help fixing it" link on the part sends the file to KERF, and hello@kerf.au handles anything the instant quote cannot.
- If a person asks about a material, thickness, finish or process not in the references, say KERF does not list it and suggest the material-request route: hello@kerf.au with the part's size, material and thickness.
- Never ask for, store or repeat payment details, sign-in links or account tokens.

## Load the needed reference

| Need | Read |
| --- | --- |
| Formats, units, contours, layers, text, nesting, STEP and IGES, rejection reasons | [file-preparation.md](references/file-preparation.md) |
| Materials, thicknesses, finishes, weight, what is by request | [materials.md](references/materials.md) |
| What KERF cuts, boundaries, size and quantity limits, lead time, pickup, shipping, contact | [services.md](references/services.md) |
| Pricing factors, price hold, checkout, payment, review, refunds, reorders, invoices, IP | [ordering.md](references/ordering.md) |
| Warnings and refusals the workspace shows, and the documented fixes | [dfm-review.md](references/dfm-review.md) |
| MCP tool fields, statuses, suggestions, refusals, idempotency, limits | [tool-results.md](references/tool-results.md) |

Treat the live quote at kerf.au/quote as authoritative for price, measured size and whether a file can be cut. Treat these references as authoritative for everything else, and as the limit of what you may claim.
