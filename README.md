# 🏷️ Label Maker

Design and print label sheets: plain labels, wire labels, QR codes and barcodes, ready for die-cut.

**▶ Open the app: https://rimand.github.io/Labeling/**

## Features

- **Paper:** A4, A3, A5, Letter or custom. Labels per sheet are calculated automatically, and printer offset/scale can be calibrated.
- **Label modes:** Normal · Wire wrap (text repeated) · Wire flag (fold around the wire, side B text, mirrored alignment, fold guide lines).
- **Auto numbering:** `A-001-01` → `A-100-08`, with a step option.
- **Manual list or Excel/CSV:** use `{1}` `{2}` or `{ColumnName}` in the template.
- **QR / Barcode:** QR (Thai text works), Code 128, Code 39, EAN-13.
- **Text:** Thai and Latin fonts, auto-fit, bold/italic, alignment, 90° rotation.
- **Phase colours (IEC):** L1 · L2 · L3 · N · PE.
- **Die-cut:** corner radius, bleed, safe zone, reg marks (crosshair or Silhouette Type 1), and die-line export as **SVG / DXF**.
- **Templates:** Save (Ctrl+S), Save as, rename, delete, export/import `.json`, share link.
- **Workspace:** Figma-style zoom/pan, undo/redo, auto-save, dark/light mode, EN/TH.

## Printing

Print at **Scale 100% / Actual size** with **headers & footers off**. Print a test sheet on plain paper first and adjust **Printer offset** if needed.

## Die-cut tips

| | Recommended |
|---|---|
| Gap between labels | ≥ 3 mm |
| Safe zone | 1.5–2 mm |
| Bleed (with a background colour) | 1–2 mm |
| Corner radius | 1–3 mm |

### Silhouette Cameo 4
1. Set Reg marks to **Silhouette Type 1**, with the same values as in Studio.
2. **Print** at 100%, with Cut line set to *Screen*.
3. Choose **Die-line → DXF**, open it in Studio, Group, then set **X = left margin, Y = top margin**.
4. Load the sheet and choose **Send → Cut**.

> Matte paper is easiest for the Cameo to read. Do a test cut before a full run.

## Shortcuts

| Keys | Action |
|---|---|
| Ctrl+S | Save template |
| Ctrl+Z / Ctrl+Y | Undo / Redo |
| Ctrl + wheel | Zoom |
| Shift+1 | Fit page |

## Notes

- It's a single `index.html` with no build step. Fonts and the QR/barcode libraries load from CDNs.
- Templates are saved in your browser. Use **Export / Import** to move them to another browser or computer.
- CSV cells that contain commas aren't supported. Paste from Excel instead.
