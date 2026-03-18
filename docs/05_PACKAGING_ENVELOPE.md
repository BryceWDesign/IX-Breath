# IX-Breath Packaging Envelope and Suit Integration Posture

**Document ID:** IXB-PKG-05  
**Revision:** A  
**Status:** Active baseline  
**Repository phase:** Architecture study  
**Parent documents:**  
- `docs/00_PROJECT_CHARTER.md`  
- `docs/01_MISSION_REQUIREMENTS.md`  
- `docs/02_OPERATING_ASSUMPTIONS.md`  
- `docs/03_MASTER_ARCHITECTURE.md`  
- `docs/04_DONOR_FEATURE_FILTER.md`

---

## 1. Purpose

This document defines the preliminary physical envelope, internal zoning posture, and suit-integration rules for IX-Breath.

The goal is not to pretend detailed CAD exists.

The goal is to do the serious thing early:

- set a believable worn envelope,
- define what counts as acceptable bulk,
- identify where major subsystems physically live,
- and block future architecture drift into something obviously impractical.

If this file is weak, the rest of the repo can lie without meaning to.

---

## 2. Packaging philosophy

IX-Breath is only credible if it remains **backpack-class**.

That means the architecture must resist four common failure modes:

1. **thickness creep**  
   every “small addition” quietly pushes the pack farther off the astronaut’s back

2. **center-of-gravity drift**  
   heavy subsystems migrate outward until the backpack becomes awkward or destabilizing

3. **rescue-mode cheating**  
   deployables are treated as “free” and their stowed geometry is ignored

4. **internal conflict**  
   hot, noisy, pressurized, and delicate subsystems are mixed together carelessly

This document exists to stop that.

---

## 3. Baseline worn-envelope target

### 3.1 Phase-1 nominal worn envelope

The current IX-Breath phase-1 **folded / worn** target envelope is:

| Dimension | Target |
|---|---|
| Height | **25 inches** |
| Width | **20 inches** |
| Thickness (front-to-back) | **10.5 inches** |

This is the **nominal worn envelope**, not the rescue-deployed span.

### 3.2 Accept / reject posture

At phase 1, the following posture is locked:

| Condition | Posture |
|---|---|
| Height at or below 26 inches | acceptable if internal layout remains coherent |
| Width at or below 21 inches | acceptable if arm/shoulder clearance is preserved |
| Thickness at or below 11 inches | acceptable target range |
| Thickness above 11 inches | reject unless later analysis proves a compelling survivability payoff |
| Permanent worn width or depth increase driven only by rescue deployables | reject |

The thickness rule matters most.

A backpack that sits too far off the body starts failing long before it “technically fits.”

---

## 4. Envelope classes

To avoid future confusion, IX-Breath uses three geometry classes.

| Envelope class | Definition |
|---|---|
| **Worn Envelope** | The pack shape when the astronaut is operating normally and all rescue-only deployables are stowed. |
| **Contingency Envelope** | The pack shape when rescue-only elements are deployed in survival mode. |
| **Service Envelope** | The footprint/clearance required for inspection, recharge, access panels, and maintenance. |

Only the **Worn Envelope** governs everyday wearability.

The other two still matter, but they must not be confused with nominal body-borne geometry.

---

## 5. Rescue-deployed envelope

### 5.1 Rescue-mode deployment posture

IX-Breath may deploy rescue-only hardware when the system enters Rescue or Critical mode.

Current allowed rescue-only deployables are limited to:

- compact solar augmentation surfaces,
- limited thermal-assist surfaces or shades,
- passive visibility / identification panel area if ever justified later.

### 5.2 Rescue-deployed geometric target

The current phase-1 **contingency deployed** goal is:

| Dimension | Target |
|---|---|
| Deployed lateral span | **34–38 inches max** |
| Added rearward projection beyond worn thickness | **no more than 2.0 inches** |
| Added vertical projection beyond nominal height | **no more than 3.0 inches** |

