# IX-Breath Life-Support Core Architecture

**Document ID:** IXB-LSS-08  
**Revision:** A  
**Status:** Active baseline  
**Repository phase:** Architecture study  
**Parent documents:**  
- `docs/00_PROJECT_CHARTER.md`
- `docs/01_MISSION_REQUIREMENTS.md`
- `docs/02_OPERATING_ASSUMPTIONS.md`
- `docs/03_MASTER_ARCHITECTURE.md`
- `docs/04_DONOR_FEATURE_FILTER.md`
- `docs/05_PACKAGING_ENVELOPE.md`
- `docs/06_REAL_WORLD_BOM.md`
- `docs/07_POWER_CONTINUITY_SPINE.md`

---

## 1. Purpose

This document defines the detailed life-support architecture for IX-Breath.

Its job is to make the core survival logic explicit:

1. how breathing gas circulates,
2. how oxygen continuity is layered,
3. how carbon dioxide and humidity are controlled,
4. how water is recovered and conditioned,
5. how bounded oxygen regeneration is allowed or denied,
6. how the system degrades when things go wrong,
7. and where the architecture draws a hard line between survivable fallback and wishful thinking.

This document covers the detailed interaction of:

- **S0 Pressure Garment Interface**
- **S1 Life-Support Core**
- **S2 Oxygen Storage and Delivery**
- **S3 Regenerative Extension**
- **S4 Water Recovery and Conditioning**

---

## 2. Life-support design posture

IX-Breath is built around one central life-support rule:

> **The pack should recycle what it can, preserve what it must, and isolate the true last-resort oxygen path until it is genuinely needed.**

That means the architecture is not:

- open-loop by default,
- dependent on one tank and one regulator,
- dependent on one controller,
- dependent on continuous regeneration,
- or allowed to count unqualified water as usable oxygen feedstock.

Instead, it uses:

- a **closed recirculating gas loop**,
- a **primary oxygen make-up path**,
- an **isolated emergency oxygen reserve**,
- a **bounded regenerative oxygen extension path**,
- explicit **CO2 and humidity control**,
- and explicit **fault-driven fallback behavior**.

---

## 3. Architecture statement

**The IX-Breath life-support core is a closed-loop breathing-gas system that conditions suit-loop gas, injects controlled oxygen make-up from layered oxygen sources, removes carbon dioxide and humidity, recovers usable water, and only enables bounded oxygen regeneration when power, water, thermal state, and fault posture make that decision defensible.**

That last part matters.

Regeneration is not assumed.
It is earned in real time.

---

## 4. Functional block structure

