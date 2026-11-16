# Style guide

This describes how mohsin-syed.co.uk looks, reads and is coded, so that changes stay consistent with what is already there. It documents the site as built in `index.html`. If the code and this guide disagree, fix whichever one is wrong in the same change.

## Design idea

A modern one-page scroll with retro-computing manners: a clean grid and generous type, with the retro confined to four places.

1. Hard offset shadows (no blur) on buttons, cards and windows.
2. A monospace voice for labels, dates, chips and terminal-style text.
3. A dot-matrix hero background that reacts to the pointer.
4. A blinking block caret after the typed line.

Do not add a fifth retro device. No rounded corners anywhere (`border-radius` is not used), no gradients, no blurred shadows, no images or icon fonts.

## Colour

All colours are CSS custom properties on `:root`. Use the token, never a raw hex value, with the two exceptions listed under "Fixed colours".

| Token | Light | Dark | Used for |
|---|---|---|---|
| `--bg` | `#edebe4` | `#15132a` | Page background, chip background |
| `--surface` | `#faf9f5` | `#1f1c3a` | Cards, windows, default buttons, stats strip |
| `--ink` | `#17141f` | `#f2efe6` | Body text, headings, contact section background |
| `--muted` | `#5b5669` | `#aaa5c2` | Mono labels, nav links, secondary text |
| `--soft` | `#d3cfc3` | `#322e5c` | Resting dots in the hero canvas |
| `--accent` | `#3a2fd0` | `#9c93ff` | Primary button, the accent word in an `h2`, the `>` prompt and `$` keys in terminal text, scroll progress bar, focus rings |
| `--pop` | `#ffb300` | `#ffb300` | Highlights: tags, active nav item, caret, heading shadow, hover states |
| `--line` | `#17141f` | `#7b75b8` | Every border and rule |
| `--shadow` | `#17141f` | `#08071a` | Offset shadows |

Dark values apply when the system is in dark mode (`prefers-color-scheme: dark`). Setting `data-theme="dark"` or `data-theme="light"` on `<html>` forces a theme. The page has no theme toggle; the attribute exists so one can be added, and the hero canvas already redraws when it changes.

When adding or changing a token, update all three blocks: `:root`, the dark media query, and `:root[data-theme="dark"]`.

### Fixed colours

- Text on an amber (`--pop`) background is always `#17141f`, in both themes, because amber does not change between themes.
- Text on the primary button is `#fff` in light mode and `#15132a` in dark mode.

### Contrast

Measured WCAG contrast ratios for the pairs in use. Keep new pairings at 4.5 or above for text.

| Pair | Light | Dark |
|---|---|---|
| `--ink` on `--bg` | 15.2 | 15.8 |
| `--ink` on `--surface` | 17.2 | 14.2 |
| `--muted` on `--bg` | 5.9 | 7.7 |
| `--muted` on `--surface` | 6.7 | 6.9 |
| `--accent` on `--bg` | 7.0 | 6.9 |
| `--accent` on `--surface` | 8.0 | 6.2 |
| `#17141f` on `--pop` | 10.1 | 10.1 |
| Primary button text on `--accent` | 8.4 | 6.9 |

## Typography

Three families, loaded from Google Fonts, each with one job.

| Token | Family | Weights loaded | Job |
|---|---|---|---|
| `--display` | Unbounded | 500, 700, 800 | `h1`, `h2`, `h3`, stat numbers, the nav mark |
| `--body` | IBM Plex Sans | 400, 500, 600 | Paragraphs and lists |
| `--mono` | IBM Plex Mono | 400, 500, 600 | Labels, dates, nav links, buttons, chips, tags, terminal text |

Only load a new weight if something uses it.

| Element | Size | Notes |
|---|---|---|
| Body | 17px, line-height 1.6 | |
| `h1` | `clamp(2.9rem, 11.5vw, 7.25rem)` | Weight 800, uppercase, line-height .92, letter-spacing -.02em, amber text-shadow offset `.05em .05em` |
| `h2` | `clamp(1.8rem, 5.2vw, 3.5rem)` | Weight 800, uppercase, line-height 1 |
| `h3` | 1.125rem | Weight 700, uppercase |
| `.mono` label | .8125rem | Uppercase, letter-spacing .04em, `--muted` |
| Chips and tags | .75rem | Mono |
| Buttons | .875rem | Mono, weight 500 |

Headings are written in sentence case in the HTML and uppercased by CSS. Do not type them in capitals. Headings use `text-wrap: balance`.

## Layout and spacing

- Content sits in `.wrap`: max width `--wrap` (1160px), side padding `--gut` (`clamp(16px, 4vw, 40px)`).
- Sections (`.sec`) have vertical padding `clamp(56px, 9vw, 112px)` and a 2px top rule between them.
- Grids are `.grid3`, `.grid2` and `.groups`, all with a 24px gap.
- One breakpoint, at 900px: grids and the hero collapse to one column, the stats strip goes from four columns to two, and the nav mark is hidden.
- The nav is sticky and 58px tall; sections have `scroll-margin-top: 72px` so anchors land below it.

Prefer `clamp()` for sizes that should scale over adding breakpoints.

## Borders and shadows

- Borders are 2px solid `--line`. Small items (chips, tags) use 1.5px. A 1.5px dashed `--muted` border means "not interactive" (skills with no linked project).
- Shadows are solid offsets with zero blur, in `--shadow`. Depth is stepped by size: buttons 4px, cards 6px, the profile window 8px.
- Hover lifts an element up and left and grows the shadow by the same amount (button: move 2px, shadow 6px; card: move 3px, shadow 9px). Pressing a button pushes it down and right with a 2px shadow.
- On the dark contact section, button shadows are amber instead of `--shadow`, and the heading's offset shadow is `--accent` instead of amber.

