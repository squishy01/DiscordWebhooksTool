# PROJECT: Discord Message Builder (Web App)

## Current State

**Core System:**
- Two-panel layout:  
  - **Left:** Textarea editor  
  - **Right:** Discord-style live preview (`.messageBody`)  
- Live preview via `renderPreview()` on input  
- `previewMessage` = main render target  
- `embedContainer` = holds embeds  
- Editor auto-focus implemented (cursor ready on page load)  

**Markdown & Formatting (Implemented):**
- Bold (`**text**`)  
- Italic (`*text*`)  
- Underline (`__text__`)  
- Strikethrough (`~~text~~`)  
- Inline code (`` `text` ``)  
- Code blocks (```` ``` ````), including optional language labels  
- Quotes:  
  - Single-line (`>`)  
  - Multi-line (`>>>`) — blocks all consecutive non-empty lines until a blank line  
- Headers (`#`, `##`, `###`)  
- Spoilers (`||text||`)  
  - Click to reveal  
  - Hover highlights background only  
- URL detection → converted to clickable `<a>` links  

**Lists (Implemented):**
- Nested lists supported up to 4 levels  
- `.listItem`, `.listBullet`, `.listContent` structure  
- Bullet size matches Discord (`22px`)  
- Tightened vertical spacing (`.listItem + .listItem { margin-top: -24px; }`)  
- Spacing from bullet to text adjustable (`.listBullet { margin-right: 7px; }`)  

**Embed System:**
- Placeholder embed generator  
- Single URL generates a “Link Preview” block  
- YouTube embeds supported via `!youtube(url)`  
- Styled `.discordEmbed` and `.embedContent`  

---

## Fixes / Improvements Since Last Version
- Multi-line and single-line quotes now correctly block content  
- Spoilers:  
  - Hidden text fully transparent (`.spoiler span { color: transparent; }`)  
  - Overlay highlights only background (`::before`)  
  - Click toggles `.revealed` → text inherits `.messageBody` color, overlay removed  
  - Spoilers now match Discord’s visual style accurately  
  - Nested list items no longer reset spoiler behavior on new lines  
- Normal preview text color: `#dbdee1` (matches Discord)  
- Cursor auto-focus ensures editor is ready to type on page load  
- Optional focus-restore after live preview updates prevents cursor loss during DOM rebuild  
- Lists now replicate Discord’s bullet size, spacing, and nesting visual style  

---

## Known Limitations
- Live preview does **not persist revealed spoilers** across typing (DOM rebuild clears them)  
- Multi-line quote spacing may still differ slightly from Discord  
- Masked links `[text](url)` not implemented  
- Only first URL generates an embed; multiple embeds not yet supported  
- Real metadata fetching (OpenGraph, etc.) not yet implemented  
- Rich text editor toolbar / structured embed UI not yet implemented  
- Some advanced markdown edge cases (escaping, subscript, nested formatting) not fully handled  
- Spoilers do not persist across page reloads (matches Discord session behavior)  

---

## CSS Notes
- `.messageBody { color: #dbdee1; }` → right-side preview text  
- `.spoiler span { color: transparent; }` → hidden text  
- `.spoiler.revealed span { color: inherit; }` → revealed text inherits `.messageBody`  
- `.spoiler::before` → overlay for hiding text, transitions hover background  
- `.spoiler.revealed::before { display: none; }` → removes overlay on click  
- Lists:  
  - `.listItem` → controls line height and vertical spacing  
  - `.listBullet` → bullet size, spacing from text  
  - `.listContent` → flex container for actual content  

---

## Next Steps / Suggestions
- Implement **masked links** (`[text](url)`) with live preview  
- Handle **multi-line quote edge cases** for full Discord accuracy  
- Build **Embed Generator UI** for multiple links and proper metadata  
- Optional: **persistent spoiler state** while typing (requires unique IDs outside DOM)  
- Implement **rich editor toolbar** (bold/italic/underline buttons, code block insertion, embed insertion)  
- Fine-tune **list alignment and spacing** if needed  
- Handle **advanced nested markdown and escaping edge cases**  

---

This README reflects all current functionality, fixes, and known limitations as of the latest development session.
