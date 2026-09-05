# zer0K visual review — desktop, mobile, landscape

**Page:** https://zerokfest.com/
**Reviewed:** 2026-09-05
**Viewports measured:** 1440×900 desktop, 390×844 phone portrait, 844×390 phone landscape
**Status:** implemented on zerokfest.com (2026-09-05)

The first review (bugs, gate, palette, copy) is largely in. This pass is about **how photos get cropped**, plus the design issues that show up when the same `background-size: cover; background-position: center` rule hits every container.

---

## What’s working

- Honu heads in the **desktop** hero: `center top` on the portrait poster is the right call. We see the top ~39% of the art (circle + turtles), not the baked-in type.
- Mobile **portrait** hero is the one place the poster almost fully fits (only ~12% cropped off each side). Content fits in the viewport with ~100px to spare.
- Gallery **main** image and lightbox use `object-fit: contain` — no head cuts there.
- 3:2 thumbs (110×72) nearly match the concept JPEGs, so the strip is fine.
- Cards on phone **portrait** (340×258) are close enough to 3:2 that they crop sides, not scalps.
- Riley is still off the page.

---

## Root cause

Almost every concept JPEG is **3:2 landscape** (1600×1066). Two files are **2:3 portrait** (`zerok_poster.jpg`, `sunrise_summit.jpg`). Every decorative surface uses the same CSS:

```css
background-size: cover;
background-position: center; /* hero is center top */
```

`cover` always fills the box and **throws away the overflow**. There is no per-photo anchor, no aspect-ratio lock, and no landscape-specific layout. Truncation is fine when it takes sky, water, or porch ceiling. It is not fine when it takes faces, hats, instruments, or the second person in a duo.

Measured crop of a 3:2 photo at `center` / `cover`:

| Surface (actual box) | Visible slice of the photo | What gets thrown away |
|---|---|---|
| Desktop card 564×258 | Y 16–84% | top 16% + bottom 16% |
| Stay section 1440×463 | Y 26–74% | top 26% — hats and hair |
| Kuleana 1440×521 | Y 23–77% | same |
| Flow 1440×546 | Y 22–79% | Dash’s headroom |
| Music 1440×619 | Y 18–82% | sax hats |
| About 1440×634 | Y 17–83% | pier walkers’ hats |
| Phone portrait **section** 390×970–1350 | X 37–63% (sometimes 40–60%) | everyone on the left and right |
| Phone landscape **card** 794×258 | Y 25–75% | a quarter of the frame from the top |
| Sister card (9k portrait) 371×218 | Y 28–72% | selfie faces at the bottom |

So: **desktop and landscape cut the top. Phone portrait cuts the sides.** Same photos, opposite injuries.

---

## Photo-by-photo: what is “key” and what to do

Anchor = `background-position` / `object-position` to use **after** the layout fixes below. `%` is CSS position (0 = top/left). Truncation that remains should be sky, water, sand, or porch ceiling.

