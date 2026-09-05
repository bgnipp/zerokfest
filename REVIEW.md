# zer0K site review

**Page:** `https://2kfest.com/zerok/` (`zerok/index.html`)
**Reviewed:** 2026-09-05
**Status:** concept site, venue TBA, dates locked (June 9–14, 2027)
**Do not implement from this doc yet** — findings only, grouped by priority.

The page already has a strong spine: elevation trilogy, invite-only voice, concept-labeled gallery, countdown, and a clear “not an RSVP” ask. Most of what follows is about making it feel less like a 2k reskin, less like a wall of identical cards, and less confusing on the one action that matters (raising a hand).

---

## What’s working

- Concept chip on the hero (`Concept · venue in progress · dates locked`) sets expectations immediately.
- Elevation stats (9,000 / 2,000 / 0) make the Federation joke land in one glance.
- Voice is consistent with 2k: “subject to tides,” “should not be trusted,” “be boring, legally.”
- Base Camp cards (photo + TBA + short copy) are the strongest section visually.
- Gallery backdrop-follows-photo matches the 2k pattern and looks good.
- `noindex` is the right call while the venue is TBA.
- Riley is correctly kept off the page.

---

## P0 — fix first (bugs / confusing UX)

### 1. Password gate fights the form that is already on the page

“Raise Your Hand” (hero + footer) is a `.rsvp-gate-link`. Clicking it `preventDefault`s and prompts for the attendee password, then smooth-scrolls to `#interest`.

The form itself is **already in the DOM, ungated**. Anyone can scroll to it. Nav “Hand Up” skips the gate entirely. The helper text says “Password-gated like 2k RSVP,” which is not what the form does.

Three conflicting behaviors on one page:

| Control | What happens |
|---|---|
| Hero / footer “Raise Your Hand” | Password prompt, then scroll |
| Nav “Hand Up” | Instant scroll, no password |
| Scrolling the page | Form is fully visible and submittable |

**Recommendation:** pick one model.

- **A (honest):** drop the gate. This is an interest list, not an RSVP. Keep `noindex`.
- **B (actually gated):** hide `#interest` until the password is entered once (`sessionStorage`), then show it. Apply that to *every* entry point.
- **C (2k-style):** move the form to `/zerok/interest/` and gate the URL, same as `rsvp/`.

Mailto + a `window.prompt` is also a rough mobile experience.

### 2. Interest form is mailto, not the Apps Script path in the plan

`PLAN.md` §12 called for the `rsvp/` Google Sheet pattern. What’s live opens `mailto:bgnipp@gmail.com`. That fails if the visitor has no mail client (most phones/desktop-web users), and long bodies get truncated by some clients.

Until a real backend exists, at least say “opens your email app” more loudly, and consider a fallback (`copy the answers` / `mailto` link they can tap again).

### 3. Google Map embed is a stub

The iframe `pb=` string uses a dummy place id (`0x7c06f0c0c0c0c0c1:0x0`) and rounded coords. It may load a generic/wrong map or an error tile. “Map is the bay, not the address” is good copy — the embed should actually be Hanalei Bay.

### 4. Lightbox is incomplete

- Prev/next are `<a>` with no `href` (not keyboard-activatable).
- No Escape / arrow-key handling.
- No focus trap; background stays tabbable.
- Clicking the main photo opens the modal; there is no hint that it is clickable.

### 5. JSON-LD says the event is scheduled at Hanalei Bay

```json
"eventStatus": "EventScheduled",
"location": { "name": "Hanalei Bay" }
```

Venue is TBA. Search/social crawlers that ignore `noindex` will treat this as a confirmed public festival at the bay. Prefer `EventScheduled` only after a venue letter, or use a vaguer `Place` (“North Shore Kauaʻi, venue TBA”) and drop precise bay geo.

---

## P1 — design & color

### Palette: planned twist is barely used

`:root` defines `--sunset` (`#ff7a59`) and `--sand` (`#e8d9b5`). Neither is applied anywhere. The page is 2k gold on navy, with teal only on the concept chip, flow-day headings, and TBA labels.

The poster already has the better palette: ink navy, honu green, teal lasers, coral/magenta, gold metal type. The UI does not pick that up.

**Concrete moves (later):**

- Use teal as a real co-primary: active tabs, outline buttons, section rules, “concept” chips.
- Use sunset sparingly on one CTA or the countdown numerals so the page doesn’t read as another gold fest.
- Use sand for body text instead of cool off-white — warmer, more North Shore.
- Keep gold as the Federation thread (logo, sister-fest, h2 shine), not every heading and button.

### Typography: Rye is Murphys, not Hanalei

