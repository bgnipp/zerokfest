# zer0K — Plan

**Implemented** at `zerok/index.html` (2026-09-05). Riley is excluded from all concept renders and crew tabs.

**What:** the sea-level entry in the Federation's elevation trilogy — 9k (Cuchara, CO) · 2k (Murphys, CA) · **0k (Hanalei, Kauaʻi)**.
**When:** **Wed June 9 – Mon June 14, 2027** (5 nights; mirrors 9k's 5-day format).
**Where:** Hanalei / North Shore Kauaʻi. Venue TBD — site launches venue-agnostic ("Base Camp: TBA").
**URL:** `https://zerokfest.com` → `bgnipp/zerokfest` (own GitHub Pages repo, same shape as 9k). `https://2kfest.com/zerok/` redirects there.
**Status:** future-dated, invite-only, interest-gathering. Not an RSVP yet.

---

## 1. Positioning & brand

- **Name:** `zer0K` (stylized; the `0` is the sea-level line). Alt-casing in copy: "zer0K", never "zerok".
- **One-liner:** *"Nine thousand feet. Two thousand feet. Zero. The Federation touches down."*
- **Sub:** *An interactive music retreat at sea level — Hanalei Bay, Kauaʻi. Everyone jams, everyone swims, everyone cooks.*
- **Voice:** 2k's deadpan ("subject to vibes… should not be trusted or believed") + 9k's manifesto warmth. Add Hawaiian words sparingly and correctly (ʻokina/kahakō): *kuleana* (responsibility), *mālama* (care for), *pau* (done), *ʻohana*.
- **Visual DNA (shared with siblings):** dark base, gold `#D4AF37`, fonts Rye / Bebas Neue / DM Sans, `.content-section` + `.section-bg` parallax, `.event-toggle` tabs, countdown, carousel.
- **zer0K palette twist:** ink navy `#06111f` instead of pure black, sea-glass teal `#2ec4b6` promoted from accent to co-primary, sunset coral `#ff7a59`, sand `#e8d9b5`. Gold stays as the Federation thread.
- **Poster lineage:** 2k = laser-eyed cats. 0k = **laser-eyed honu (green sea turtles)** over a beach stage at Hanalei Bay with Nāmolokama's waterfalls behind. Same psychedelic circular-frame composition so the three posters read as a set.

## 2. Venue strategy (Hanalei-specific)

The earlier analysis was Big Island–centric. For Hanalei the shortlist is narrower and the noise problem is sharper (dense small community, one road, one-lane bridges, strong local sensitivity to visitor events). Order of attack:

| # | Option | Why | Catch | First question to ask |
|---|--------|-----|-------|-----------------------|
| 1 | **Waipā Foundation** (Hanalei, ahupuaʻa on the bay; Keanolani Hale cap. 48, Hale Imu outdoor space, campsites + "outdoor spaces available for larger events") | Already hosts music events (ʻĀina Festival: 3 bands, 1000+ people at Halulu Fishpond). Mission-aligned: community, food, ʻāina. Commercial kitchen + imu on site. | Nonprofit cultural org — must be pitched as a *partnership / fundraiser contribution*, not a rental. Amplified late-night is a real ask. | "Would Waipā host a small invite-only music retreat as a partner, with a donation + volunteer workday built in? What's the latest amplified music has run on site?" |
| 2 | **YMCA Camp Naue** (Hāʻena, 12 beachfront acres, 50 bunks + tent camping, kitchen + 60-seat dining hall, pavilion) | Cheapest turnkey 25–100-person buyout on the North Shore. Groups of 25+ book the whole site. Tunnels reef is a walk away (Ocean-lympics venue). | Posted rules: quiet hours, **no alcohol**, no amplified music. Cash only, book many months out, summer is peak. | "Full-site buyout, June 2027 — can amplified acoustic-ish music run to 10pm? Is BYO alcohol a hard no?" |
| 3 | **Private ag/ranch land lease** (Wainiha / Hāʻena valleys, or Kīlauea–Anini side) via a local host | Closest analogue to Murphys: freedom, buffer distance. | Build everything (power, water, toilets, shade). Year-two move unless a host appears. | Post to 2k + 9k lists: *who has Kauaʻi connections?* |
| 4 | **County facilities** (Hanalei Pavilion / Black Pot Beach Park, Anini camping) | Cheap, iconic. | County permits, hard curfews, no amplified, camping caps. Day-use only (e.g., sunrise session, beach open mic unplugged). | Use as *satellite* venues, not base camp. |
| ✗ | Vacation rental "party" | — | Kauaʻi STR enforcement + no-events clauses. Off the table. | — |

**Recommended shape:** Base camp = Camp Naue or Waipā (sleep + eat + acoustic/day programming); loud night stage = **one** private-land or partner location with a clear end time; rest = unplugged and ocean-based. Pilot headcount **30–50**, not 100.

**Rules to hold (legally boring):** no ticket sales, no public advertising, BYO alcohol only, headcount well under 300, talk to neighbors first, hard end time on amplified sound.

## 3. Site architecture

Single file `zerok/index.html` + `zerok/assets/` (concept renders, poster, og image). Reuse CSS/JS patterns from `hilltop/index.html` by copy (siblings are intentionally self-contained).

| # | Section (id) | Content | Reuse from |
|---|---|---|---|
| 0 | **Nav** | zer0K wordmark + date badge "June '27"; links below; hamburger. | 2k nav |
| 1 | **Hero** (`#home`) | Poster bg, H1 "zer0K", date span "June 9 – 14, 2027 · Hanalei, Kauaʻi", countdown to `2027-06-09T12:00:00-10:00` (HST, no DST), buttons: *Raise Your Hand* → interest form, *What is zer0K* → about. | 2k hero + countdown |
| 2 | **Sea Level** (`#about`) | Elevation trilogy graphic: 9,000 ft → 2,000 ft → 0 ft. Three stat cards. The IJHF pitch in one paragraph. "Concept status" banner: *venue in progress, dates locked*. | 9k "What 9K Is", 2k sister-stat grid |
| 3 | **The Flow** (`#schedule`) | Day-by-day *shape* (not a lineup): Wed arrive/sunset jam · Thu Ocean-lympics I + Poke & Post-Rock · Fri sunrise hike, night stage · Sat Ocean-lympics II, Loco Moco & Lo-Fi, late night · Sun Musubi & Math Rock, talent show, pau hana · Mon breakdown + beach open mic. "Subject to tides." | 2k schedule tabs (labels only, no grid) |
| 4 | **Base Camp** (`#the-site`) | 4 cards: *Where We Sleep* (camp buyout / camping), *Where It Gets Loud* (one night stage, end time), *Where We Eat* (field kitchen, imu night), *Where We Swim* (Hanalei Bay, Tunnels). Each marked TBA until venue locks. | 2k site cards |
| 5 | **Ocean-lympics** (`#oceanlympics`) | Glowlympics rewrite, teams of 3: Snorkel Scavenger Hunt (Tunnels), SUP/Outrigger Relay (Hanalei Bay), Tide-Pool Treasure Hunt, Night Glow Swim (LED, no lasers), Sunrise Summit (Okolehao Trail), Sand Mini Golf, Coconut Bocce. Same bracket → Sunday playoff. | 2k glowlympics grid |
| 6 | **Sound at Sea Level** (`#music`) | Three-tier gear plan: rent backbone on-island (PA, kit, amps), BYO one instrument as checked bag, projector + Resolume instead of laser cube. Diatonic "can't-hit-a-wrong-note" kit. Salt-air care tips. | 2k backline cards |
| 7 | **Food × Music** (`#food`) | Pairings: Poke & Post-Rock · Loco Moco & Lo-Fi · Musubi & Math Rock · Imu & Ambient · Shave Ice & Shoegaze. Community-cooked, chip in on ingredients. | 9k BOYAs styling |
| 8 | **Kuleana** (`#kuleana`) | Respect section (new, important here): we're guests; mālama ʻāina; volunteer workday with host org; no single-use plastics; reef-safe sunscreen; quiet by X pm; support Hanalei businesses; Hawaiian place names spelled right. | — |
| 9 | **Getting There** (`#location`) | LIH airport, ~1 hr to Hanalei, one-lane bridges, rental car / shuttle pooling, Hāʻena State Park reservation note, June weather (dry season, trades). Map embed centered on Hanalei Bay (approximate). | 2k location |
| 10 | **Stay** (`#accommodations`) | Base-camp bunks/camping (primary), Hanalei/Princeville rentals for those who want (with "no parties at rentals" note), Airbnb link prefilled `2027-06-09`→`2027-06-14`. | 2k accommodations |
| 11 | **Concept Gallery** (`#photos`) | Carousel of generated concept renders, labeled **"Concept renders — the crew, transported. Not real (yet)."** Backdrop mirrors active image (same behavior as 2k). Tabs: *Concept* · *2k crew* · *9k crew* (real photos from siblings for continuity). | 2k photo carousel |
| 12 | **Raise Your Hand** (`#interest`) | Interest form (not RSVP): name, contact, likelihood (in / probably / curious), days you could make, **Kauaʻi connections** (host? land? gear? locals?), instruments you'd fly with, roles you'd take (sound, kitchen, ocean safety, Ocean-lympics). Backend: Apps Script → Google Sheet, same pattern as `rsvp/`. Gate with the attendee password like 2k. | `rsvp/` scaffolding, `.rsvp-gate-link` |
| 13 | **FAQ** (`#logistics`) | Cost (free + contribute), kids?, non-musicians?, what to bring (reef-safe sunscreen, rash guard, headlamp, instrument, reusable cup), noise policy, weather. | 2k logistics |
| 14 | **Federation** (`#sister-fests`) | 2k + 9k cards, IJHF link; "Three Houses. Three Elevations." | 2k sister-fest |
| 15 | **Footer** | © zer0K / IJHF, IG, attendee link. | 2k footer |

**Meta:** title `zer0K '27 · Hanalei, Kauaʻi · June 9–14`, og image = poster render, JSON-LD `Festival` with `eventStatus: EventScheduled`, `noindex` until venue is locked (optional — discuss).

**Cross-links to add later (separate small commits):** 2k sister-fest section + IJHF page ("Two Houses" → "Three") + 9k sister-fest card.

## 4. Concept image plan (GenerateImage calls)

**Approach:** `GenerateImage` with `reference_image_paths` pointing at real crew photos so the same people appear, relocated to Hanalei. Photorealistic editorial style, golden-hour / blue-hour, natural skin, no text baked in except the poster and wordmark. Every render is labeled "concept" on the site.

**Fidelity check first:** run renders **A, C, D** below as a test batch. If likenesses don't hold, fall back to *silhouette/back-lit/wide* compositions where identity is implied by outfit + instrument (orange hat + shirtless guitar, serape poncho, rose-print Strat, kimono) rather than faces.

**Consent flag:** these are real friends' faces. Get an OK from the people pictured before publishing (Nathan, Bill, Roland, Dash, John, the two sax players, the 9k hikers). **Do not use Riley in any concept render or crew tab** (`riley.jpg`, `rileydance.jpg` stay off the page). Plan the copy so removing any one image doesn't break the page.

### Reference pool
- `hilltop/spring26/saxes.jpg` — two sax players (woman in psychedelic kimono + alto; man in cap/pink shades + tenor)
- `hilltop/spring26/nathan.jpg` — Nathan, rose-print shirt, Strat, singing
- `hilltop/spring26/billroland.jpg` — Bill (orange hat, shirtless, guitar) + Roland (purple paisley, bass)
- `hilltop/spring26/dashponcho.jpg` — Dash in serape poncho on mic, smoke
- `hilltop/spring26/johngab.jpg` — John in red-cloud kimono, round shades, red cup
- `9k/images/feature_photo_1.jpg` — hiking crew with flute + guitar (selfie, OR hat, blue cap flutist, bearded guitarist)
- `9k/images/feature_photo_3_everybody_is_a_musician.jpg` — lanai acoustic jam with dog
- `hilltop/summer26photos/4.jpeg` — sunset stage + pole silhouette (composition ref)
- `hilltop/spring26/outdoor_stage_night.jpg` — night stage lighting (style ref)
- `hilltop/2kfest_poster.jpg` — poster lineage (style ref)

### Renders

| ID | File | AR | Refs | Prompt sketch | Used in |
|----|------|----|------|---------------|---------|
| A | `zerok_poster.png` | 3:4 | `2kfest_poster.jpg` | Psychedelic circular-frame festival poster in the same illustrated style as the reference: two green sea turtles (honu) with glowing laser eyes crossing beams over a small beach stage at Hanalei Bay; Nāmolokama mountain with multiple waterfalls behind; taro fields; stacked speakers; gold/teal/coral palette; ornate border. Text: "zer0K" large, "HANALEI · KAUAʻI" and "JUNE 9–14, 2027" small. | Hero bg, og image |
| B | `zerok_wordmark.png` | 1:1 | — | Minimal wordmark "zer0K" in a Rye-like western serif, gold on ink navy; the 0 rendered as a horizon line with a half-sun rising out of water; tiny "SEA LEVEL · IJHF" underneath. Transparent-feel dark bg. | Nav, favicon source |
| C | `crew_pier_walk.png` | 16:9 | `feature_photo_1.jpg` | The same group of hikers from the reference — same faces, same OR bucket hat, blue cap flutist mid-note, bearded guitarist strumming — now walking down the wooden Hanalei Pier at golden hour, bay and green cliffs behind, longboards under arms of people in the back, warm backlight, candid selfie framing. | About / hero alt |
| D | `saxes_on_sand.png` | 4:3 | `saxes.jpg` | Same two saxophonists, same outfits (psychedelic kimono, cap + pink mirrored shades), playing barefoot on wet sand at Hanalei Bay at sunset, small PA on a pallet stage behind, Makana ridge in haze, spray in the air. | Sound at Sea Level |
| E | `nathan_taro_stage.png` | 16:9 | `nathan.jpg` | Same singer-guitarist, rose-print shirt and Strat, singing into a mic on a tiny plywood stage at the edge of flooded taro (loʻi kalo) fields in Hanalei Valley, mist on the cliffs, blue hour, string lights. | Flow / Base Camp |
| G | `billroland_lanai.png` | 4:3 | `billroland.jpg` | Same two players (orange hat, shirtless, guitar; purple paisley shirt, bass) jamming on a plantation-house lanai, surfboards racked behind, ferns, trade-wind light, ukulele leaning on a chair. | Sound at Sea Level |
| H | `dash_beach_openmic.png` | 16:9 | `dashponcho.jpg` | Same man in the serape poncho on a mic, barefoot on a sand stage; the stage smoke from the reference becomes ocean mist; drum kit + amps on pallets; sunset over the bay; a few people in lawn chairs on the sand. | Flow (open mic) |
| I | `john_imu_night.png` | 4:3 | `johngab.jpg` | Same man in the red-cloud kimono and round shades, red cup in hand, MC'ing next to a glowing imu pit at night, tiki torches, tables of food, ocean dark behind. | Food × Music |
| J | `oceanlympics_snorkel.png` | 16:9 | `feature_photo_1.jpg` | The hiking crew in snorkel masks and fins at the surface over Tunnels reef, holding up a found glow-stick "flag", Makana ridge behind, midday sun, playful. | Ocean-lympics |
| K | `night_glow_swim.png` | 16:9 | `outdoor_stage_night.jpg`, `summer26photos/4.jpeg` | Wide night shot: swimmers with LED bracelets floating in a calm bay, a small stage on the sand lit teal/gold like the reference, Milky Way overhead, no lasers. | Ocean-lympics / hero alt |
| L | `sunrise_summit.png` | 9:16 | `feature_photo_1.jpg` | Same crew at dawn on a ridge trail (Okolehao) above Hanalei Bay and taro fields, guitar out, flute up, pink sky. | Flow (Fri sunrise) |
| M | `lanai_jam_dog.png` | 4:3 | `feature_photo_3_everybody_is_a_musician.jpg` | Same acoustic jam (guitarists, dog asleep on the floor) transplanted to an open-air Hawaiian lanai with bamboo screen, string lights, ukulele added, rain outside. | Kuleana / FAQ bg |
| N | `sea_level_line.png` | 16:9 | — | Abstract section texture: a single gold horizon line across ink navy, faint teal wave interference below, grain. | Section dividers / bg |

Order of generation: **A, C, D** (test) → review → **B, E, G–M** → **N**. No Riley render. Expect 2–3 variants each for the people shots; keep the best, keep files under ~300 KB after `sips` compress (same as other seasons).

## 5. Data to lock in code

- Countdown start `2027-06-09T12:00:00-10:00`, end `2027-06-14T23:59:59-10:00`.
- Days: Wed 9 · Thu 10 · Fri 11 · Sat 12 · Sun 13 · Mon 14.
- Airbnb prefill: `checkin=2027-06-09&checkout=2027-06-14`, location Hanalei/Princeville.
- Map: Hanalei Bay approx (22.21, -159.50), zoom out; "exact location sent to confirmed attendees".
- JSON-LD `location.addressLocality: "Hanalei"`, `addressRegion: "HI"`.

## 6. Build phases (when approved)

1. **Scaffold** `zerok/index.html` from 2k CSS/JS; palette swap; sections 0–15 with placeholder copy; countdown; nav. Deploy to `/zerok/`.
2. **Renders** A/C/D test → full set → compress → gallery + section backgrounds.
3. **Interest form** (`zerok/interest/` or inline) on the `rsvp/` Apps Script pattern; password gate.
4. **Copy pass** — Kuleana section reviewed by someone with Kauaʻi ties; Hawaiian diacritics checked.
5. **Cross-links** — 2k sister-fest, 9k sister-fest, IJHF "Three Houses".
6. **Venue outreach** (parallel, off-site): Waipā and Camp Naue emails this month; post "who knows Kauaʻi?" to 2k/9k lists.

## 7. Open questions

1. Headcount target for year one — 30 pilot or 50–100? (Drives venue shortlist and whether the night stage exists.)
2. Publish venue-agnostic now, or wait for a venue letter of intent?
3. `noindex` until locked?
4. Interest form: reuse the 2k attendee password, or open (still invite-link only)?
5. OK to render real friends' likenesses, or go silhouette-first?
6. Name lock: `zer0K` vs `0k Fest` in copy (URL stays `/zerok/`).