This means rescue mode may widen the system, but it may not turn it into a giant sail or a rigid scaffold.

### 5.3 Rescue-only discipline rule

If a rescue-only deployment concept requires:
- large rigid booms,
- long spars,
- multi-foot rigid radiators,
- or a deployment sequence too complex for emergency use,

it is out of family for IX-Breath phase 1.

---

## 6. Suit-integration posture

### 6.1 Integration rule

IX-Breath is designed to interface with a suited astronaut, not to redefine the entire pressure garment.

The pack must therefore preserve a clean split between:

- **backpack-contained survival functions**, and
- **pressure-garment-contained crew interface / breathing volume functions**.

### 6.2 Primary integration points

The current intended integration points are:

| Interface | Location posture | Function |
|---|---|---|
| Main gas interface | Upper central suit-back hard interface | Closed-loop gas out / gas return |
| Power/data interface | Adjacent protected interface zone | Sensor, control, and system handshake |
| Emergency crew input | Suit-accessible, simple, gloved operation path | Rescue entry / acknowledgement / isolation input |
| Service/recharge interface | Dockside / service-accessible exterior zone | Recharge, log extraction, maintenance, inspection |

### 6.3 Integration constraints

The architecture shall preserve the following suit-integration rules:

1. No major front-mounted life-support mass migration.
2. No rescue hardware that blocks head, shoulder, or arm clearance in nominal wear.
3. No routine operation dependent on complex EVA-time reconfiguration.
4. No hidden assumption that the astronaut can perform fine motor service actions during emergency use.

---

## 7. Internal packaging zones

IX-Breath uses explicit internal zoning so the package is engineered by hazard class rather than by convenience.

### 7.1 Zone map overview

| Zone ID | Zone name | Primary contents | Packaging priority |
|---|---|---|---|
| Z1 | Upper Interface Zone | suit gas/power/data interface manifold, protected routing | accessibility, clean routing |
| Z2 | Central Life-Support Core Zone | blower path, gas manifolds, CO2/humidity handling | low leak risk, short gas path |
| Z3 | Oxygen Continuity Zone | primary O2 storage, isolated emergency O2 path, regulators | pressure protection, isolation |
| Z4 | Water / Regeneration Zone | condensate handling, conditioned water storage, electrolyzer support | leak management, separation from electronics |
| Z5 | Power Spine Zone | battery, pulse buffer, PMU, protected buses | CG control, thermal monitoring |
| Z6 | Control / Watchdog Zone | supervisory controller, watchdog, blackbox, sensor conditioning | EMI control, survivability |
| Z7 | Thermal Transport / Rejection Zone | heat transport components, thermal buffers, deployable interfaces | heat path efficiency |
| Z8 | Structural Survivability Shell | shell, liners, partitions, load paths | protection, containment |

### 7.2 Zoning rule

High-pressure gas, water-handling, power storage, and critical controls do **not** get casually co-located.

If later subsystem definitions blur these boundaries, the architecture is regressing.

---

## 8. Preferred internal layout

### 8.1 Layout posture

The current preferred IX-Breath package layout is:

- **upper-center:** suit interface manifold and protected routing
- **centerline:** life-support core for short gas paths
- **left/right midline near torso:** oxygen storage and regulation
- **lower-mid near body:** battery and pulse-buffer mass
- **lower-center / lower-rear:** water handling and regenerative support hardware
- **rear shell / edge banding:** thermal routing and rescue-only deployables
- **shielded quiet bay:** control electronics and sensor-conditioning hardware

This is not arbitrary.

It is chosen to keep:
- gas paths short,
- heavy components close to the astronaut,
- noisy electronics away from delicate sensing,
- and emergency reserve physically protected.

### 8.2 Center-of-gravity rule

The heaviest dense masses should sit:
- low-to-mid torso height,
- as close to the astronaut’s back plane as possible,
- and symmetrically about the vertical centerline unless strong reason exists otherwise.

