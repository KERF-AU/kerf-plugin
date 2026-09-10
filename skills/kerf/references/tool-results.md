# KERF MCP tool results

Field-level behaviour of the KERF MCP tools (server `https://kerf.au/mcp`, streamable HTTP; sign in with OAuth, or send an API key as `Authorization: Bearer`). Every tool returns the same JSON the v1 REST API returns; an error comes back as `isError: true` with `{ error, code }`. Prices are AUD including GST; delivery is added at checkout.

## find_materials

- No arguments: `materials[]` (`id`, `name`, `thicknessesMm[]`, `densityGcm3`, `note`), `finishes[]`, `formats[]`, `limits`, and a `note` while the price book is provisional.
- With `material` and `thickness`: `match_status` is `matched` or `no_exact_match`. On a miss, `suggestions[]` carries every stocked thickness with `match_notes`. Show them and ask; never pick one silently. An unknown material is `material_unknown` with the material list.

## create_upload

- `name` must end in one of dxf, dwg, svg, ai, pdf, step, stp, igs, iges, eps. Returns `upload_id`, `upload_url`, `method` (PUT), `headers`, `expires_in_seconds` (900).
- PUT the raw bytes to `upload_url` from a shell or script (for example `curl -X PUT --data-binary @part.dxf -H "Content-Type: application/octet-stream" <upload_url>`). The URL is single use. The PUT response is `{ part, existing }`: `existing: true` means the same bytes were uploaded before and that part is reused.
- Do not read the file into the conversation or send base64 through any tool.

## part

`id` (starts with `up`), `name`, `format`, `size { widthMm, heightMm }`, `cutLengthMm`, `pierces`, `areaMm2`, `netAreaMm2`, `holes` (text), `cuttable`, `errors[]`, `warnings[]`, `defaults { material, thickness, finish }`, and for STEP `modelThicknessMm`. `cuttable: false` means the part can be quoted but not ordered; `errors` say why in the workspace's own words (see dfm-review.md). A `modelThicknessMm` sets the default thickness; confirm it with the person. Upload refusals: `format_unsupported`, `file_empty`, `conversion_failed` (a bent or unreadable file, with the message), `converter_busy` (retry in a few seconds), `too_complex`, `storage_full`.

- `list_parts` leaves archived parts out unless `include_archived` is true; a part carries `archived: true` or `resized` (a scale) when the person did that in the workspace. Uploading the same bytes as an archived part brings it back (`existing: true, unarchived: true`). An `upload_not_found` on the PUT can also mean the service restarted since the URL was issued: call `create_upload` again.

## create_quote

- `items[]`: `part`, `material` (required), `thickness` (required; mm, a number), `quantity` (1 to 10000, default 1), `finish` (`as_cut` default, `deburred` for parts up to 200 mm), optional `name`. Up to 100 lines. A part's `defaults` are for the person to confirm; the server never fills a missing material or thickness in.
- Refusals: `material_required`, `thickness_required`, `thickness_not_stocked` (carries `material` and `stocked[]`: show and ask), `material_unknown`, `quantity_invalid`, `finish_invalid`, `part_not_found`, `too_many_lines`.
- Returns `quote`: `id` (KF-YYMM-XXXX), `name`, `status`, `priceBook`, `holdUntil` and `holdStatus` (`current` or `held`: prices held until `holdUntil`; `lapsed` or `unavailable`: `holdUntil` is null and these are today's prices, say so), `items[]` (`uid`, `part`, `partName`, `material`, `thickness`, `quantity`, `finish` as priced, with a `finishNote` when it differs from what was asked, `priced`, `unitPrice`, `lineTotal`, `weightG`, `size`, `holes`, `heldPrice`), `total` (parts only), `checkoutReady` (false once ordered), `problems[]` (one sentence per line that cannot be ordered), `url` (the quote on kerf.au, shareable with the person). A `finish_invalid` refusal with `maxPartMm` means the part is too large to deburr: offer `as_cut`.
- `idempotency_key`: one key per write, reused only for an identical retry; a replay returns the same quote with `idempotentReplayed: true`. The same key with different arguments, or a quote's key reused on `checkout`, is `idempotency_key_reused`: mint a new key. A refusal is not remembered, so after fixing the quote the same key can be retried.

## get_quote, list_quotes, update_quote

- `get_quote` re-prices now and returns the same shape. `list_quotes` returns summaries.
- `update_quote`: `items[]` with a `uid` update that line (the fields given); without a `uid` they append and must name `part`, `material` and `thickness` (`material_required` or `thickness_required` otherwise; `quantity` defaults to 1); `remove[]` drops lines by uid; `name` renames. A paid quote answers `quote_locked`.

## checkout

- Returns `checkout`: `checkoutUrl`, `orderNumber`, `ref` (use either with `get_order`), `total` and `totalNote` (parts only), `totalWeightG`, `expiresAt` (an unpaid checkout lives 7 days), `items[]`, `delivery` (`parcel`, or `quote_freight` with a `deliveryNote` when the order is over the parcel limits: tell the person the checkout offers no parcel rates and freight is quoted after the order), `mode`. Show the total and the link exactly; the person pays there and chooses the delivery service. Nothing is ordered until paid, and you never pay.
- `quote_not_ready` (with `problems[]`) when any line cannot be ordered; `quote_locked` when the quote was already paid; `rate_limited` (with the wait) after 30 checkouts in an hour; `idempotency_key_reused` when the key was used for another request.

## list_orders, get_order

`status` is one of `awaiting_payment`, `expired`, `paid`, `shipped`, `cancelled`, `refunded`. Unpaid orders carry `checkoutUrl` and `expiresAt`; an `expired` one carries `expiredAt` and, when a newer checkout of the same quote replaced it, `supersededBy` (the new ref: that is the live pay link); paid ones `paidAt` (plus `amountPaid` and `shipping` when known), then `shippedAt`, `tracking { company, number, url }` and `taxInvoiceUrl`. `total` is the parts only (`totalNote` says so); `delivery` is `parcel` or `quote_freight`. `get_order` takes the `ref` or the order number as printed (`#K1007`). Do not imply an order was placed or paid unless its status says so.

## Limits

1200 calls, 120 uploads, 30 checkouts and 500 MB of uploads an hour per key; 25 MB per file; 4,000 mm per part; a 429 carries `Retry-After`.
