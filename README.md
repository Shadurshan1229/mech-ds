# MECH Design System

**Version 2.0.0** - The MECH Studio

Live at [ds.themech.studio](https://ds.themech.studio)

---

## What this is

This is the central design system for all products built under The MECH Studio. It serves two things from one URL: a visual reference you can browse, and a token file you can import directly into any project.

The design language is flat, sharp, and light-first. Old-paper warmth with scifi precision. Orange is a signal color only - never decorative. Corners are square by default. No elevation shadows.

---

## Files

```
/
- index.html      Visual design system reference with light/dark toggle
- tokens.css      Importable CSS variable token file (CORS enabled)
- tokens.json     Same tokens as JSON for JS projects
- MECH_DS.md      Full design system specification document
- vercel.json     Vercel config - CORS headers and rewrites
```

---

## Using the tokens in a project

Add one line to the `<head>` of any MECH project:

```html
<link rel="stylesheet" href="https://ds.themech.studio/tokens.css">
```

Then use tokens directly in your CSS:

```css
.my-component {
  background: var(--mech-paper);
  color: var(--mech-dark);
  border: 1px solid var(--border-col);
}

.my-button {
  background: var(--mech-orange);
  font-family: var(--font-grotesk);
  border-radius: var(--radius-none);
}
```

For dark mode, set the attribute on the root element:

```js
document.documentElement.setAttribute('data-theme', 'dark')
document.documentElement.setAttribute('data-theme', 'light')
```

The token file ships both `[data-theme="light"]` and `[data-theme="dark"]` variable sets. Dark mode works automatically once you wire up the toggle.

---

## Token reference

### Colors

| Token | Light | Dark | Role |
|---|---|---|---|
| `--mech-orange` | `#FF5B24` | `#FF5B24` | Signal - CTA, active states, data |
| `--mech-dark` | `#1D1A19` | `#F1E7E2` | Primary text, headings |
| `--mech-paper` | `#F9F2EE` | `#1D1A19` | Base background |
| `--mech-paper-s` | `#F1E7E2` | `#252220` | Cards, sidebar surfaces |
| `--mech-ink-80` | `#3A3330` | `#C4B8B2` | Secondary text |
| `--mech-ink-50` | `#8C7D76` | `#8C7D76` | Placeholder, disabled |
| `--mech-ink-20` | `#D4C8C2` | `#3A3330` | Borders, dividers |
| `--mech-signal-green` | `#2ECC71` | `#2ECC71` | Success |
| `--mech-signal-red` | `#E74C3C` | `#E74C3C` | Error, destructive |

### Semantic aliases (use these in component code)

```css
--bg-base           /* page background */
--bg-surface        /* card and panel surfaces */
--text-primary      /* main text */
--text-secondary    /* secondary text */
--text-muted        /* placeholder, captions */
--border-col        /* default border color */
--border-strong-col /* heavy border, table headers */
```

### Typography

| Font | Role |
|---|---|
| Space Grotesk | All UI - headings, buttons, labels, nav |
| Poppins | Body text and descriptions only |
| JetBrains Mono | All numbers and data values |
| Termina Test | Formal document headings only |

### Spacing

Base unit is 4px. All spacing tokens are multiples of 4.

```css
--space-1: 4px    --space-2: 8px    --space-3: 12px
--space-4: 16px   --space-5: 20px   --space-6: 24px
--space-8: 32px   --space-10: 40px  --space-12: 48px
--space-16: 64px  --space-20: 80px
```

### Border radius

```css
--radius-none: 0px      /* default - all components */
--radius-sm:   2px      /* toggle thumb only */
--radius-full: 9999px   /* toggle track only */
```

### Motion

```css
--ease-default: cubic-bezier(0.4, 0, 0.2, 1)
--ease-out:     cubic-bezier(0, 0, 0.2, 1)
--ease-sharp:   cubic-bezier(0.4, 0, 0.6, 1)

--dur-instant: 80ms
--dur-fast:    150ms
--dur-base:    200ms
--dur-slow:    300ms
--dur-crawl:   500ms
```

### Legacy aliases

For backward compatibility with older MECH tools. Use canonical tokens in new code.

```css
--cream:        #F9F2EE   /* = mech-paper */
--charcoal:     #1D1A19   /* = mech-dark */
--brand-orange: #FF5B24   /* = mech-orange */
```

---

## Using tokens.json in JS/TS projects

```js
const res = await fetch('https://ds.themech.studio/tokens.json')
const tokens = await res.json()

const orange = tokens.color.orange.value   // '#FF5B24'
const paper  = tokens.color.paper.value    // '#F9F2EE'
```

Or import it as a static asset in a Next.js or Vite project and use it to generate Tailwind config values, style constants, or type-safe theme objects.

---

## Tailwind integration

If your project uses Tailwind, paste this into `tailwind.config.js`:

```js
module.exports = {
  theme: {
    extend: {
      colors: {
        'mech-orange':          '#FF5B24',
        'mech-dark':            '#1D1A19',
        'mech-paper':           '#F9F2EE',
        'mech-paper-secondary': '#F1E7E2',
        'mech-ink-80':          '#3A3330',
        'mech-ink-50':          '#8C7D76',
        'mech-ink-20':          '#D4C8C2',
        'mech-signal-green':    '#2ECC71',
        'mech-signal-red':      '#E74C3C',
        cream:    '#F9F2EE',
        charcoal: '#1D1A19',
        brand: { orange: '#FF5B24' },
      },
      fontFamily: {
        grotesk: ['Space Grotesk', 'sans-serif'],
        poppins: ['Poppins', 'sans-serif'],
        termina: ['Termina Test', 'sans-serif'],
        mono:    ['JetBrains Mono', 'monospace'],
      },
      borderRadius: {
        none: '0px',
        sm:   '2px',
        full: '9999px',
      },
      boxShadow: {
        none:   'none',
        signal: '0 0 0 2px rgba(255, 91, 36, 0.30)',
      },
    },
  },
}
```

---

## Design rules (short version)

**Geometry** - `border-radius: 0` everywhere by default. Toggle track is the only exception. If you are reaching for a rounded corner, question it first.

**Shadows** - None. Depth is communicated through border contrast and background color differences. The only permitted shadow is the focus ring: `0 0 0 2px rgba(255,91,36,0.30)`.

**Orange** - Signal color only. One CTA per view. Never as a background fill. Never decorative.

**Buttons** - Always Space Grotesk. Always square. No shadows. Link buttons (underlined, no border) are for in-context secondary actions only - table rows, card footers, "View all", "Edit".

**Numbers** - Every numerical value that represents data must use JetBrains Mono. Counts, IDs, timestamps, percentages, prices, durations.

**Uppercase** - Metadata labels only. Category tags, status badges, section nav headers, breadcrumbs, table column headers, form labels. Never page titles, card titles, button labels, or error messages.

Full rules and component patterns are in `MECH_DS.md`.

---

## Deploying updates

This repo is deployed via Vercel with a CNAME on Spaceship DNS.

```
ds  CNAME  cname.vercel-dns.com
```

To update the design system, edit the relevant files locally and push to `main`. Vercel redeploys automatically, usually under 30 seconds.

To update only the token values across all projects, edit `tokens.css` and push. All projects importing via the CDN link pick up the change on next page load (after the 24h cache expires, or immediately in dev).

---

## Projects using this DS

| Project | URL | Status |
|---|---|---|
| MECH DOS | dos.themech.studio | Active |
| MECH Finance | finance.themech.studio | Active |
| High! (mobile) | - | In development |
| ds.themech.studio | ds.themech.studio | This repo |

---

## Changelog

### v2.0.0
- Added full dark mode token set
- Mobile responsive layout with horizontal pill nav
- Light/dark toggle in topbar
- Square physical toggle component (MECH custom)
- Fixed checkbox - orange fill, white checkmark, works in both modes
- `tokens.css` and `tokens.json` served with CORS headers for cross-origin import
- `MECH_DS.md` downloadable directly from topbar button

### v1.2.0
- `mech-paper` and `mech-paper-secondary` values swapped
- Removed `mech-white`, `mech-ink-10`, `mech-orange-light`, `mech-orange-dim`, `mech-warning`
- Added legacy aliases: `cream`, `charcoal`, `brand.orange`

### v1.1.0
- Moved to zero border-radius system
- Flat shadow system - no elevation
- Square radio buttons and checkboxes
- Link button variant added
- Buttons moved to Space Grotesk
- Dark mode removed (reinstated in v2.0.0)

### v1.0.0
- Initial release

---

**The MECH Studio** - Build. Ship. Iterate.
