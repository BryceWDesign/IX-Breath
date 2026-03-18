# IX-Breath Master Architecture

**Document ID:** IXB-ARCH-03  
**Revision:** A  
**Status:** Active baseline  
**Repository phase:** Architecture study  
**Parent documents:**  
- `docs/00_PROJECT_CHARTER.md`  
- `docs/01_MISSION_REQUIREMENTS.md`  
- `docs/02_OPERATING_ASSUMPTIONS.md`

---

## 1. Purpose

This document defines the top-level IX-Breath system architecture.

Its job is to answer five questions clearly:

1. What are the major subsystem blocks?
2. How do gas, water, power, thermal load, and control signals move through the system?
3. What stays on the backpack, what stays outside it, and what belongs to the pressure garment?
4. What functions are life-critical versus supportive?
5. How does the architecture avoid turning one failure into total system loss?

This is a **serious master architecture document**, not a marketing summary.

---

## 2. Architecture statement

**IX-Breath is a rescue-biased regenerative portable life support system architecture built around a closed breathing loop, layered oxygen continuity, hardened electrical continuity, explicit fault isolation, and sustainment-readiness feedback.**

The architecture is intentionally built as a **layered survival stack**, not as a single “hero” subsystem.

That means survivability does **not** depend on one clever trick.

Instead, IX-Breath uses:

- one closed breathing loop,
- one isolated emergency oxygen reserve,
- one bounded oxygen-regeneration extension path,
- one hardened power continuity spine,
- one thermal survivability layer,
- one supervisory fault-management layer,
- one sustainment/readiness layer outside the backpack.

---

## 3. Top-level architectural logic

IX-Breath is built on one central rule:

> **No single essential survival function should depend on one unprotected component, one fragile control decision, or one optimistic assumption.**

That rule drives the entire architecture:

- oxygen continuity is layered,
- electrical continuity is layered,
- sensing is layered,
- thermal response is layered,
- control authority is layered,
- readiness evidence is layered.

This is the opposite of concept theater.

---

## 4. System boundary

### 4.1 Inside the IX-Breath backpack boundary

The backpack boundary includes:

- breathing-loop gas circulation hardware,
- carbon-dioxide and humidity control hardware,
- oxygen storage, regulation, and make-up hardware,
- bounded regenerative oxygen extension hardware,
- water recovery and conditioning hardware,
- thermal management hardware,
- electrical storage and protected distribution hardware,
- supervisory control / watchdog / blackbox hardware,
- crew emergency controls and alert outputs,
- structural shell and internal fault-containment layout.

### 4.2 Outside the IX-Breath backpack boundary

Outside the backpack boundary are:

- the astronaut pressure garment itself,
- helmet and suit garment gas volume,
- habitat / station / vehicle recharge infrastructure,
- dockside / ground-side readiness software and maintenance records,
- rescue operations and maneuvering systems,
- external communications infrastructure beyond local interface needs.

### 4.3 Boundary discipline

IX-Breath is **not** allowed to quietly solve architecture problems by assuming:
- a totally redesigned full suit,
- a totally redesigned spacecraft,
- unlimited external umbilical support,
- magical offboard computation,
- or unbounded external rescue assets.

The backpack must stand on its own as a serious contingency-survival architecture.

---

## 5. Top-level subsystem blocks

The IX-Breath architecture is divided into nine top-level subsystem blocks.

| Block ID | Name | Primary role |
|---|---|---|
| S0 | Pressure Garment Interface | Connects backpack to suit loop and crew controls |
| S1 | Life-Support Core | Closed breathing-loop circulation and gas conditioning |
| S2 | Oxygen Storage and Delivery Layer | Primary O2, isolated emergency O2, regulators, make-up manifold |
| S3 | Regenerative Extension Layer | Bounded O2 regeneration from conditioned water |
| S4 | Water Recovery and Conditioning Layer | Condensate capture, separation, storage, and conditioning |
| S5 | Thermal Survivability Layer | Heat transport, rejection, derating support, thermal buffering |
| S6 | Power Continuity Spine | Battery, pulse buffer, PMU, source combining, protected buses |
| S7 | Supervisory Control / FDIR / Crew Interface | State logic, watchdog, latching faults, alerts, logging |
| S8 | Structural Survivability Shell | Mechanical packaging, compartmentalization, puncture/impact mitigation |
| S9 | Sustainment and Readiness Layer | Offboard readiness, calibration evidence, maintenance intelligence |

