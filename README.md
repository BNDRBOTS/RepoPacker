# Payload Unpacker · Dual Engine

A single‑file HTML tool that converts any text‑based payload (`.txt`, `.md`, `.js`, `.ts`, `.log`, etc.) into a structured `.zip` archive—ready to unzip and push directly to GitHub. Every block of code is extracted, filenames are detected or inferred, and folders are automatically created to mirror the original project.

## How it works
1. **Upload or fetch** – drag & drop a local file, click to browse, or provide a remote URL to a raw text payload (e.g., from raw.githubusercontent.com).
2. **Forensic extraction** – the engine isolates code blocks using dividers, fences, file‑path headers, and implicit prose hints (like “Here’s the server: index.js”).
3. **Intelligent naming** – when no explicit path is found, heuristic analysis guesses the filename and directory. It detects TypeScript and JSX, identifies config files (vite.config.ts, tailwind.config.ts, next.config.ts, etc.), and places files in `src/`, `public/`, `tests/`, etc., based on content.
4. **Client‑side packaging** – a `.zip` is generated entirely in your browser—no server, no data sent anywhere.

## Key features
- **Universal text input** – works with `.txt`, `.md`, `.js`, `.ts`, `.jsx`, `.tsx`, `.log`, `.json`, `.html`, `.css`, and more.
- **Drag & drop** – simply drag a file onto the drop zone or click to browse.
- **Remote payload support** – fetch a raw text file from any URL (CORS permitting).
- **Zero‑loss extraction** – every byte of the original payload is preserved.
- **Config fingerprinting** – automatically detects and names files like `vite.config.ts`, `tailwind.config.ts`, `next.config.js`, `tsconfig.json`, `.env`, `.gitignore`, and more.
- **TypeScript & JSX awareness** – heuristic content analysis correctly applies `.ts`, `.tsx`, `.jsx` extensions.
- **Phantom‑file prevention** – never creates empty or single‑line files from stray paths.
- **Deduplication** – if two blocks would produce the same path, a numeric suffix is added to avoid overwriting.
- **Binary‑safe** – payloads containing null bytes or high amounts of non‑text content are treated as binary.
- **Traversal‑safe** – path sanitisation prevents directory traversal attacks.
- **Instant deployment** – open the HTML file locally or serve it behind any static host.

## Usage
1. Open `index.html` in Chrome, Edge, or any modern browser.
2. Use the **Local Payload** tab to drag & drop or select a file from your computer.
3. Optionally, use the **Remote Payload** tab to fetch a file from a URL.
4. Click **Download .ZIP** and save the archive.
5. Unzip and drag the folder directly into a new GitHub repository.

## Technical notes
- Built with vanilla JavaScript, JSZip, Three.js, and GSAP.
- All processing is client‑side; your files are never uploaded.
- Works offline after the initial page load (CDN scripts will be cached by the browser).

## License
Provided as‑is for personal and commercial use.

### PLEASE NOTE: ALL those claims were made by AI, I'm not claiming truth to all of them. I do know it's done a fairly good job for me so far. So please feel free to push it and test it. Share your results with me if you'd like @ bndr.labs@gmail.com
