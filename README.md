# PROJECT: Discord Message Builder (Web App)

## Current State

**Core System:**
- Two-panel layout:  
  - **Left:** Textarea editor  
  - **Right:** Discord-style preview (`.messageBody`)  
- Live preview via `renderPreview()` on input  
- `previewMessage` = main render target  
- `embedContainer` = holds embeds  

**Markdown Support (Implemented):**
- Bold (`**text**`)  
- Italic (`*text*`)  
- Underline (`__text__`)  
- Strikethrough (`~~text~~`)  
- Inline code (`` `text` ``)  
- Code blocks (```` ``` ````), including language labels  
- Quotes:  
  - Single-line (`>`), multi-line (`>>>`)  
- Headers (`#`, `##`, `###`)  
- Spoilers (`||text||`) **with click-to-reveal**, hover only highlights background  
- URL detection → converted to `<a>`  

**Important Notes:**
- Code blocks are extracted and restored safely  
- Only **one URL regex** rule exists; runs **last** to avoid conflicts  
- Formatting order controlled to prevent nesting issues (italic last)  

**Embed System:**
- Basic link preview block (placeholder)  
- Pulls first detected URL, displays “Link Preview” + URL text  
- YouTube embeds supported via `!youtube(url)`  

---

## Current Fixes / Improvements
- **Multi-line and single-line quotes** now correctly handled  
- **Spoiler behavior:**  
  - Hidden behind a pseudo-element overlay (`::before`)  
  - Hover changes **background only**, text stays hidden  
  - Click toggles `.revealed`: text color inherits `.messageBody` color, overlay removed  
  - Fully prevents text “bleed” during background transition  
  - Matches Discord’s style visually  
- **Normal text color** in right preview panel: `#dbdee1` (Discord accurate)  
- Left editor panel text remains white (`#ffffff`)  
- CSS transitions handle hover effect (`background-color`) only, not text  
- Spoilers no longer show dull gray text after reveal  

---

## Known Limitations
- Live preview does **not persist revealed spoilers across typing** (DOM is rebuilt on input)  
- Multi-line quotes may still have edge-case spacing differences compared to Discord  
- Masked links `[text](url)` not implemented  
- Lists (`-`, `*`) and indentation not implemented  
- Real metadata for embeds not yet fetched (currently placeholder)  
- Only first URL in message generates an embed  
- Spoilers do not persist across page reloads (matches Discord session behavior)  

---

## CSS Notes
- `.messageBody { color: #dbdee1; }` → right-side text color  
- `.spoiler span { color: transparent; }` → hidden text  
- `.spoiler.revealed span { color: inherit; }` → revealed text inherits `.messageBody`  
- `.spoiler::before` → overlay for hiding text, transitions hover background  
- `.spoiler.revealed::before { display: none; }` → removes overlay on click  

---

## Next Steps / Suggestions
- Fix **masked links** (`[text](url)`) live preview  
- Implement **lists** (`-`, `*`) and indents  
- Build **Embed Generator UI** for multiple links and proper metadata  
- Optionally add **persistent spoiler state** while typing (requires tracking IDs outside DOM)  
- Handle **multi-line quote edge cases** for full Discord accuracy  
