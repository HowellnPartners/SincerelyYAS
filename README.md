# Sincerely YAS — Vol. 01

Static marketing site for Sincerely YAS. Two pages, no build step, no dependencies —
open `index.html` in a browser and it runs.

Structure and interaction are modelled on [casperscaviar.com](https://casperscaviar.com);
the gallery page follows [noartmusic.com/gallery](https://www.noartmusic.com/gallery).

---

## Structure

```
.
├── index.html      # homepage — hero, quote band, featured product, shop grid, made, lookbook, newsletter
├── gallery.html    # full gallery, lightbox viewer
├── assets/         # images + signature wordmark PNGs
└── README.md
```

Everything is inline — one `<style>` block and one `<script>` block per page. That's
deliberate: it keeps the handoff to a Shopify/Webflow theme trivial, since each section
can be lifted out whole.

### Homepage sections, in order

| Section | Anchor | Notes |
|---|---|---|
| Announcement ticker | — | CSS marquee, pauses under `prefers-reduced-motion` |
| Nav | — | Transparent over the hero, pins to a solid bar past it |
| Hero | `#top` | Still image; `<video>` tag is commented in and ready |
| Quote band | `#about` | The sleeve embroidery line |
| Featured product | `#hat` | 50/50 split with a rotating seam stamp |
| Shop grid | `#shopgrid` | 3 columns, parallax side columns, row hover |
| Made | `#made` | Editorial split + spec table |
| Footer | `#contact` | Join the list, brand statement, two link columns, bottom bar. Form posts nowhere yet — see Backend below |
| Product detail | — | Full-screen overlay, opens from any shop card |

---

## Design tokens

Defined as CSS custom properties at the top of each file.

```css
--cream:   #FFFFFF;   /* page ground */
--cream-2: #FFFFFF;   /* image wells, panels */
--ink:     #16110F;   /* text */
--red:     #D30000;   /* brand red */
--lacquer: #7E1512;   /* deep red panel */
```

**Type** is Roboto Mono throughout (300/400/500/700), uppercase, wide tracking,
justified body copy. Display headings sit at 500. `Mrs Saint Delafield` is loaded for
the red script accent lines and is a stand-in for the real handwriting — swap it when
the logo font is finalised.

**Brand artwork** supplied by Brooke Rogers Design, exported from the master files:

- `assets/logo-{red,white,ink}.png` — signature wordmark (nav, hero, footer, gallery bar)
- `assets/lockup-{red,white,ink}.png` — bouquet over signature (footer newsletter block)
- `assets/mark-bouquet.png` — bouquet alone (About band, seam stamp centre)
- `favicon.ico`, `assets/favicon-512.png`, `assets/apple-touch-icon.png`

Swap for SVG when the vector files are available — the PNGs are 960–1400px, fine at
current sizes but not future-proof.

---

## Product data

The three products live in a single `PRODUCTS` object near the bottom of `index.html`.
Name, origin, price, description, sizes, image list and accordion rows all come from
there — edit that object and both the shop cards and the product overlay update.

---

## Placeholders to replace

- [ ] **Prices** — every product is `$00.00`
- [ ] **Hero video** — `assets/hero.mp4`; the `<video>` block is commented in above the still
- [ ] **Logo SVG** — replacing the extracted PNGs
- [ ] **Hat product rendering** — currently using a lifestyle shot in the featured split
- [ ] **Cart** — the count is hardcoded to `(0)`

---

## Deploying to GitHub Pages

1. Push this folder to a repository.
2. Settings → Pages → Source: `Deploy from a branch`, branch `main`, folder `/ (root)`.
3. It'll serve at `https://<user>.github.io/<repo>/`.

For a custom domain, add a `CNAME` file containing the domain and point a `CNAME` DNS
record at `<user>.github.io`.

---

## Backend

Nothing here talks to a server yet. Three things will need one:

- **Cart and checkout** — the product overlay's Add to cart is a UI stub
- **Newsletter** — the form prevents default and shows a confirmation
- **Inventory** — sizes are hardcoded in the `PRODUCTS` object

The lightest path is Shopify's Storefront API or Buy Button behind this exact markup,
which keeps the design intact and hands PCI, tax and fulfilment to Shopify.

---

## Browser support

Modern evergreen browsers. Uses CSS grid, `aspect-ratio`, custom properties,
`IntersectionObserver` and `backdrop-filter`. Motion is disabled under
`prefers-reduced-motion`. Layout is responsive down to 375px.
