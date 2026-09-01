# AIXSMILE — Veneers (standalone microsite)

A single interactive page for the porcelain-veneers treatment, built from a
design handoff (`/home/muhammad-uzair/Downloads/Dental veneer study model.zip`).
It is deliberately **separate from the main aixsmile.de Next.js site** — no
build step, no framework, just static HTML/CSS/JS. Host it anywhere (Vercel,
Netlify, GitHub Pages, or a subdomain) independently of the main site.

## What's here

- `index.html` — the treatment page. A programmatically-built 3D upper-arch
  model is pinned beside scrolling copy; scroll drives the camera and seats
  six porcelain veneer shells onto the teeth (canines first, centrals last).
- `viewer.html` — a standalone inspect-and-download viewer for the same
  model (orbit controls + OBJ/MTL and GLB export), linked from the treatment
  page's "3D-Modell erkunden" button.
- `js/veneer-model.js` — the 3D model itself. Pure three.js geometry, no
  framework, ported verbatim from the handoff.
- `js/three-d-stage.js` — the `<three-d-stage>` web component used only by
  `viewer.html` (orbit + export toolbar). UI strings translated to German.
- `assets/tooth-mark.svg` — the real AIXSMILE tooth-mark logo, copied from
  the main site (`app/icon.svg`).

## What was changed from the handoff

The handoff's placeholder practice ("Marrow & Vale Dental") and colour
palette were swapped for the real thing:

- **Colours** now use AIXSMILE's actual design tokens (from the main site's
  `app/globals.css`): warm cream background (`--color-card`), deep navy ink
  (`--color-deep`), muted blue-grey (`--color-muted`), and the site's own
  AA-safe accent blue (`--color-teal-deep`) in place of the handoff's maroon.
  The 3D model's material colours (enamel, porcelain, gingiva) were left
  untouched — those are anatomically realistic, not brand colours.
- **Copy** is the real content already written for AIXSMILE's veneers page
  (`content/treatments/veneers.ts` in the main repo) — the "what it is"
  paragraphs and the 5-step process are reused verbatim; the handoff's
  fictional "2 appointments" stat was swapped for the site's real process
  (5 steps) and its real 15-year durability claim (from the site's own FAQ).
- **Branding**: header now shows the real AIXSMILE tooth-mark + wordmark
  (linking to aixsmile.de), footer shows the real address
  (Großkölnstraße 22–28, 52062 Aachen), and the primary CTA is a `tel:`
  link to the real practice phone number (0241 31202).
- **Language**: built in German, matching the practice's primary market and
  the tone of its existing copy. (The main site is bilingual DE/EN with a
  language switcher; this microsite is not — ask if you want an English
  version too.)
- Typography keeps the handoff's editorial pairing (Instrument Serif display
  + a sans body) rather than the main site's Space Grotesk/Inter, since this
  is a distinct campaign page — colours and content are what tie it to the
  brand. Body font was swapped from Karla to Inter (the main site's own body
  font) as a middle ground.

## Before going live

- **Fonts**: currently loaded from Google Fonts via `<link>`. The main site
  self-hosts fonts via `next/font` for GDPR reasons (no third-party request
  on page load) — do the same here before launch if that policy applies to
  this page too (e.g. self-host the two weights with `@font-face`).
- **Clinical claims**: the stat row and suitability checklist reflect what's
  already published on the main site's veneers page/FAQ, but a clinician
  should still sign off on this page's claims before it goes live, same as
  any other patient-facing content.
- **Analytics/consent**: nothing is wired up here (no cookie banner, no
  analytics). Add whatever the practice's other pages use if this needs to
  match.

## Running locally

No build step — just serve the folder statically, e.g.:

```
npx serve .
# or
python3 -m http.server 8080
```

Then open `index.html`. Opening the file directly via `file://` will NOT
work — ES module imports and the `<three-d-stage>` component both require
being served over http(s).