| File | Key stuff | Tight edge | Use as full-bleed section BG? | Anchor | Notes |
|---|---|---|---|---|---|
| `zerok_poster.jpg` (2:3) | circle, both honu heads, lasers, stage | type lives in the bottom third | hero: yes, `center top`. interest: only if we pin high | hero `50% 0%` (keep). interest `50% 18%` | Desktop hero already good. Phone portrait: CTA sits on the poster’s own “HANALEI / JUNE 9–14”. Landscape: hero grows to ~668px and the stack covers the art. |
| `crew_pier_walk.jpg` | three walkers + guitar + flute + shaka; mountains | heads ~20–40% down; selfie brim at the bottom; people spread full width | desktop: yes if pinned. mobile: no — 37% side crop drops flute and shaka | `50% 28%` | About + sister + gallery backdrop. Worst mobile group-shot. |
| `dash_beach_openmic.jpg` | Dash’s face, poncho, mic; drums / amps are secondary | head in the upper third | desktop Flow is short — currently crops ~22% off the top | `50% 32%` | Prefer this on a taller section or a card, not a 546px letterbox. |
| `saxes_on_sand.jpg` | both players’ heads and horns | **man’s hat is at the top edge** | no — any `center` cover scalps him | `50% 8%` | Used as Music BG, “gets loud” card, 0k sister card. Highest-priority scalp. |
| `nathan_taro_stage.jpg` | face, mic, guitar, taro, lights | real headroom above him | yes if we don’t also park a card on his face | `50% 38%` | Base Camp BG. Sleep card sits on his head. Mismatch: he is singing, card says “where we sleep.” |
| `john_imu_night.jpg` | John’s head, mic, imu pit | **hair at the top edge** | Food section is tall enough; cards are not | `50% 10%` | Food card + “where we eat” card both scalp him today. |
| `oceanlympics_snorkel.jpg` | cluster of faces + glow frame | faces at the **bottom** and both sides; flute on the far left | mobile section: no (side crop). desktop card: pin down | `50% 72%` | `center` throws away the selfie faces. |
| `night_glow_swim.jpg` | band on the left, audience heads in the water, sky is expendable | audience in the bottom third; band on the left | pin down-left; do not center | `22% 78%` | Oceanlympics BG + glow card. Center crop keeps empty sky and cuts swimmers. |
| `sunrise_summit.jpg` (2:3) | selfie faces at the bottom, guitar/flute mid-frame, ridge | already a bottom-edge selfie | **desktop Getting There is the worst crop on the site**: only Y 36–64% — mid-torsos, no faces, no sky | `50% 72%` | Cards also cut Y 25–75%. This file should not be a wide short banner. |
| `billroland_lanai.jpg` | orange hat + Roland’s hair, both guitars | **hats at the top edge** | Stay is 463px tall — crops 26% off the top. Faces gone. | `50% 6%` | Also the Loco Moco card. |
| `lanai_jam_dog.jpg` | two faces, guitars, dog | heads in the top ~15% | Kuleana 521px crops 23% — faces gone | `50% 18%` | Dog is expendable; faces are not. |
| `ninek_pier_crew.jpg` | four faces across the width | center guy’s head at the top; sides are people | gallery only — thumbs OK; backdrop will drop the ends | `50% 12%` | Do not promote to a tall mobile section BG. |
| `ninek_shaka_bay.jpg` / `ninek_beach_family.jpg` / `ninek_guitar_duo_sand.jpg` / `ninek_glow_beach.jpg` | faces / duo / glow crowd | assume heads high, groups wide | gallery only for now | `50% 20%` until a closer pass | Same rules as the first-pass group shots. |
| `9k/feature_1.jpg` + `headline.jpg` (2:3) | hiking crew, selfie faces at the **bottom** | bottom-weighted | sister card crops Y 28–72% — loses the selfie faces | `50% 78%` | Portrait photo in a wide short card. |
| `sea_level_line.jpg` | texture, no faces | n/a | yes — this is what short sections should use | `50% 50%` | FAQ already uses it. Give Stay / Kuleana / Flow this treatment if we can’t give them height. |

---

## Surface-by-surface

### 1. Hero

**Desktop (1440×836 art area)** — poster `cover` + `center top` is correct. Remaining issues are design, not crop:

- Circular logo stacked on the poster’s own circle. Two marks, same job.
- Chip + date + manifesto + four countdown cells + CTA still sit on the lasers / mountains. The poster already says zer0K / Hanalei / June 9–14; we repeat it in live type **and** the art still peeks those words under the button.
- Overlay is lighter in the middle (`0.28` at center). Body copy flickers on the waterfalls.

**Phone portrait (390×780)** — best poster crop on the site. Problems: CTA overlaps the poster’s baked-in date line; chip contrast on busy art; ~12% of each turtle is gone (acceptable).

**Phone landscape** — this is the broken orientation.

- Hero `min-height` is `100vh - 64px` (326px) but **content is 576px**, so the hero grows to **668px** — almost two landscape screens of logo + type before you can leave.
- Logo stays `min(220px, 52vw)` = **220px** in a 390px-tall window.
- No `max-height`, no `orientation: landscape` / `max-height: 500px` rules at all.
- Hamburger hit area is 24×26 (below the 44px thumb target).

**Plan:**

1. Keep desktop `center top`.
2. Add a short-viewport rule (`max-height: 520px` or `(orientation: landscape) and (max-height: 500px)`): logo ~88–110px, drop or single-line the manifesto, shrink countdown, **cap hero at `100dvh - nav`** so it cannot grow past the screen.
3. Darken the hero scrim in a band behind the type only (a panel or a tighter radial), not a wash over the turtles.
4. On portrait, hero type should sit above the poster’s own wordmark, or we fade the bottom of the poster so the two date lines don’t collide.
5. Optional later: drop the big circular logo from the hero and let the poster + nav mark carry the brand (one circle, not three).

