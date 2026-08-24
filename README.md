# oligarchy.fyi

A Cloudflare Worker serving one static page for `oligarchy.fyi`: the OLIGARCHY
banner over a tagline that rotates through the pitch for Omarchy's patronage
model — billionaire money in, free operating system out.

Colors and type are lifted from omarchy-site's `assets/css/root.css`
(Tokyo Night, JetBrains Mono), and `public/fonts/` holds copies of the two
woff2s it ships. Self-hosted rather than pulled from Google Fonts, so a
swapped fallback never reflows the page.

## Deploying

```sh
npx wrangler deploy
```

Routes `oligarchy.fyi/*` and `*.oligarchy.fyi/*` are declared in
`wrangler.jsonc`. DNS is two proxied `A` records pointing at `192.0.2.1` — a
placeholder, since the Worker intercepts at the edge and no origin is ever
contacted.

Unmatched paths serve the same page rather than a 404, so mistyped deep links
still land on the joke.

## Editing the copy

`logo.txt` and `taglines.txt` are the sources. Run `bin/build-logo` after
editing the banner; taglines are a plain array in `public/index.html`, edited
by hand.

The banner is vector rather than a `<pre>`, which is how omarchy.org draws its
logo. At the type sizes a phone lands on — around 5px, since this banner is 100
columns to OMARCHY's 81 — Chrome's per-glyph rounding walks the rows out of
column and the word turns to mush. `bin/build-logo` emits the same art as one
SVG path instead, using JetBrains Mono's own geometry (block glyphs fill the
full 0.6em advance and −0.3 to 1.02em vertically, half blocks split at 0.36em)
and omarchy-site's 0.0425em of tracking. So it draws what the `<pre>` was meant
to look like, at any size, on any device.

Taglines are held to two lines. The box is a fixed `3em` and the measure a flat
`40ch` — the widest that still wraps the longest tagline into two — so the
rotation never moves anything on the page. A longer tagline than
"Your free operating system…" (76 characters) breaks that and needs the measure
rechecked.
