# Penn MEDIATED — Team & Leadership

The team page for the [Center on Media, Technology and Democracy](https://infodem.upenn.edu) — leadership, staff, Knight Fellows, faculty advisors, and part-time staff. Static HTML/CSS, no build step.

- `index.html` — page markup
- `styles.css` — all styling (design tokens live at the top in `:root`)
- `Photos/` — headshots, one file per person

This repo's design system is copied from [`about`](https://github.com/PennMEDIATED/about) — see that repo's README for the canonical spacing/color/type tokens and component conventions. Don't redefine a token or component pattern here that already exists there; pull the value from `about` instead so the two pages don't drift apart. The one intentional difference: `about`'s `--pad-x` is currently fixed (not responsive), but this page's is responsive (32px under 900px, 20px under 480px) — a multi-column card grid needs it, and `about`'s own README flags this as something to backport when a page has content wider than a headline/paragraph.

This page does **not** include a `postMessage` iframe-communication script of any kind (neither a scroll bridge nor a height-report), to stay consistent with `about`'s current no-postMessage state.

## Updating content

Each of the four sections (Leadership and Staff, Knight Fellows, Faculty Advisors, Part Time Staff) is a `.team-section` containing a `.team-section__grid` of `.person-card`s. To add someone:

1. Upload their headshot to `Photos/`. Square-ish, reasonably tight crop on the face works best — the image is displayed as a 176px circle (`object-fit: cover`), so anything with the subject roughly centered will crop cleanly. If a photo's subject sits high or low in the frame, add an inline `style="object-position: top;"` (or a percentage value) on the `<img>`, following the pattern already used on several cards.
2. Copy an existing `.person-card` block in the right section and update the photo `src`, `alt`, name, and role text.
3. If they have a personal/lab website worth linking, make the card an `<a href="..." target="_blank" rel="noopener">` and include the `<span class="card-arrow" aria-hidden="true">↗</span>` — this is what shows the "opens an external site" badge on hover. If they don't have a link, use a plain `<div class="person-card">` instead (no `<a>`, no arrow badge) — see the Part Time Staff section for examples of both.
4. For a former member who should stay listed but marked as alumni, add `person-card--alumni` to the card's class list and include `<span class="person-card__tag">Alumna</span>` (or `Alumnus`) as the first child inside the card.
5. The reveal-on-scroll stagger is defined by CSS `nth-child` selectors up to `.person-card:nth-child(8)` in `styles.css`. If a section ever grows past 8 cards, add the next `nth-child` rule (following the existing `0.05s` increment pattern) or the extra cards just won't have a stagger delay — they'll still fade in, just without the cascading timing.

## Components

- **Eyebrow + hero**: `.eyebrow` ("Our Team") above the serif, accent-purple `.team-hero__title` ("Leadership & Staff") and a plain sans lede paragraph (full padded width, no `max-width` of its own — per `about/README.md`'s "Heading and body-copy positioning" rule) — the same shape as `about`'s Mission Statement block. The eyebrow is used once, at the top of the page.
- **Section headings** (`.team-section__title`): 24px / weight 600 / `--c-dark` with a bottom border — `about`'s documented "section header" pattern for a plain (non-gradient) background, reused here since this page repeats a heading four times rather than using it once on a colored block. `.team-section--gradient` (currently on the first section, "Core Leadership & Team") overrides the section background to the shared `--c-gradient` and the heading to white with a translucent white border, using `padding-block: var(--space-1000)` like `about`'s Orbital/Partners/Related-Centers color-block sections — the other three sections stay plain white.
- **Person cards** (`.person-card`): a plain white tile (no accent top-border), a circular grayscale-to-color headshot, name, and role. Hover state is `box-shadow` + `translateY(-4px)`, matching `about`'s logo-tile hover exactly — never a border-color change. Alumni cards (`.person-card--alumni`) are marked via the `.person-card__tag` badge only. (This one intentionally departs from `about`'s `aspect-ratio: 4/3` logo-tile pattern — circular headshots are this page's own established look, kept even though it's not a documented `about`/`home`/`grants` component.)
- **External-link arrow badge** (`.card-arrow`): copied verbatim from `about/styles.css` — same 26px circle, same sliding purple-to-orange gradient hover. Only appears on cards that are links.
- **Scroll-triggered reveal**: same two-tier system as `about/index.html`. The hero is one-shot (fades in once, stays visible). Each `.team-section` carries `.reveal--toggle`, so the section and its staggered `.person-card` tiles fade back out and back in as you scroll past in either direction. See `about/README.md`'s "Scroll-triggered reveal" entry for the full mechanics — it's unchanged here, just repointed at `.person-card` instead of `.school-block`/`.center-block`/`.partner-card`.

## Keeping in sync

If you change a shared token or component pattern here, check whether `about` (and by extension `home`/`grants`) should get the same change, and vice versa — these repos duplicate CSS rather than sharing a stylesheet, so consistency is a discipline, not something enforced automatically.
