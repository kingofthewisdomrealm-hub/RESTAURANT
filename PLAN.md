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
