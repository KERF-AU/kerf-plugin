# Materials, thicknesses and finish

Source: the kerf.au quote workspace's material table. Thickness lists, and what is currently unavailable, follow the price book and can change. When the person is at kerf.au/quote, the thickness menu on the part is the truth. With the MCP tools, `find_materials` is. Never present a thickness as stocked unless it is in this table. Never present one as available now if the table marks it currently unavailable. Never quote a price.

## Stocked metals and thicknesses (mm)

How to read the Thicknesses column: the thicknesses listed first can be ordered now. The ones after "currently unavailable" are stocked and listed on the site, but cannot be ordered for now. A row that reads only "currently unavailable" is a material with nothing that can be ordered now.

<!--kerf:materials-table-->
| Material | Site key | Thicknesses | Site note |
| --- | --- | --- | --- |
| Aluminium 5005 | alu5005 | 1.6, 3, 4, 5, 6 | Soft general-purpose aluminium, easy to form, good for panels and signs |
| Aluminium 5083 | alu5083 | 3, 4, 5, 6 (currently unavailable: 8, 10, 12, 16, 20, 25) | Marine grade, lives outdoors untreated, weldable |
| Aluminium 6061 | alu6061 | currently unavailable (4.76, 6.35, 7.94, 9.53, 12.7, 15.88, 19.05, 25.4) | Structural aluminium, strong, machines cleanly, stocked in imperial plate |
| Stainless 304 | ss304 | 3, 4, 5, 6, 8, 10 (currently unavailable: 12, 16, 20, 25) | The everyday stainless, hard-wearing, food-safe |
| Stainless 316 | ss316 | 6, 8, 10 (currently unavailable: 12, 16) | Marine stainless, best corrosion resistance, coastal-proof |
| Mild steel (grade 250) | steel250 | 5, 6, 8, 10, 12, 16 (currently unavailable: 20, 25) | Strong and economical, paint or coat it for outdoor use |
| Brass | brass | currently unavailable (0.8, 1, 1.2, 1.6, 2, 2.5, 3, 4, 5, 6) | Warm gold tone, decorative, develops a patina |
| Copper | copper | currently unavailable (0.7, 0.9, 1.2, 1.6, 2, 3, 4) | Distinctive and conductive, patinas over time |
<!--/kerf:materials-table-->

6061 is stocked as imperial plate, so its thicknesses are the millimetre equivalents of 3/16, 1/4, 5/16, 3/8, 1/2, 5/8, 3/4 and 1 inch. Quote them as the millimetre figures above; they are exact, not rounded.

Anything not in the table (thicker plate, other alloys, non-metals such as stone, glass, tile, rubber or plastics) is by request: hello@kerf.au with the part's size, material and thickness. Do not say KERF cannot cut it; say the instant quote does not price it.

## Currently unavailable stock

- It stays in the catalogue. On kerf.au/quote the option shows in the Material or Thickness menu as "(currently unavailable)" and cannot be picked.
- If the person asks for it, say that material or thickness is currently unavailable. Show what is available now in that material and ask which to use.
- If the whole material is unavailable, say so and ask which other material to use.
- Never substitute a thickness or a material yourself.
- Do not give a reason and do not say when it will be available. Neither is documented.
- In the API and the MCP tools: `thicknessesMm` is what can be ordered now, `unavailableMm` is stocked but cannot be ordered for now, and `available` is false when nothing in the material can be ordered. See tool-results.md.

## Finish

- **As cut** is the only finish. Edges are supplied as cut.
- Every part is supplied as cut. The quote page has no Finish option, so do not ask the person to choose one.
- Deburring is not offered. In a quote item, `finish` may be left out (it defaults to `as_cut`), and `deburred` is refused.
- No coatings, paint, anodising, polishing or powder coating.

## Weight

Part weight is calculated from the net area, thickness and the material's density, and drives the delivery price at checkout. Densities used (g/cm³): aluminium 2.66 to 2.70, stainless 8.00, mild steel 7.85, brass 8.50, copper 8.96.

## What is not documented

Tolerances, kerf width, minimum hole or slot size per thickness, edge taper and surface finish grades are not published. Say so and point to hello@kerf.au.
