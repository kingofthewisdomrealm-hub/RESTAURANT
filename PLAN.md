# RESTAURANT Build-Out — PLAN

Built 7 Sep 2026. Third game in the shell that started with **ROOF Shingle** and
continued with **HOUSE Block**. Josias's ask: *"like the house, make a restaurant
visual model."* Chosen scope: a **commercial tenant build-out** — a leased retail bay
turned into a licensed full-service restaurant.

> Vocabulary note, standing since HOUSE: what he calls a *model* is a **simulation** —
> a model that reacts. The word for the drawing alone is a *schematic*.

---

## 1. Why this one is different from HOUSE

HOUSE has one authority: the county building department, walking a published
residential priority list. RESTAURANT has **four**, and they do not share a calendar:

| Authority | Stops you on |
|---|---|
| Zoning + the utility | Change of occupancy (M → A-2), parking, sewer capacity/ERC charge |
| DBPR, Division of Hotels & Restaurants | Plan review, then the opening inspection and the licence |
| Fire marshal | Hood suppression permit + discharge test, then the fire final |
| Building department | Every trade inspection, then the CO |

That is the whole teaching point of the game, and it is why gate 1 is paper and
gate 10 is the state rather than the county.

---

## 2. The engine

Unchanged from HOUSE. It is data-driven and knows nothing about restaurants:

- `FAMS` — 10 families, hue lives here
- `ROWS` — 10 gates, in order
- `PARTS` — 45 tiles; order lives in `needs`, never in code
- `altOf` pairs a real tile with its trap; `failsIf` on a gate names the traps it burns
- `layer` on a tile switches an SVG group on; `lit` on a gate is what glows in Inspector view

If you ever write `if(part.id === 'hood')` the engine is dead. Add a tile, not a branch.

Ring layout carries both HOUSE upgrades: **arc-length spacing** (`spaceRing()`) and the
**wrapping paper shelf** (two-pass `layout()`). Verified no-scroll at 1440×900,
1024×700 and 390×780 with 45 tiles, zero overlaps.

---

## 3. Counts

- **45 tiles** — 32 on the ring, 13 on the paper shelf (3 pre-permit + 10 gates)
- **10 gates**, **3 traps**, **two levels** (3D and flat) that share every rule
- **Two ways to look at the cards**: the deck (default) and the ring (`See all 45`)
- Verified headless: clean playthrough passes 10/10 with 0 leaks and 0 fails; each trap
  burns on exactly its intended gate (sc→5, nm→7, hp→10)

---

## 4. The catalog

### Gate 1 — Plan review
*He sees:* Three desks said yes before one wall moved: zoning, the state, and the building department.

| z | sym | tile | family | needs | grade |
|---|-----|------|--------|-------|-------|
| 1 | Zo | Change of use, parking and capacity fees | pp | — | 2 |
| 2 | Dp | DBPR plan review package | pp | zo | 1 |
| 3 | Bp | Building permit and the sub-permits | pp | dp | 1 |
| 4 | G1 | **GATE — Plan review approved** | pp | bp | 1 |

### Gate 2 — Underground
*He sees:* Two waste systems under one slab — grease on one, sanitary on the other — open, sloped and holding a test.

| z | sym | tile | family | needs | grade |
|---|-----|------|--------|-------|-------|
| 5 | Dm | Demo and saw-cut the slab | sh | g1 | 2 |
| 6 | Gw | Under-slab waste — grease side and sanitary side | wa | dm | 1 |
| 7 | Fd | Floor sinks and floor drains | wa | gw | 1 |
| 8 | G2 | **GATE — Underground plumbing inspection** | pp | fd | 1 |

### Gate 3 — Interceptor
*He sees:* A grease interceptor outside, sized, with a lid a pump truck can reach — and a backflow preventer on the water.

| z | sym | tile | family | needs | grade |
|---|-----|------|--------|-------|-------|
| 9 | Gi | Grease interceptor, outside and sized | wa | g2 | 2 |
| 10 | Bf | Water service, meter and backflow preventer | wa | g2 | 1 |
| 11 | G3 | **GATE — Interceptor and backflow inspection** | pp | gi, bf | 4 |

