# Toyota Sienta Owner's Manuals: English Editions

This folder contains English translations of two Japanese Toyota Sienta owner's manuals:

| File | What it is |
|---|---|
| `sienta_201606.pdf` | Original Japanese manual: **Sienta (gasoline)**, June 2016 edition, 436 pages. |
| `Toyota_Sienta_2016_Owners_Manual_EN.pdf` | English translation of the above (443 pages). |
| `sienta_hybrid_201708.pdf` | Original Japanese manual: **Sienta Hybrid**, August 2017 edition, 432 pages. It covers models built from **August 2017 to August 2018**, so it is also the manual for 2018 models. |
| `Toyota_Sienta_Hybrid_2017-2018_Owners_Manual_EN.pdf` | English translation of the above (440 pages). |

Both English editions are **unofficial translations**. They were made by Claude (Anthropic's AI assistant) in November 2026 for Frost. Toyota did not produce or check them. For anything safety-critical, ask a Toyota dealer to confirm.

---

## Goal

The manuals are technical documents, so their diagrams had to stay **exactly as in the original**. The method was to swap out the text and leave everything else untouched, instead of retyping or rebuilding the pages:

- every drawing, icon, photo, table border and coloured box stays in its original position
- only the Japanese text is removed and replaced with English in the same spot
- page numbers don't change, so the table of contents and every "→ P. xxx" cross-reference still work

## How it was done

The same method was used for both manuals. The 2016 manual was translated first. The Hybrid manual then reused its work.

### 1. Inspection
- Both PDFs have real, selectable text, so no OCR was needed. Diagrams are almost all vector drawings, and their labels are mostly numbered callouts (①, ②, …).
- 2016 manual: about 207,000 Japanese characters. Seven dashboard screen images had Japanese baked into the picture.
- Hybrid manual: about 4,900 unique text blocks, of which about 74% were word-for-word identical to the 2016 manual. That left about 50,000 characters of new text, mostly hybrid-specific. 17 screen images had Japanese baked in.
- The sideways chapter names on the page-edge tabs are set in a vertical Japanese font in both manuals.

### 2. Pilot (2016 manual only)
- Ten representative pages were translated first. Frost approved the look and wording before the full run.
- The Hybrid manual went straight to a full build, because the method was already proven.

### 3. Text extraction
- Each page's text was read with its exact position, font size, colour and weight, using PyMuPDF (`extract.py`).
- Lines were grouped back into paragraphs so each sentence could be translated as a whole. Table cells, short labels and dotted-leader lines (e.g. `Hood ........ P. 285`) were kept as separate units.

### 4. Translation
- Text was translated into English in batches, keeping the reading order of the pages.
- For the Hybrid manual, every block identical to the 2016 manual reused its 2016 translation, so the shared sections read the same in both books. Only new text was translated.
- One glossary based on Toyota's standard English terms was used for both books. Examples:
  - *Malfunction indicator lamp*, *Pre-Collision System (PCS)*, *Lane Departure Alert (LDA)*, *engine switch*, *Toyota dealer*
  - Hybrid terms: *power switch* (ACCESSORY / ON mode), *READY indicator*, *hybrid system*, *hybrid battery*, *12-volt battery*, *EV drive mode*, *eco drive mode*, *energy monitor*, *Hybrid System Indicator*, *regenerative braking*, *Vehicle Proximity Notification System*, *Electronically Controlled Brake (ECB)*, *accessory power outlet (100 V AC)*
- Numbers, units, torque values, capacities, tyre pressures, voltages and part numbers were copied exactly.
- Symbols were kept: ★ means "availability depends on grade and options", and ※ marks a footnote.

### 5. Rebuilding the pages
- **Removing Japanese text:** the Japanese text was deleted from each page using text-only redaction. Graphics and images are never touched.
- **Tab labels:** the vertical tab labels couldn't be removed by redaction. Those text commands were stripped straight from the page content (`strip_vertical.py`).
- **Placing English text:** English was placed into each original text box (`render.py`) in Liberation Sans Narrow, a narrow Arial-style font, because English runs about 30–50% longer than Japanese.
  - The text box can grow into empty space to the right or below. It never grows over other text, lines or images.
  - Text shrinks to fit if needed, down to a 6 pt minimum. A few very tight spots go down to 5 pt.
  - Table cells stay centred, bullet indents are kept, and the side tabs are written rotated.
- **Cover and screen images:** Japanese drawn as outlines (the cover title) or baked into screen images was covered with small patches in the background colour, with English labels on top. In the Hybrid manual, the patched screens are the eco score, eco wallet, gasoline price and reference fuel economy settings, settings menus, date/birthday screens, trip summary, ACCESSORY / IGNITION ON, "BRAKE!" and the lead-vehicle alert.

### 6. Index
- Each original index is sorted in Japanese kana order. It was translated in place and kept as **"Index (Japanese order)"**.
- A new **A–Z English index** was built from it and inserted just before it (`build_index.py`):
  - 2016 manual: 7 pages, 625 entries
  - Hybrid manual: 8 pages, 643 entries
- This is why the English PDFs have more pages than the originals. The page numbers printed on the original pages are unchanged.

### 7. Quality checks (both manuals)
- **No Japanese left:** an automatic scan confirmed that no Japanese characters remain in the text of either file.
- **No overlapping text:** an automatic check found no lines of text overlapping each other on any page (`qa_overlap.py`). The few overlaps found were fixed, for example by shortening table headers and merging sentences that were split across a table row.
- **Numbers:** every number in each Japanese block was compared with its English translation. The only differences were numbers written as words (such as "twice") or numbers that sit in a neighbouring block.
- **Visual review:** sample pages across all chapters were checked by eye. For the Hybrid manual, this included a second read of the new safety-critical sections: high voltage and the hybrid battery, accidents and towing, EV drive mode, the 12-volt battery and jump-starting, overheating, warning lights and warning messages, and the 100 V accessory power outlet.
- **File size:** the final files were compressed (duplicate fonts removed), bringing them to about 15 MB each.

## Known limitations

- Crowded areas such as diagram callouts and table cells use small text, down to 6 pt. The customization tables near the end of each book are the tightest.
- In the table of contents, entries that were already in English kept the original font, so text sizes are mixed there.
- The small pictures of the actual Japanese caution labels on the vehicle (Hybrid manual, page 71) were left as they are, because they show what the real label looks like.
- Some content applies only to Japan, for example references to Japanese law, the radio certification of the keys ("use only in Japan"), the 100 V / 50 Hz power outlet and local idling rules. It is translated as written.
- Not every page was checked by eye. If you spot a problem, note the file and page number and it can be fixed and the file rebuilt.

## Tools used

- Python 3 with PyMuPDF (text extraction, redaction, placing text, index pages) and pikepdf (removing the vertical tab text)
- Liberation Sans Narrow and DejaVu Sans fonts (DejaVu for symbols such as ★ ※ ● →)
- Translation by Claude (Anthropic)
