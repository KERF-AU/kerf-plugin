# Warnings, refusals and fixes

The quote workspace checks every file the moment it lands and shows one of four states on the part card: Checked, 1 warning (or more), Can't cut yet, or File not saved. The wording below is the workspace's own. Explain a flag in these words; do not invent causes or fixes beyond the documented ones.

## Refusals ("Can't cut yet"): the part cannot be priced until fixed

| The workspace says | Meaning and documented fix |
| --- | --- |
| "The outline has an open contour (or N open contours), so the cut path never closes. Close the path ..." | Gaps where lines nearly meet, or a stroke that does not return to its start. Close every loop and re-export. |
| "N shapes have unreadable coordinates, so the file can't be measured, re-export it and try again." | Corrupt or malformed path data. Re-export from the source program. |
| Live text found | Convert text to outlines and export again. |
| Raster content on a vector sheet, or a scanned drawing | A picture is not geometry. Redraw in CAD, or email hello@kerf.au for a checked quote. |
| A STEP or IGES part that is not flat (bends, folds, machined features, an assembly) | Export the flat pattern, or email the file. |
| Larger than 4,000 mm in any dimension | Too large for an instant price; email hello@kerf.au. |
| Unknown material or thickness at checkout | Pick them again from the list; the combination is not stocked. |

## Warnings: priced, but check before ordering

| The workspace says | What to tell the person |
| --- | --- |
| "The file doesn't declare its units, we've assumed millimetres (W × H mm). Wrong? Re-export with units ..." | Confirm the measured size on the part card; use Resize or re-export with units. |
| "This Illustrator export has no physical size, so we read it in points, the Illustrator convention (1 unit = 0.353 mm): W × H mm. Not the size you meant? Use Resize" | Same: confirm the size. |
| "This part measures under 5 mm, double-check the units before ordering." | Almost always a units problem. |
| "At W × H mm this exceeds our standard 3200 × 1600 mm bed, we'll confirm before cutting." | Priced, but KERF confirms before cutting. |
| "N duplicate outlines (the same shape drawn twice, usually a fill and a stroke) were ignored." | Harmless; the duplicate was dropped so it is not cut or charged twice. |
| "Ignored ... only cut outlines are quoted. Text to be cut must be converted to outlines/paths." | Text, dimensions or annotations were in the file and were skipped. Remove them or convert text. |
| "N hidden objects were ignored (display:none / visibility:hidden), unhide anything that should be cut." | Something invisible in the file was skipped. |
| "Embedded images were ignored, only vector outlines are quoted." | The image will not be cut. |
| "N linked shapes (<use>) couldn't be resolved and were ignored." | Flatten or expand symbols before exporting SVG. |
| "This file contains N separate outlines, quoted as one job, all cut from the same sheet." | Several parts in one file price as one line. If they need different materials, thicknesses or quantities, split the file. |

## Checkout refusals

Checkout refuses the whole quote, and says why, if any line has a problem: an unknown part, a part flagged "can't cut yet", a design file that did not save ("upload it again"), an invalid quantity (whole numbers 1 to 10,000), an unknown material or thickness, a part that cannot be measured in time ("simplify the file, or email it"), or a part over 4,000 mm. More than 100 lines on one quote is refused. Fix or remove the line named, then Checkout again.

## Severity, in plain terms

- A refusal blocks the price. Fix the file or use "Get help fixing it" on the part, which sends the file and the problem to KERF.
- A warning does not block the price. The only one that changes what is cut is size, so always have the person confirm the measured width and height before ordering.
- Nothing here is exhaustive: a person at KERF reviews every file before cutting and contacts the customer if the automatic check missed something.
