# KERF MCP tool results

Field-level behaviour of the KERF MCP tools (server `https://kerf.au/mcp`, streamable HTTP, `Authorization: Bearer <API key>`). Every tool returns the same JSON the v1 REST API returns; an error comes back as `isError: true` with `{ error, code }`. Prices are AUD including GST; delivery is added at checkout.

## find_materials

- No arguments: `materials[]` (`id`, `name`, `thicknessesMm[]`, `densityGcm3`, `note`), `finishes[]`, `formats[]`, `limits`, and a `note` while the price book is provisional.
- With `material` and `thickness`: `match_status` is `matched` or `no_exact_match`. On a miss, `suggestions[]` carries every stocked thickness with `match_notes`. Show them and ask; never pick one silently. An unknown material is `material_unknown` with the material list.

## create_upload

- `name` must end in one of dxf, dwg, svg, ai, pdf, step, stp, igs, iges, eps. Returns `upload_id`, `upload_url`, `method` (PUT), `headers`, `expires_in_seconds` (900).
- PUT the raw bytes to `upload_url` from a shell or script (for example `curl -X PUT --data-binary @part.dxf -H "Content-Type: application/octet-stream" <upload_url>`). The URL is single use. The PUT response is `{ part, existing }`: `existing: true` means the same bytes were uploaded before and that part is reused.
- Do not read the file into the conversation or send base64 through any tool.

## part

`id` (starts with `up`), `name`, `format`, `size { widthMm, heightMm }`, `cutLengthMm`, `pierces`, `areaMm2`, `netAreaMm2`, `holes` (text), `cuttable`, `errors[]`, `warnings[]`, `defaults { material, thickness, finish }`, and for STEP `modelThicknessMm`. `cuttable: false` means the part can be quoted but not ordered; `errors` say why in the workspace's own words (see dfm-review.md). A `modelThicknessMm` sets the default thickness; confirm it with the person. Upload refusals: `format_unsupported`, `file_empty`, `conversion_failed` (a bent or unreadable file, with the message), `converter_busy` (retry in a few seconds), `too_complex`, `storage_full`.

## create_quote

- `items[]`: `part`, `material`, `thickness` (mm, a number), `quantity` (1 to 10000, default 1), `finish` (`as_cut` default, `deburred` for parts up to 200 mm), optional `name`. Up to 100 lines.
- Refusals: `thickness_not_stocked` (carries `material` and `stocked[]`: show and ask), `material_unknown`, `quantity_invalid`, `finish_invalid`, `part_not_found`, `too_many_lines`.
- Returns `quote`: `id` (KF-YYMM-XXXX), `name`, `status`, `priceBook`, `holdUntil` (prices held 14 days), `items[]` (`uid`, `part`, `partName`, `material`, `thickness`, `quantity`, `finish`, `priced`, `unitPrice`, `lineTotal`, `weightG`, `size`, `holes`, `heldPrice`), `total` (parts only), `checkoutReady`, `problems[]` (one sentence per line that cannot be ordered), `url` (the quote on kerf.au, shareable with the person).
- `idempotency_key`: reuse only for an identical retry; a replay returns the same quote with `idempotentReplayed: true`.

## get_quote, list_quotes, update_quote

- `get_quote` re-prices now and returns the same shape. `list_quotes` returns summaries.
- `update_quote`: `items[]` with a `uid` update that line; without a `uid` they append; `remove[]` drops lines by uid; `name` renames. A paid quote answers `quote_locked`.

## checkout

- Returns `checkout`: `checkoutUrl`, `orderNumber`, `total`, `totalWeightG`, `expiresAt` (an unpaid checkout lives 7 days), `items[]`, `mode`. Show the total and the link exactly; the person pays there and chooses the delivery service. Nothing is ordered until paid, and you never pay.
- `quote_not_ready` (with `problems[]`) when any line cannot be ordered; `quote_locked` when the quote was already paid.

## list_orders, get_order

`status` is one of `awaiting_payment`, `expired`, `paid`, `shipped`, `cancelled`, `refunded`. Unpaid orders carry `checkoutUrl` and `expiresAt`; paid ones `paidAt`, then `shippedAt`, `tracking { company, number, url }` and `taxInvoiceUrl`. Do not imply an order was placed or paid unless its status says so.

## Limits

1200 calls, 120 uploads, 30 checkouts and 500 MB of uploads an hour per key; 25 MB per file; 4,000 mm per part; a 429 carries `Retry-After`.