### Gate 4 — Rough-in
*He sees:* Open walls: pipe, wire, gas, and wood behind every place a heavy thing will hang.

| z | sym | tile | family | needs | grade |
|---|-----|------|--------|-------|-------|
| 12 | Wl | Walls, soffits and the walk-in enclosure | sh | g3 | 1 |
| 13 | Bl | Blocking — hood hangers, shelves, grab bars | sh | wl | 1 |
| 14 | El | Panel, feeders and branch circuits | pw | wl | 2 |
| 15 | Rp | Rough plumbing — every sink and the water heater | wa | wl | 1 |
| 16 | Gl | Gas line and pressure test | gs | wl | 1 |
| 17 | G4 | **GATE — Rough-in inspection** | pp | bl, el, rp, gl | 1 |

### Gate 5 — Hood & duct
*He sees:* A Type I hood and a grease duct welded liquid-tight, before anything covers it.

| z | sym | tile | family | needs | grade |
|---|-----|------|--------|-------|-------|
| 18 | Hb | Type I hood set | hd | g4 | 1 |
| 19 | Gd | Grease duct, continuous liquid-tight weld | hd | hb | 1 |
| 19 | Gd | Screw and mastic the duct seams 🪤 | hd | hb | 5 |
| 20 | Dw | Duct enclosure and clearance to combustibles | hd | gd|sc | 1 |
| 21 | Ef | Exhaust fan, curb, and where it blows | hd | dw | 1 |
| 22 | G5 | **GATE — Hood and duct inspection** | pp | ef | 1 |

### Gate 6 — Fire suppression
*He sees:* A UL 300 system that drops the gas, the power and the fans when it fires — proven on a test.

| z | sym | tile | family | needs | grade |
|---|-----|------|--------|-------|-------|
| 23 | An | Wet-chemical system, UL 300, a nozzle per appliance | fi | g5 | 1 |
| 24 | Fl | Fusible links and the manual pull | fi | an | 1 |
| 25 | Il | The interlock — gas, power and fans | fi | fl | 1 |
| 26 | G6 | **GATE — Fire suppression discharge test** | pp | il | 1 |

### Gate 7 — Above ceiling
*He sees:* Make-up air, sprinkler heads and alarm devices — the last look before the grid goes in.

| z | sym | tile | family | needs | grade |
|---|-----|------|--------|-------|-------|
| 27 | Ma | Make-up air unit, interlocked | hd | g6 | 1 |
| 27 | Ma | Skip the make-up air — the fan is enough 🪤 | hd | g6 | 5 |
| 28 | Sp | Sprinkler heads and fire alarm devices | fi | ma|nm | 2 |
| 29 | G7 | **GATE — Above-ceiling inspection** | pp | sp | 1 |

### Gate 8 — Finish & equipment
*He sees:* Surfaces that can be cleaned, a restroom anyone can use, and every food drain broken by air.

| z | sym | tile | family | needs | grade |
|---|-----|------|--------|-------|-------|
| 30 | Qt | Quarry tile floor with a coved base | fn | g7 | 1 |
| 31 | Fr | Washable walls, ceiling and shielded lights | fn | qt | 1 |
| 32 | Wk | Walk-in cooler and freezer | cd | fr | 1 |
| 33 | Eq | Set the line and connect it | pw | fr | 1 |
| 34 | Ag | Air gap every food drain | wa | eq | 1 |
| 34 | Ag | Hard-pipe the sink drains 🪤 | wa | eq | 5 |
| 35 | Rr | The accessible restroom | ac | fr | 1 |
| 36 | Rt | The route, the tables and the counter | ac | qt | 1 |
| 37 | G8 | **GATE — Finish and equipment inspection** | pp | wk, ag|hp, rr, rt | 1 |

### Gate 9 — Fire marshal final
*He sees:* Doors that open under a push, lights that stay on, and a Class K on the wall.

| z | sym | tile | family | needs | grade |
|---|-----|------|--------|-------|-------|
| 38 | Ex | Exits, panic hardware, signs and lights | fi | g8 | 2 |
| 39 | Ke | Class K and the rest of the extinguishers | fi | ex | 1 |
| 40 | G9 | **GATE — Fire marshal final** | pp | ke | 1 |