Rye on “zer0K” is the 2k cowboy stamp. It works as a Federation family resemblance and fights the poster (ornate metal serif) and the wordmark (`assets/zerok_wordmark.jpg`, unused). The wordmark’s horizon-`0` is the better brand mark and is sitting unused in `/assets`.

Favicon is still `../photos/hattransp.png` (2k hat).

### Hero is overcrowded and fights its own poster

Stack on a busy illustrated background:

1. Concept chip
2. Giant Rye “zer0K”
3. Date line (poster already has the date)
4. Two-sentence manifesto
5. Four countdown cells
6. Two stacked buttons
7. Scroll chevron
8. Poster’s own “zer0K / HANALEI / JUNE 9–14” type at the bottom of the art

That’s the name three times in one viewport (nav, h1, poster). Overlay (`radial-gradient` ~55% center) is not dark enough over the laser crossing — body copy will flicker as the art shows through.

**Later:** let the poster be the hero. One short line + one button over a stronger scrim, or split: full-bleed poster with no text, then a tight intro block in the next section.

### Mid-page visual drop-off

After Base Camp, six sections in a row are the same thing: gold gradient `h2` + subtitle + 2-column `.plain-card` grid on a darkened photo.

| Section | Pattern |
|---|---|
| Ocean-lympics | 6 plain cards |
| Sound | 4 plain cards |
| Food × Music | 4 plain cards |
| Kuleana | 2 list cards |
| Stay | 2 plain cards |
| FAQ | 4 plain cards |

Base Camp proves photo-cards work. Ocean-lympics and Food especially want images (`oceanlympics_snorkel`, `night_glow_swim`, `john_imu_night`) instead of text boxes.

`sea_level_line.jpg` (the one abstract texture) is reused for both Stay and FAQ — those two sections become twins.

Section overlays are all the same navy wash (`0.72 / 0.42 / 0.72`). Photos get crushed to the same mood. Vary opacity, or put copy on a frosted panel and let more of the photo show.

### Gold gradient headings

`h2` uses a metallic gradient with a white spike at 50%. Looks expensive on “SEA LEVEL”; gets noisy on longer titles (“Sound at Sea Level”, “Three Houses. Three Elevations.”). On some backgrounds the transparent-fill text also loses contrast.

---

## P1 — information architecture

### Nav is crowded and incomplete

Eight desktop items at `0.74rem`: Sea Level, Flow, Base Camp, Ocean-lympics, Sound, Kuleana, Gallery, Hand Up.

Missing from nav (and easy to miss): Food × Music, Getting There, Stay, FAQ, sister fests.

“Hand Up” is cute in isolation and unclear next to “Raise Your Hand” / “Raise your hand →”.

**Later:** 5–6 items max. Example: About · Flow · Site · Gallery · Interest. Park Food / Ocean / Sound / Kuleana as in-page jumps from About or Flow.

### Flow is a shape, but you can’t see the shape

Six day tabs, one paragraph at a time. You never see the week. A single horizontal timeline (Wed arrive → … → Mon pau) with the active day expanded would match “a shape, not a lineup.”

Food pairings are duplicated (Flow copy + Food section). Fine if Food becomes visual; redundant if both stay as text cards.

### Sister-fests section is thin

Three numbers + two links. 2k’s sister section has real sentences. Give each house a one-liner and a card image so this feels like a destination, not a footer.

---

## P1 — content

### Inside-baseball for a first-time reader

A Kauaʻi friend opening this cold hits:

- “Don’t ship the Murphys rig”
- “Glowlympics goes saltwater”
- “Class 4 outdoors”
- “Resolume”
- “diatonic workshop kit”
- “STR”

The Federation audience will get it. Anyone you’re hoping has a *Kauaʻi connection* may not. One “if you know 2k / if you don’t” sentence on Sea Level would help, and Sound can say “don’t fly the California PA” without the proper nouns.

### Hawaiian language — use less, spell-check more

Plan said: sparingly and correctly. Current page: kuleana, mālama ʻāina, pau hana, imu, ʻokina usage in Kauaʻi / Līhuʻe / Hāʻena. That’s the right set.

Still needs a pass from someone with Kauaʻi ties (`PLAN.md` phase 4). Watch:

- `pau hana` vs `pau` used as “we’re done being loud”
- `Ocean-lympics` (hyphen looks like a typesetting accident; `Oceanlympics` or `Ocean Olympics` will read cleaner)
- `zerok` in the URL vs `zer0K` in copy (fine) vs wordmark art that reads closer to `zeroK`

### FAQ is short vs the plan

Plan listed: cost, kids?, non-musicians, what to bring, noise, weather. Live FAQ drops kids and weather. For a 5-night island trip those two will be the first questions.

Also missing, and people will ask:

