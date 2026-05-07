---
name: testing-deepstorage
description: Test the Deep Storage static blog end-to-end visually. Use when validating design system / theme changes, SVG diagrams, conversor badges, recursos cards, or global search.
---

# Testing the Deep Storage blog

## Setup (zero deps, 100% static)

Serve the repo root with Python:

```sh
cd ~/deepstorage && python3 -m http.server 8765 &
```

No build step. No CI (`CLAUDE.md` says "Sem CI/CD"; deploy = push to `main` via GitHub Pages). Validation is visual.

## Pages to cover for a full design-system test

| Page | URL | Why |
|---|---|---|
| Home | `http://localhost:8765/` | Logo (`--primary`), hero h1 (`--secondary`), card surface, post-tag badges. |
| Conversor | `http://localhost:8765/conversor.html` | **Best discriminative page** — has 3 contextual badges side by side: `SI` Purple, `IEC` Pink, `BITS` Orange. If theme is wrong, one of these will look off. |
| Post id=6 (Snapshots) | `http://localhost:8765/post.html?id=6` | Embeds `snapshot-diagram.svg` (Volume Purple, shared blocks Pink, B-novo Purple). |
| Post id=7 (Replicação) | `http://localhost:8765/post.html?id=7` | Embeds `replication-diagram.svg` (sync arrows Purple, ACK arrows Pink, RPO badges Green-tinted). |
| Conceitos | `http://localhost:8765/conceitos.html` | Long page — readability sanity check. h1 in `--secondary`, body in `--text`. |
| Recursos | `http://localhost:8765/recursos.html` | YouTube section uses Red Dracula `#ff5555` on red-tinted bg — only place where `--secondary` is overridden by a contextual color. |

For a regression check, type something into the global search input on the home page (e.g. `snapshot`) — confirms `script.js` wasn't broken by CSS changes.

## Where the SVGs live

Both diagrams are referenced as `<img src="<name>-diagram.svg">` from inside post content in `script.js`:
- `snapshot-diagram.svg` → post id=6 (~line 888 of `script.js`)
- `replication-diagram.svg` → post id=7 (~line 1173 of `script.js`)

If you change SVG colors, you must also update `CLAUDE.md` rule 7 and `SPEC.md` (line ~240) which document the SVG palette.

## Current design system (Dracula) — verify these tokens are reachable

In `styles.css :root`:

```
--bg:        #282a36   (Background)
--surface:   #44475a   (Current Line)
--surface-2: #383a4a
--surface-3: #6272a4   (Comment)
--primary:   #bd93f9   (Purple)
--secondary: #ff79c6   (Pink)
--text:      #f8f8f2   (Foreground)
--text-muted:#6272a4   (Comment)
```

Contextual colors used in components: Cyan `#8be9fd`, Green `#50fa7b` (RPO badges), Orange `#ffb86c` (bits badge), Red `#ff5555` (YouTube/error states), Yellow `#f1fa8c`.

## Quirks / gotchas in this VM

- **Chrome devtools console returns "Chrome not in foreground" even when `wmctrl` confirms it's the active window.** This might be a tool-version mismatch and could be fixed in the future, but for now: don't rely on `getComputedStyle` via `computer.console`. Fall back to:
  1. Zoom into the rendered region with `computer.act` zoom
  2. Compare against the source files via `grep` for hex codes
  3. Use the legend in the SVGs themselves (snapshot SVG has a built-in legend distinguishing Purple vs Pink blocks)
- **`404.html` references `var(--primary-color)` and `var(--text-light)` — these tokens do NOT exist in `styles.css`.** This is a pre-existing bug independent of any theme migration. Don't fix it as part of a design-system PR.
- **Maximize Chrome before recording**: `sudo apt-get install -y wmctrl 2>/dev/null; wmctrl -r :ACTIVE: -b add,maximized_vert,maximized_horz`. Do NOT use `xdotool key super+Up` (tiles to half-screen).

## Discriminative assertions (anti-patterns to fail on)

When testing any theme change, these signals mean the migration is incomplete:
- Any terracota orange `#D97757` or `rgb(217,119,87)` visible → old Claude Code primary still bleeding through
- Any pure-black bg `#080808` or surface `#111111` → old base layer
- Conversor BITS badge in saturated yellow `#f0b429` → old yellow not migrated to Dracula Orange `#ffb86c`
- Replication SVG RPO badges with neon-cyan tint `rgba(0,255,159,...)` → old neon palette not migrated to Dracula Green `rgba(80,250,123,...)`

## Devin Secrets Needed

None. Site is 100% static and served locally.