### Gate 10 — Opening inspection
*He sees:* The state walks it once more, the licence prints, and the building department signs the CO.

| z | sym | tile | family | needs | grade |
|---|-----|------|--------|-------|-------|
| 41 | Hs | Stock it — hot water, soap, towels, thermometers | fn | g9 | 1 |
| 42 | G10 | **GATE — Opening inspection, licence, and the CO** | pp | hs | 1 |
---

## 5. The three traps, and why they are these three

| id | Tile | Burns at | Why a crew really does it |
|---|---|---|---|
| `sc` | Screw and mastic the duct seams | Gate 5 | It is how every other duct in the building is built, it holds air, and it passes a smoke test |
| `nm` | Skip the make-up air | Gate 7 | It is an $11k unit that does nothing visible in an empty building with the fan off |
| `hp` | Hard-pipe the sink drains | **Gate 10** | It drains faster, it never overflows, and it is one afternoon instead of three |

`hp` is deliberately the last gate. It clears every building-department inspection on
the dial and dies at the state's opening inspection — staff hired, menu printed, date
announced. That is the shape of the lesson: **the building department is not the
customer.**

Candidate 4th trap for V1.1: a Type II hood over a fryer, or nozzles left aimed at the
old fryer after the owner swaps in a wider one (the suppression system is listed to the
specific appliances).

---

## 6. Evidence and sources

Grades on every card. What is graded ✓ came from a primary source read this session:

- **DBPR plan review triggers** — verbatim from the state page: "Newly built · Converted
  from another use · Remodeled · Re-opened after being closed at least 18 months";
  submittal is the application, a scale floor plan with equipment labelled, and a sample
  menu; ~30-day review.
- **Grease duct welding** — verbatim: "All joints and seams shall be made with continuous
  liquid-tight weld or braze made of the external surface." ¼ in./ft slope to 75 ft.
- **Make-up air** — CFM approximately equal to exhaust; "shall be electronically
  interlocked with the exhaust system"; windows and doors do not count.
- **Suppression** — IFC 904.13 (UL 300 listed), 904.13.1 (manual pull 10–20 ft from the
  system, 42–48 in. AFF), 904.13.2 (automatic fuel + power shutoff, manual reset),
  904.13.5 (extinguisher within 30 ft travel; Class K for vegetable/animal fats),
  904.13.6 (service every six months and after activation). NFPA 96: fusible links
  semiannually.
- **Air gap** — verbatim from the Florida DOH plan review guide: "All drains from any
  equipment in which food (including ice), portable equipment, or utensils are placed
  must be indirectly wasted by means of an air gap… a minimum of twice the effective
  opening of the indirect waste pipe."
- **Finishes** — verbatim: "All construction finishes must be smooth, easily cleanable
  and nonabsorbent. All junctures between walls and floors shall be coved and sealed."
- **Sinks** — three-compartment sink with drainboards on each end; hot and cold under
  pressure to all handwash sinks; mop sink required (not for Tier II).
- **Accessibility** — 2010 ADA §§304 (60 in. turning space), 604 (WC centreline 16–18 in.,
  grab bars), 606 (lav rim ≤ 34 in., pipes protected), 226/902 (5% of dining surfaces,
  28–34 in.), 904 (counter ≤ 36 in.).

Graded ~ or ◈ on purpose — **these are the ones to verify on the first real job:**

1. **Gate 3 exists as its own gate** (◈). Interceptor and backflow may be rolled into the
   site plumbing inspection locally. Confirm the sequence with Indian River County
   Utilities and the plumbing inspector.
2. **Interceptor size** (~). 1,000–1,500 gal is the common range, not a rule. The size
   comes from the county's FOG ordinance and your fixture/seat count.
3. **Capacity / ERC charge on a change of use** (~). The dollar figure is the county's
   current schedule. Get it in writing before the lease is signed — this is the single
   biggest surprise on a restaurant conversion.
