# Toyota Sienta (2016) Owner's Manual: English Edition

This folder contains:

| File | What it is |
|---|---|
| `sienta_201606.pdf` | The original Japanese owner's manual (June 2016 edition, 436 pages). |
| `Toyota_Sienta_2016_Owners_Manual_EN.pdf` | The English translation (443 pages). |
| `Sienta_2016_EN_pilot.pdf` | A 10-page trial version made before the full run. You can delete it. |

The English edition is an **unofficial translation**. It was made by Claude (Anthropic's AI assistant) in November 2026 for Frost. Toyota did not produce or check it. For anything safety-critical, ask a Toyota dealer to confirm.

---

## Goal

The manual is a technical document, so its diagrams had to stay **exactly as in the original**. The method was to swap out the text and leave everything else untouched, instead of retyping or rebuilding the pages:

- every drawing, icon, photo, table border and coloured box stays in its original position
- only the Japanese text is removed and replaced with English in the same spot
- page numbers don't change, so the table of contents and every "→ P. xxx" cross-reference still work

## How it was done

### 1. Inspection (Phase 0)
- The original PDF has real, selectable text, so no OCR was needed. It holds about 207,000 Japanese characters in roughly 7,700 text blocks.
- The diagrams are almost all vector drawings. Their labels are mostly numbered callouts (①, ②, …), which needed no translation.
- The PDF contains 111 embedded images. Seven of them (dashboard screen images) had Japanese text baked into the picture.
- The sideways chapter names on the page-edge tabs were set in a vertical Japanese font.

### 2. Pilot
- Ten representative pages were translated first: the cover, search guide, pictorial index, SRS airbags, child-seat table, warning lights, trip meter, a WARNING box, a maintenance note and the brake specifications.
- Frost approved the look and wording before the full run.

### 3. Text extraction
- Each page's text was read with its exact position, font size, colour and weight, using PyMuPDF (`extract.py`).
- Lines were grouped back into paragraphs so each sentence could be translated as a whole. Table cells, short labels and dotted-leader lines (e.g. `Hood ........ P. 285`) were kept as separate units.

### 4. Translation
- All 5,000+ unique text blocks were translated into English in batches, keeping the reading order of the pages.
- A consistent glossary based on Toyota's standard English terms was used throughout, for example *Malfunction indicator lamp*, *Pre-Collision System (PCS)*, *Lane Departure Alert (LDA)*, *jam protection function*, *engine switch / IGNITION ON mode*, *Toyota dealer*, and WARNING / NOTICE / Information boxes.
- Numbers, units, torque values, capacities, tyre pressures and part numbers were copied exactly.
- Symbols were kept: ★ means "availability depends on grade and options", and ※ marks a footnote.

### 5. Rebuilding the pages
- **Removing Japanese text:** the Japanese text was deleted from each page using text-only redaction. Graphics and images are never touched.
- **Tab labels:** the vertical tab labels couldn't be removed by redaction. Those text commands were stripped straight from the page content (`strip_vertical.py`).
- **Placing English text:** English was placed into each original text box (`render.py`) in Liberation Sans Narrow, a narrow Arial-style font, because English runs about 30–50% longer than Japanese.
  - The text box can grow into empty space to the right or below. It never grows over other text, lines or images.
  - Text shrinks to fit if needed, down to a 6 pt minimum. A few very tight spots go down to 5 pt.
  - Table cells stay centred, bullet indents are kept, and the side tabs are written rotated.
- **Images with Japanese text:** the 7 screen images were covered with small patches in the screen's own background colour, with English labels on top. These were the eco score, date/birthday settings, trip summary, ACCESSORY / IGNITION ON, "BRAKE!" and the lead-vehicle alert.

### 6. Index
- The original index is sorted in Japanese kana order. It was translated in place and kept as **"Index (Japanese order)"**.
- A new **A–Z English index** (7 pages, 625 entries) was built from it and inserted just before it (`build_index.py`). This is why the PDF has 443 pages instead of 436. The page numbers printed on the original pages are unchanged.

### 7. Quality checks
- **No Japanese left:** an automatic scan confirmed that no Japanese characters remain anywhere in the file.
- **No overlapping text:** an automatic check found no lines of text overlapping each other on any page (`qa_overlap.py`).
- **Numbers:** every number in each Japanese block was compared with its English translation. The only differences were numbers written as words, such as "twice" or "all four wheels".
- **Visual review:** about 40 pages across all chapters were checked by eye, including the warning-light tables, child-seat tables, specifications and the index.
- **File size:** the final file was compressed (duplicate fonts removed), bringing it down to 14.7 MB.

## Known limitations

- Crowded areas such as diagram callouts and table cells use small text, down to 6 pt. The customization tables on pages 402–403 are the tightest.
- In the table of contents, entries that were already in English kept the original font, so text sizes are mixed there.
- Some content applies only to Japan, for example references to Japanese law and the radio certification of the keys ("use only in Japan"). It is translated as written.
- Not every page was checked by eye. If you spot a problem, note the page number and it can be fixed and the file rebuilt.

## Tools used

- Python 3 with PyMuPDF (text extraction, redaction, placing text, index pages) and pikepdf (removing the vertical tab text)
- Liberation Sans Narrow and DejaVu Sans fonts (DejaVu for symbols such as ★ ※ ● →)
- Translation by Claude (Anthropic)
