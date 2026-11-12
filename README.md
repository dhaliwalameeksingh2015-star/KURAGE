# Kurage Dashboard

A dark AI-assistant dashboard: a collapsible sidebar, a greeting with a rotating headline, a glass prompt composer with starting points, a History and command palette, and an animated background of ASCII jellyfish drawn live on a canvas.

Made by HorizonX · Version 1.0.0 · October 2026

## Start here

1. Install and start [Ollama](https://ollama.com/), then download at least one model with `ollama pull <model-name>`.
2. Start a local web server inside this folder:
   - With Python (already installed on Mac and Linux): `python3 -m http.server 8080`
   - Or with Node.js: `npx serve .`
3. Open `http://localhost:8080` (with `npx serve`, use the address it prints) and choose an installed model in the composer. If the model list does not load, make sure Ollama is running and allows the page origin through `OLLAMA_ORIGINS`.

There is no build step. Opening `index.html` by double-click also works, but a local server is recommended.

## What's inside

- `index.html`: the whole dashboard: markup, styles (in the `<style>` block) and behavior (in the `<script>` block at the end). There are no image files: the logo, the icons and the jellyfish avatar are inline SVG, and the jellyfish background is drawn in code.
- `README.md` (the buyer guide), `CREDITS.md` (fonts and libraries), and `AGENTS.md` plus `CLAUDE.md` (instructions for AI coding agents).

## Page sections

- **Sidebar** (`.rail`): New chat, History, Explore, Library and Activity, a list of recent prompts, then Pause motion, Shortcuts, expand or collapse, and the profile button. On desktop it collapses to icons with tooltips; on phones it opens as a drawer from the menu button.
- **History button** (`.history-pill`), top right: opens the History and commands palette. The shortcut is ⌘K on Mac and Ctrl K elsewhere.
- **Greeting** (`#greeting`): says good morning, afternoon or evening from the visitor's clock, followed by the user's name.
- **Rotating headline** (`#headline`): seven task lines that change every two seconds or so with a word-by-word blur. Screen readers hear one fixed line. The rotation stops while there is text in the composer, while the tab is hidden and while motion is paused.
- **Composer** (`#composer`): a translucent glass prompt box with an Ollama model picker, response mode menu (Auto, Fast, Think), attach file, web search and voice buttons, and a send button. Enter sends to the selected local model; Shift Enter adds a line.
- **Model compare** (`#compare-wrap`): a side-by-side chat panel below the composer. Choose two or three local Ollama models, type the same prompt in each card, and send to compare answers on the spot. Cards keep their own conversation, and "Swap prompt" copies the active card's prompt to the others before sending. The panel opens with the **Compare** tool button in the composer's toolbar or with `Ctrl+Shift+M` / `Cmd+Shift+M`.
- **Ollama responses**: models are loaded from your local Ollama instance and prompts are sent to `http://localhost:11434/api/chat`. Replies stream into the page as they are generated and are saved with prompts in this tab's History. Smaller chat-capable models appear first; the selected model is kept loaded for 10 minutes after a request to speed up follow-up prompts.
- **Starting points** (`#chips`): four suggestions that drop a ready prompt into the composer. Explore in the sidebar swaps in a new set of four.
- **Jellyfish background** (`#ocean-canvas`): three to five jellyfish, depending on the screen width, drawn as monospace characters. They drift toward the composer, react to the pointer and to typing, and change color and pace with the response mode.
- **History and commands palette** (`#palette`) and the **Keyboard shortcuts** sheet (`#shortcuts`).

## Make it yours

- **Colors, fonts, sizes:** edit the `:root` block at the top of the `<style>` section in `index.html`. The color scale comes first (`--ink-*` backgrounds, `--mist-*` text, `--cyan-*` and `--violet-*` accents), then the roles that read from it (`--bg`, `--surface`, `--border`, `--text`, `--accent`, `--accent-2`, `--focus`), then `--rail-w` and `--rail-w-open` (sidebar widths), `--composer-radius` and `--control`.
- **Jellyfish colors:** they are set in code, in `PALETTES` near the top of the `Ocean` part of the script. Each response mode has two colors (`a` and `b`, as RGB numbers), a `tempo` and a `size`.
- **User name:** change `USER_NAME` near the start of the `App` part of the script. It feeds the greeting, the profile button and its tooltip. The same name is also written twice in the HTML (the profile button and `#greeting`) as the text shown before the script runs; change it there too.
- **Text:** the headline lines are in `LINES` (inside `Headline`; words between asterisks get the accent color), the starting points are in `CHIP_SETS`, the saved prompts of the Library are in `LIBRARY`, and the mode descriptions are in `MODES`.
- **Ollama setup:** prompts are sent only to Ollama running on the same device; there is no remote backend. If the browser blocks requests, configure Ollama's `OLLAMA_ORIGINS` to allow the page's origin (for example `http://localhost:8080`) and restart Ollama. A deployed page connects to the visitor's own localhost, not your machine. Attachments and the web-search control are visual only and are not sent to Ollama; voice input still uses keyboard dictation.
- **Saved settings:** the sidebar state, the response mode and the motion setting are kept in `localStorage` under keys that start with `kurage:`.
- **Font:** the page loads Geist and Geist Mono from Google Fonts. To change them, swap the Google Fonts `<link>` in the `<head>`, the `--font-sans` and `--font-mono` values, and `FONT` in the `Ocean` part of the script (the jellyfish need a monospace font).
- **Breakpoints:** the layout adapts at 65rem, 48rem (the sidebar becomes a drawer), 30rem and 23rem wide, plus a compact layout for short desktop windows (search for `@media` in the styles).

## Motion

- There are no animation libraries. The headline uses the Web Animations API, the jellyfish are drawn on a `<canvas>` with `requestAnimationFrame`, and small transitions are CSS.
- **Pause motion** in the sidebar stops the jellyfish and the headline, and the choice is remembered. The jellyfish also stop while the tab is hidden.
- If a visitor has **Reduce motion** turned on in their system settings (the `prefers-reduced-motion` setting), the page starts with motion paused, and if they press play the headline lines only fade. The layout also respects `prefers-reduced-transparency` (solid panels instead of glass) and Windows high-contrast mode.

## Deploy

- Upload the folder to any static host: Cloudflare Pages, Netlify, Vercel, GitHub Pages or your own server. No build step is needed.
- On Netlify or Cloudflare Pages you can drag the unzipped folder into the dashboard.

## Keep building with AI

Open this folder in an AI coding agent such as Claude Code or Codex and ask in plain words. The folder includes `AGENTS.md` (and a `CLAUDE.md` that points to it), so Codex, Claude Code, Cursor and other agents learn how the project is built and how to check their work before they change anything. Some examples:

- "Read README.md and index.html, then change the brand colors to my palette: …"
- "Replace the copy with content for my studio. Keep the layout and the animations."
- "Port this page to Next.js with Tailwind CSS without changing how it looks or moves."

To add more HorizonX components to the page from your agent, connect the HorizonX MCP server (included in your membership): https://horizonx.so/mcp

## License

Your HorizonX membership gives you a commercial license for this template: use it and adapt it in personal, client and commercial projects. You may not resell, redistribute, sublicense or publicly share the original source files. Full terms: https://horizonx.so/terms.
Libraries and images are listed with their own licenses in `CREDITS.md`. Fonts are not covered by the HorizonX license: if you change them, use fonts you own or have a license for.

## Need help?

Write to support@horizonx.so and mention **Kurage Dashboard**.
