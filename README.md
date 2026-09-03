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
## Embedding this page

WordPress renders the real site; this repo is the source. The launch plan is direct-to-disk deployment, which needs no iframe — but iframe embedding still works and is the documented fallback, so keep this snippet accurate if you rename the repo or change its Pages URL.

Paste into a **Custom HTML block** as one line. The site runs **Twenty Twenty-Five**, a block theme, and a Custom HTML block has no width control of its own — so wrap it in a **Group block set to Full width**. This is not optional for these pages: Twenty Twenty-Five's `theme.json` sets `contentSize: 645px` (`wideSize: 1340px`), so an unwrapped embed renders in a 645px column, and every full-bleed colour band in the design collapses with it:

```html
<iframe id="pm-team-leadership" src="https://pennmediated.github.io/team-leadership/" title="Leadership & Staff — Penn MEDIATED" loading="lazy" style="width:100%;height:6450px;border:0;display:block"></iframe><script>(function(){var f=document.getElementById('pm-team-leadership');window.addEventListener('message',function(e){if(e.source!==f.contentWindow)return;var d=e.data||{},h=d.frameHeight||(d.type==='partners-page-resize'?d.height:0);if(h)f.style.height=h+'px';});})();</script>
```

The `height` in the snippet is only the starting value. Every Penn MEDIATED page posts its real height to the parent as `{ frameHeight: <int> }` — on load, on resize, once webfonts settle, and on any `ResizeObserver` change, so reveal animations, expanding cards and `<details>` toggles all resize the frame. The listener in the snippet applies it. `grants-rfp` also emits an older `{ type: 'partners-page-resize', height }` message; the snippet accepts both.

The page checks `window.self === window.top` before posting, so opening it directly does nothing. If you add a new page repo, copy the script from the bottom of this `index.html` so it behaves the same way.


## Images and video

This applies to every image, GIF and video added to any Penn MEDIATED repo. It is written to be followed directly — by a person or by a Claude session — without further instruction.

### The one rule that is never optional

**Every `<img>` and `<video>` carries explicit `width` and `height` attributes, holding the file's real intrinsic pixel dimensions.**

```html
<img src="assets/example.webp" width="640" height="334" alt="…">
```

They do not set the display size — CSS does. They give the browser the aspect ratio *before* the file downloads, so it reserves a correctly shaped box instead of collapsing to nothing and shoving everything below it down the page as each file lands. That shift is measured by search engines (Cumulative Layout Shift) and is worse for a reader, who loses their place or clicks a link that just moved.

Every repo has a global `img, video { max-width: 100%; height: auto; display: block; }` reset, so the CSS keeps winning and the attributes only ever contribute the ratio. **Never guess the numbers** — read them off the file.

### Pick the format by what the file is

| Content | Format | Never use |
| --- | --- | --- |
| Photo, screenshot, artwork | **WebP**, quality 88 | PNG or JPEG at full camera resolution |
| Logo, wordmark, icon | **SVG** if you have it, else WebP | — |
| Anything that moves | **MP4** (H.264) + a WebP poster | **GIF, ever** |

GIF is the big one. It has no interframe compression, so a screen recording is roughly ten times the size it needs to be: `research-compendium.gif` was 11.3MB for 290 frames; the identical recording as H.264 is 1.2MB.

### Size it to the box it displays in, not to what you were sent

Find the CSS box the image renders into, then export at **2×** that width for retina. Anything beyond that is bytes the browser downloads and immediately throws away. (`gni-membership.png` was 7992px wide, rendering into a 319px box — a 470KB file doing a 33KB job.)

In this repo:

| Where | CSS box at 1440px | Export at |
| --- | --- | --- |
| Person portrait (`.person-card__photo`) | 208px circle, cropped | ~416px square |

Portraits are center-cropped into a circle and greyscaled until hover, so frame the face centrally; a photo whose subject sits off to one side loses its head to the crop.

If you are adding an image somewhere not listed, measure the box first (`getBoundingClientRect().width` in the browser, at a 1440px viewport) and double it.

### Commands

Stills — resize and convert in one pass:

```python
from PIL import Image
TARGET = 640                      # 2x the CSS box
im = Image.open('source.png')
w, h = im.size
if w > TARGET:
    im = im.resize((TARGET, round(h * TARGET / w)), Image.LANCZOS)
im.save('out.webp', quality=88, method=6)
print(im.size)                    # <- these are the width/height attributes
```

Animation — MP4 plus a poster frame:

```bash
ffmpeg -i source.gif -movflags +faststart -pix_fmt yuv420p \
       -vf "scale=1280:-2:flags=lanczos" -crf 24 out.mp4
ffmpeg -i source.gif -frames:v 1 -vf "scale=1280:-2:flags=lanczos" poster.png
python3 -c "from PIL import Image; Image.open('poster.png').convert('RGB').save('out-poster.webp', quality=80, method=6)"
ffprobe -v error -show_entries stream=width,height -of default=nw=1 out.mp4
```

`-crf 24` is a good default; raise it toward 30 for a smaller file, lower it toward 20 for a sharper one. `-pix_fmt yuv420p` is required for Safari and iOS.

### Markup for video

```html
<video src="assets/name.mp4" poster="assets/name-poster.webp" width="1280" height="622"
       autoplay muted loop playsinline preload="metadata" aria-label="…"></video>
```

Each attribute earns its place: `muted` is what permits autoplay at all, `playsinline` stops iOS opening it fullscreen, `poster` means the slot is never empty while the video loads, and `aria-label` replaces `alt` (a `<video>` has no `alt`).

CSS cannot stop autoplay, so **a page with video needs the reduced-motion script** at the end of `<body>`. If the page already has one, leave it alone; if you are adding the first video to a page, add it:

```html
<script>
  if (window.matchMedia('(prefers-reduced-motion: reduce)').matches) {
    document.querySelectorAll('video[autoplay]').forEach(function (v) {
      v.autoplay = false; v.pause(); v.currentTime = 0; v.removeAttribute('loop');
    });
  }
</script>
```

Also check the CSS: any rule that sizes or crops an image needs to name `video` too, or the video slot will not match the image slot it replaced (`.card__image img` becomes `.card__image img, .card__image video`).

### Before you call it done

- [ ] File is WebP, SVG or MP4 — no GIF, no full-resolution PNG or JPEG
- [ ] Its width is about 2× the CSS box it renders into
- [ ] `width`/`height` attributes match the file's real dimensions
- [ ] Real `alt` text (or `aria-label` on a video) that describes the image; empty `alt=""` only if it is purely decorative
- [ ] Lives in this repo's `assets/`, not hotlinked from another site
- [ ] Page opened in a browser at 1440px and ~400px — nothing overflows, nothing jumps on load
- [ ] Originals are not committed alongside the optimised file; git history is the backup

Do not commit an unoptimised original "just in case" — the previous commit already holds it, and a duplicate in the working tree also ships to the server.

## Hyperlinks

One taxonomy, five categories, shared by every page repo. Pick the category by what the link *is*, not by which repo you happen to be editing.

**1. In-text links** — embedded mid-sentence in flowing prose.

| ground | text | underline | hover |
| --- | --- | --- | --- |
| white / light | `--c-red-dark` | none | fade to `opacity: 0.7` |
| colour / gradient | `--c-white` | `border-bottom: 1px solid rgba(255, 255, 255, 0.5)` | fade to `opacity: 0.7` |

Both grounds use `font-weight: 500` and `transition: opacity 0.15s`, and both fade rather than change hue. On a white ground **colour is the affordance** — no underline; the underline is category 2's job. On a coloured ground the red is invisible, so the link goes white and takes the hairline rule instead. Where an underline is used it is a `border-bottom`, never `text-decoration`.

#### Why interactive red is `--c-red-dark`, not `--c-red`

`--c-red-dark` (`#df3611`) is the closing stop of `--c-gradient`, promoted to a token of its own and declared in all twelve repos.

`--c-red` (`#f03d1f`) measures roughly **3.9:1** against white — under the 4.5:1 WCAG AA threshold for body text, and the same 3.9:1 applies to white text sitting on a `--c-red` fill. `--c-red-dark` measures about **4.5:1** either way and clears it. The two are near-indistinguishable at text sizes, so this is a contrast fix, not a visual change.