## Components

| Class | What it is |
|---|---|
| `.btn`, `.btn.primary` | Mono button with border and offset shadow. One primary button per group. |
| `.card` | Bordered surface with shadow. Add `.rv` so it rises in on scroll. |
| `.tag` | Small amber label at the top of a card ("Now", "First Class", "Backend"). At most one per card. |
| `.chips` | List of mono tech labels at the bottom of a card. |
| `.mono` | Small uppercase label above headings and for dates. |
| `.sec-h` | Section header: a `.mono` label, then an `h2` with exactly one word wrapped in `<span>` for the accent colour. |
| `.win` | Retro window with a title bar (`.win-bar`), used for the profile card. |
| `.stats` / `.stat` | Strip of four headline numbers under the hero. |
| `.sk` | Skill pill. A `button.sk` is solid and clickable; a `span.sk` is dashed and inert. |
| `.readout` | Dark terminal-style bar showing where the selected skill was used. |
| `.tl` | Experience timeline with amber square markers. |

Reuse these before writing a new component. Class names are short, lowercase and unprefixed.

## Motion

- Motion is short and functional: 0.12s to 0.15s transitions on hover, a 0.65s rise for cards entering the viewport, a 1s stepped blink for the caret.
- Everything respects `prefers-reduced-motion`. CSS disables all animation and transition, and JavaScript checks the `REDUCE` flag before starting the typed line, pointer effects, scroll-in and smooth scrolling. Any new animation must do both.
- Content already on screen at load does not animate in.

## Accessibility

- `lang="en-GB"` on `<html>`.
- Focus is always visible: 3px `--accent` outline with 3px offset (amber on the dark contact section). Never remove an outline without replacing it.
- Decorative elements (`canvas`, caret, pointer follower) carry `aria-hidden="true"`.
- Toggle-like buttons use `aria-pressed`; the skill readout is `aria-live="polite"`; landmark regions without a visible heading have an `aria-label`.
- The typed line has a static `aria-label` so screen readers do not hear it being retyped.
- Use real elements: `button` for actions, `a` for navigation, `dl` for name/value pairs.

## Writing

- First person, plain and specific. Prefer a number to an adjective ("50+ stores", "91% test accuracy").
- British English spelling ("optimised", "analysed").
- Dates read `Sept 2026 – Present`: month, four-digit year, en dash with spaces. Months follow the existing entries: Sept, Oct and Jan are shortened; May, June and July are written in full.
- Separate inline items with a middle dot (`·`).
- Page title format: `Mohsin Syed | Software Engineer`.

## Icon

- `favicon.svg` is the source: an "M" in Unbounded 800, cream `#faf9f5` on indigo `#3a2fd0`, with an amber `#ffb300` copy offset down and right, echoing the `h1` shadow. The letter is stored as outlines, so it does not depend on the web font loading.
- `favicon.ico` holds 16, 32 and 48px renders; `apple-touch-icon.png` is 180px. Regenerate both whenever `favicon.svg` changes.

## Code conventions

### Structure

- The whole site is one file, `index.html`: one `<style>` block, the markup, one `<script>` block. No build step, no frameworks, no dependencies apart from Google Fonts.
- Icon files live next to it at the repository root, which is also the folder Netlify publishes. Anything added to the root, including this file, is publicly reachable on the site.

### CSS

- Tokens first, then base rules, then components grouped under a short `/* comment */`, then the 900px media query, then the reduced-motion rule last.
- Most rules sit on one line each, with properties on the same line and no space after colons. Only long rules (the token block, the pointer follower) wrap.
- Use tokens for colour and font family. Use `color-mix()` for tints instead of new tokens.

### JavaScript

- Content lives in data at the top of the script: `ME`, `WORK`, `GROUPS`, `EDU`, `STATS`. To change what the site says about experience, projects, skills, education or headline numbers, edit the data, not the markup.
- Behaviour lives in one strict-mode IIFE below the data.
- The existing style is ES5: `var`, function expressions, string concatenation. There are no arrow functions, `let`/`const` or template literals; keep new code in the same style.
- HTML is built as strings and assigned with `innerHTML`. Every data string must pass through `esc()` first. The stat numbers in `STATS` are the one value inserted unescaped, so keep them plain text.
- Use the `$` and `$$` helpers for queries, and the single delegated `click` listener on `document` for new click behaviour.
- Hooks are data attributes: `data-jump` (smooth-scroll link), `data-goto` (jump to a work card), `data-skill` (skill button).
- Feature-detect before use (`IntersectionObserver`, `ResizeObserver`, clipboard) and leave the page readable when a feature is missing.

### How skills link to work

A skill in `GROUPS` becomes a clickable button only if it appears in the `tech` list of at least one `WORK` entry. Items in a work entry's `extra` list show as chips on its card but do not link from the skills section. When adding a skill or a project, check both lists so the right pills become clickable.

## Making changes

- `main` deploys automatically to mohsin-syed.co.uk through Netlify.
- Work on a branch and open a pull request. Netlify builds a preview for each pull request; check it in light and dark mode, at a narrow width, and with reduced motion on, before merging.
- Commit messages: one imperative sentence in sentence case ("Add favicon and rename page title").
