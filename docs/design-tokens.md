# Plan Manager — Design Tokens & Standards

**Created:** 2026-04-23
**Scope:** `public/index.html` — the entire SPA
**Canonical location of tokens:** `public/index.html` — `:root` block (dark) + `html[data-theme="light"]` block

This document is the canonical reference for how the Plan Manager looks and
behaves. Every visual rule below is enforced via CSS variables in
`public/index.html`. If you change a token, change it here — then change it
in the CSS. Do not ship a colour, radius, or spacing value that isn't in this
table.

> **2026-06 — Sentinel dVPN App Design System reskin.** The palette and type
> were retoned to match the **Sentinel dVPN App Design System** (the mobile
> client's Figma source). The source-of-truth tokens now live in the
> `:root` block of `public/index.html` as a two-layer system:
>
> - **Base palette** — `--sentinel-*` (e.g. `--sentinel-brand-600: #0156FC`,
>   `--sentinel-surface-2: #191F31`). Extracted verbatim from the dVPN Figma.
> - **Semantic aliases** — `--color-*` (e.g. `--color-primary`,
>   `--color-surface`, `--color-text-secondary`) plus the legacy app
>   variables below (`--bg`, `--accent`, …) which now resolve **onto** the
>   `--sentinel-*` base. Build new UI against the semantic aliases.
>
> The values in the tables below reflect the dVPN palette.

---

## Plan Manager design tokens

### Font stack

The dVPN source face is **TT Hoves** (commercial); per the design-system
handoff it is substituted with **Poppins** (Google Fonts). **Manrope** is
genuine to the source for large display numerics; **Roboto Mono** stands in
for the source's mono on addresses/codes.

- **UI / display:** `Poppins` (Google Fonts: 400/500/600/700), fallback
  `-apple-system → Segoe UI → Roboto → sans-serif`.
  - Applied universally via `*, *::before, *::after { font-family: 'Poppins'
    ... !important }` near the top of the `<style>` block.
  - **Weight ceiling: Medium (500).** Nothing renders heavier than 500
    anywhere — no SemiBold/Bold. Headings, buttons, stat values, card
    titles and the wallet balance are all 500; `b`/`strong`/`th` are
    normalised to 500 too. Hierarchy comes from **size + colour**, not
    weight. Regular (400) is used for body/secondary text.
  - Base body size `14px`, line-height `1.5`.
- **Display numerics:** `Manrope` ExtraBold (`--font-num`) — reserved for
  large hero balances. Dense data-grid figures stay in mono so columns align
  (per the dVPN "figures read as data" rule).
- **Monospace (addresses, hashes, denoms, code):** `Roboto Mono` (Google
  Fonts, 400/500/700) → falls back to `Noto Sans Mono → Liberation Mono →
  Consolas → ui-monospace`. Exposed via `.mono` / `.mono-inline` / `code`
  (forced with `!important` so it escapes the universal Poppins rule) and
  `var(--font-mono)`.
- **Font vars:**
  - `--font-display: 'Poppins', -apple-system, 'Segoe UI', Roboto, sans-serif`
  - `--font-serif:   'Poppins', -apple-system, 'Segoe UI', sans-serif` (alias)
  - `--font-num:     'Manrope', 'Poppins', -apple-system, sans-serif`
  - `--font-mono:    'Roboto Mono', 'Noto Sans Mono', 'Liberation Mono',
    Consolas, ui-monospace, monospace`

**Rule:** Poppins for every human-readable string; Roboto Mono for every
machine string; Manrope only for large hero numerics. No other families, no
icon-font requirement. (Europa was the pre-reskin face — do not reintroduce.)

### Themes

- **Dark (default)** — `:root` block.
- **Light** — opts in via `html[data-theme="light"]` attribute on `<html>`.
  Toggle via the `toggleTheme()` button in the topbar; preference persists
  in `localStorage` and is re-applied on boot before first paint to prevent
  a flash of wrong theme.
- Both themes use the **same variable names.** Only values differ. Every
  new rule MUST reference a var — never a raw colour. This is how we keep
  the themes in sync.
- Before merging a UI change, screenshot both themes. A forgotten var
  renders white-on-white or black-on-black silently.

### Core colour vars

| Var | Dark | Light | Use |
|-----|------|-------|-----|
| `--bg` | `#000610` | `#f4f6fc` | Page background (dVPN deep navy) |
| `--bg-base` | `#000610` | `#f4f6fc` | Solid background under layered surfaces |
| `--bg-card` | `rgba(19,24,41,0.82)` | `rgba(255,255,255,0.94)` | Card surface (surface-1 over navy) |
| `--bg-card-solid` | `#131829` | `#ffffff` | Solid card (surface-1) |
| `--bg-card-hover` | `rgba(25,31,49,0.92)` | `#eef1fb` | Hover state on interactive cards (surface-2) |
| `--bg-input` | `#191F31` | `#ffffff` | Form inputs, code blocks (surface-2) |
| `--glass-bg` | `rgba(10,14,26,0.90)` | `rgba(255,255,255,0.88)` | Sidebar, topbar, modals |
| `--border` | `rgba(38,48,76,0.55)` | `rgba(31,54,124,0.12)` | Default divider (surface-3 hairline) |
| `--border-hover` | `rgba(38,48,76,0.95)` | `rgba(31,54,124,0.22)` | Hover / focus outline |
| `--border-strong` | `#1F367C` | `#1F367C` | Emphasised borders (indigo brand stroke) |
| `--text` | `#FFFFFF` | `#00102e` | Primary text |
| `--text-dim` | `#9CABC9` | `#44516e` | Secondary text (slate-400) |
| `--text-muted` | `#6B7A97` | `#6B7A97` | Placeholders, disabled, meta (slate-500) |
| `--accent` | `#0156FC` | `#0156FC` | dVPN brand blue — primary actions |
| `--accent-bright` | `#0184FC` | `#0046CE` | Interactive blue — links / hover |
| `--accent-glow` | `rgba(1,86,252,0.20)` | `rgba(1,86,252,0.12)` | Soft ambient glow behind accent elements |
| `--accent-dim` | `rgba(1,86,252,0.14)` | `rgba(1,86,252,0.08)` | Accent fill on badges / chips / active nav |
| `--accent-hover` | `#0184FC` | `#0046CE` | Button hover |
| `--green` | `#1FD18B` | `#0a9f63` | Success, registered state (cooled for navy) |
| `--green-bright` | `#34E89C` | `#08b06c` | Success hover / emphasis |
| `--green-dim` | `rgba(31,209,139,0.14)` | `rgba(10,159,99,0.10)` | Success badge fill |
| `--red` | `#D35D5D` | `#c2424a` | Error / destructive (dVPN danger) |
| `--red-strong` | `#9E1C29` | `#9E1C29` | Strong error fill (dVPN danger-strong) |
| `--red-dim` | `rgba(211,93,93,0.16)` | `rgba(194,66,74,0.10)` | Error badge fill, danger callout bg |
| `--yellow` | `#C4B130` | `#9a7b14` | Warning, low-balance, pending (dVPN gold) |
| `--yellow-bright` | `#F4EB34` | `#c4b130` | Highlight accent (dVPN accent-yellow) |
| `--yellow-dim` | `rgba(196,177,48,0.16)` | `rgba(196,177,48,0.14)` | Warning badge / callout bg |
| `--purple` | `#3B84EB` | `#3B84EB` | Ambient gradient accent (dVPN brand-400) |
| `--purple-dim` | `rgba(59,132,235,0.14)` | `rgba(59,132,235,0.10)` | Secondary gradient wash |

**Variant convention:** every semantic colour ships with a `-glow` and/or
`-dim` soft variant for ambient fills (backgrounds behind a badge, glow
under a button, callout surface). Never use a raw `rgba()` inline —
reference the `-dim` variant.

### Radii

| Var | Value | Use |
|-----|-------|-----|
| `--radius-lg`   | `16px`  | Hero cards, large surfaces / sheets |
| `--radius`      | `10px`  | Default card |
| `--radius-sm`   | `8px`   | dVPN default — buttons, inputs, tags, chips |
| `--radius-xs`   | `4px`   | Tag corners, scrollbar thumb |
| `--radius-pill` | `999px` | Switch tracks, status pills |

### Elevation (shadows)

| Var | Dark | Light | Use |
|-----|------|-------|-----|
| `--shadow-sm` | `0 2px 8px rgba(0,6,16,0.35)` | `0 2px 8px rgba(0,16,46,0.06)` | Resting card (cool navy) |
| `--shadow-md` | `0 8px 24px rgba(0,6,16,0.45)` | `0 8px 24px rgba(0,16,46,0.10)` | Hover, modals (dVPN card shadow) |
| `--shadow-lg` | `0 20px 60px rgba(0,6,16,0.60)` | `0 20px 50px rgba(0,16,46,0.14)` | Top-level overlays (import, low-balance) |
| `--shadow-accent` | `0 2px 18px rgba(18,49,109,0.45)` | `0 2px 18px rgba(1,86,252,0.18)` | Glow under primary CTAs |
| `--shadow-green`  | `0 0 24px rgba(31,209,139,0.20)`  | `0 0 24px rgba(10,159,99,0.16)`  | Glow under success states |

### Layout

- **Consistent gutter:** `--gutter: 32px` is the single horizontal padding
  shared by the topbar, demo banner and `.page` content, so every page's
  content aligns under the header. Page vertical padding:
  `--page-pad-top: 22px` / `--page-pad-bottom: 28px`.
- **Card padding:** centralised — `--card-pad-x: 20px` (header + body
  horizontal), `--card-pad-y: 18px` (body vertical),
  `--card-head-pad-y: 16px` (header vertical). All cards across all pages use
  these; `.card-body.tight` (`14px 16px`) is the only denser variant.
- **Shell:** `--sidebar-w: 272px` left sidebar + `--header-h: 84px` topbar.
  (Token values; do not hardcode `240px` / `64px` — these have been
  widened from defaults for the dense Plan Manager nav.)
- **Page padding:** `28px` vertical / `32px` horizontal on `.page`
  containers. Compact pages use `.page-compact` (smaller card padding).
  Homepage uses `.page-home` (wider hero, centered stats row).
- **Background wash:** two radial gradients layered on `body` — dVPN brand
  blue (`#0156FC`) glow in the top-right, indigo (`#1F367C`) in the
  bottom-left. Fixed-attached so the wash stays put during scroll. Mirrors
  the dVPN home-screen "soft radial brand-blue glow."
- **Max content width:** hero title container caps at `1100px`
  (`.page-home .page-hero`). Default `.page` inherits the shell width.

### Primitives

- **`.stat-card` / `.stat-card-enhanced`** — centred label + value pair,
  104px min-height, used in the dashboard stats row (wallet balance, P2P
  spot price, chain-active nodes, provider status). Value uses `20px` /
  `600` weight; label uses `11px` uppercase `500` weight with `0.4px`
  letter-spacing.
- **`.card` / `.card-header` / `.card-body`** — glass surface
  (`--bg-card`), `var(--radius-lg)` corners, `var(--shadow-sm)` at rest.
  Header: 16px/20px padding, `.card-title` at 16px/600. Body: 22px default
  (18px/20px on home, 14px/16px on compact).
- **`.tbl`** — data table primitive. Header row sits in `--bg-input` with
  `11px` uppercase muted label type. Rows separated by `--border`. Never
  use for flex content — grid-template-columns must match header to row.
- **`.chain-badge`** — monospace denom tag (e.g. `udvpn`, `uP2P`). Dim
  background, `--text-muted` text, `var(--radius-xs)` corners.
- **`.chip`** — small rounded pill, mono font, used for metadata
  (deposit amounts, fee-grant periods, short addresses).
- **`.nav-item`** — sidebar row. Inactive: `--text-dim` label. Active: adds
  a 3px left-edge accent bar (`--accent`), promotes label to `--text`, and
  lifts the background to `--accent-dim`. The edge bar is the single
  "you are here" signal — do not also change icon colour.
- **`.callout`** — inline notification inside a page. Variants via
  modifier classes: `.callout-warn` (yellow), `.callout-danger` (red),
  `.callout-info` (accent). Always paired with a `.callout-icon` svg and a
  flexbox row — never `text-align`-centered.
- **`.mono-inline` / `.mono-tag`** — inline monospace spans for CLI
  command names and denom tags; use sparingly.
- **`.loading-state`** — centred spinner + label. Hosted at
  `public/index.html:2146`. Only show when `#app` has no content; never
  wipe a painted page with a spinner (S3 blink rule).
- **`.mobile-gate`** — full-screen desktop-only overlay at viewport
  ≤1024px or coarse pointer. See `Desktop/standards/standards/S3-ux-frontend.md`
  "Mobile Desktop-Gate Rules".

---

## S3 rules baked into Plan Manager

These are the enforced rules. Every PR that touches the SPA must pass this
list before merge.

### Visual consistency

- **CSS variables for ALL colours.** Single source of truth; no raw hex or
  `rgba()` in component CSS. If the value isn't a var, add one to the
  token table above first.
- **Light + dark tested visually.** Screenshot both themes for any UI
  change before merging. A forgotten var renders white-on-white silently.
- **Font: `Poppins` everywhere, `Roboto Mono` for machine strings**
  (`Manrope` only for large hero numerics). No other families. Do not
  reintroduce Europa or Filson Soft.
- **Overflow visible on indicator bars.** Progress bars, status dots, and
  accent edges render outside their parent on purpose — a clipped glow is
  a bug.

### Layout

- **Flexbox for icon/text alignment.** `text-align: center` does not
  centre mixed content (emoji/svg + text). Use `display: flex;
  align-items: center; gap: Xpx`.
- **Grid header + row columns MUST match.** Data tables define
  `grid-template-columns` once and reuse; mismatched columns put data
  under the wrong headers.
- **Fixed-width wrappers for emoji and flags.** `display: inline-block;
  width: 1.5em; text-align: center` — emoji widths vary by OS.
- **Page-hero titles use `white-space: nowrap` on desktop** and revert to
  `normal` under 1024px (see `public/index.html:923`). Prevents an awkward
  wrap on the homepage line.

### Performance & feel

- **Loading states within 100ms.** Users perceive >100ms of nothing as
  lag. Show a spinner or skeleton immediately; do not wait for data.
- **One paint per navigation.** Don't wipe `#app` with a spinner when the
  current page is painted and readable — show a non-destructive "Updating…"
  badge instead. See S3 "Rendering & Transitions" rules.
- **Skeleton-first for async pages.** `renderDashboard()` paints its
  skeleton immediately with safe fallbacks (`w.balanceDvpn || 0`,
  `!w.provider ?`) and re-renders when `loadWallet()` resolves. Do NOT
  block first paint on an API call.
- **Stale-while-revalidate must diff before re-rendering.** Background
  refreshes that fire `onRefresh(fresh)` unconditionally cause visible
  re-renders when data hasn't changed. Diff first.
- **Cache API responses with TTL.** Node lists, balances, provider info
  all live in `_dataCache` with per-endpoint TTLs.
- **Debounce user input at 300ms.** Filter and search fields never fire
  per-keystroke.

### Language

- **P2P, not DVPN, in user-facing text.** The token is called P2P. DVPN
  is the on-chain denom (`udvpn`). Every label, stat, and callout says
  P2P. `udvpn` only appears as a denom tag in `.chain-badge`.
- **Mono spelling for denoms.** `uP2P` is always mono-formatted and
  lowercase-`u`.
- **Action verbs on buttons.** "Create plan", "Link nodes", "Grant
  subscriber" — not "Submit" or "Go".

### Data integrity

- **All visible state survives restart.** Dashboard values read from
  `my-plans.json` and `nodes-cache.json` on boot; never render "—" where
  disk had data.
- **Append-only for on-chain state files.** `my-plans.json` only grows.
  Never truncate without explicit user confirmation.
- **Don't wipe caches mid-flow.** A failed transaction does not justify
  dropping `nodes-cache.json`.

### Accessibility

- **Every interactive surface is keyboard-reachable.** `data-page`
  elements receive Enter/Space handling via the global listener at
  `public/index.html:3250`.
- **Focus outlines visible in both themes.** Never `outline: none`
  without an alternative focus ring.
- **`aria-hidden` on elements hidden from sighted users** (e.g. decorative
  svg icons, the mobile-gate-obscured SPA).

---

## How to add a new token

1. Add the variable to **both** the `:root` block AND the
   `html[data-theme="light"]` block in `public/index.html`.
2. Add a row to the table above with dark value, light value, and "Use".
3. Reference the variable (never the raw value) in your component CSS.
4. Screenshot both themes before merging.

## How to add a new primitive

1. Check the list above — if an existing primitive covers 80% of the use
   case, extend it with a modifier class instead of creating a new one.
2. Write the primitive so it reads from tokens (not raw values).
3. Add a row to the "Primitives" list above with its class name,
   purpose, and any modifier variants.
4. Test in both themes.

## Cross-references

- `Desktop/standards/standards/S3-ux-frontend.md` — universal UX standard
  across all Sentinel projects. This doc is the Plan Manager-specific
  binding.
- `Desktop/plans/CLAUDE.md` — project mission, focus areas, session
  startup.
- `Desktop/plans/MANIFESTO.md` — why Plan Manager exists.
