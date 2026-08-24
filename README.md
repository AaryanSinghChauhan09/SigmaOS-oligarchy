# oligarchy.fyi

A Cloudflare Worker serving one static page for `oligarchy.fyi`: the OLIGARCHY
banner over a tagline that rotates through the pitch for Omarchy's patronage
model — billionaire money in, free operating system out.

Colors and type are lifted from omarchy-site's `assets/css/root.css`
(Tokyo Night, JetBrains Mono), and `public/fonts/` holds copies of the two
woff2s it ships. Self-hosted rather than pulled from Google Fonts: a swapped
fallback reflows the page, and the banner's alignment depends on the exact
metrics.

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

`logo.txt` and `taglines.txt` are the sources; `public/index.html` has both
inlined. Change a source, then re-inline by hand — there's no build step, and
one page doesn't justify one.

The banner's size, tracking and leading are omarchy-site's `.pre pre` verbatim,
so it renders identically to the logo on omarchy.org. Only the clamp floor
differs — this banner is 100 columns to OMARCHY's 81, so it has to shrink
further before it fits a phone. A wider replacement needs that floor lowered
again. It's left-aligned inside a centered box on purpose: centering the text
trims each line's trailing spaces and knocks the rows out of column.

Taglines are held to two lines. The box is a fixed `3em` and the measure a flat
`40ch` — the widest that still wraps the longest tagline into two — so the
rotation never moves anything on the page. A longer tagline than
"Your free operating system…" (76 characters) breaks that and needs the measure
rechecked.
