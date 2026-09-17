# gammaxrs-website

The Gamma XRS website and brand system.

Everything here is a static file. There is no build step, no bundler and no dependencies —
open `index.html` in a browser and it works.

```
index.html                        the website
assets/                           photography and the stage elevation
brand/index.html                  the brand system, GX-BRAND-001 Rev A
docs/brand-and-stage-reference.md the short version, plus what is still unconfirmed
```

## The website

A single page with hash routing — `#/`, `#/signature-stage`, `#/atelier-stage`,
`#/creator-partnerships`, `#/about`, `#/contact`, `#/enquiries`. One file keeps the whole
identity in one place; splitting it into separate documents is a reasonable thing to do later,
but it is not necessary to ship.

The Aperture is **generated in the page** from the parameters in section 03 of the brand system —
17 blades per array, 23u pitch, a fixed 32u gate, smoothstep crown, parabolic bed. It is not an
image file, so it is sharp at any size and cannot drift from the specification. If you change
those numbers, you have changed the logo.

Type comes from Google Fonts: Archivo (variable, the width axis carries the hierarchy), Michroma
for the logotype only, and JetBrains Mono for every measured value.

### Connecting the forms

The enquiry form and the newsletter sign-up both go through one function. Near the top of the
script block:

```js
var FORM_ENDPOINT = "";                        // ← your endpoint
var CONTACT_EMAIL = "bookings@gammaxrs.com";   // ← fallback shown to the visitor
```

Set `FORM_ENDPOINT` to anything that accepts a JSON `POST` — Formspree, Netlify Forms, a Lambda,
your own API route — and submissions go there. The payload is flat:

```json
{ "kind": "Stage booking", "name": "", "email": "", "phone": "",
  "org": "", "dates": "", "message": "", "createdAt": "ISO-8601" }
```

Leave it empty and the page falls back to the Claude artifact store when it is running inside
one, and otherwise tells the visitor to email `CONTACT_EMAIL`. Nothing silently fails.

The `#/enquiries` page reads that same artifact store. On a normal web host it will report that
storage is unavailable — once you point `FORM_ENDPOINT` at your own backend, read the submissions
there instead and drop the route.

### Deploying

Any static host. For GitHub Pages: Settings → Pages → Deploy from a branch → `main`, `/ (root)`.
The brand system is then at `/brand/`.

## Before this goes live

The copy deliberately shows unconfirmed values in brackets, in Pulse pink, so nobody mistakes a
placeholder for a fact. Search the source for `class="ph"` — every one is a decision waiting on
you:

- Street address, PIN, bookings and partnerships email, phone
- Atelier Stage LED panel size, pixel pitch, and the rate card
- Signature Stage power, rigging and load-in
- Current lead time and the Signature Stage rate card
- Environment library catalogue
- Creator Partnerships annual commitment
- Post options on the Atelier

## The brand system

`brand/index.html` is GX-BRAND-001 Revision A, as published. It is the single source of truth for
the marks, colour, typography, motion, art direction and voice — where a template and that
document disagree, the document wins and the template gets fixed.

Two rules that are easy to break by accident and expensive to unpick:

- **The ramp switches by ground, not by preference.** Spectrum Prime on dark, Spectrum Deep on
  light. The Prime ramp opens on a pale yellow that scores 1.2:1 against white.
- **The logotype is artwork, not a font you install.** Michroma is the face it derives from; the
  wordmark itself has custom terminals and is never reset.

## Licence

All rights reserved. The photography is production stills from Gamma XRS shoots and is not
licensed for reuse.