### 2. Section backgrounds

Short, wide desktop sections are where people lose their heads. Tall, narrow phone sections are where group shots lose their sides.

**Do not** keep using a tight-headroom portrait as a 2.5–3:1 banner. Either:

- **A (preferred for short sections):** swap Stay, Kuleana, Flow, Music, Getting There, Interest to a texture (`sea_level_line`) or a landscape with sky/water to sacrifice — **or**
- **B:** keep the photo but give the section a min-height (~720px) and a per-image anchor from the table, **or**
- **C:** stop using the photo as `cover` on the whole section. Put one contained, well-framed still in a panel and let the section be a color field.

Phone portrait Base Camp / Oceanlympics / Food / FAQ are **1300px+ tall**. A 3:2 photo shown `cover` on a 0.29 box keeps only the center 20% of the width. Pier walk, snorkel, saxes, ninek pier — those are stories about a *group*. They become a blurry middle sliver behind a navy wash.

**Plan:**

1. Apply the anchor table to every `.section-bg`.
2. Reassign short sections (Stay, Kuleana, Flow, Getting There) off the tight-headroom people photos.
3. On viewports narrower than ~700px, either (a) don’t use group shots as section BGs, or (b) lower the wash and pin `background-position` to the group’s center of mass so the sliver is at least the right sliver.
4. Getting There: `sunrise_summit` as a 1440×617 banner is the single worst crop — mid-bodies only. Use a landscape bay/mountain still, or the portrait in a column next to the map (`object-fit: cover; object-position: 50% 72%` in a 2:3 frame).

### 3. Photo cards

Desktop cards are 564×258 (~2.2:1) with a bottom gradient. That is a letterbox. `center` + 16% top crop hits every tight-headroom shot. Landscape-at-the-900px-breakpoint is worse: **one column, 794×258**.

Copy also sits on the remaining faces (`site-card::after` is a hard bottom-to-top black gradient).

**Plan:**

1. Lock cards to the photo’s ratio: `aspect-ratio: 3 / 2` (sunrise / 9k portraits: `2 / 3` or a shared `4 / 3` compromise). Drop the 260px `min-height`.
2. That alone fixes most desktop scalps (564×376 shows the whole 3:2 frame).
3. Per-card `background-position` from the table for the files that are still tighter than 3:2 (saxes, john, billroland).
4. Keep the gradient in the bottom ~40% so type sits on sand/water/torsos, not noses.
5. “Where we sleep” stays typographic until there is a bunks/tents still — but move it off Nathan’s face as a section BG.
6. Between 700–900px, either stay two-up or let `aspect-ratio` make the single-column card tall (~530px). Never 258px tall at 800px wide.

### 4. Gallery

- Main photo: keep `contain`. Good.
- Backdrop (`#photos > .section-bg`, opacity 0.5): same cover problem as sections. When the featured still is a group or a portrait, the backdrop is a bad crop of that same photo. Either reuse the per-image anchor, or stop mirroring the photo into the section BG and use a navy / poster texture.
- Thumbs: add `object-position` from the same map. `sunrise_summit` and `9k/feature_1` thumbs currently show a mid-slice.
- Lightbox: fine on desktop; on landscape `max-height: 80vh` is ~312px — still OK because of `contain`.
- 2k tab still **hotlinks** `https://2kfest.com/spring26/...`. Mirror those files into `assets/2k/` (originals already live in hilltop and the zerokfest `_archive/seeds/spring26/` set is incomplete vs this list).

### 5. Sister cards

9k card uses a portrait (`feature_1`, 0.75) in a 1.7 box at `center` — crops the bottom selfie faces. Pin `50% 78%` and/or use a 3:2 9k still (`feature_2` / `4` / `5`). 2k card is a hotlink. 0k card is saxes again (third use) and will scalp the hat until cards get a real ratio.

---

## Other design issues (not crop, still fix)

### Desktop

- Nav is clean at five items. Desktop links sit on the right; they are easy to miss only if you never look past the logo.
- Mid-page still repeats: gold `h2` + subtitle + grid on a crushed photo. Photo cards helped Oceanlympics / Food; Sound, Stay, FAQ, Kuleana are still the same card wall.
- Same three photos do too much work: `crew_pier_walk` (About, Gallery, Sister), `saxes_on_sand` (Music, loud card, 0k card), `john_imu_night` (Food BG + two cards). Recycle less; we already archived unused stills.
- Interest section uses the poster at `center`, so it shows the **middle** 27% of a tall poster (stage, no turtles, no type). Pin high or use a texture.
- Fade-in still starts at `opacity: 0`. First paint of each section can sit blank for a beat.