4. **Sprinkler threshold for A-2** (~). Fire area, occupant load and level of exit
   discharge each trigger it. The plan reviewer says which one you hit.
5. **Occupant load and panic hardware** (~). ~15 sq ft/person for tables and chairs;
   50+ occupants means assembly, two exits and panic hardware.
6. **Gate codes.** Indian River County publishes a *residential* inspection priority
   sheet (used in HOUSE). No commercial equivalent was found, so RESTAURANT's gates are
   named by discipline rather than by county code number. If the commercial permit card
   turns out to carry codes, put them in `ROWS[].code` — one line each.

---

## 7. Open decisions for Josias

1. **Full-service vs fast casual.** Drawn as full-service with a Type I hood, walk-in and
   a dish room. Fast casual drops a gate and adds the commissary question.
2. **Gas vs all-electric line.** Drawn as gas — it carries the interlock lesson. An
   induction line deletes the gas valve and half of gate 6.
3. **Existing sprinklers, yes or no.** Drawn as a sprinklered building with heads
   relocated. An unsprinklered A-2 that crosses the threshold is a different project.
4. **Whether gate 3 stays separate** (see 6.1).
5. **Should the CRM connect?** Same idea as THE PORCH: a finished playthrough could post
   a lead. Not built.
6. **Repo naming.** The two cross-links (`HOUSE_URL`, `ROOF_URL`) resolve on GitHub Pages
   only because the repos are named exactly `HOUSE` and `ROOF`. **Do not rename either
   repo.** If RESTAURANT ships as its own repo, name it `RESTAURANT`.

---

## 8. Shipping

Same recipe that put ROOF and HOUSE live: new public repo named `RESTAURANT`, push these
files at the root, Settings → Pages → main / root. The result is
`kingofthewisdomrealm-hub.github.io/RESTAURANT/`, and the sister links in the hub and in
two tile cards resolve to the ROOF and HOUSE repos automatically.


---

## 9. The 3D level (`apps/tenant3d/`)

three.js **r134 UMD from cdnjs** (`three.js/r134/three.min.js` — UMD, exposes a global
`THREE`; r150+ on cdnjs may be ESM). **Hand-rolled orbit**, no OrbitControls dependency.
One unit = one foot. ~590 meshes. Textures are drawn into canvases at load, with per-mesh
repeat matched to the real size of the thing they sit on. If `window.THREE` never arrives,
the page falls back to a link to the flat level.

**The bay is 44 ft wide × 40 ft deep**, street at +z, back of house at −z, roof deck at
16 ft, ceiling grid at 10 ft. The kitchen deliberately lives at **x < 0** so the cutaway
half is the half worth looking at.

- **Cutaway = a real clipping plane** at x = 1.5, normal (−1,0,0), enabled only in that
  view. It cuts *down the length* of the bay rather than across it, so one section shows
  the whole sequence: interceptor outside the back → under-slab waste → cook line and hood
  → ceiling → dining room → storefront.
- **`site` vs `bay`.** Materials flagged `userData.site` are never clipped. Only the
  asphalt, the sidewalk and the light pole are `site`. The **bay shell is its own group,
  `bay`** — always visible, never a tile, but clipped and dimmed like everything else.
  (First render put the shell in `site`; the cutaway then cut nothing at all.)
- **Inspector view** ghosts every layer the open gate does not check to 0.07 opacity —
  *including the bay shell*, or the shell hides the very thing that is lit. Lit layers get
  a warm `emissive`.
- **Dining room view hides the inspectors.** A customer does not see them, and that is the
  point of the view.
- `flyTo()` moves the **target** as well as the orbit, so the dining-room view can stand
  inside the room at table height (r ≈ 27) instead of orbiting the outside of a closed box.
- All layer groups are built **once** at load; `redraw()` only flips `.visible`.
  `OVERRIDE` handles the one state-dependent layer: the saw-cut trenches close when the
  new floor goes down.

### Geometry notes for the next level
1. Openings are real holes: `segs()` splits a wall into the rectangles an opening leaves,
   and `bayWalls()` + `PARTS_W` feed the same hole lists to every shell (block, FRP, coved
   base) at a different inset.
