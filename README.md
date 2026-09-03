# Penn MEDIATED — Team & Leadership

The team page for the [Center on Media, Technology and Democracy](https://infodem.upenn.edu) — leadership, staff, Knight Fellows, faculty advisors, and part-time staff. Static HTML/CSS, no build step.

- `index.html` — page markup
- `styles.css` — all styling (design tokens live at the top in `:root`)
- `assets/` — headshots, one file per person

This repo's design system is copied from [`about`](https://github.com/PennMEDIATED/about) — see that repo's README for the canonical spacing/color/type tokens and component conventions. Don't redefine a token or component pattern here that already exists there; pull the value from `about` instead so the two pages don't drift apart. The one intentional difference: `about`'s `--pad-x` is currently fixed (not responsive), but this page's is responsive (32px under 900px, 20px under 480px) — a multi-column card grid needs it, and `about`'s own README flags this as something to backport when a page has content wider than a headline/paragraph.

This page does **not** include a `postMessage` iframe-communication script of any kind (neither a scroll bridge nor a height-report), to stay consistent with `about`'s current no-postMessage state.

## Updating content

Each of the four sections (Core Team, Knight Fellows, Faculty Advisors, Part Time Staff) is a `.team-section` containing a `.team-section__grid` of `.person-card`s. To add someone:

1. Upload their headshot to `assets/`. Square-ish, reasonably tight crop on the face works best — the image is displayed as a 208px circle (`object-fit: cover`), so anything with the subject roughly centered will crop cleanly. If a photo's subject sits high or low in the frame, add an inline `style="object-position: top;"` (or a percentage value) on the `<img>`, following the pattern already used on several cards.
2. Copy an existing `.person-card` block in the right section and update the photo `src`, `alt`, name, and role text.
3. If they have a personal/lab website worth linking, make the card an `<a href="..." target="_blank" rel="noopener">` and include the `<span class="card-arrow" aria-hidden="true">↗</span>` — this is what shows the "opens an external site" badge on hover. If they don't have a link, use a plain `<div class="person-card">` instead (no `<a>`, no arrow badge) — see the Part Time Staff section for examples of both.
4. For a former member who should stay listed but marked as alumni, add `person-card--alumni` to the card's class list and include `<span class="person-card__tag">Alumna</span>` (or `Alumnus`) as the first child inside the card.
5. The reveal-on-scroll stagger is defined by CSS `nth-child` selectors up to `.person-card:nth-child(8)` in `styles.css`. If a section ever grows past 8 cards, add the next `nth-child` rule (following the existing `0.05s` increment pattern) or the extra cards just won't have a stagger delay — they'll still fade in, just without the cascading timing.

## Typography

Sitewide convention. The `--fs-*`/`--lh-*` block at the top of `styles.css` is canonical and identical in every page repo.

**Two families, no third.** `--f-serif` (EB Garamond) for page and section titles and pull-quote copy; `--f-sans` (DM Sans) for everything else. There is no monospace face — uppercase micro-labels are DM Sans 700 uppercase with `letter-spacing: 0.08em`.

**Sizes come from tokens, never raw px.**

| Token | Mobile (=<480px) | Desktop (>=1440px) | Used for |
| --- | --- | --- | --- |
| `--fs-display` | 36px | 76px | full-bleed hero |
| `--fs-h1` | 36px | 56px | page title |
| `--fs-h2` | 26px | 40px | section titles |
| `--fs-h3` | 20px | 24px | card and third-level titles |
| `--fs-lede` | 18px | 20px | intro paragraphs |
| `--fs-body` | 16px | 16px | body copy |
| `--fs-small` | 14px | 14px | captions, meta, form controls |
| `--fs-small-serif` | 15px | 15px | EB Garamond at small sizes |
| `--fs-micro` | 12px | 12px | uppercase labels, tags, counts |

The top five are `clamp()` values that interpolate across the viewport, so tablet widths need no separate `@media` override. Only add a breakpoint font-size when a specific layout actually demands it.

**12px is the floor.** Nothing ships smaller. EB Garamond and uppercase-with-letter-spacing both read smaller than their nominal size, which is what `--fs-small-serif` and the 12px floor exist to absorb.

**Line heights are tokens too** — `--lh-display` 1.05, `--lh-heading` 1.15, `--lh-lede` 1.26, `--lh-title` 1.3, `--lh-body` 1.55. Never set a line-height in px; it breaks the fluid sizes.

**Heading gaps.** Section title to first content is `var(--space-300)` (24px); page or hero title to content is `var(--space-250)` (20px).

**Section rhythm.** A full-width colored section carries `var(--space-1000)` (80px) top and bottom padding, so its heading never sits flush against the band's edge. The page hero's bottom padding is `var(--space-600)` (48px) — shorter than 80px because the section below supplies its own.

## Components

- **Hero**: the serif, accent-purple `.team-hero__title` ("Leadership & Staff") and a plain sans lede paragraph (full padded width, no `max-width` of its own — per `about/README.md`'s "Heading and body-copy positioning" rule) — the same shape as `about`'s Mission Statement block. **No eyebrow/kicker label above the hero heading** — removed 2026-08-28 (previously `.eyebrow` "Our Team"); the sitewide convention now is hero/section headings stand alone with nothing above them.
- **Section headings** (`.team-section__title` + `.team-section__lede`): 40px / weight 600 / `-0.02em` / line-height 1.15 heading, copied directly from `about`'s repeated colored-block chapter titles (`.partners__title` / `.related-centers__title`), paired with a one-line 20px/300 lede at the documented flat 24px gap (`.partners__lede` / `.related-centers__lede`'s pattern) instead of jumping straight to the card grid. No border-bottom, matching `about`'s version of this heading. All four sections share `padding-block: var(--space-1000)` (`.team-section`), matching `about`'s Orbital/Partners/Related-Centers color-block rhythm — it's not exclusive to the colored ones. `.team-section--gradient` (the first section, "Core Team") and `.team-section--purple` (Faculty Advisors) additionally override the section background to the shared `--c-gradient` / `--c-accent` and both the heading and lede to white — Knight Fellows and Part Time Staff stay plain white with red headings.
- **Person cards** (`.person-card`): on Core Team and Faculty Advisors (the colored sections), a plain white tile. On Knight Fellows and Part Time Staff (plain white sections), the tile inverts to a white fill with a 2px `--c-red` border instead, so there's still a clear accent even without a colored backdrop. Alumni status (`.person-card--alumni`) overrides either of those to a white fill with a 2px `--c-gray` border, plus the `.person-card__tag` badge — same "border, not fill" language, just muted. Circular grayscale-to-color headshot (208px, sized up from an earlier 176px once the grid columns widened, so it doesn't float in leftover space), name, and role. Hover state is `box-shadow` + `translateY(-4px)`, matching `about`'s logo-tile hover exactly — never a border-color change on hover. (The border treatment overall intentionally departs from `about`'s `aspect-ratio: 4/3` logo-tile pattern — circular headshots are this page's own established look, kept even though it's not a documented `about`/`home`/`grants` component.)
- **External-link arrow badge** (`.card-arrow`): copied verbatim from `about/styles.css` — same 26px circle, same sliding purple-to-orange gradient hover. Only appears on cards that are links.
- **Scroll-triggered reveal**: single-tier, one-shot only — every `.reveal` element (hero + all four sections) fades in once and stays visible, unlike `about`'s continuous toggle-in/toggle-out treatment on its own colored sections. This was a deliberate departure: a toggling reveal on a section that ends up sitting at the very bottom of the page (no more room to scroll) is prone to a scroll-jitter "spasming" bug where overscroll bounce rapidly re-triggers the fade. One-shot avoids that entirely and reads calmer for a static roster page anyway.
- No scroll-hint arrow (unlike `about`'s Orbital-linked one) — tried it, but with only one hero-to-first-section transition and no long marketing scroll, it didn't add anything, so it was removed.
## Hyperlinks

One taxonomy, five categories, shared by every page repo. Pick the category by what the link *is*, not by which repo you happen to be editing.

**1. In-text links** — embedded mid-sentence in flowing prose.

| ground | text | underline | hover |
| --- | --- | --- | --- |
| white / light | `--c-dark` | `border-bottom: 1px solid rgba(13, 13, 12, 0.35)` | text and underline both turn `--c-red` |
| colour / gradient | `--c-white` | `border-bottom: 1px solid rgba(255, 255, 255, 0.5)` | fade to `opacity: 0.7` — no colour swap |

The underline is a `border-bottom`, not `text-decoration`, so its colour can be transitioned independently of the text on hover. Pair it with `transition: color 0.15s, border-color 0.15s` on light grounds and `transition: opacity 0.15s` on coloured ones.

White-to-anything reads poorly on a saturated ground, which is why the coloured case fades instead of changing hue.

**2. Independent links** — a standalone text link that isn't inside a sentence ("Learn More About the Center", "Download the Full Schedule"). Same colours, decoration and hover as category 1, **plus a thin arrow** `⟶` after the text. Use `⟶` (`&#10230;`), not the `↗` badge from category 4.

**3. Document buttons** — an independent link that opens a document (a PDF, a report). A filled button box, not text:

| ground | box | text |
| --- | --- | --- |
| white / light | `--c-red` | `--c-white` |
| colour / gradient | `--c-white` | `--c-dark` |

Hover is **movement, not colour** — a lift or nudge. Do not darken or recolour the box.

**4. Links to another web page** — this site or an external one. The containing box carries the shared `.card-arrow`: a 26px dark circle with a white `↗`, in the box's top corner. On hover the arrow scales slightly and its background becomes a sliding purple-to-orange gradient (`@keyframes card-arrow-slide`), and the box itself animates. No separate text button — the whole box is the link.

**Exception:** a link to a research paper is category 2, not this — thin arrow, no badge.

**5. Hyperlinked headings** — a heading that is itself a link (a post title, a card title). Colour shift on hover per the ground rules above, and **no arrow and no underline**.

### Dropdowns and disclosures

A dropdown, `<details>` block or expand/collapse control uses one affordance sitewide: a **chevron SVG** (`M2 5l5 5 5-5`, 13×13, `--c-red` stroke, `stroke-width: 1.8`) beside a `--c-red` label at `--fs-small`, rotating `180deg` on open with `transition: transform 0.25s`. See `llm-civic-discourse`'s "Full summary & details" toggle for the reference implementation.

Never leave the marker to the browser — style `<select>` with `appearance: none` and supply the chevron, and hide the native `<summary>` marker. The `↗` circle badge is category 4's language and does not belong on a disclosure control.

- **`overflow-anchor: none`** (on `html` and `body`): fixes a browser "scroll anchoring" bug where the page silently scrolled itself down past the hero title right after load. The hero title sits at the very top with only 20px of padding above it, and when the web fonts swap in a beat after first paint, its height shifts slightly; the browser's default scroll-anchoring then nudged the viewport to compensate, cropping the title. This turns that off. If this page's structure ever changes to be embedded inside another document (an iframe, or injected into a different site's DOM) rather than served as its own standalone page, revisit whether this still belongs on `html`/`body` or needs to move somewhere scoped to this page's own markup.

## Keeping in sync

If you change a shared token or component pattern here, check whether `about` (and by extension `home`/`grants`) should get the same change, and vice versa — these repos duplicate CSS rather than sharing a stylesheet, so consistency is a discipline, not something enforced automatically.
