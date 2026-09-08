# RESTAURANT — Build-Out

A working model of what it actually takes to turn a leased retail bay into a licensed
Florida restaurant. Same shell as **ROOF Shingle** and **HOUSE Block**: a ring of parts
around a building, ten inspection gates in the real order, and traps that look finished
from a customer's table.

Open `index.html`. Two levels, same rules:

```
RESTAURANT/
  index.html                the hub
  apps/tenant3d/index.html  the 3D model you can spin  (needs WebGL + three.js from cdnjs)
  apps/tenant/index.html    the flat one               (no dependencies, no internet)
  PLAN.md                   the catalog, the engine, the open decisions
  README.md                 this
```

**Reverse** takes the last step back — button, or Ctrl+Z / Cmd+Z. Press it repeatedly to walk
the restaurant back to an empty bay. A refused drop is not a step, so Reverse never undoes
something you cannot see.

## The deck

The 45 cards sit in a **deck** on the left, not in a ring around the model — the building
gets the screen. The top card is the next step that is ready. Drag it in, or press
**Put it in**. The counter under the deck always says where you are:

```
Card 19 of 45
Step 5 of 10 · 27 left
```

The deck is a real stack: it has thickness, it thins as you play it down, it tilts toward
your pointer, and a card you place flies out of your hand into the building.

Every card is **drawn**, not lettered: the welded duct shows weld beads, the screwed one
shows screw heads, the missing make-up air shows an empty dashed box. The card number and
the step number are the two biggest things on it, readable across a truck cab.

- **‹ ›** riffles the deck if you want to play out of order (and find out what that costs)
- **See all 45** puts every card back in the ring around the building, the old way
- **The table** opens the periodic grid — the same 45 cards as data

## What it teaches

A house is a building. A restaurant is a **licence** that happens to live in a building.
Four different authorities can stop you, and only one of them is the building department:

| Who | What they can stop |
|---|---|
| Zoning / the utility | The use itself, the parking, and the sewer capacity charge — *before the lease* |
| DBPR, Division of Hotels & Restaurants | The floor plan, the sinks, the finishes, the licence |
| The fire marshal | The hood system, the exits, the certificate |
| The building department | Every trade, and the CO |

## The ten gates

1. **Plan review** — zoning, DBPR, building and fire all say yes before one wall moves
2. **Underground** — two waste systems under one slab: grease and sanitary
3. **Interceptor** — the grease interceptor outside, and the backflow preventer
4. **Rough-in** — framing, blocking, plumbing, power, gas under test
5. **Hood & duct** — Type I hood, grease duct welded liquid-tight
6. **Fire suppression** — UL 300, interlocked, proven on a discharge test
7. **Above ceiling** — make-up air, sprinkler heads, alarm devices
8. **Finish & equipment** — cleanable surfaces, coving, the accessible restroom, air gaps
9. **Fire marshal final** — exits, panic hardware, emergency lights, Class K
10. **Opening inspection** — DBPR walks it, the licence prints, the county signs the CO

## The three traps

Each one is a real shortcut a real crew takes, and each one is invisible from the
dining room:

| Trap | Looks like | Fails at |
|---|---|---|
| Screw and mastic the duct seams | Every other duct in the building | Gate 5 — hood & duct |
| Skip the make-up air | A finished ceiling | Gate 7 — above ceiling |
| Hard-pipe the sink drains | Clean plumbing under a stainless sink | **Gate 10 — the state, on opening day** |

The third one is the lesson: it passes every building-department gate on the dial and
dies at the last one, with staff hired and an opening date announced.

## The three views

- **Cutaway** — a plane cuts the bay down its length. Everything you placed, in order.
- **Dining room** — stand where a customer stands. All three traps look like this too.
- **Inspector** — only what the open gate checks is lit. Everything else goes to a ghost.

## Evidence grades

Every card is graded, and the grade is on the card.

- ✓ **Robust** — in the code, the rule, or the state form
- ~ **Range** — real, but the number is on your plans or your utility's ordinance
- ⚠ **Contested** — jurisdictions differ
- ◈ **Model** — simplified for the sim
- ✗ **Cartoon** — the common lie (the traps, revealed only after they burn you)

## Sources

- Florida DBPR, Division of Hotels & Restaurants — [Plan Review](https://www2.myfloridalicense.com/hotels-restaurants/licensing/plan-review/) (61C-1.002, 61C-4.010)
- Florida DOH food establishment plan review guide — construction standards, air gaps, coving
- Florida Building Code 8th Ed. — Mechanical Ch. 5 (Type I hoods, grease ducts, make-up air), Plumbing Ch. 10 (interceptors), Accessibility
- Florida Fire Prevention Code / IFC 904.13, NFPA 96, NFPA 17A, UL 300
- Indian River County Utilities — FOG program and interceptor sizing (**call them; the number is local**)

Built by **Covenant Builders**, Vero Beach FL · CBC1253676. The job sign is on the model,
same as it is on the site.

Not a permit. Not a code book. A training model that tells the truth about the order.
