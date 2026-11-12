# AGENTS.md — Kurage Dashboard

Instructions for AI coding agents (Codex, Claude Code, Cursor and others) working in this folder. The person you are working with always has the final say: when their request and these notes disagree, follow them.

## Project

A dark AI-assistant dashboard: a collapsible sidebar, a greeting with a rotating headline, a glass prompt composer with starting points, a History and command palette, and an animated background of ASCII jellyfish drawn live on a canvas.

- Stack: static HTML, CSS and JavaScript in a single `index.html`, no build step and no libraries; Geist and Geist Mono from Google Fonts
- `README.md` is the buyer guide. `CREDITS.md` lists every library and image with its license. `LICENSE.md` (when present) is the HorizonX license.

## Run it

   - With Python (already installed on Mac and Linux): `python3 -m http.server 8080`
   - Or with Node.js: `npx serve .`

Then open `http://localhost:8080` (with `npx serve`, use the address it prints).

## Where things are

- `index.html`: the whole dashboard: markup, styles (in the `<style>` block) and behavior (in the `<script>` block at the end). There are no image files: the logo, the icons and the jellyfish avatar are inline SVG, and the jellyfish background is drawn in code.
- `README.md` (the buyer guide), `CREDITS.md` (fonts and libraries), and `AGENTS.md` plus `CLAUDE.md` (instructions for AI coding agents).

## How to change things

- **Colors, fonts, sizes:** edit the `:root` block at the top of the `<style>` section in `index.html`. The color scale comes first (`--ink-*` backgrounds, `--mist-*` text, `--cyan-*` and `--violet-*` accents), then the roles that read from it (`--bg`, `--surface`, `--border`, `--text`, `--accent`, `--accent-2`, `--focus`), then `--rail-w` and `--rail-w-open` (sidebar widths), `--composer-radius` and `--control`.
- **Jellyfish colors:** they are set in code, in `PALETTES` near the top of the `Ocean` part of the script. Each response mode has two colors (`a` and `b`, as RGB numbers), a `tempo` and a `size`.
- **User name:** change `USER_NAME` near the start of the `App` part of the script. It feeds the greeting, the profile button and its tooltip. The same name is also written twice in the HTML (the profile button and `#greeting`) as the text shown before the script runs; change it there too.
- **Text:** the headline lines are in `LINES` (inside `Headline`; words between asterisks get the accent color), the starting points are in `CHIP_SETS`, the saved prompts of the Library are in `LIBRARY`, and the mode descriptions are in `MODES`.
- **Ollama integration:** prompts are sent only when submitted, directly to the local Ollama API at `http://localhost:11434`; model discovery uses `/api/tags` and generation uses `/api/chat`. Do not add a remote proxy or send prompts elsewhere unless the user asks. Attachments and the web-search toggle are not forwarded, and voice input still uses keyboard dictation.
- **Model compare:** the "Compare" tool button in the composer's toolbar opens a side-by-side panel with up to three Ollama models. Each card keeps its own conversation in `localStorage` under the `kurage:compare` key. Cards are streamed using the Ollama `/api/chat` endpoint. Functions are in `CMP` in the model-compare script section of the App part.
- **Saved settings:** the sidebar state, the response mode and the motion setting are kept in `localStorage` under keys that start with `kurage:`.
- **Font:** the page loads Geist and Geist Mono from Google Fonts. To change them, swap the Google Fonts `<link>` in the `<head>`, the `--font-sans` and `--font-mono` values, and `FONT` in the `Ocean` part of the script (the jellyfish need a monospace font).
- **Breakpoints:** the layout adapts at 65rem, 48rem (the sidebar becomes a drawer), 30rem and 23rem wide, plus a compact layout for short desktop windows (search for `@media` in the styles).

## Motion

- There are no animation libraries. The headline uses the Web Animations API, the jellyfish are drawn on a `<canvas>` with `requestAnimationFrame`, and small transitions are CSS.
- **Pause motion** in the sidebar stops the jellyfish and the headline, and the choice is remembered. The jellyfish also stop while the tab is hidden.
- If a visitor has **Reduce motion** turned on in their system settings (the `prefers-reduced-motion` setting), the page starts with motion paused, and if they press play the headline lines only fade. The layout also respects `prefers-reduced-transparency` (solid panels instead of glass) and Windows high-contrast mode.

## Rules for this project

- All styles are in the `<style>` block and all scripts in the `<script>` block of `index.html`. Keep it a static page with no build step unless the user asks to port it.
- Colors and sizes come from the `:root` variables. Change those instead of hard-coding new values. The jellyfish colors are in `PALETTES` in the `Ocean` script.
- The visual headline is `aria-hidden` and screen readers read `#headline-text`. Keep that split, and keep every headline line short enough for one or two lines.
- The palette, the drawer and the shortcuts sheet are keyboard accessible (focus moves in and returns on close, Esc closes). Keep that when you change them.
- The page stores prompts only in this tab. Do not send prompts to any service unless the user asks you to connect one.
- Keep the current look and motion unless the user asks to change them. Before a large change, say what you plan to do.
- Every animation you add or change must keep working with `prefers-reduced-motion` and when its script fails to load.
- Fonts are not covered by the HorizonX license. If the user changes the fonts, use ones they own or have a license for, and never download font files they did not provide.
- When you add a library, font or image, add it to `CREDITS.md` with its version and license.
- Do not add analytics, tracking or other third-party scripts unless the user asks for them.

## Check your work

1. Serve the folder and open every section of the page.
2. Make sure the browser console shows no errors and no file returns 404.
3. Check the layout at 390 px and 1440 px wide.
4. Turn on reduced motion in the operating system and check that the content still appears.

## License

The template is licensed to its owner through their HorizonX membership. Building and shipping sites from it is allowed; publishing or reselling the source files as a template is not. Terms: https://horizonx.so/terms