That means the worst packaging habits are:

- hanging heavy modules far aft,
- putting too much mass high and outward,
- stacking every “extra” subsystem in the rear shell void.

Those habits are forbidden unless later evidence justifies them explicitly.

---

## 9. Preliminary volume allocation

This is a first-order **architecture budgeting view**, not detailed CAD.

| Subsystem family | Approx. share of internal volume |
|---|---|
| Life-Support Core (S1) | **22%** |
| Oxygen Storage and Delivery (S2) | **18%** |
| Emergency O2 Isolation Hardware (subset of S2) | **6%** |
| Regenerative Extension Layer (S3) | **10%** |
| Water Recovery and Conditioning (S4) | **9%** |
| Thermal Survivability Layer (S5) | **11%** |
| Power Continuity Spine (S6) | **15%** |
| Control / FDIR / Crew Interface Electronics (S7) | **4%** |
| Structural partitions / shell / liner overhead (S8) | **5%** |

Total: **100%**

### 9.1 Why this matters

The point of this table is not precision.

The point is to prevent later documents from pretending that:
- regeneration takes no room,
- thermal hardware takes no room,
- or power protection is “basically free.”

It is not free.

---

## 10. Text sketch of the package

### 10.1 Rear view concept

```text
+--------------------------------------------------+
|                Z1 Upper Interface Zone           |
|       [protected gas / power / data manifold]    |
|--------------------------------------------------|
|   Z3 O2 Zone     |    Z2 Life-Support Core   |   Z3 O2 Zone
| [primary + iso]  | [blower / scrub / humid ] | [primary + iso]
|                  | [gas path / manifolds     ] |
|--------------------------------------------------|
|   Z6 Control     |  Z4 Water / Regen Zone    |   Z7 Thermal
| [quiet bay]      | [condensate / condition / | [transport /
|                  |  electrolysis support]    |  edge hardware]
|--------------------------------------------------|
|           Z5 Power Spine Zone (low / close)      |
|        [battery / pulse buffer / PMU / dump]     |
+--------------------------------------------------+

10.2 Side view concept

      suit side / astronaut back
                ||
                ||  [torso plane]
+---------------------------------------------+
| Z1 interface bulkhead                       |
|---------------------------------------------|
| Z2 core / Z3 O2 kept near body             |
|---------------------------------------------|
| Z5 battery + PMU kept low and inboard      |
|---------------------------------------------|
| Z4 water / regen support                   |
|---------------------------------------------|
| rear shell + Z7 thermal path + stowed      |
| rescue deployables kept flush              |
+---------------------------------------------+
                ||
                ||  outward aft direction

These sketches are intentionally simple.

They exist to show layout logic, not industrial design styling.

11. Thickness management strategy
11.1 Why thickness is the hardest dimension

Height and width can sometimes be negotiated.

Thickness is the dimension that punishes the astronaut fastest because it:

moves CG outward,

hurts posture and translation,

complicates hatch/pass-through interactions,

and makes every pound feel worse.

That is why IX-Breath treats thickness as the first packaging red line.

11.2 Thickness control rules

Thick subsystems must be pushed inboard, not stacked aft.

Rescue-only hardware must fold into shell depth, not become permanent thickness.

Sensor and control boards should live in low-profile shielded bays, not in bulky stacked boxes.

Oxygen tank geometry should be selected to minimize wasted voids and aft protrusion.

Thermal hardware should use edge bands / shell integration where practical rather than rear “lumps.”

12. Human-mobility posture

This document does not pretend to provide full human factors validation.

It does establish the mobility posture the design must respect.

12.1 Mobility design intent

IX-Breath should preserve a believable ability to:

pass through nominal suit operational clearances,

translate and rotate without absurd aft bulk,

maintain reasonable astronaut balance,

avoid persistent interference with arm and shoulder motion,

and avoid rescue hardware that snags or dominates nominal operations.

12.2 Mobility warning signs

The following conditions should be treated as major review warnings:

thickness grows beyond 11 inches with no major payoff,

major mass shifts above shoulder line,

rescue hardware requires large hard appendages,

service panels or routing protrude into snag-prone geometry,

the architecture depends on deployment during every normal EVA.

If any of those appear later, the package is drifting in the wrong direction.

13. Structural and containment posture
13.1 Structural shell role

The shell is not decorative packaging.

It must do real work:

carry subsystem loads,

protect pressure hardware,

isolate water-handling risks,

isolate electrical hazard zones,

support thermal routing,

and keep emergency oxygen path physically protected.

13.2 Containment partitions

The internal package should maintain at least the following barriers:

Partition	Purpose
O2 zone to power zone	reduce shared-failure risk between stored oxygen and electrical fault energy
Water zone to control zone	reduce leak-driven control degradation
Noisy power zone to quiet sensing zone	preserve signal integrity
Thermal hotspot region to emergency reserve region	prevent hot subsystem bias from degrading last-line survivability
13.3 Liner posture

If a puncture/impact-mitigation liner is later retained, it should be used conservatively:

around oxygen tanks,

around the battery/PMU bay,

and around high-value routing corridors.

It should not be sold as invulnerability.

14. Rescue deployables integration posture
14.1 Allowed deployment philosophy

Rescue-only deployables are acceptable only if they satisfy all of the following:

stay flush or nearly flush when stowed,

deploy with simple bounded logic,

fail in a way the core backpack can survive,

do not block core life-support operation if they never deploy,

do not force the nominal worn envelope to become ridiculous.

14.2 Preferred deployment locations

The current preferred locations for rescue-only deployables are:

shell-edge foldout bands,

rear-surface layered panels,

upper-side compact foldouts that clear shoulder motion when stowed.

14.3 Rejected deployment families

The following are rejected for phase 1:

long booms,

skeletal truss radiators,

rigid sails,

large spin-out panels,

external assemblies that require astronaut fine-motor setup.

15. Serviceability posture

IX-Breath must also be maintainable.

A serious life-support architecture that cannot be inspected is not serious.

15.1 Service access goals

The pack should support service access to:

oxygen path inspection points,

power isolation and discharge points,

control/log extraction interface,

water path service and drain points,

sensor calibration access,

replaceable life-limited components.

15.2 Service-envelope rule

Service access may use a larger footprint than the nominal worn envelope, but:

it must not require total disassembly for routine readiness checks,

and it must not create maintenance procedures so fragile that the sustainment layer becomes theater.

16. Package-related design rules now locked

The following package rules are now baseline IX-Breath rules:

Nominal worn envelope target is 25 x 20 x 10.5 inches.

Thickness above 11 inches is a major red flag.

Heavy masses belong low-to-mid and close to the astronaut.

Gas, water, power, and control zones must remain intentionally separated.

Rescue-only deployables must stow nearly flush.

The package may not quietly turn into a mini spacecraft.

Serviceability counts as seriousness, not luxury.

17. What this document does not yet claim

This file does not claim:

verified fit through a specific hatch or airlock,

verified suit articulation compatibility,

verified anthropometric optimization,

validated structural margins,

final tank geometry,

final battery size,

final deployable geometry.

Those belong to later work.

What this file does claim is narrower and more important right now:

IX-Breath has a believable packaging discipline and a bounded physical target.

That is enough for this stage.

18. Immediate implications for later commits

Because the package envelope is now locked, later documents must obey it.

That means:

the fresh BOM must remain believable inside this envelope,

the power-spine architecture cannot sprawl,

the life-support core cannot pretend its manifolds and scrubber hardware take no room,

the regeneration layer must justify its footprint honestly,

the later analysis must not claim endurance that secretly assumes invisible hardware.

This is the point where IX-Breath starts being boxed into reality.