**The rule: anything you click is `--c-red-dark`.** Links and buttons take it wherever they would otherwise be red-orange — as text colour, as a box fill, as a hover or active state, and on the markers inside them (disclosure chevrons and their labels). It applies in every category and every state.

**`--c-red` stays the brand accent for everything you don't click**: section headings, eyebrow and metadata labels, tag and pill backgrounds, accent bars and card borders, full-width colour bands, the `.card-arrow` hover gradient, and focus rings. These are either large text, non-text UI at the 3:1 threshold, or sit on a tinted rather than white ground.

The one deliberate hold-out is red link text on a **dark** ground (`home`'s `.footer__email`), where the darker red would *reduce* contrast rather than improve it. That link has a separate outstanding issue — on a dark ground the standard is white text with an opacity fade, not red at all.

**2. Independent links** — a standalone text link that isn't inside a sentence ("Learn More About the Center", "Download the Full Schedule"). Unlike category 1 these carry the underline and are set in the body colour, so they read as a control rather than as emphasis inside a sentence:

| ground | text | underline | hover |
| --- | --- | --- | --- |
| white / light | `--c-dark`, `font-weight: 600` | `border-bottom: 1px solid rgba(13, 13, 12, 0.35)` | text and underline both turn `--c-red-dark` (`transition: color 0.15s, border-color 0.15s`) |
| colour / gradient | `--c-white`, `font-weight: 600` | `border-bottom: 1px solid rgba(255, 255, 255, 0.5)` | fade to `opacity: 0.7` |

Plus a **thin arrow** `⟶` after the text. Use `⟶` (`&#10230;`), not the `↗` badge from category 4.

**3. Document buttons** — an independent link that opens a document (a PDF, a report). A filled button box, not text:

| ground | box | text |
| --- | --- | --- |
| white / light | `--c-red-dark` | `--c-white` |
| colour / gradient | `--c-white` | `--c-dark` |

Hover is **movement, not colour** — a lift or nudge. Do not darken or recolour the box.

**4. Links to another web page** — this site or an external one. The containing box carries the shared `.card-arrow`: a 26px dark circle with a white `↗`, in the box's top corner. On hover the arrow scales slightly and its background becomes a sliding purple-to-orange gradient (`@keyframes card-arrow-slide`), and the box itself animates. No separate text button — the whole box is the link.

**Exception:** a link to a research paper is category 2, not this — thin arrow, no badge.

**5. Hyperlinked headings** — a heading that is itself a link (a post title, a card title). Sits in the body colour and shifts to `--c-red-dark` on hover (or fades, on a coloured ground), with **no arrow and no underline**.

### Dropdowns and disclosures

A dropdown, `<details>` block or expand/collapse control uses one affordance sitewide: a **chevron SVG** (`M2 5l5 5 5-5`, 13×13, `--c-red-dark` stroke, `stroke-width: 1.8`) beside a `--c-red-dark` label at `--fs-small`, rotating `180deg` on open with `transition: transform 0.25s`. See `llm-civic-discourse`'s "Full summary & details" toggle for the reference implementation.

Never leave the marker to the browser — style `<select>` with `appearance: none` and supply the chevron, and hide the native `<summary>` marker. The `↗` circle badge is category 4's language and does not belong on a disclosure control.

- **`overflow-anchor: none`** (on `html` and `body`): fixes a browser "scroll anchoring" bug where the page silently scrolled itself down past the hero title right after load. The hero title sits at the very top with only 20px of padding above it, and when the web fonts swap in a beat after first paint, its height shifts slightly; the browser's default scroll-anchoring then nudged the viewport to compensate, cropping the title. This turns that off. If this page's structure ever changes to be embedded inside another document (an iframe, or injected into a different site's DOM) rather than served as its own standalone page, revisit whether this still belongs on `html`/`body` or needs to move somewhere scoped to this page's own markup.

## Keeping in sync

If you change a shared token or component pattern here, check whether `about` (and by extension `home`/`grants`) should get the same change, and vice versa — these repos duplicate CSS rather than sharing a stylesheet, so consistency is a discipline, not something enforced automatically.