2. Wall and finish thicknesses are exaggerated (~2 in instead of 1/2 in) so the sandwich
   reads at building scale. Grade ◈ Model.
3. The `nomua` trap is drawn as **an empty curb** — the absence has to be visible in the
   cutaway or the trap teaches nothing.
4. The welded duct shows weld beads as torus rings; the trap duct shows screw heads and a
   taped seam band. Same box, two readings, and neither is visible from a table.

## 10. Reverse (both levels)

A **Reverse** button left of *Start over*, plus **Ctrl+Z / Cmd+Z**.
- Every action that **changes the building** pushes one state snapshot (`placed`, `passed`,
  `burned`, `failedRows`, `leaks`, `fails`) before it runs; Reverse pops and restores.
- **A leak is not a step** — a refused drop changes nothing, so it never enters the stack.
- Reversing a passed gate un-stamps it. Reversing a failed gate un-burns the trap, puts the
  tile back, and returns the fail count. Stamps are **rebuilt from state**, never patched.
- Messages stay honest: *"in the field a stamp is a stamp — and the state keeps its own
  file"* / *"on a real job that step is a demo crew, a change order, and a week of the
  lease."*


---

## 11. The deck (both levels)

Josias, Sep 7: *"instead of the tiles surrounding the restaurant, collapse them into a
convenient place, like cards in a deck, and pull them out sequentially… make each card
represent their step by how it looks… make the number and the STEP big enough to see at
first glance."*

**What changed.** The ring is now the *second* view. By default the 45 cards are a deck
pinned to the left (bottom-centre on a phone), and the model gets the rest of the screen —
the drawing box went from 745 px wide to **1043 px**, roughly **double the area**.

- `state.mode` is `'deck'` (default) or `'ring'`. `layout()` forks to `layoutDeck()`;
  everything else — engine, gates, traps, Reverse, the peek, the periodic table — is
  untouched and shared.
- `layoutDeck()` **renders the deck first and then measures it** (`offsetWidth/Height`)
  before placing the model box. Sizing it from constants put the deck off the bottom of a
  390-px phone; measuring fixed it. A phone also reserves 54 px at the bottom for the score
  row, or the deck lands on top of the buttons.
- The deck is **in `z` order**, and the top card is the first *available* card that is
  `ready()`. `‹ ›` riffles through the available cards, so playing out of order — and
  taking the leak — is still possible. Both halves of a trap pair sit next to each other at
  the same `z`, so the choice between them is preserved.
- Placed, burned and covered cards drop out of the deck. When it empties, the card face
  says so and **Put it in** disables.
- The big card is the drag source; the ghost is a clone of the card, not a tile.

### Card faces
`ART[id]` holds one inline-SVG glyph per card (viewBox `0 0 40 34`, drawn in the family's
ink). Gates reuse the inspector-with-a-magnifier figure with the gate number in the lens.
All 45 are covered — a headless test fails the build if any card falls back to its letters.

Typography, since the old tiles were unreadable: the **`z` number is 36 px serif** and the
**STEP number 21 px** in a dark pill (25 px / 15 px on a phone). The test asserts both
sizes, so a future restyle cannot quietly shrink them again.

