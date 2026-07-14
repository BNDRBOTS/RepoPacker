# Payload Unpacker · Dual Engine (v5.0.0-PRO)
​
A single-file, 100% client-side tool that turns messy multi-file code pastes into a verified, GitHub-ready `.zip` — with a character-exact integrity audit that makes silent data loss impossible.
​
Paste a raw dump (AI chat output, concatenated sources, a `.txt`/`.md` code drop), or feed it local files or a remote URL. The engine isolates every code block, preserves folder paths, and packages a structured archive ready for GitHub's site uploader.
​
---
​
## Quick start
​
1. Download `unpacker.html` and open it in any modern browser. No build, no install, no server.
2. Drop one or more files (any type, any size) onto the drop zone — or paste a remote raw-payload URL in the **Remote** tab.
3. Review the extraction summary and **Preview** the file tree.
4. Download `<name>_extracted.zip` and drag it into GitHub's uploader.
​
Everything runs in your browser. Your code never leaves your machine (the Remote engine only contacts the URL you provide).
​
---
​
## Supported input formats
​
| # | Format | Example |
|---|--------|---------|
| 1 | Named code fences | ` ```ts:src/utils/helper.ts ` |
| 2 | Native headers | `--- FILE: src/app.js ---` |
| 3 | Labeled headers | `File: config.json` |
| 4 | Comment headers | `// src/index.js` |
| 5 | Standalone paths | `server.js` on its own line |
| 6 | Heuristic inference | blocks separated by `=====` dividers |
​
Explicit end markers are recognized as structure, never as file content: `--- EOF ---`, `--- END ---`, `--- END FILE ---`, `--- END OF FILE ---`, and ` ```eof `. All line endings (LF, CRLF, lone CR) are preserved byte-for-byte.
​
---
​
## The integrity guarantee
​
Every run performs a **character-exact accounting audit**: each character of the source payload must be provably present as
​
- extracted file content,
- logged structural markers (fences, dividers, path headers, end markers),
- inter-block whitespace, or
- verbatim text in `_RECOVERED_UNMATCHED.txt` (indexed with per-run source-line provenance).
​
Code-like unmatched blocks are automatically **rescued** as standalone inferred files (flagged `rescued`, with exact source-line ranges) instead of being junk-drawered. Path inference covers shebangs, Dockerfiles, SQL, Markdown docs, and YAML-style configs.
​
If the arithmetic does not balance to exactly 100%, the archive **still ships**: the complete original payload is additionally embedded verbatim under `_ORIGINAL_PAYLOAD/` and the variance is reported in the UI and in the manifest. Nothing is ever changed, dropped, or omitted — and no failure is ever silent.
​
---
​
## Archive layout
​
```
<name>_extracted.zip
├── ...your files, folder structure preserved...
├── _MANIFEST.json              # generator, SHA-256 per source, per-file audit,
│                               # provenance, warnings, integrity verdict
├── _RECOVERED_UNMATCHED.txt    # only if unmatched text exists (with line index)
├── _ORIGINAL_PAYLOAD/          # only if the audit is not exactly 100%
└── _FALLBACK/                  # only if the archiver rejects a path (content kept verbatim)
```
​
`_MANIFEST.json` records, for every entry: path, character count, inferred/recovered/rescued/passthrough/fallback/binary flags, and source-line ranges — so the archive is self-auditing.
​
---
​
## Failsafes (zero silent fails)
​
| Failure | Behavior |
|---------|----------|
| Archiver (JSZip) CDN blocked | Loud error screen; input untouched; retry after reconnect |
| three.js blocked / WebGL unavailable | Background disabled; tool fully functional |
| GSAP blocked | Views switch instantly without animation |
| Lucide blocked | Icons absent; functionality intact |
| DEFLATE compression fails | Automatic uncompressed (STORE) retry + warning |
| Zip rejects a filename | Content shipped verbatim under `_FALLBACK/` + warning |
| Audit below 100% | Archive still ships + `_ORIGINAL_PAYLOAD/` + reported variance |
| Binary or undecodable input | Verbatim passthrough, flagged `binary` — never mangled |
​
All warnings surface in the UI and are written into `_MANIFEST.json`.
​
---
​
## Filename safety
​
- Path traversal is stripped; Windows separators become folders.
- Windows-reserved device names (`CON`, `NUL`, `COM1`, …) are neutralized (`_CON.txt`) so archives extract cleanly on Windows.
- Duplicate paths are deterministically deduplicated (`app.js`, `app_2.js`, …) without ever touching content.
​
---
​
## Tech stack
​
Self-contained HTML + vanilla JS. Pinned CDN dependencies:
​
- Tailwind CSS 3.4.5 · Three.js r128 (decorative WebGL background) · GSAP 3.12.2 · Lucide 0.263.1 · JSZip 3.10.1
​
Every dependency is optional at runtime except JSZip — and its absence fails loudly, not silently.
​
---
​
## Testing
​
90 automated Node tests ship with the development workflow:
​
- **57 parser regression tests** — format matrix, CRLF preservation, BOM, binary passthrough, empty/blank inputs, rescue + provenance, fallback ladder, fuzzing.
- **33 hardening tests** — Unicode (emoji, ZWJ sequences, RTL, combining marks, astral plane), classic-Mac lone-CR endings, unclosed fences at EOF, 10 MB single-line bundles, 500-way name collisions, determinism, 1,000-iteration mixed-EOL fuzz, and stubbed zip-layer integration covering every failsafe path.
​
---
​
## Views
​
- **Local** — drag & drop or select files (any type, multiple allowed)
- **Remote** — fetch a raw payload by URL
- **Preview** — inspect the extracted file tree before download
- **Rules** — the full format grammar and integrity guarantee, in-app
​
