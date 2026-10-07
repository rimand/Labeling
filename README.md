# 🏷️ Label Maker

A single-file web app for designing and printing label sheets (A4, A3, A5, Letter or custom). It has wire-label modes, auto-numbering, QR codes and barcodes, and die-cut output for print shops and the Silhouette Cameo.

เว็บแอปทำ Label ลงกระดาษ A4/A3/… รองรับ Label สายไฟ, วิ่งเลขอัตโนมัติ, QR/Barcode และไดคัท (รวม Silhouette Cameo 4) — ไฟล์เดียว ไม่ต้องติดตั้ง

## Getting started

Open `index.html` in Chrome or Edge. That's all you need.

To use **cookies** and **share links**, serve the folder over HTTP instead of opening the file directly:

```bash
python -m http.server 8000
```

Then open <http://localhost:8000>. You can also host the folder on GitHub Pages.

> Thai fonts, the QR library and the barcode library load from the internet (Google Fonts and cdnjs). Without a connection, text falls back to system fonts and codes won't render.

## Features

### Paper & layout
- A4, A3, A5, Letter or a custom size, in portrait or landscape.
- Margins, label size and X/Y gap. Columns, rows and labels per sheet are calculated automatically.
- **Printer calibration**: X/Y offset in mm and scale in %, to correct printer drift.

### Label modes
| Mode | Use |
|---|---|
| **Normal** | Plain rectangular label |
| **Wire · wrap** | Text repeated N times along the label, so it reads from any side of the wire |
| **Wire · flag** | Two halves with a wrap zone in the middle. Fold it around the wire |

Wire modes also have:
- A **wire Ø calculator** that sets the label length to π·D + overlap, or sets the flag's wrap zone to π·D.
- For flags only:
  - **Side B text**: a different back side, e.g. `→ {To}`.
  - **Mirror side B alignment**: both sides hug the outer edges, or both hug the fold.
  - **Fold guide lines**: off, center, or center + edges. These print.

### Content
- **Auto sequence**: `A-001-01` → `A-100-08`. Every number group and single letter counts on its own, the rightmost first, and leading zeros are kept. **Step** applies to the last number group.
- **Manual list**: one label per line.
- **Columns**: paste from Excel (Tab-separated) or use commas, then reference them as `{1}` `{2}` …
  - With **First row = column names** ticked, use `{ColumnName}` instead.
  - **Load CSV** reads a `.csv`, `.tsv` or `.txt` file into the list.
- **Text template**: `{n}` is the value (the first column). Multiple lines are supported.
- **Copies each**: prints each value N times, e.g. 2 for both ends of a wire.
- **Skip first**: leaves slots empty, for a partly used sheet.
- **Phase colours (IEC/TIS)**: labels whose first column is `L1`, `L2`, `L3`, `N` or `PE`/`G`/`E` are coloured brown, black, grey, blue or green-yellow.

### QR / Barcode
- QR Code (UTF-8, so Thai text works), Code 128, Code 39 and EAN-13.
- Code data uses the same `{n}` / `{1}` / `{Name}` placeholders.
- Position: left, right, top, bottom, or code only. The size is set in mm.

### Text style
- Fonts: Sarabun, Prompt, Kanit, Noto Sans Thai, Tahoma, Arial, Roboto Mono and Courier New.
- Size in pt with **auto-shrink to fit**, bold, italic and colour.
- Horizontal and vertical alignment, and text rotation of 0° or 90°.

### Die-cut
- Cut line: show on screen only, print it, or hide it.
- Corner radius, background colour and **bleed**.
- **Safe zone**: a blue dotted guide on screen, never printed.
- **Registration marks**: crosshair for print shops, or **Silhouette Type 1** for Cameo/Portrait with adjustable length, thickness and inset.
- Warnings when labels overlap the reg marks or the bleed overlaps a neighbour.
- **Export die-line**:
  - **SVG**: red hairline at real mm size, for print shops, Illustrator and CorelDRAW.
  - **DXF**: R12, mm, opens in Silhouette Studio **Basic**.
- **Apply die-cut defaults**: gap 3, safe zone 2, radius 2, bleed 1.5, reg marks on.

#### Die-cut spacing guide
| Setting | Recommended | Why |
|---|---|---|
| Gap | ≥ 3 mm | Leaves room for the blade and waste strip. Use 0 only for guillotine cuts |
| Safe zone | 1.5–2 mm | Die-cutting drifts ±0.5–1 mm |
| Bleed | 1–2 mm | Only with a background colour. The gap must be ≥ 2× bleed |
| Corner radius | 1–3 mm | Peels better and matches standard dies |

#### Silhouette Cameo 4 workflow
1. Set Reg marks to **Silhouette Type 1**, with length, thickness and inset the same as in Studio.
2. Clear any "overlaps reg marks" warning by raising the margin.
3. Set Cut line to **Screen**, then **Print** at 100% / Actual size.
4. Choose **Die-line → DXF file**.
5. In Studio, open Page Setup and set the same paper size and orientation. Under Registration Marks, choose Type 1 with the same values.
6. Open the DXF, select all, Group, then in Transform set **X = left margin, Y = top margin**.
7. Load the printed sheet and choose Send → Cut. The Cameo scans the marks itself.

> Matte paper reads best. Glossy or laminated sheets can fail the mark scan. Print one test sheet first, and if the marks aren't detected, compare it with a sheet printed from Studio.

### Workspace
- **Figma-style preview**:
  - The wheel pans, and Shift + wheel pans sideways.
  - Ctrl + wheel or a trackpad pinch zooms toward the cursor.
  - Drag to pan.
  - **Fit** (Shift+1) fits the page to the view.
- **Undo / redo**: Ctrl+Z, and Ctrl+Y or Ctrl+Shift+Z, up to 100 steps. Text boxes keep their own native undo.
- **Templates**:
  - **Save** (Ctrl+S) overwrites the template that's loaded, and **Save as…** creates a new one.
  - Load or delete a template from the list.
  - **Export / Import** all templates as `.json`.
  - **Share link**: the settings are packed into the URL hash and never sent to a server.
- **Auto-save**: the latest config goes to localStorage and to a cookie (cookies are skipped when the config is over about 4 KB).
- **Dark mode** (the default) or light mode, with an EN / TH interface.

## Printing tips
- Set the print dialog to **Scale 100% / Actual size**, with **Headers & footers off**.
- **Save as PDF** gives a print-ready file for a print shop.
- Print one sheet on plain paper first, hold it against the label stock, then adjust **Printer offset / scale** if needed.

## Keyboard shortcuts
| Keys | Action |
|---|---|
| Ctrl+S | Save template |
| Ctrl+Z | Undo |
| Ctrl+Y / Ctrl+Shift+Z | Redo |
| Shift+1 | Fit page in preview |
| Ctrl + wheel | Zoom preview |

## Tech
- One HTML file, vanilla JS, no build step.
- [qrcode-generator](https://github.com/kazuhikoarase/qrcode-generator) and [JsBarcode](https://github.com/lindell/JsBarcode) load from cdnjs.
- Layout uses CSS in real millimetres and `@page` for the paper size.

## Known limits
- CSV parsing is a plain split, so quoted cells containing commas aren't supported. Paste from Excel (Tab-separated) instead.
- Templates and settings are stored per browser and per address. Use **Export / Import** to move them.
- Chrome and Edge don't keep cookies for `file://` pages; localStorage still works there.
- Silhouette mark geometry follows the standard Type 1 layout. Verify it with a test cut on your machine.