- Rough cost *to get there* (flight + car + food chip-in), even as a range
- Headcount target (30 vs 50–100 — still an open question in the plan)
- Alcohol (BYO is in Kuleana, not FAQ)
- Ocean safety / night swim (reef, currents, no night swim if surf is up)
- Hāʻena / Tunnels reservation reality (mentioned in Getting There, not in the event that uses Tunnels)

### “June is dry-season trades”

True-ish for Kauaʻi overall; the North Shore is the wet side. June is better than January, not reliably dry. Softer copy: “shoulder-to-dry, still bring a rain layer.”

### Consent is not on the page

Plan: get OK from people in the renders before publishing. Gallery subtitle says “Not real (yet)” — good — but does not say these are likenesses of real friends. Add one line, and keep any one image removable (already true in the JS arrays).

### Gear / legal copy is a little louder than the invite

Sound + Kuleana read like an internal briefing (Class 4, complaint-driven island, occupant-load 300). That’s useful for hosts. For the public page, one short Kuleana card and a quieter “we rent the PA on-island” is enough. Park the rest for a private attendee note later.

---

## P2 — imagery

| Issue | Detail |
|---|---|
| Unused wordmark | `assets/zerok_wordmark.jpg` never appears (nav, favicon, footer). |
| Unused as a featured still | Several strong renders only live in the carousel (`billroland_lanai`, `dash_beach_openmic`, `night_glow_swim`). |
| Card/photo mismatch | “Where we sleep” uses a night lanai jam. Sleep should look like bunks/tents/camp, or stay typographic until you have that render. |
| Hotlinked 9k tab | `https://9kfest.com/images/...` — will 404 if 9k moves files. Mirror into `zerok/assets/9k/` or `../` if you add a local copy. |
| Generic alt text | Thumbs are `Photo 1`…`Photo N`. Use short captions (“Pier crew, concept”, “Sax on the sand, concept”). |
| AI tells | Some renders will read as generated on a 2-second look (hands, extra people, treasure-chest / wreath extras). Either crop tighter or add “concept render” in the lightbox caption, not only the section subtitle. |
| 2k / 9k crew tabs | Fine for continuity; label them “real photos from the other houses” so they don’t look like Hawaii documentary. |

---

## P2 — accessibility & motion

- No `:focus-visible` styles. Gold-on-navy buttons disappear in keyboard nav.
- Hamburger has `aria-label` but no `aria-expanded` / `aria-controls`.
- `prefers-reduced-motion` is ignored (`scroll-behavior: smooth`, fade-ins, countdown).
- Checkboxes are `name="role"` with no `fieldset` / `legend` (only a `<p>`).
- Map iframe has no title.
- Modal close control is a `<span>`, not a button.
- Contrast: `--text-dim` (`#a09888`) on navy is close to the line for small nav labels.

---

## P2 — technical / meta

- Password is `atob('bm9kcm9wZA==')` in page source. Fine for a friendly latch, not a secret. Don’t imply it is one.
- Countdown JS is solid (HST offset, happening-now / pau states). Good.
- Fade-in at `threshold: 0.08` is fine; first screen of each section can still sit invisible for a beat on slow scroll.
- Instagram is `@twokfest` — correct for now; a zer0K tag later would help the concept feel real.
- Cross-links: 2k sister-fest and IJHF already mention zer0K. 9kfest.com still needs a card (called out in the plan as a later commit).
- `robots: noindex` + rich `og:` + JSON-LD is a slightly mixed signal. Fine while private; strip JSON-LD location precision first if you keep sharing the OG poster.

---

## Suggested order when you do implement

Not doing these now. When you want them, this sequence pays off fastest:

1. **Decide the interest-form model** (A/B/C above) and make every entry point match. Prefer Sheet-backed like `rsvp/` if this list is actually going to get used.
2. **Hero + brand:** stronger scrim or less type on the poster; use the wordmark; swap favicon; start using teal/sunset/sand.
3. **Fix the map embed.**
4. **Break the plain-card streak** — photo cards for Ocean-lympics + Food; retire the duplicate `sea_level_line` backdrop.
5. **Nav trim** + week-at-a-glance Flow.
6. **Copy pass:** less Murphys jargon, FAQ kids/weather/cost-to-get-there, Kuleana reviewed locally, consent line on the gallery.
7. **A11y + lightbox keyboard.**
8. **Mirror 9k images** and add a 9k → zer0K sister card.

---

## Open product questions (from the plan, still open)

These are content decisions, not CSS:

1. Year-one headcount: 30 pilot or 50–100?
2. Stay venue-agnostic, or wait for a letter of intent before pushing the page harder?
3. Keep `noindex` until venue lock?
4. Gate the interest list, or leave it open behind the private URL?
5. Likeness OK from Nathan, Bill, Roland, Dash, John, the sax pair, the 9k hikers?
6. Name in sentences: always `zer0K`, or also “0k Fest”?