### Mobile portrait

- Flow is six chunky tabs. On 390 they wrap to two+ rows and the week still isn’t visible as a shape. A single horizontal scroller, or a compact Wed→Mon strip, would match the copy.
- Hamburger works (drawer is 390×334, `aria-expanded` is wired). Drawer has no `z-index` of its own (relies on `#navbar`). Give the `ul` a z-index so it cannot slip under a later stacking context. Close on backdrop tap is missing.
- Thumb-target: hamburger 24×26, flow tabs OK, gallery arrows OK.
- No `env(safe-area-inset-*)` — notch / home indicator will kiss the CTA and the footer.
- Map is 380px tall: most of a phone screen. Cap at ~220px on small viewports.
- Password field + mailto interest path is still a rough mobile submit (known from review 1; still true).

### Mobile landscape

- This orientation was not designed. Hero is two screens of UI. Cards become letterboxes. Nav stays the phone drawer (hamburger) at 844px because the breakpoint is `900px`.
- Consider treating `max-height: 500px` as a first-class layout: compact hero, two-column cards if width > 700, map ~180px.

### A11y / polish leftovers that still matter here

- Reduced-motion is handled for fade and countdown; gallery thumb `scrollTo` still smooth-scrolls unless we pass `auto` (code already branches — good).
- Focus-visible exists. Modal still doesn’t trap focus.
- 2k / 9k gallery alts are better than “Photo N”. Keep that when adding `object-position`.

---

## Implementation plan (when we do it)

Do not start from random `background-position` tweaks on one card. Fix the **system**, then the leftovers.

### P0 — stop cutting key stuff

1. **Card aspect ratio.** `aspect-ratio: 3 / 2` on `.site-card` (special-case portraits). This is the highest-leverage fix on desktop and landscape.
2. **Per-image anchors.** One map (data attribute or class, e.g. `data-pos="50% 8%"`) shared by `.section-bg`, `.site-card-bg`, gallery thumbs, and gallery backdrop. Use the table above.
3. **Rehome the worst section BGs.** Stay, Kuleana, Flow, Getting There, Interest should not be tight-headroom people photos at their current heights. Texture, or a contained still, or more height + a pin.
4. **Hero landscape / short viewport.** Cap height, shrink the logo, keep the turtles. Darken type, not the honu.

### P1 — mobile group shots and hierarchy

5. **Phone-wide section BGs.** Don’t put pier / snorkel / saxes / ninek-pier behind 1300px-tall columns. Pin if we must keep them; better to use a landscape with a real center of mass or no people.
6. **Hero portrait collision.** Separate live type from the poster wordmark (scrim at the bottom, or less hero chrome).
7. **One circle.** Decide: poster is the hero mark, or the synthwave logo is. Not both at 220px on top of the circle.
8. **Flow on a phone.** Horizontal week strip, not a wrapped button pile.

### P2 — cleanup

9. Mirror 2k gallery + sister-card files into `assets/2k/`.
10. Safe-area padding; 44px hamburger; map max-height on small screens.
11. Recycle fewer hero stills; pull unused archives (`ninek_*` we already like) into the cards that currently reuse saxes/john.
12. Gallery backdrop: texture, or the same anchor map — not a second un-anchored `cover`.
13. Fade-in: set a fallback `opacity: 1` after load so a missed observer never leaves a blank section.

### Out of scope for this pass (still true, not visual)

- Mailto vs Sheet-backed interest list.
- Headcount / venue letter / `noindex`.
- Likeness sign-off.

---

## Suggested build order

1. Card `aspect-ratio` + stop `min-height: 260px`.
2. Anchor map on every photo surface (including thumbs).
3. Reassign Stay / Kuleana / Flow / Getting There / Interest backgrounds.
4. Short-viewport hero.
5. Mobile section-BG policy + Flow strip.
6. Hotlink mirrors, safe-area, hamburger, map height, unused-still swap.

Verify after implementation at **1440×900, 390×844, 844×390, and ~768×1024**. For each people-photo, the test is: *can I see every face the photo is about?* Sky, water, and feet may go. Hats, eyes, and the second sax may not.
