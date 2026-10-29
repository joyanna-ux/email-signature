# Email Signature Generator

Vanderbilt Mortgage email signature generator. Fill in name, title, department,
phone and licensing; copy an Outlook-safe signature or download it as a `.htm`.

**Prototype.** Built to the 2026-10 signature design. Image hosting still needs a
decision from the team (see below).

## Run it

It's one static page — no build, no dependencies.

```bash
# any static server works; e.g.
npx serve .
# or just open index.html in a browser (preview works; copy needs a hosted URL — see below)
```

## Deploy it

Publish the folder as-is to Netlify, GitHub Pages, or any static host. When the
page is served over `https://`, the generated signature automatically points its
images at `<page URL>/assets/...` (works at a domain root or under a subpath such
as GitHub Pages' `/email-signature/`), which is what recipients' mail clients
load. Opened from `file://` or `localhost`, the page shows a warning and the
copied signature uses a placeholder URL — set "Hosted site URL" under *Image
hosting* to override.

## Why the signature HTML looks the way it does

Classic Outlook on Windows renders with **Word's HTML engine**, and re-saves the
signature through it on every edit. Word drops CSS classes, ignores `flex`/`grid`/
`max-width`, rounds line-heights and rewrites spacing. The only markup that
survives a paste *and* a later edit:

- plain `<table>` layout, one cell per line
- every style inline, repeated on every cell (`font-family`, `font-size` in `pt`,
  `line-height` with `mso-line-height-rule: exactly`, `color`)
- a web-safe font stack (recipients don't have CinaGEO)
- images at absolute `https://` URLs with explicit `width`/`height` attributes
- link color and `text-decoration` set on each `<a>`

The design's skewed value badges can't be drawn this way, so they ship as one
PNG (`assets/value-badges.png`, the official brand asset). The Vanderbilt wordmark
is the official PNG too; the Clayton one is rendered from the ui-library SVG for now.

## Files

- `index.html` — the whole tool (markup, styles, generator)
- `assets/vanderbilt.png`, `clayton.png`, `clayton-tech.png`, `home-first.png` — the
  four brand logos, exported from one Photoshop canvas (`All logos.psd`, 1172×526) so
  they share alignment and quality. Resized to 320×144 (2× of the 160×72 display size)
  and palette-compressed with `pngquant`; each is a few KB.
- `assets/value-badges.png` — 779×69 export (text optically centered), compressed, displayed at 525×46
- `assets/favicon.svg` — the tool's browser-tab icon

To update a logo: export from the PSD at full size, then
`sips --resampleWidth 320 x.png --out x.png && pngquant --quality 80-98 --speed 1 --strip --force --output x.png x.png`.

Brand toggle: Vanderbilt Mortgage (default), Clayton, Clayton Tech, or HomeFirst Agency.
It swaps the logo and the website link, and sets the "Licensed (show NMLS #s)" default —
on for Vanderbilt, off for the others — which can still be overridden for any brand.

## Install instructions (for the people using it)

They're on the page itself, but in short:

- **Classic Outlook (Windows):** *Download .htm* → drop it in
  `%APPDATA%\Microsoft\Signatures\` → restart Outlook → File → Options → Mail →
  Signatures. Or *Copy for Outlook* and paste into that dialog's editor.
- **New Outlook / Mac / web:** *Copy for Outlook*, then Settings → Accounts →
  Signatures, paste.
- Never edit the signature inside Outlook; regenerate it here.