S9 is architecturally part of IX-Breath as a system, but it is **not** carried on the astronaut’s back.

---

## 6. Master block diagram

```mermaid
flowchart LR

    Crew[Suited Crewmember / Pressure Garment Volume]

    subgraph IXB[IX-Breath Backpack]
        S0[ S0 Pressure Garment Interface ]
        S1[ S1 Life-Support Core ]
        S2[ S2 Oxygen Storage and Delivery ]
        S3[ S3 Regenerative Extension ]
        S4[ S4 Water Recovery and Conditioning ]
        S5[ S5 Thermal Survivability ]
        S6[ S6 Power Continuity Spine ]
        S7[ S7 Supervisory Control / FDIR / Crew Interface ]
        S8[ S8 Structural Survivability Shell ]
    end

    S9[ S9 Sustainment and Readiness Layer\nDockside / Groundside ]
    Ext[ Habitat / Station / Vehicle Recharge Interface ]

    Crew <-- Breathing gas loop --> S0
    S0 --> S1
    S1 --> S0

    S2 --> S1
    S3 --> S2
    S4 --> S3
    S1 --> S4

    S1 --> S5
    S3 --> S5
    S6 --> S1
    S6 --> S2
    S6 --> S3
    S6 --> S4
    S6 --> S5
    S6 --> S7

    S7 <--> S1
    S7 <--> S2
    S7 <--> S3
    S7 <--> S4
    S7 <--> S5
    S7 <--> S6

    S8 --- S1
    S8 --- S2
    S8 --- S3
    S8 --- S4
    S8 --- S5
    S8 --- S6
    S8 --- S7

    Ext <--> S6
    Ext <--> S2
    Ext <--> S4
    S7 <--> S9

7. Domain-flow view

The master architecture has five major flow domains:

Breathing-gas flow

Water flow

Electrical power flow

Thermal flow

Control / data / readiness flow

These are separated below because engineering discussions get sloppy fast when all five are blurred together.

8. Breathing-gas architecture
8.1 Breathing-gas design posture

The breathing side of IX-Breath uses a closed-loop / recirculating architecture rather than an open-loop consumables posture.

That choice is fundamental.

Open-loop behavior wastes oxygen too quickly for the rescue-survivability objective.

8.2 Breathing-gas functional path

The intended gas path is:

exhaled suit-loop gas returns from the pressure garment through S0,

gas enters S1 Life-Support Core,

gas passes through humidity / condensate handling,

gas passes through carbon-dioxide removal hardware,

gas is recirculated by blower hardware,

oxygen make-up from S2 is injected as needed,

conditioned gas returns to the pressure garment through S0.

8.3 Life-support core internal functions

S1 contains the functional equivalents of:

loop circulation blowers,

breathing-gas manifolds,

loop pressure sensing,

oxygen-state sensing,

carbon-dioxide-state sensing,

humidity-state sensing,

condensate capture interface,

make-up oxygen injection point,

optional trace-contaminant control placeholder volume if later justified.

8.4 Life-critical posture

The following S1 functions are treated as life critical:

loop circulation,

oxygen-state awareness,

carbon-dioxide management,

pressure integrity of the internal gas path,

emergency fallback operability.

8.5 Architecture rule

IX-Breath does not permit the isolated emergency oxygen path to depend on the exact same software decision chain as nominal breathing-loop optimization.

Emergency breathing support must remain simpler than nominal optimization logic.

9. Oxygen continuity architecture
9.1 Layered oxygen continuity

IX-Breath does not treat oxygen as one tank and one regulator.

It uses three oxygen layers:

Primary stored oxygen path

Isolated emergency oxygen reserve

Bounded regenerative oxygen extension path

9.2 S2 — Oxygen Storage and Delivery Layer

S2 contains the oxygen continuity hardware required to keep the breathing loop alive.

Its major roles are:

store primary breathable oxygen,

store isolated emergency oxygen,

regulate and meter oxygen delivery,

isolate emergency reserve from routine depletion,

accept regenerated oxygen from S3,

distribute make-up oxygen into S1.

9.3 Primary oxygen path

The primary path supports nominal closed-loop operation and rescue-mode operation until conditions warrant stronger conservation.

It is expected to include:

primary O2 tankage,

primary regulator path,

flow control / metering,

backflow protection,

pressure sensing,

feed into the make-up manifold.

9.4 Isolated emergency oxygen path

The isolated emergency oxygen reserve exists for one reason:

to prevent the architecture from “using up the last line of survival” during ordinary recovery attempts.

This path is separated from normal depletion logic and is only entered through explicit triggers.

That trigger posture will be defined later in the FDIR/state-machine commit.

9.5 S3 — Regenerative Extension Layer

S3 provides bounded oxygen regeneration based on retained, real-world mechanisms.

Its major roles are:

receive conditioned water from S4,

use electrical power from S6,

generate oxygen for controlled return into S2/S1,

vent or safely manage byproduct hydrogen,

support rescue extension,

remain subordinate to survivability accounting.

9.6 Critical architecture rule

S3 is never allowed to justify deleting or trivializing S2 emergency oxygen storage.

The regenerative layer is an extension layer, not a replacement for reserve oxygen.

10. Water recovery and conditioning architecture
10.1 Why water matters here

In IX-Breath, water is not just a thermal burden.

It is also:

a breathing-loop byproduct,

a thermal-control resource,

and potentially a feedstock for bounded oxygen regeneration.

That means water handling is a first-class subsystem, not an accessory.

10.2 S4 — Water Recovery and Conditioning Layer

S4 exists to capture, separate, store, and qualify water for two downstream purposes:

thermal / loop management,

oxygen-regeneration support.

10.3 Water functional path

The intended S4 path is:

S1 sends humid gas and condensate opportunity into S4,

S4 separates liquid water from the gas stream,

S4 stores recovered water in conditioned reservoirs or bounded intermediate volume,

S4 conditions or qualifies water before S3 can claim it,

S4 rejects water that cannot be honestly credited to regeneration.

10.4 Water honesty rule

IX-Breath is not allowed to count “recovered water” in survivability claims unless the water has:

a source path,

a storage path,

a conditioning path,

and a usable feed path.

Anything else is fake accounting.

11. Thermal survivability architecture
11.1 Thermal posture

Thermal control is treated as a survival-limiting function equal in seriousness to breathing and electrical continuity.

A system can have oxygen and power and still fail the crew if it cannot manage heat.

11.2 S5 — Thermal Survivability Layer

S5 exists to keep thermally sensitive hardware and crew-support processes within defined operating limits for as long as practical.

Its major roles are:

collect heat from electronics and life-support hardware,

move heat to rejection hardware,

support thermal buffering,

support rescue-mode derating,

define how operation changes across sunlit and eclipse conditions.

11.3 Intended thermal functions

S5 may include, at architecture level:

heat transport loop elements,

heat exchanger interfaces,

bounded thermal storage / phase buffer logic,

deployable or rescue-only rejection aids if justified,

thermal sensing and hotspot monitoring,

thermal derating logic inputs to S7.

11.4 Thermal discipline rule

Deployables or enhanced thermal surfaces are not “free gains.”

They cost:

packaging volume,

mechanical complexity,

failure modes,

deployment logic,

and operational handling burden.

That burden must be counted later.

12. Electrical architecture
12.1 Electrical posture

The electrical side of IX-Breath is not just a battery with wires.

It is a survivability bus architecture.

That means it exists to do three things at once:

power critical functions,

isolate faults,

keep enough awareness alive to manage recovery.

12.2 S6 — Power Continuity Spine

S6 is the electrical backbone of IX-Breath.

Its major roles are:

store primary electrical energy,

buffer transients and ride through pulsed loads,

combine and isolate power sources,

manage inrush and startup behavior,

distribute power by criticality tier,

protect branches,

dump or isolate unsafe stored energy,

receive recharge input from external interfaces,

optionally accept rescue-only solar augmentation.

12.3 Power sources

At the architecture level, S6 is expected to include:

primary battery energy storage,

pulse-buffer / supercapacitor layer,

protected recharge path,

optional rescue-only solar augmentation path,

PMU / bus control electronics,

branch-level protection,

safe-dump path.

12.4 Power buses by function

S6 conceptually divides the electrical distribution into at least four tiers:

Bus tier	Purpose
B1 Critical life-support bus	S1 core circulation and essential sensing
B2 Critical control bus	S7 supervisory core, watchdog, event logging minimum
B3 Recovery / extension bus	S3 regeneration hardware, nonessential analytical loads
B4 Noncritical support bus	rich telemetry, service functions, nonessential convenience features

The exact bus map may later refine, but the criticality split is already locked.

12.5 Electrical survival rule

No noncritical load is allowed to sit electrically “in front of” a life-critical load in a way that can starve survival hardware during a major fault or transient.

13. Control, FDIR, and human interface architecture
13.1 Why this layer exists

Without a real supervisory layer, IX-Breath is just a collection of subsystems hoping not to collide.

That is not serious enough.

13.2 S7 — Supervisory Control / FDIR / Crew Interface

S7 exists to make the backpack behave coherently under both nominal and off-nominal conditions.

Its major roles are:

observe the system,

classify the system state,

detect faults,

isolate or contain faults where possible,

command derating and load shedding,

preserve crew-understandable behavior,

log events,

interface to sustainment systems after the mission.

13.3 S7 internal roles

At the architecture level, S7 includes the functional equivalents of:

supervisory controller,

independent watchdog / health daemon,

state machine logic,

crew alert outputs,

emergency mode inputs,

blackbox / event recorder,

maintenance data export path,

minimal safe-hold interface.

13.4 Control design posture

S7 should be designed around bounded states, not free-form autonomy.

The minimum state set remains:

Nominal

Conserve

Rescue

Critical

Safe-Hold

Service

Detailed state logic comes later, but the architecture already assumes those states.

13.5 Human factors rule

In an emergency, the crew should not need to infer what the backpack is “trying to do.”

The pack must communicate simple, clear posture:

normal,

conserving,

rescue running,

critical,

safe-hold,

service only.

14. Structural survivability architecture
14.1 S8 — Structural Survivability Shell

S8 is not cosmetic housing.

It exists to preserve survivability when the environment is harsh and when one internal event should not automatically kill the rest of the pack.

Its major roles are:

mechanical support and packaging,

internal compartmentalization,

puncture / impact mitigation,

thermal shielding support,

controlled internal routing,

damage-containment posture.

14.2 Structural design posture

The shell is expected to separate and protect at least the following internal hazard zones:

oxygen storage and regulation zone,

power storage / PMU zone,

life-support gas-processing zone,

water-handling zone,

control and sensor zone.

14.3 Structural seriousness rule

IX-Breath does not assume an advanced shell makes the pack invulnerable.

The shell exists to improve survivability margins and fault containment, not to claim impossible durability.

15. Sustainment and readiness architecture
15.1 S9 — Sustainment and Readiness Layer

S9 is the offboard layer that prevents IX-Breath from becoming “looks fine until it suddenly isn’t.”

Its major roles are:

track calibration state,

track life-limited parts,

track scrubber / membrane / valve / battery health trends,

log readiness evidence,

log post-fault evidence,

support mission-go / no-go decisions,

feed maintenance intelligence back into operations.

15.2 Why S9 matters

The architecture assumes survival credibility is heavily influenced by what happened before the astronaut ever left the airlock.

That is why S9 is part of the system even though it is not worn on the back.

16. Flow-by-domain interface summary
16.1 Breathing-gas interfaces
From	To	Flow	Purpose
S0	S1	return gas from suit	bring exhaled / loop gas into conditioning path
S1	S0	conditioned breathing gas	return breathable gas to pressure garment
S2	S1	make-up oxygen	maintain loop oxygen state
S3	S2 / S1	regenerated oxygen	extend survivability under bounded conditions
16.2 Water interfaces
From	To	Flow	Purpose
S1	S4	humid gas / condensate opportunity	recover water from breathing loop
S4	S3	conditioned water	feed bounded regeneration path
S4	S5	thermal/water management linkage	support integrated water/thermal accounting
Ext	S4	recharge/service fluid path	support turnaround and sustainment
16.3 Electrical interfaces
From	To	Flow	Purpose
S6	S1	critical power	keep breathing loop alive
S6	S2	regulated power	actuate valves/regulators/sensing support
S6	S3	controlled power	support bounded regeneration only when affordable
S6	S5	thermal-control power	support thermal transport/rejection hardware
S6	S7	control/logging power	preserve minimum supervisory function
Ext	S6	recharge power	dockside / habitat recharge
Rescue solar path	S6	supplemental power	rescue-only augmentation if conditions allow
16.4 Thermal interfaces
From	To	Flow	Purpose
S1	S5	process heat	remove life-support hardware heat burden
S3	S5	regeneration heat	handle electrolyzer and conversion thermal burden
S6	S5	electrical waste heat	manage battery and PMU thermal load
S5	environment	rejected heat	preserve internal subsystem margins
16.5 Control / data interfaces
From	To	Flow	Purpose
S1/S2/S3/S4/S5/S6	S7	state and health telemetry	permit coherent supervision
Crew	S7	mode commands / emergency inputs	allow human-commanded rescue behavior
S7	subsystem blocks	commands / derates / isolations	manage faults and survivability posture
S7	S9	logs / readiness evidence	support sustainment and post-event reconstruction
17. Criticality tiers

IX-Breath uses explicit criticality tiers so that reviewers can see what must remain alive longest.

Tier	Meaning	Typical functions
T1	Life critical	loop circulation, O2 continuity, CO2 control minimum, minimum sensing, safe control core
T2	Survival critical	thermal-control essentials, emergency reserve actuation, event logging minimum
T3	Recovery-supporting	bounded regeneration, extended telemetry, richer state estimation
T4	Noncritical	convenience telemetry, service-only functions, nonessential analytics

Anything later added to the architecture must land in one of these tiers.

If it does not, it is not yet engineered clearly enough.

18. Normal operating logic at architecture level
18.1 Nominal

S1 runs closed-loop conditioning.

S2 supplies controlled oxygen make-up.

S3 is normally off or tightly limited.

S6 powers all necessary buses.

S7 observes and records.

S5 maintains thermal margins.

18.2 Conserve

S7 begins load shedding.

noncritical buses are reduced,

oxygen preservation logic tightens,

thermal and water handling begin rescue-minded behavior,

emergency reserve remains isolated.

18.3 Rescue

survival becomes the only objective,

noncritical loads are aggressively shed,

rescue-only augmentation may deploy,

S3 may operate when power/water conditions permit,

emergency reserve is preserved until defined criteria are crossed,

S7 remains in clear bounded behavior.

18.4 Critical

only the most important survival functions remain energized,

S7 issues persistent crew alerts,

the system prepares for last-line oxygen and minimum-control posture.

18.5 Safe-Hold

the system enters a latched degraded state when return to normal would be unsafe,

minimum protected function remains alive if physically possible,

the crew is not left guessing whether the system is nominal again.

Detailed logic comes later.
This section exists to show that the architecture already has a coherent posture.

19. Fault containment philosophy

IX-Breath does not assume failures will be rare enough to ignore.

It assumes some failures will happen and tries to stop them from cascading.

19.1 Intended containment principles

Compartmentalize hazards

power faults should not instantly kill the oxygen path,

oxygen-path faults should not instantly wipe the control core,

thermal hotspots should not automatically poison the entire pack.

Separate emergency reserve from optimization logic

the “last line” stays out of routine depletion.

Separate pulse loads from quiet sensing

noisy hardware should not destabilize critical sensing.

Prefer latched safe degradation over ambiguous auto-recovery

false normal is worse than clearly degraded.

Keep minimum awareness alive when possible

dead-silent failure is unacceptable if any awareness can still be preserved.

19.2 What this architecture is explicitly trying to survive better

At master-architecture level, IX-Breath is specifically trying to improve posture against:

loss of immediate ingress opportunity,

electrical transient or partial bus fault,

non-catastrophic sensor or controller fault,

thermal overburden forcing derate,

gradual degradation missed before EVA,

depletion of nominal oxygen path before rescue.

This is not yet a complete hazard analysis.
That comes later.

20. Architecture decisions locked by this document

The following decisions are now locked unless later evidence forces revision:

IX-Breath is a closed-loop breathing architecture.

IX-Breath uses layered oxygen continuity, not one undifferentiated oxygen source.

IX-Breath includes an isolated emergency oxygen reserve.

IX-Breath may use bounded regeneration, but regeneration is subordinate to real accounting.

IX-Breath treats water handling as first-class architecture.

IX-Breath uses a protected multi-tier power spine.

IX-Breath uses bounded operating states, not open-ended autonomy.

IX-Breath includes an offboard sustainment layer as part of system seriousness.

IX-Breath treats thermal control as survival-critical.

IX-Breath requires compartmentalized fault containment in the pack structure.

These are no longer optional design vibes.
They are baseline architecture rules.

21. Architecture choices deliberately rejected at this stage

The following are deliberately not part of the IX-Breath master architecture baseline:

open-loop primary rescue survival strategy,

infinite or unbounded oxygen claims,

ambient RF as primary life-support power,

triboelectric/piezoelectric primary-power claims,

over-unity or perpetual-motion framing,

Element 115 dependence,

undefined “field” effects,

giant external energy-harvesting structures,

stealth/weapon/offense framing,

anything that quietly turns the backpack into a one-person spacecraft without admitting it.

A more detailed donor keep/reject table will be added next.

22. Unresolved architecture risks

A serious architecture document should admit what is still unresolved.

At this stage, the biggest unresolved items are:

whether the bounded regeneration layer can be package-efficient enough for the backpack target envelope,

whether the thermal-control burden of extended rescue mode is affordable inside the package envelope,

how much conditioned water can be honestly credited under multiple rescue cases,

what minimum critical sensor set is sufficient without overcomplicating the pack,

where the best line sits between fail-operational complexity and fail-safe simplicity,

how much deployable hardware is acceptable before wearability degrades too far,

how the emergency reserve trigger logic should be set to avoid premature use and avoid unsafe delay.

These are real engineering questions.
They are not weaknesses of the repo.
They are evidence that the repo is being honest.

23. What this architecture is trying to look like to a serious reviewer

A serious reviewer should read this document and conclude:

the scope is bounded,

the subsystems are identifiable,

the flows are coherent,

the survival logic is layered,

the architecture is not pretending to be tested hardware,

the repo is trying to earn credibility by structure.

That is the correct outcome for this stage.

Anything more would be maturity inflation.

24. Next documents forced by this architecture

Because this architecture is now defined, the repository must next provide:

the donor-feature keep/reject matrix,

the packaging envelope and suit-integration posture,

the fresh real-world BOM,

the power-spine detailed architecture,

the life-support-core detailed architecture.

That order is intentional.