```mermaid
flowchart LR

    Crew[Suited Crewmember / Pressure Garment Volume]
    S0[S0 Pressure Garment Interface]
    S1A[Gas Return Manifold]
    S1B[Humidity / Condensate Interface]
    S1C[CO2 Removal Assembly]
    S1D[Circulation Blower Set]
    S1E[Conditioned Gas Delivery Manifold]

    S2A[Primary O2 Path]
    S2B[Isolated Emergency O2 Path]
    S2C[O2 Metering / Injection]

    S4A[Condensate Separation]
    S4B[Recovered Water Reservoir]
    S4C[Water Conditioning]
    S4D[Sterile Water Reserve]

    S3A[PEM Electrolyzer]
    S3B[Gas / Liquid Separation]
    S3C[Regenerated O2 Conditioning]
    S3D[Hydrogen Vent Path]

    Crew --> S0 --> S1A --> S1B --> S1C --> S1D --> S1E --> S0 --> Crew
    S1B --> S4A --> S4B --> S4C --> S3A
    S4D --> S3A
    S3A --> S3B --> S3C --> S2C
    S3B --> S3D
    S2A --> S2C
    S2B --> S2C
    S2C --> S1E

5. Top-level life-support logic

IX-Breath uses four linked continuity loops:

Gas loop

keep breathable gas circulating through the suit

Oxygen continuity loop

replace metabolized oxygen without wasting reserve oxygen unnecessarily

Water recovery loop

capture and condition water honestly

Regeneration permission loop

only allow electrolytic oxygen production when the system can afford it

These loops are connected, but they are not equal.

The order of importance is:

keep breathing viable,

keep CO2 controlled,

keep oxygen continuity viable,

keep thermal state survivable,

extend endurance only when the first four remain protected.

6. S0 — Pressure Garment Interface
6.1 Role

S0 is the physical and functional boundary between the backpack and the pressure garment loop.

It exists to:

carry returning suit gas into the pack,

send conditioned gas back into the suit,

preserve clean gas routing,

support protected sensor and control handoff,

and keep emergency crew-accessible controls simple.

6.2 Interface channels

The S0 boundary includes:

Channel	Function
Return gas line	carries exhaled / circulated suit gas into S1
Supply gas line	returns conditioned gas into the suit volume
Power/data path	carries protected sensor and command interface where needed
Emergency input path	allows crew-commanded rescue behavior / acknowledgement
6.3 Boundary rule

S0 must stay simple enough that the backpack can be understood as a backpack-class life-support pack, not a hidden full-suit redesign.

7. S1 — Life-Support Core
7.1 Role

S1 is the breathing engine of IX-Breath.

Its job is to keep the suit gas loop viable by:

moving gas,

conditioning gas,

monitoring gas state,

and presenting a stable injection point for oxygen continuity.

7.2 S1 major functions
Function	Purpose
Gas return handling	bring suit gas into controlled conditioning path
Humidity / condensate interface	remove water burden and feed recovery logic
CO2 removal	prevent toxic accumulation
Circulation	maintain closed-loop flow
Gas-state awareness	support safe control and alerting
O2 injection acceptance	accept make-up oxygen cleanly from S2
7.3 Preferred S1 flow order

The intended order is:

gas returns from suit through S0,

gas enters return manifold,

humidity / condensate opportunity is extracted,

gas passes through CO2 removal assembly,

gas is driven by circulation blower path,

oxygen make-up is injected downstream at a controlled point,

conditioned gas returns through supply manifold to the suit.

7.4 Why this order is preferred

This order is chosen to:

keep the gas path short,

recover water before it becomes hidden loss,

avoid unnecessary dilution of sensor interpretation,

let oxygen injection occur into a controlled conditioned stream,

and keep emergency oxygen delivery simple.

8. Circulation architecture
8.1 Circulation posture

IX-Breath does not assume one perfect blower.

The architecture requires at least:

one primary viable blower path,

one second path or redundant blower element,

isolated drive logic,

and a fallback posture if one path is lost.

8.2 Minimum survivability rule

The life-support architecture must preserve at least one viable circulation path for as long as power and gas integrity allow.

8.3 Blower failure posture

A blower failure should not instantly collapse the entire life-support chain if:

a second blower exists,

gas routing can isolate the faulted path,

and the electrical architecture can preserve B1 power.

That is why circulation sits on B1.

9. CO2 control architecture
9.1 CO2 posture

Closed-loop breathing is useless if CO2 control is weak or ambiguous.

CO2 handling is therefore treated as a life-critical function, not an efficiency accessory.

9.2 Baseline CO2 control lane

The preferred IX-Breath baseline is a backpack-class amine-based CO2 and humidity handling lane, adapted from real suit-relevant architecture patterns rather than copied blindly from scuba rebreathers.

9.3 Why this lane was chosen

It survives the filter because it offers:

a real closed-loop suit-relevant path,

strong mission fit,

integration with humidity handling,

and a credible aerospace review posture.

9.4 Architecture rule

IX-Breath does not claim the exact final cartridge chemistry or bed geometry yet.

What is locked is the function:

real CO2 removal,

in a backpack-class lane,

with clear replacement/service implications,

and measurable performance state.

10. Humidity and condensate interface
10.1 Why humidity matters

Humidity is not just a comfort issue.

In IX-Breath it affects:

breathing-loop stability,

condensate burden,

water recovery opportunity,

sensor interpretation,

and the honesty of regeneration claims.

10.2 Humidity-control posture

The architecture assumes humidity control must:

remove water from the circulating loop,

create a real liquid-water recovery path,

avoid pretending all water magically reappears in useful form.

10.3 Condensate honesty rule

Water is only credited to S4 if it passes through:

a defined collection path,

a measurable storage state,

a defined conditioning path.

Anything else is bookkeeping fiction.

11. Gas-state sensing architecture
11.1 Why sensing is first-class

A closed-loop suit system without credible state awareness is just a sealed guess.

That is unacceptable.

11.2 Minimum critical sensor set

The phase-1 minimum critical set is:

Sensor family	Baseline posture
O2 state sensing	triplex or functionally equivalent cross-check posture
CO2 state sensing	dual sensing with plausibility logic
Loop pressure sensing	redundant
Humidity sensing	dual
Tank / regulator pressure sensing	redundant across key paths
Water state sensing	recovered and reserve state awareness
11.3 Sensor architecture rule

Sensing must support three things:

control,

crew alerting,

fault classification.

It is not enough for sensors to merely exist on a diagram.

11.4 Sensor disagreement rule

Sensor disagreement must never be silently hidden.

The system must explicitly degrade confidence, isolate the bad source if possible, and bias toward crew safety rather than quiet optimism.

12. S2 — Oxygen Storage and Delivery
12.1 Role

S2 is the oxygen continuity backbone of IX-Breath.

Its job is to ensure that oxygen enters the loop from the right source, at the right time, through the right path.

12.2 Three oxygen layers

S2 operates with three oxygen layers:

Layer	Role
L1	Primary stored oxygen for nominal and early rescue operation
L2	Isolated emergency oxygen reserve for true last-line continuity
L3	Regenerated oxygen accepted from S3 when conditions allow
12.3 Why the layers exist

They exist because one undifferentiated oxygen source invites bad behavior:

early reserve depletion,

ambiguous emergency posture,

and false confidence about survivability margin.

IX-Breath refuses that design habit.

13. Primary oxygen path
13.1 Role

The primary path carries ordinary oxygen make-up demand during:

nominal operation,

conserve operation,

and early rescue operation.

13.2 Primary-path functions

The primary path includes:

primary storage vessel(s),

primary regulation,

pressure sensing,

metering control,

backflow prevention,

feed into the make-up manifold.

13.3 Primary-path rule

The primary path may support optimization.
The emergency path may not depend on optimization.

That separation is deliberate.

14. Isolated emergency oxygen path
14.1 Role

The isolated emergency oxygen path exists for one reason:

to remain available after earlier recovery attempts have already consumed primary margin.

14.2 Isolation posture

This path is isolated by:

separate storage,

separate valve posture,

separate regulator path,

explicit trigger logic,

protected physical routing.

14.3 Emergency-path rule

The emergency oxygen path must remain simpler than the nominal oxygen-management path.

In plain English:

if the pack is confused, damaged, or deeply derated, the last-line oxygen path must still be understandable and defensible.

14.4 Forbidden behavior

The architecture may not blur primary and reserve oxygen accounting in a way that lets “reserve” get spent gradually without making that fact explicit.

15. O2 make-up injection posture
15.1 Injection point

Oxygen should be injected into a controlled conditioned section of the gas loop downstream of major conditioning functions and upstream of return to the suit.

15.2 Why that matters

That placement helps:

stabilize gas mixing,

simplify make-up interpretation,

avoid contamination of upstream sensing logic,

and keep emergency oxygen delivery more direct.

15.3 Injection rule

The oxygen injection hardware must be compatible with:

primary oxygen,

regenerated oxygen,

and emergency oxygen entry,

while still preserving source isolation upstream.

16. S3 — Regenerative Extension Layer
16.1 Role

S3 exists to do one thing only:

extend survivability when doing so is physically affordable.

It does not exist to justify fantasy language like:

infinite oxygen,

no tank dependence,

or no battery dependence.

16.2 Baseline mechanism

The retained baseline mechanism is:

conditioned water input,

PEM electrolysis,

oxygen-side separation/conditioning,

controlled oxygen return into the oxygen continuity path,

hydrogen vent handling,

thermal integration with S5,

power permission from S6/S7.

16.3 Why PEM survived

It survived because it is:

a real oxygen-generation mechanism,

already aligned with real ECLSS logic,

understandable to aerospace reviewers,

and not dependent on exotic claims.

16.4 What S3 is not allowed to do

S3 may not:

replace the need for stored oxygen,

run whenever it wants,

consume dirty/unqualified water,

ignore thermal burden,

ignore B1/B2 electrical priority,

or silently turn a rescue pack into a power-hungry science project.

17. Regeneration permissive logic
17.1 Why permissive logic matters

Just because the electrolyzer exists does not mean turning it on is wise.

S3 requires a permission gate.

17.2 Minimum conditions for regeneration enable

Regeneration should only be enabled when all of the following are true:

B1 and B2 are stable

water feed is available and qualified

thermal state can absorb the added burden

the control layer is not in a deeper fault posture that forbids S3

the predicted survival payoff is positive

hydrogen vent path is confirmed available

reserve oxygen policy is not being violated by bad timing

17.3 Regeneration deny logic

S3 should be denied or dropped offline when any of the following are true:

battery state is too weak,

thermal margin is too low,

water quality is not trusted,

hydrogen vent path is suspect,

sensor confidence is degraded below a safe threshold,

the system has already entered deep Critical or Safe-Hold posture.

17.4 Important rule

The pack must be willing to say:
“regeneration is not affordable right now.”

That willingness is part of what makes the architecture serious.

18. Hydrogen byproduct handling
18.1 Why it matters

Hydrogen is an unavoidable byproduct of the retained regeneration path.

It is not a magical second energy source.
It is a byproduct that must be handled safely.

18.2 Hydrogen posture

The baseline posture is:

controlled routing,

separation from oxygen path,

vent or safe byproduct handling,

no reliance on complex hydrogen reuse to justify phase-1 architecture.

18.3 Rejected posture

IX-Breath phase 1 rejects:

hydrogen-as-free-bonus-power rhetoric,

ad hoc secondary fuel-loop claims,

or any closure logic that bloats the pack beyond credibility.

19. S4 — Water Recovery and Conditioning
19.1 Role

S4 exists to make water handling honest.

Its job is to:

collect condensate,

store recovered water,

track reserve water,

condition water before regeneration,

and support maintenance evidence.

19.2 Why S4 is not optional

Without S4, the architecture would be tempted to make fake claims like:

“the suit recovers water somehow,”

“electrolysis uses recovered moisture,”

or “regen is self-sustaining.”

That is exactly the nonsense this repo is trying to avoid.

19.3 Water sources

The baseline water sources are:

Source	Meaning
W1	recovered condensate from breathing loop
W2	isolated sterile reserve water
W3	service-side replenishment before EVA
19.4 Water priority

In phase 1, the architecture gives highest confidence to:

sterile reserve water,

conditioned recovered condensate,

anything else only after explicit validation.

19.5 Water rule

Recovered water is useful only after it has:

been captured,

measured,

conditioned,

and routed successfully.

20. Water-conditioning posture
20.1 Why conditioning exists

Recovered water may not be automatically suitable for PEM use.

Conditioning exists to reduce the chance that:

contamination,

particulates,

or unsuitable chemistry

destroy the regeneration path or create false confidence.

20.2 Conditioning rule

S3 should not be allowed to claim W1 water unless S4 has positively passed it through the conditioning path or equivalent permissive logic.

20.3 Service-side relevance

Water quality sampling and conditioning evidence also matter to S9 sustainment logic.

That means S4 is not just an onboard plumbing block.
It is part of the readiness story.

21. Life-support operating modes
21.1 Nominal mode

In Nominal mode:

closed-loop circulation is active,

primary oxygen path handles make-up demand,

CO2 and humidity control run normally,

water is recovered and logged,

regeneration is normally off or tightly limited,

emergency reserve remains isolated.

21.2 Conserve mode

In Conserve mode:

oxygen make-up policy tightens,

noncritical functions begin to shed,

S4 water accounting becomes more protective,

S3 may still remain off if the energy bargain is poor,

reserve oxygen remains isolated.

21.3 Rescue mode

In Rescue mode:

survival time becomes the main objective,

primary oxygen is preserved more aggressively,

regeneration may run only if permission logic supports it,

water handling becomes tightly coupled to survivability math,

reserve oxygen remains protected until trigger logic says otherwise.

21.4 Critical mode

In Critical mode:

life-support essentials remain,

regeneration may be denied completely,

only minimum sensing and O2 continuity remain defended,

the system prepares for reserve-oxygen criteria if needed,

crew alerts become persistent and simple.

21.5 Safe-Hold mode

In Safe-Hold:

the pack enters a latched degraded posture,

ambiguous auto-recovery is blocked,

essential survivability functions remain if physically possible,

diagnostic confidence is explicitly reduced if needed.

22. Fault handling posture
22.1 Life-support fault classes

At architecture level, the core recognizes these fault families:

Fault ID	Fault class
G1	blower failure or circulation degradation
G2	CO2-control degradation
G3	humidity / condensate path blockage or failure
G4	oxygen sensor disagreement
G5	CO2 sensor disagreement
G6	primary oxygen path failure
G7	reserve oxygen path actuation failure
G8	water contamination or unavailable water
G9	regeneration hardware failure
G10	gas leak / unexpected pressure loss
G11	control-command ambiguity during oxygen continuity event
22.2 Fault response posture
Fault	Intended first response
G1	shift to surviving circulation path, cut nonessential loads, alert crew
G2	classify severity, preserve survivable loop, bias toward simpler fallback posture
G3	continue breathing support, deny water credit until S4 state is re-established
G4	degrade confidence, cross-check remaining sensors, do not silently trust the outlier
G5	same as G4, with explicit crew-visible degraded confidence if needed
G6	preserve loop, evaluate reserve criteria, protect remaining oxygen sources
G7	declare deeper emergency posture; preserve any remaining primary path if available
G8	deny regeneration permission and preserve stored oxygen continuity
G9	isolate S3 if needed; continue core breathing support without pretending regen still exists
G10	preserve pressure awareness, simplify gas routing, move toward life-preserving fallback
G11	bias toward simpler protected oxygen-delivery logic and latched degraded state
22.3 Core safety rule

The pack is never allowed to hide that a degraded sensor set or degraded oxygen continuity decision is being made.

If confidence drops, the architecture must say so.

23. Leak and pressure-loss posture
23.1 Why this matters

No life-support architecture deserves trust if it ignores the possibility of gas loss.

23.2 Pressure-loss posture

Loop pressure and distribution pressure must be monitored for:

slow leakage,

sudden abnormal drop,

routing-path abnormality,

regulation failure.

23.3 Architecture rule

IX-Breath does not claim it can defeat every leak event.

What it does claim is narrower:

it can observe pressure behavior,

classify off-nominal loss posture,

simplify routing,

and protect remaining oxygen continuity logic from being spent blindly.

That is the honest lane.

24. Reserve-oxygen trigger posture
24.1 Why this needs explicit rules

If reserve oxygen opens too early, survivability margin is wasted.

If reserve oxygen opens too late, the crew may be endangered.

So the architecture requires explicit trigger logic, not vibes.

24.2 Trigger inputs

Reserve-oxygen entry should consider at minimum:

primary oxygen path availability,

loop oxygen state,

loop pressure state,

sensor confidence,

current mode state,

crew-commanded emergency input,

whether the regeneration path is available or denied.

24.3 Architecture rule

The reserve trigger must favor:

preserving the reserve,

but not gambling the crew on an over-optimistic delay.

The exact thresholds come later.
The rule is locked now.

25. Interfaces to S6 and S5
25.1 Interface to S6 Power Continuity Spine

The life-support core depends on S6 for:

B1 power to essential circulation and oxygen continuity hardware,

B2 power to critical sensing and supervision,

B3 power only when regeneration or selected water/thermal support is truly justified.

The life-support core sends S6/S7 the information needed to decide whether B3 can stay alive.

25.2 Interface to S5 Thermal Survivability

The life-support core depends on S5 for:

handling blower and control heat,

handling CO2-control thermal burden,

handling electrolyzer heat if S3 runs,

keeping reserve oxygen and control zones from being thermally abused by nearby hardware.

25.3 Important integration rule

S3 may not run merely because B3 exists.

It must also pass the S5 thermal affordability test.

26. Manual-control posture
26.1 Why manual posture exists

In a survival system, not every critical action should require rich automation to be healthy.

26.2 Manual minimums

The architecture should support a manual or guarded emergency path for at least:

rescue-mode entry,

acknowledgement of major alerts,

service isolation,

and protected emergency oxygen behavior.

26.3 Manual-control rule

Manual controls should be:

gloved-operable,

simple,

bounded,

and not dependent on the user decoding obscure state logic.

27. Verification posture

This document does not claim a validated life-support bench article exists yet.

It does define what later seriousness requires.

27.1 Future verification themes

A serious future validation path must eventually examine:

circulation continuity under one-path failure,

CO2-control performance under representative loop conditions,

humidity extraction and water-accounting closure,

reserve oxygen isolation integrity,

oxygen make-up stability,

sensor disagreement handling,

regeneration enable/deny correctness,

regeneration shutdown under power or thermal stress,

leak/posture classification behavior,

and deep-derate survival logic.

27.2 Seriousness rule

A happy-path gas-loop demo is not enough.

This architecture must be challenged under:

bad sensors,

denied regeneration,

one failed blower path,

weak battery conditions,

low thermal margin,

and explicit reserve-preservation tests.

Anything less is theater.

28. Locked design decisions from this document

The following life-support decisions are now locked unless later evidence forces revision:

The breathing loop is closed-loop / recirculating.

CO2 control is life critical.

Humidity handling must feed honest water accounting.

Oxygen continuity is layered into primary, reserve, and bounded regeneration.

Reserve oxygen remains isolated from routine depletion.

Regeneration is permission-gated, not assumed.

Hydrogen byproduct is handled safely and not counted as a bonus energy source.

Recovered water must be conditioned before it is credited to PEM use.

Sensor disagreement must degrade confidence explicitly, not silently.

The system must prefer simple last-line continuity over fragile optimization when conditions are bad.

29. What this document deliberately does not claim

This document does not claim:

final gas-loop flow rates,

final vessel pressures,

final scrubber sizing,

final PEM output rate,

final water recovery efficiency,

final reserve-oxygen trigger thresholds,

final oxygen partial-pressure limits,

or final survival duration.

Those belong to later analysis and validation.

What this document does claim is the important part for this stage:

IX-Breath now has a coherent detailed life-support architecture with explicit gas flow, layered oxygen continuity, honest water handling, and bounded regeneration logic.

That is enough for this commit.

30. What this file forces next

Because the life-support core is now defined, the next commits must show:

how thermal control, rescue solar, and water-oxygen-energy interaction behave together,

how the system state machine and FDIR logic command the core under failure,

how the mass, power, and consumables model judges whether the architecture is honest.

That is exactly the pressure this file is supposed to create.
