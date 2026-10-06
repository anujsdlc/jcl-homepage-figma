# JCL Homepage Redesign — Context

Resume-from-cold brief. Read this and the three memory files in
`~/.claude/projects/-Users-anuj-Desktop-JCL/memory/` before touching the code.

## What this is

A static-HTML design study for Jefferson County Library (Missouri).
Five self-contained pages — no build step, no framework, no backend.
Addresses specific jeffcolib.org pain points: search is buried, library
card CTA is hidden, no clear "my account" affordance, weak mobile.

- **Repo**: https://github.com/anujsdlc/jcl-homepage-figma
- **Deployed**: https://jcl-homepage-teal.vercel.app  (stable alias)
- **Deploy cmd**: `vercel deploy --prod --yes`

## Pages

| File              | Nickname              | What it is |
|-------------------|-----------------------|------------|
| `index.html`      | Original              | Apple-inspired baseline, hero + events preview + calendar + branches |
| `variation-1.html`| **The Stacks**        | Editorial, hard-shadow, search-first. Copy voice is the reference tone. |
| `variation-2.html`| **The Commons**       | Warm/community, chunky borders, full-month calendar centerpiece |
| `variation-3.html`| **The Bento (events)**| Apple-clean rounded cards, right rail = today's programs |
| `variation-4.html`| **The Bento (stats)** | Same bento shell as v3, right rail = live stats + annual impact |
| `preview.html`    | Picker                | Four cards linking to each variation |

v3 and v4 share layout but differ by right rail content.

## Design tokens — locked

Never add new colors or fonts. All work maps back to these.

```
--navy:  #052460    --red:    #da2c38    --orange: #ff6b35
--lilac: #c8b8db    --ink:    #0a0a0a    --ink-muted: #4d4d4d
--surface: #ffffff  --surface-alt: #f5f5f7   --surface-warm: #fafafa

--font-display: 'Arimo'       (body)
--font-brand:   'Comfortaa'   (headings, kickers, CTAs)
```

Font floor: **12px minimum** anywhere (user requirement — readability).

## Rules & conventions

- **Tone**: Match variation-1 copy voice everywhere — human, warm, first-person plural ("we", "our librarians"), concrete nouns, no corporate buzzwords. See `memory/feedback_v1_tone.md`.
- **State**: Missouri (not Colorado). All ZIPs, cities, "St. Louis County Library".
- **No My Account** sections or on-page account dashboards — account lives on an external portal. Keep "Get a Card" header CTA.
- **No arrows (→) inside buttons** (`.btn`, `.pill-*`). Pagination arrows (← →) that *are* the label are fine.
- **No icons in button labels** on v2 (user explicitly asked).
- **"See all" links** on card rows: plain text, no arrow, no capsule.
- **Book titles**: original/fictional (The Silent Season by Lena Ortiz, etc.) — never real copyrighted books.
- **Branches**: Arnold, Cedar Hill, Northwest, Windsor, Imperial, High Ridge (7 total mentioned in copy).

## Features & systems

- **EN/ES language toggle** (index + v2 only; v1 strip has "Español" link).
- **Dark/light theme toggle** — inline, placed next to the language switch on every page.
  - Persisted in `localStorage['jcl-theme']`, falls back to `prefers-color-scheme`.
  - Dark palette = Facebook-style neutrals: `#18191A` base, `#242526` cards, `#1E1F20` warm.
  - Text hierarchy in dark: `#E4E6EB` primary / `#B0B3B8` secondary / `#8A8D91` tertiary.
  - **Dark-mode rule**: never use colored text (navy / red for body copy) on dark surfaces. Use the grey hierarchy. Colored backgrounds are OK for badges.
  - Logo in dark: `filter: brightness(0) invert(1)` so it renders white.
- **Mobile bottom bar** (≤720px) on every page: Search · Card · Events · Menu — each styled to match the variation's language. Desktop quick-actions sections are hidden on mobile since the bar replaces them.
- **Notice bar** (top of every page): gradient navy, orange 3px bottom accent, pulse-green dot, 3 facts separated by `·`.
- **Newsletter**:
  - Mobile (≤720px): compact strip right below the notice.
  - Desktop on `index.html` only: dismissible floating card bottom-right (`#btmSignup`), slides in ~2.6s after load, `localStorage['jcl-nl-dismissed']`.
- **Mobile overflow guard**: `html, body { overflow-x: hidden; max-width: 100% }` on every page (fixes a right-side gap on small screens).

## Shelf pattern (v1/v2/v4-era v3)

Three category rows per books section:
1. New arrivals (just added)
2. Recently added · For adults & teens
3. Recently added · For children

Each row = 8 book covers, same 24 original titles across variations.
Heading format: `<kicker muted>` + `<h3 heading>` on one line (user asked for this specifically for v4; applied to v1/v2 too).

## Right-rail content (v3 & v4)

Both have "Open now · 7 of 7 branches" with pulse dot at the top.

- **v3** — events-focused:
  Today's four programs (time + title + branch · audience) · Support JCL CTA · Get in touch links (branch, contact, newsletter).
- **v4** — stats-focused:
  Today at JCL (events, study rooms, hotspots, new arrivals) · Support JCL CTA · This year, together (1.4M items, 312K cardholders, 2,400 programs, 18K volunteer hours) · Get in touch links.

## Deploy workflow

```
# normal
git add -A
git commit -m "..."       # include Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>
git push origin main
vercel deploy --prod --yes
```

The alias `jcl-homepage-teal.vercel.app` automatically points to the latest prod deployment.

## Known not-done / scope boundaries

- No build system. Pure HTML + inline CSS + inline JS per page.
- No real backend. Forms are `onsubmit="event.preventDefault()"` demos.
- v3/v4 don't have an EN/ES toggle (no language switch widget).
- `preview.html` references `.v-portal` CSS that is unused since the old v3 (Dark Portal) was dropped — harmless, left in place.