### The deck is a real stack (Sep 7, *"make it 3d"*)
CSS 3D, no library. `.dwrap` holds the perspective; `.d3` inside it is the `preserve-3d`
scene containing three things: `#dstack` (the card backs), `#dedge` (the top card's own
thickness, painted in that card's family colour) and the card face itself.
- **The stack thins as you play.** `drawStack(left)` renders one back per ~5 cards
  remaining, each pushed further back in Z with a small offset and a darker `brightness()`.
  45 left = 9 backs; 5 left = 1; empty = none, and the edge slab is hidden.
- **The deck tilts toward the pointer** (`rotateX 7±7.5°, rotateY −9±10°`) and settles back
  to rest on leave. A `.gloss` layer sweeps with it, its angle driven by a `--ga` CSS var.
- **A placed card flies into the building.** `flyCard()` clones the outgoing face into a
  fixed `.flycard` and animates it to the centre of the model box with WAAPI, then removes
  it. `renderDeck._prev` holds the last face and its rect so the outgoing card still exists
  to animate. Riffling deals sideways (`renderDeck._dir`), a fresh card deals up from the
  stack.
- **`prefers-reduced-motion` turns all of it off** — the deck is static and everything still
  works.

🚨 **Two things this cost.** The card leans down and right, so `.dwrap` needs
`margin:6px 16px 24px 2px` or the tilted face sits on the counter. And `flyCard` must cap
itself (`> 3 live → drop the oldest`) and bail in ring mode: driving the game fast — a
script, or a very quick player — otherwise spawns one flying card per placement. A test
asserts the peak never exceeds 4 and that none are left in the DOM.

### Two traps this cost
1. **`.blk` was already taken.** The peek card's info blocks use `.blk`; the new card's link
   mark reused the name and inherited a border and a dark background — it rendered as a grey
   box next to the number. Renamed `.cardlink`. **Grep the stylesheet before naming a class
   in these files.**
2. **Text inside a glyph collides with the glyph.** "WELD" written across the welded-duct
   drawing landed on the duct. Say it with marks, not words — the card's name already
   carries the word.

## 12. Covenant Builders branding

The model wears the job the way a real one does, not as a watermark:
- **The job sign out front** — posts and a board with the wordmark, "BUILDING THE TREASURE
  COAST" and CBC1253676. It goes up with the permit and comes down with Reverse. In 3D it is
  a canvas texture (`brandTex()`); in the flat section it is a banner on the parapet.
- **The dumpster** carries the same panel, because every real one does.
- **A plaque by the door** once the finishes are in — visible from a table in the dining view.
- The brand line in the corner and the footer: *A Covenant Builders training model ·
  CBC1253676 · not a permit.*

🚨 **Clipping trap:** the yard sign first vanished in Cutaway because it sits at x = 14 and
the clip plane keeps x ≤ 1.5. Outdoor furniture must be built with `siteMat()` so it is
never clipped. The interior plaque was moved to x = −8 so it survives the cut instead.

---

## 13. The card was dead (Sep 8) — and how it hid

*"the build is unresponsive now, please fix it."*

He was right, and it had been broken since the deck first shipped. `startDrag` is bound
inside `tileNode(p, ring)` — **only to ring tiles.** The deck's `#bigcard` is static markup;
`renderDeck()` only rewrites its `innerHTML`. So the big card had a `grab` cursor, a hover
shadow and a gold ready-edge, and **nothing was listening.** Dragging it placed nothing.
Tapping it opened nothing. Only `Put it in` and the `‹ ›` arrows worked, because those
buttons have their own listeners.

**Why four passing test files never saw it:** every one of them drove the game through
JavaScript — `attempt(id)`, `deckStep(1)`, `setMode('ring')`. A JS-driven test cannot see an
unbound listener or a dead hit area. It only ever tested the engine.

### The fix
- **The whole deck well is the handle.** `pointerdown` is bound to `#dwrap`, not the card,
  and `startDrag` falls back to `state.cursor` when the target carries no `dataset.id`.
  This also fixes a second, subtler deadness: during the 240 ms deal-in animation the card
  is transformed out from under the pointer, so a tap right after a riffle used to land on
  the empty `#d3` behind it.
- `.dedge` gets `pointer-events:none`. An empty deck face is not a card (`if(!id) return`).
- Enter/Space on the focused card plays it (`click` with `detail === 0`).
- The hover-out timer that closes the peek now exempts `#deck` — it was closing the card
  you were standing on, half a second after you opened it.

### 🚨 The standing rule
**Every interactive element needs a real-pointer test.** `/tmp/rest/testpointer.js` drives
`page.mouse.move/down/up/click` at measured coordinates and asserts: what `elementFromPoint`
returns at the card's centre, that a real drag from the card to the model places a card,
that `Put it in` and the riffle arrows respond to real clicks, that a real tap opens the
peek, and (3D) that a real drag on the model spins the camera. It found this in one run.
Note: `e.className` on an SVG node is an `SVGAnimatedString`, which serialises as `{}` —
resolve hit tests with `closest('#dwrap')`, not by string-matching the class.


---

## 14. The cards speak in orders now (Sep 8)

His standing rule from HOUSE, finally applied here: **a card is an instruction to the man
who is about to go do the thing, not a description of an event.** All 45 cards rewritten,
both levels. The Spec block heading is now **"What you do"** on every card, gates included.

Before → after:

> *Building permit with sealed plans, plus separate permits pulled by the licensed subs…*
> **You pull the master building permit with the sealed plans. Then you make each licensed
> sub pull his own: plumbing, electrical, mechanical, gas, fire suppression for the hood
> system…**

Rules held:
- **Second person, present tense, verb first.** Say who does it — *you* set it, *he* checks
  it, *the utility* releases the meter. No passive voice, no noun-phrase inventories.
- **Gates are instructions too.** *"You call in the underground plumbing inspection, and you
  call it before anybody talks about concrete."* Calling the inspection IS a step.
- **Fails stay warnings but go second person.** *"Pour over it before he comes and you rent
  a saw and start again."*
- **Law is left alone.** It is the line he repeats on the ladder, not an order.
- **Every number survives.** A quarter inch per foot, 16-gauge, 75 feet, 10–20 ft and 42–48
  in., 30 feet, 33–36 in., 16–18 in., 34 in., 60-inch circle, five percent, 28–34 in., 36
  in., 15 sq ft per person, 1,000–1,500 gallons, 180°F, 18 months, ~30 days, 50 or more.
  A verification pass diffs the numeric tokens of every card against the previous version;
  stripping a spec number to make a sentence flow is a defect, not a style choice.
- **Curly apostrophes only.** Card text lives in single-quoted JS strings and a straight
  apostrophe breaks the file silently.

Two cards carry an embedded cross-link (`dm` → HOUSE, `ef` → ROOF) built by string
concatenation. Those were rewritten around the link, never through it.

---

## 15. THE SIX CALLS — the advanced level (`apps/calls/`)

*"Make an advanced version with these choices."* The choices are §7 — the six things this model
graded ~ or ◈ because no code book answers them. §7 was a to-do list. This level makes it
playable: **you make the six calls, the job carries on either way, and the bill finds you later.**

### The shape
26 cards, not 45: **six CALL cards, ten gates, ten build cards** that bundle what the base level
splits. Ten minutes to play, so the weight sits on the decisions instead of the parts list. Same
shell, same deck, same flat scene — a build card carries `also:['layer',…]` and `L()` was widened
to read it, so one card can light five layers and the building still draws itself.

A CALL card does not go into the building. It opens a sheet with two to four options, each one a
real thing a contractor actually does. You pick. `state.calls[id]` records it, and the consequence
is **deferred**: `{days, dollars, at}` where `at` is the gate that will hand you the bill. Some land
at gate 1. One lands at gate 9.

Score row is **Gates · Calls · Days lost · Caught out**. "Caught out" is money that arrived with no
budget line — a `verdict:'bad'` choice. The same dollars on a `verdict:'good'` choice are
**budgeted**, not caught out, because that is the entire difference between asking and not asking.

Ends on **THE BILL**: one row per call, what you chose, the days, the dollars, how it landed, why,
and the source underneath. Reverse un-makes a call (snapshots carry `calls`, `pending`, `bill`,
`days`, `exposed`, `spend`).

### The measured spread
- **Ask everything:** 10 gates, **18 days**, $28,376 spent, **$0 caught out.**
- **Ask nothing:** 10 gates, **107 days**, $37,376 spent, **$37,376 caught out.**

Both open. That is the lesson — the building gets built either way, and the calls are the only
thing standing between those two numbers.

### What the research pass actually confirmed (§7, answered)
1. **A-2 sprinklers — CONFIRMED, with numbers.** IBC/FBC 903.2.1.2: a Group A-2 fire area needs
   sprinklers if it exceeds **5,000 sq ft**, or has an occupant load of **100 or more**, or sits on
   a floor other than the level of exit discharge. This bay is 2,400 sq ft and about 81 people —
   it clears the occupant-load trigger by nineteen people.
2. **Occupant load — CONFIRMED.** Table 1004.5: assembly unconcentrated (tables and chairs)
   **15 net**, concentrated 7 net, commercial kitchens **200 gross**, business 150 gross.
   Panic hardware for Group A at **50 or more** (IBC 1010.2.9); assembly classification at 50
   (IBC 303.1.1). 2,400 sq ft with ~1,150 sq ft of dining lands at about 81 — squarely A-2.
3. **Interceptor size — CONFIRMED as a method, not a number.** Gravity interceptors size by
   drainage fixture units: **8 DFU → 500 gal, 21 → 750, 35 → 1,000, 90 → 1,250**, and they go
   **outside the building unless the AHJ approves otherwise**. Hydromechanical units size by flow,
   commonly 20–50 gpm. The local FOG ordinance still controls the final number. The old
   "1,000–1,500 gallons" line was a rule of thumb standing in for a table.
4. **The capacity charge — CONFIRMED, with real money.** Indian River County Department of Utility
   Services, impact fees effective **1 October 2022**: water **$1,300.00/ERU**, sewer
   **$2,796.00/ERU** — **$4,096 a connection** — plus service connection fees around $2,785 water
   and $2,895 sewer. **The ERU count for a non-residential change of use is not published
   anywhere.** That is the call: the price is public, the quantity is not.
   ⚠️ Three years old. Confirm the current schedule.
5. **The inspection sequence — CONFIRMED, and worse than assumed.** The sheet Indian River County
   publishes is `2019_BRCOM_Scheduling`, headed **"BRCOM RESIDENTIAL COMBINATION NEW
   CONSTRUCTION"** — footing, tie beam, truss bracing, roof sheathing, strapping, dry-in. A house.
   And the document states the system **will not allow scheduling a priority 2 until all
   priority 1s are resulted and passed**. So calling out of order is not a late call, it is a call
   the system declines. No commercial equivalent found published.
6. **Who accepts the interceptor and the backflow — STILL UNANSWERED, on purpose.** No published
   source: county, state or code. It stays graded ◈ and the card says so out loud: *"This is the
   only step in this game with no published source; the answer lives in somebody's head until you
   ask for it."* That honesty is the point — a training model that pretends to know is worse than
   one that names its own gap.

### 🚨 Traps this level cost
1. **CSS inserted mid-stylesheet loses to the original rules.** The bill panel and its `z-index`
   went into the middle of the sheet, so `#done{z-index:45}` and `#done p{max-width:44ch}` further
   down kept winning — a flying card rendered *over* the bill and the intro paragraph stayed 44
   characters wide. **Overrides go at the END of the stylesheet, or carry higher specificity.**
   Nothing about the rule was wrong; it was in the wrong place.
2. **A dialog must outrank a `.flycard`.** `#callbox` and `#done` now sit above z-index 60, and
   `flyCard()` bails while either is open.
3. **The scene's own numbers are the source of truth.** The cards said 1,760 sq ft (44 × 40, from
   the 3D level) while the flat scene label reads **2,400 SF**. Match the drawing the player is
   looking at, then redo the arithmetic — the corrected figure made the sprinkler card *better*,
   because 81 people against a limit of 100 is a sharper lesson than 70 against 100.
4. **`ART` is keyed by card id**, so a new catalog draws letters instead of pictures. Cards now
   carry `art:'xx'` to borrow an existing glyph, and `artFor` reads `ART[p.art || p.id]`. A test
   fails if any card has no drawing. One new glyph was drawn: `call`, a fork in the road.

### Tests
`testcalls.js` — engine: 26 cards / 6 calls / 10 gates, every card draws, a best-call run and a
worst-call run both reach ten gates, the bill has six lines with nothing left pending, a best run
is caught out for $0, a worst run loses more days, Reverse un-makes a call, Start over clears them,
and the scene lights 40 layers.
`testcallsui.js` — real pointer and real touch: the deck button opens the sheet, a real click on an
option records the call and advances the deck, dragging a call card into the building opens the
sheet, the sheet fits at 1440×900 / 1024×700 / 390×780, and the bill renders six rows inside a
390 px phone.
