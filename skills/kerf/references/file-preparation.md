# File preparation

Source: kerf.au/guides/file-preparation, reviewed 4 September 2026.

## Accepted formats

Eight formats, up to 25 MB per file: DXF, DWG, SVG, AI, PDF, STEP, IGES, EPS. Several files can be dropped at once; each becomes its own part, checked and priced on arrival.

Preference order: DXF first (carries real-world units, best understood by the checker, exported by nearly every CAD program); DWG is as good. SVG, AI, PDF and EPS suit artwork and signage from design software. STEP and IGES suit parts modelled in 3D CAD, provided the part is flat.

No file: build the part from a template in the quote workspace, or email a sketch to hello@kerf.au.

## Units and scale

- DXF and DWG carry their units: a 100 mm bracket arrives as 100 mm.
- SVG, AI, PDF and EPS do not carry units; they are measured at their printed size. Artwork laid out at half scale gives a half-size part.
- A file that declares no units is assumed to be millimetres and the workspace says so. An Illustrator export with no physical size is read in points (1 unit = 0.353 mm), the Illustrator convention, with a warning.
- Every part card shows the measured width and height next to the preview. That is the size that is cut. The Resize control fixes a wrong size in one step, with one-click corrections for files drawn in inches or out by a factor of ten.

## Closed contours

The jet follows outlines. The outer edge must be one closed loop and every hole another closed loop. Gaps where lines nearly meet, doubled-up lines and stray segments leave the checker unsure what is part and what is not. The checker flags what it cannot resolve as soon as the file lands, and the part card explains. The "Get help fixing it" link on the part sends the file and the problem to KERF.

## Layers and colours

KERF cuts geometry, not styling. Colours, line weights and fills are ignored; there is no colour code. Send only what should be cut: delete dimensions, notes, title blocks, borders and construction lines before exporting. Anything left reads as a cut.

## Text

Fonts do not travel with a file. Convert text to outlines (paths, curves) before exporting. A file with live text is refused with a note asking for exactly that. The cut goes right through: the middle of an O falls out unless a stencil font or bridges are used.

## Nested parts

One part per file. Each upload becomes one part with its own material, thickness, quantity and price. Quantity is set on the quote; do not repeat an outline ten times to order ten. A drawing with several disconnected outlines prices as a single part, which is rarely wanted; split it into separate files and upload them together.

## STEP and IGES

3D files are checked for flatness before pricing. A flat part prices instantly and, for STEP, the thickness is read from the model (the person confirms it; do not treat it as chosen). Bent, folded or machined parts cannot be priced automatically: export the flat pattern and KERF cuts the blank, or email the file.

## Common rejection reasons

- A scanned or photographed drawing: a picture, not geometry. Redraw in CAD, or email it for a checked quote.
- Live text: convert to outlines and export again.
- AI or EPS saved without PDF compatibility: in Illustrator, save with "Create PDF Compatible File" ticked, or export SVG or PDF.
- Open contours: close the gaps so every outline is a complete loop.
- A 3D file with bends: export the flat pattern, or email the file.
- Wrong size: not a refusal, the near miss caught most often. Check the size on the part card and use Resize.
- Over the limits: 25 MB per file, 4,000 mm in any dimension, 100 line items per quote. Email first for anything past those.
